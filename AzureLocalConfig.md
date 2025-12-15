# Azure Local Config Script
### 24H2 using Dell Golden Image

**Change the following lines to match your deployment**
- ***Integrated NIC 1 Port 1-1***
- ***VlanID 3220*** 
- ***HostName*** 

```
Get-NetAdapter | ? InterfaceDescription -inotmatch "NDIS" | Set-NetIPInterface -Dhcp Disabled
Get-NetAdapter | Where-Object {$_.status -eq "disconnected"} | Disable-NetAdapter -confirm:$false

New-NetIPAddress -InterfaceAlias "Integrated NIC 1 Port 1-1" -IPAddress 10.10.10.101 -DefaultGateway 10.10.10.1 -PrefixLength 24 -AddressFamily IPv4 -Verbose

Get-NetAdapter -Name "Integrated NIC 1 Port 1-1" | Set-NetAdapter -VlanID 3220 -Confirm:$false

Set-DnsClientServerAddress -InterfaceAlias "Integrated NIC 1 Port 1-1" -ServerAddresses 1.1.1.1,1.1.2.2

Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server" -Name "fDenyTSConnections" -Value 0
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
New-Item -Path HKLM:\system\currentcontrolset\services\clussvc
New-Item -Path HKLM:\system\currentcontrolset\services\clussvc\parameters
New-ItemProperty -Path HKLM:\system\currentcontrolset\services\clussvc\parameters -Name ExcludeAdaptersByDescription -Value "Remote NDIS Compatible Device"

w32tm /config /manualpeerlist:"1.1.1.1,1.1.2.2" /syncfromflags:manual /update

w32tm /query /status
winrm quickconfig
netsh advfirewall firewall add rule name="ICMP Allow incoming V4 echo request" protocol=icmpv4:8,any dir=in action=allow

Rename-Computer -NewName HostName -Restart

```

# Azure Registration/Onboarding
### This will Install MS updates and begin the Arc Registration. Server may reboot after updates are applied. If this happens just rerun the script and it will pick up where it left off. 

```
$Subscription = "SubcriptionID"
$RG = "ResourceGroup"
$Region = "eastus"

Invoke-AzStackHciArcInitialization -SubscriptionID $Subscription -ResourceGroup $RG -Region $Region -Cloud "AzureCloud" 
```

# Display Dell Golden Image BuildInfo

```
Get-ItemPropertyValue -Path "HKLM:\Software\Dell\AX\GoldenImage\BuildInfo" -Name "ImageFileName"
```
