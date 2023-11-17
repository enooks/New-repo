# passwd
```bash
echo "linuxpassword" | passwd --stdin linuxuser
```

or

```bash
echo -e "linuxpasswordnlinuxpassword" | passwd linuxuser
```

# fdisk

```bash
(echo n; echo p; echo3; echo -e "\n" echo -e "\n" echo t; echo -e "\n"; echo 8e; echo w;) | /usr/sbin/fdisk /dev/sda
```