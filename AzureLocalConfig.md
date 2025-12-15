# Azure Local Config Script
Change the following lines to match your deployment
InterfaceAlias "Integrated NIC 1 Port 1-1" 
VlanID in "Get-NetAdapter -Name "Integrated NIC 1 Port 1-1" | Set-NetAdapter -VlanID 3220 -Confirm:$false"

HostName in Line "Rename-Computer -NewName HostName -Restart" 

```
Get-NetAdapter | ? InterfaceDescription -inotmatch "NDIS" | Set-NetIPInterface -Dhcp Disabled
Get-NetAdapter | Where-Object {$_.status -eq "disconnected"} | Disable-NetAdapter -confirm:$false

New-NetIPAddress -InterfaceAlias "Integrated NIC 1 Port 1-1" -IPAddress 10.3.220.13 -DefaultGateway 10.3.220.1 -PrefixLength 24 -AddressFamily IPv4 -Verbose
Get-NetAdapter -Name "Integrated NIC 1 Port 1-1" | Set-NetAdapter -VlanID 3220 -Confirm:$false

Set-DnsClientServerAddress -InterfaceAlias "Integrated NIC 1 Port 1-1" -ServerAddresses 10.30.22.26,10.1.220.13

Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server" -Name "fDenyTSConnections" -Value 0
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
New-Item -Path HKLM:\system\currentcontrolset\services\clussvc
New-Item -Path HKLM:\system\currentcontrolset\services\clussvc\parameters
New-ItemProperty -Path HKLM:\system\currentcontrolset\services\clussvc\parameters -Name ExcludeAdaptersByDescription -Value "Remote NDIS Compatible Device"

w32tm /config /manualpeerlist:"10.30.22.26,10.1.220.13" /syncfromflags:manual /update
w32tm /query /status
winrm quickconfig
netsh advfirewall firewall add rule name="ICMP Allow incoming V4 echo request" protocol=icmpv4:8,any dir=in action=allow

Rename-Computer -NewName HostName -Restart
```

#Get Dell Golden Image BuildInfo

```
Get-ItemPropertyValue -Path "HKLM:\Software\Dell\AX\GoldenImage\BuildInfo" -Name "ImageFileName"
```
