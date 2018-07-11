# 1c-server-repo

## Что это?

1c-server-repo -- это сервер хранилища конфигураций 1С:Предприятия в контейнере Docker.

## Как это установить?

Для установки и начального запуска проверьте каталог deb репозитория и если нет необходимой версии, то получите дистрибутив сервера 1С:Предприятия: https://users.v8.1c.ru/ -> Скачать обновления -> Технологическая платформа 8.3 -> ВЕРСИЯ -> Cервер 1С:Предприятия для DEB-based Linux-систем -> Скачать дистрибутив. Полученный дистрибутив с расширением tar необходимо переименовать 1c-server-<Версия конифгурации> (например: 1c-server-8.3.11-3034.tar).

Клонируйте репозиторий:

    git clone git@git.wwind.ua:docker/1c-server-repo.git

Скопируйте полученный tar-файл дистрибутива сервера 1С:Предприятия в каталог `1c-server-repo\debs\`. Проверьте наличие файла 1c-server-repo-<номер версии сервера>.yml для необходимой версии и при необходимости скопируйте из существующего файла. Измените в нем значения аргументов (номер версии и порт на котором будет работать сервер)

    - SRV1CV8_VERSION=8.3.11-3034
    - SRV1CREPO_PORT=1562

а также версию сервиса, образа и его имя (заменить номер версии)

    services:
        1c-server-repo-8.3.11-3034:

        image: xelon/1c-server-repo:8.3.11-3034
        ...
        container_name: 1c-server-repo-8.3.11-3034
    
и затем выполните команды (здесь и далее вам понадобятся права администратора):

    cd 1c-server-repo\
    ./run.sh `номер версии сервера`
    
Например:

    ./run.sh 8.3.11-3034

Узнать UID и GID пользователя, с правами которого сервер 1С:Предприятия работает в контейнере, можно с помощью команды

    docker exec 1c-server id usr1cv8

которая выдаст примерно такие данные

    uid=999(usr1cv8) gid=1000(grp1cv8) groups=1000(grp1cv8)

## Как остановить/запустить/перезапустить контейнер?

Для управления контейнером используйте команды:

    docker stop 1c-server-repo-<номер версии сервера>
    docker start 1c-server-repo-<номер версии сервера>
    docker restart 1c-server-repo-<номер версии сервера>

## Где мои данные?

Данные сервера 1С:Предприятия вы можете найти в каталогах `/var/lib/docker/volumes/1c-server-repos/_data/` (каталог репозиториев для всех версий сервера. В нем в подпапках с номером версии будут лежать репозитории конкретного сервера).

## Как это удалить?

Удалите контейнер:

    docker rm -f 1c-server-repo-<номер версии сервера>

Удалите образ:

    docker rmi xelon/1c-server-repo:<номер версии сервера>

:fire: Удалите данные:

    docker volume rm 1c-server-repos
