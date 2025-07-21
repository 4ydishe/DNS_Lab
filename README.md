# Домашнее задание: Split-DNS на основе BIND9, Vagrant и Ansible

## Использование
```bash
vagrant up
```
## Особенности
- DNS-серверы `ns01` (master) и `ns02` (slave)
- Настройка разделения видимости зон через `view` и `acl` в `named.conf`
- Конфигурация и развертывание автоматизированы с помощью Ansible

  На машине ns01 запустить playbook ansible
```bash
cd /vagrant/provisioning
```

```bash
ansible-playbook -i inventory.ini playbook.yml
```
