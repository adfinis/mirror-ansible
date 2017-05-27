# Ansible Playbook - Package Mirror

## Information
This playbook is used to initially deploy a mirror server. Currently there are
roles for the following distributions available:
* Arch Linux
* CentOS
* Debian
* Debian Security
* Dotdeb
* EPEL
* openSUSE
* Ubuntu

## Deployment Instructions

### Whole Mirror
To deploy the whole mirror setup, just execute the following commands:
```bash
$ echo "<server>" > /tmp/production.txt
$ ansible-playbook -i /tmp/production.txt site.yml -u root --ask-pass
```

### Custom Mirror
If you just want to deploy a single part of the mirror, you have to create a 
new playbook. For example:
```yaml
---
# file: mymirror.yml
- hosts: all
  roles:
    - common
    - goaccess
    - archlinux
```
And use it like site.yml
```bash
$ echo "<server>" > /tmp/mymirror.txt
$ ansible-playbook -i /tmp/mymirror.txt mymirror.yml -u root --ask-pass
```
### nginx configuration

As of Ansible 1.7, there was no blockinfile available. 
We decided to manually edit the file `/etc/nginx/conf.d/mirror-locations`
so we have full control over the mirrors we expose.

You can add configuration pieces like the following to expose the mirror:

```
location /opensuse {
    alias /var/www/mirror/opensuse;
    autoindex on;
}
```

## License

GNU GENERAL PUBLIC LICENSE Version 3
