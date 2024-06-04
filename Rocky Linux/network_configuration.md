# 설정할 Device 이름 확인 방법

```shell
nmcli connection show

NAME              UUID                                                             TYPE    DEVICE
System ens192  03da7500-2101-c722-2438-d0d006c28c73  ethernet  ens192
```

# edit 메뉴 확인

```shell
nmcli connection edit "System ens192"
> NAME에 적혀있는 System ens192 입력

===| nmcli interactive connection editor |===

Editing existing '802-3-ethernet' connection: 'ens192'

Type 'help' or '?' for available commands.
Type 'print' to show all the connection properties.
Type 'describe [<setting>.<prop>]' for detailed property description.

You may edit the following settings: connection, 802-3-ethernet (ethernet), 802-1x, dcb, sriov, ethtool, match, ipv4, ipv6, hostname, tc, proxy
nmcli>
```

# network 설정 확인

```shell
nmcli> print ipv4
['ipv4' setting values]
ipv4.method:                            manual
ipv4.dns:                               8.8.8.8,9.9.9.9
ipv4.dns-search:                        --
ipv4.dns-options:                       --
ipv4.dns-priority:                      0
ipv4.addresses:                         10.10.214.89/24
ipv4.gateway:                           --
ipv4.routes:                            --
ipv4.route-metric:                      -1
ipv4.route-table:                       0 (unspec)
ipv4.routing-rules:                     --
ipv4.ignore-auto-routes:                yes
ipv4.ignore-auto-dns:                   yes
ipv4.dhcp-client-id:                    --
ipv4.dhcp-iaid:                         --
ipv4.dhcp-timeout:                      0 (default)
ipv4.dhcp-send-hostname:                yes
ipv4.dhcp-hostname:                     --
ipv4.dhcp-fqdn:                         --
ipv4.dhcp-hostname-flags:               0x0 (none)
ipv4.never-default:                     yes
ipv4.may-fail:                          yes
ipv4.required-timeout:                  -1 (default)
ipv4.dad-timeout:                       -1 (default)
ipv4.dhcp-vendor-class-identifier:      --
ipv4.link-local:                        0 (default)
ipv4.dhcp-reject-servers:               --
nmcli>
```

# IP 설정

```shell
nmcli> set ipv4.address 'ip'/'prefix'
> prefix를 붙이지 않으면 default로 /32 설정
```

# Gateway 설정

```shell
nmcli> set ipv4.gateway 'gateway_ip'
```

# DNS 설정

```shell
nmcli> set ipv4.dns 'dns_ip'
```

# NAME 변경 방법

```shell
nmcli> set connection.id '변경할 NAME'
```

# 삭제 방법

```shell
nmcli> remove ipv4.address 'ip'/'prefix'
nmcli> remove ipv4.gateway 'gateway_ip'
nmcli> remove ipv4.dns 'dns_ip'
```
