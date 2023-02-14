# passwd
```shell
echo "linuxpassword" | passwd --stdin linuxuser
```

or

```shell
echo -e "linuxpasswordnlinuxpassword" | passwd linuxuser
```

# fdisk

```shell
(echo n; echo p; echo3; echo -e "\n" echo -e "\n" echo t; echo -e "\n"; echo 8e; echo w;) | /usr/sbin/fdisk /dev/sda
```