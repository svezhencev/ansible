# Stencil ansible repo

Шаблон репозитория для ансибл проекта

- работа с плейбуками
- работа с ролями

## Основные компоненты

- galaxy - для установки и работы с ролями из репозитория галакси
- molecule - создание своих ролей
- vagrant - для тестирования ролей локально

## структура

- **app**: папка для размещения шаблонов j2 (к примеру nginx.conf.j2) - jinja шаблоны
- **playbooks**: плейбуки - почему несколько допустим
  - main.yml - для основного сетапа сервера
  - deploy.yaml - для деплоя приложения
  - rollback.yaml - для отката
- **roles**: кастомные роли приложения
- **vars**: конфиги для ролей - допустим используем с галакси nginx и делаем такой файл `vars/nginx.yml`

```yml
---
nginx_enable: true
nginx_cleanup_config: true
nginx_http_template_enable: true
nginx_http_template:
  default:
    template_file: ../app/nginx.conf.j2
    conf_file_name: default.conf
```

- **vendor**: тут устанавливаются роли из репозитория
- **requirements.yml**: в этом файле размещаем роли которые хотим использовать из реп galaxy

```yml
- src: linuxhq.yum
- src: robertdebock.selinux
- src: weareinteractive.users
- src: nginxinc.nginx
- src: andrewrothstein.gcc-toolbox
- src: geerlingguy.git
- src: geerlingguy.repo-epel
- src: geerlingguy.php
- src: geerlingguy.repo-remi
- src: linuxhq.percona
- src: robertdebock.reboot
```

## подготовка к разработке

> python3 -m venv .venv

> pip-compile requirements.in

> ansible-lint playbooks/playbook.yaml

> vagrant plugin install vagrant-hostsupdater

> ansible-galaxy install -r requirements.yml

## создание роли

> molecule init role -r itsumma.python
