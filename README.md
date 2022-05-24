# ansible-role-workstation-env-switcher

Use this role to easily switch configuration of a workstation Linux system.

For example you can configure an environment for your `home` and one for your `company`.

The script can configure the proxy or the DNS for multiple services.

Supported configurations:

- proxy:
  - environment (/etc/environment)
  - apt
  - docker
  - gnome

- dns:
  - dnsmasq
  - docker


## Requirements

- Ansible > 2.10


## Role Variables

TODO


## Example Playbook

```yaml
- hosts: localhost
  connection: local
  become: True

  vars:
    environments:
      hpe:
        proxy:
          enabled: True
          config:
            http_proxy: http://web-proxy.bbn.hpecorp.net:8080
            https_proxy: http://web-proxy.bbn.hpecorp.net:8080
            no_proxy: localhost,127.0.0.1,::1,hpecorp.net,c4lab.local
          services:
            - environment
            - apt
            - docker
            - gnome
        dns:
          config:
            nameserver:
              - 1.1.1.1
          services:
            - dnsmasq
            - docker
      home:
        proxy:
          enabled: False
          services: # we need to declare services anyway so we can check that the proxy config is not applied
            - environment
            - apt
            - docker
            - gnome
        dns:
          config:
            nameserver:
              - 16.110.135.52
          services:
            - dnsmasq
            - docker

  roles:
    - ansible-role-workstation-env-switcher
```

```
ansible-playbook -v playbook.yml -K -e "env=home"
```

## License

MIT
