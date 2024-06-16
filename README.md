# Ansible Set-Hostname Role

## Actions of the Role

* Set hostname via systemd
* Copy /etc/hostname template in place
* Copy /etc/hosts template in place

## Common Usage

```yaml
roles:
- { role: francomile.set-hostname,
    # Set default instance hostname, by default it will pick the inventory hostname.
    set_hostname_default_hostname: "{{ inventory_hostname }}",
    set_hostname_default_ip: "{{ ansible_default_ipv4.address|default(ansible_all_ipv4_addresses[0]) }}",
    set_hostname_set_hosts_file: false, # boolean to define if you whish to replace /etc/hosts with your template. Default is false
    #  If you set your set_hostname_set_hosts_file to 'true', add your desired entries for /etc/hosts file here in this dictionary below.
    # The entry will be added to /etc/hosts in format below:
    # <ipaddress> <hostname/fqdn>
    set_hostname_hosts_list: {
      entry0: {
        ip_address: "{{ default_ip }}",
        host_name: "{{ default_hostname }}"
      },
      entry1: {
        ip_address: "172.23.132.17",
        host_name: "www.myhost.tld"
      }
    },
    tags: ["set-hostname"]
    }
```

## Run the playbook

Run without setting /etc/hosts file:

```shell
ansible-playbook -i inventory playbook.yaml --tags "set-hostname"
```

If you need to copy your /etc/hosts template, you can enable it on runtime by defining an extra variable for setting `set_hostname_set_hosts_file` to `true`:

```shell
ansible-playbook -i inventory playbook.yaml --tags "set-hostname" -e '{"set_hostname_set_hosts_file": true}'
```
