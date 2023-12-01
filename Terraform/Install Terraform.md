# Linux

## CentOS/RHEL

- Install `yum-config-manager` to manage your repositories.

```shell-session
$ sudo yum install -y yum-utils
```

- Use `yum-config-manager` to add the official HashiCorp Linux repository.

```shell-session
$ sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
```

- Install Terraform from the new repository.

```shell-session
$ sudo yum -y install terraform
```

## Enable tab completion

```shell-session
$ terraform -intsall-autocomplete
```

