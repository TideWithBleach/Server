# Basic Script to configure iDRAC from front USB Port of server. 
### Must have iDRAC RAC Tools installed. 
### Update iDRAC Variables before running. 
```
$u='root'
$p='calvin'
$ip='169.254.0.3'
$hostname='FS-HCL-H1'
$idracip='10.2.80.11'
$idracgw='10.2.80.1'
$idracmask='255.255.255.0'
$idracdns1='192.168.151.41'
$idracdns2='172.16.15.41'
$idracntp1='10.110.30.156'
$idracntp2='10.110.93.223'
$timezone='EST5EDT'

racadm --nocertwarn -r $ip -u $u -p $p set idrac.Tuning.DefaultCredentialWarning 0
racadm --nocertwarn -r $ip -u $u -p $p set idrac.ipv4.dhcpenable 0
racadm --nocertwarn -r $ip -u $u -p $p set idrac.ipv4.enable enable
racadm --nocertwarn -r $ip -u $u -p $p set idrac.ipv4.gateway $idracgw
racadm --nocertwarn -r $ip -u $u -p $p set idrac.ipv4.address $idracip
racadm --nocertwarn -r $ip -u $u -p $p set idrac.ipv4.netmask $idracmask
racadm --nocertwarn -r $ip -u $u -p $p set idrac.ipmilan.enable enabled
racadm --nocertwarn -r $ip -u $u -p $p set idrac.nic.dnsracname $hostname"-iDRAC"
racadm --nocertwarn -r $ip -u $u -p $p set idrac.ipv4.DNS1 $idracdns1
racadm --nocertwarn -r $ip -u $u -p $p set idrac.ipv4.DNS2 $idracdns2
racadm --nocertwarn -r $ip -u $u -p $p set idrac.time.timezone $timezone
racadm --nocertwarn -r $ip -u $u -p $p set idrac.ntpconfiggroup.ntp1 $idracntp1
racadm --nocertwarn -r $ip -u $u -p $p set idrac.ntpconfiggroup.ntp1 $idracntp2
racadm --nocertwarn -r $ip -u $u -p $p set idrac.ntpconfiggroup.ntpenable 1
racadm --nocertwarn -r $ip -u $u -p $p get idrac.ipv4

```
