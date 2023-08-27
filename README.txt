Цели:
  Установить зафисимости для системы автоматического тестирования molecule
  Настроить molecule с использованием docker
  Создать роль, таски
  Создать тесты
  Прогнать тесты с ролью

0. Подготовка среды и создание роли

1. Создание таск

2. Создание тестов

3. Тестирование

0.0 (Для запуска)

$ source molecule/bin/activate

0.1. Установить зависимости

Установить python3, ansible, molecule

0.2. Создать вирутальное окружения и доустановить внесистемные зависимости

$ virtualenv molecule
$ source molecule/bin/activate
$ pip3 install "molecule[lint,ansible,docker]" && pip3 install molecule-docker

0.3. Создание роли

$ mkdir roles
$ cd roles
$ molecule init role my.webapp --driver-name=docker

0.4. Настройка для docker
Внутри файла ./roles/webapp/molecule/default/molecule.yml меняю платформу на ubuntu от geerlingguy. В стандартной поставке не стоит python, поэтому ansible падает.

platforms:
  - name: ubuntu
    image: geerlingguy/docker-ubuntu2204-ansible:latest
    pre_build_image: true


1.1. Создание таск

Внутри файла ./roles/webapp/tasks/main.yml описано:
Обновление репозитория и обновление дистрибутива
Установка gpg # Нужен для ppa
Установка dirmngr # Нужен для ppa
Добавление репозитория ppa с Go # В ubuntu нет Go, поэтому нужен ppa
Установлка Go

2.1 Создание тестов

Внутри файла ./roles/webapp/molecule/default/verify.yml описано:
Проверка кода возврата команды go version # Очевидно, что если Go не встал, то будет не 0
Вывод stdout этой команды

3.1 Тестирование
Запускаю:
$ molecule converage
$ molecule verify

И радуюсь выводу!


