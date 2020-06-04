## Laboratory work VIII

Данная лабораторная работа посвящена изучению систем автоматизации развёртывания и управления приложениями на примере **Docker**


## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab08** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Выполнить инструкцию учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

Создание переменных среды и установка их значений, а также связывание команд с их "новыми" названиями.
```sh
$ export GITHUB_USERNAME=kbrotskiy
```

Начало работы в каталоге `workspace`
```sh
# Переход в рабочую директорию
$ cd ${GITHUB_USERNAME}/workspace
$ pushd . #Сохранение текущего каталога
$ source scripts/activate

```
Настройка git-репозитория **lab07** для работы
```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab07 lab08
$ cd lab08
$ git submodule update --init
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab08

```
Создаем `Dockerfile`. В самом начале указываем базовый образ, который определяет рабочую среду, пакеты и утилиты. В Dockerfile содержатся инструкции по созданию образа.
```sh
$ cat > Dockerfile <<EOF
FROM ubuntu:18.04
EOF
```
Выполняем обновление списка пакетов **APT** в базовом образе - утилиты для управления пакетами. Затем с его помощью устанавливаем компиляторы и `CMake`.
```sh
$ cat >> Dockerfile <<EOF
```

```sh
$ cat >> Dockerfile <<EOF
COPY . print/
WORKDIR print
EOF
```

```sh
$ cat >> Dockerfile <<EOF
# Выполняем сборку проекта
RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
RUN cmake --build _build
RUN cmake --build _build --target install
EOF
```

Инструкция `ENV` позволяет задавать постоянные переменные среды, которые будут доступны в контейнере во время его выполнения.
```sh
$ cat >> Dockerfile <<EOF
ENV LOG_PATH /home/logs/log.txt
EOF

```
Инструкция `VOLUME` позволяет указать место, которое контейнер будет использовать для постоянного хранения файлов и для работы с такими файлами.
```sh
$ cat >> Dockerfile <<EOF
# Указываем в какой директории будут храниться файлы, которые останутся после работы с контейнером
VOLUME /home/logs
EOF

```
Переход в созданную в процессе сборки директорию `_install/bin`
```sh
$ cat >> Dockerfile <<EOF
WORKDIR _install/bin
EOF

```
Инструкция `ENTRYPOINT` позволяет задавать команду с аргументами, которая должна выполняться при запуске контейнера. 
$ cat >> Dockerfile <<EOF
ENTRYPOINT ./demo
EOF

```
Сборка образа с тегом `logger` и путем к `Dockerfile`
```sh
$ docker build -t logger .
```
```
Команда, которая выводит всю информацию об образах
```sh
$ docker images
```
```sh
$ mkdir logs
$ docker run -it -v "$(pwd)/logs/:/home/logs/" logger /bin/bash

```
Вывод подробной информации о контейнере
```sh
$ docker inspect logger
```
Проверяем, что файлы, полученные во время работы в контейнере были сохранены
```sh
$ cat logs/log.txt

```
Замена lab07 на lab08
```sh
$ gsed -i 's/lab07/lab08/g' README.md
```

Изменение `.travis.yml` для сборки в контейнере.
```sh
$ vim .travis.yml

```
Добавление **Docker** в репозиторий.
```sh
$ git add Dockerfile
$ git add .travis.yml
$ git commit -m"adding Dockerfile"
$ git push origin master

```
Активация непрерывной интеграции с **Travis CI**.
```sh
$ travis login --auto --com
Successfully logged in as kbrotskiy!
$ travis enable --com
kbrotskiy/lab08: enabled :)
```

## Report

```sh
Создание отчета по ЛР № 8
$ popd
$ export LAB_NUMBER=08
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Links

- [Book](https://www.dockerbook.com)
- [Instructions](https://docs.docker.com/engine/reference/builder/)

```
Copyright (c) 2015-2019 The ISC Authors
```
