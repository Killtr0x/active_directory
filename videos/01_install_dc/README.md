# 01 Installing the Domain Controller 

1. Use `sconfig` to:
    - Change the hostname
    - Change the IP address to static
    - Change the DNS server to our own IP address

2. Install the Active Directory Windows Feature

```shell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```
3. Reconfigured DNS after Active Directory installation
    - Used to identify `InterfaceIndex`
```shell
Get-NetIPAddress 

Ex.

IPAddress         : 192.168.xx.xx
`InterfaceIndex    : 7`
InterfaceAlias    : Ethernet0
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Dhcp
SuffixOrigin      : Dhcp
AddressState      : Preferred
ValidLifetime     : 00:29:00
PreferredLifetime : 00:29:00
SkipAsSource      : False
PolicyStore       : ActiveStore
```

4. Used to ifentify current DNS assignment
```shell
Get-DnsClientServerAddress
 
 Ex. 

InterfaceAlias               Interface Address ServerAddresses
                            Index     Family
--------------               --------- ------- ---------------
`Ethernet0                            7 IPv4    {192.168.xx.2}`
Ethernet0                            7 IPv6    {}
Bluetooth Network Connection         5 IPv4    {}
Bluetooth Network Connection         5 IPv6    {fec0:0:0:ffff::1, fec0:0:0:ffff::2, fec0:0:0:ffff::3}
Loopback Pseudo-Interface 1          1 IPv4    {}
Loopback Pseudo-Interface 1          1 IPv6    {fec0:0:0:ffff::1, fec0:0:0:ffff::2, fec0:0:0:ffff::3}
```

5. Used to set DNS for desired interface
```shell
Set-DnsClientServerAddress -InterfaceIndex `Id` -ServerAddresses `IP Address`
 ```

6. Joined client workstation to domain
```shell
Add-Computer -DomainName domain.com -Credential AD\User -Force -restart
```