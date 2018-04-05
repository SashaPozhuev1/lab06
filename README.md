[![Build Status](https://travis-ci.org/SashaPozhuev1/lab06.svg?branch=master)](https://travis-ci.org/SashaPozhuev1/lab06)
# lab06
Laboratory work V

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса Travis CI

$ open https://travis-ci.org

Tasks

    1. Авторизоваться на сервисе Travis CI с использованием GitHub аккаунта
    2. Создать публичный репозиторий с названием lab06 на сервисе GitHub
    3. Ознакомиться со ссылками учебного материала
    4. Включить интеграцию сервиса Travis CI с созданным репозиторием
    5. Получить токен для Travis CLI с правами repo и user
    6. Получить фрагмент вставки значка сервиса Travis CI в формате Markdown
    7. Установить Travis CLI
    8. Выполнить инструкцию учебного материала
    9. Составить отчет и отправить ссылку личным сообщением в Slack

Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_TOKEN=<полученный_токен>
```
```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```
```ShellSession
$ \curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles
$ echo "source $HOME/.rvm/scripts/rvm" >> scripts/activate
$ rvm autolibs disable
$ rvm install ruby-2.4.2
$ rvm use 2.4.2 --default
$ gem install travis
```
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab06
$ cd projects/lab06
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06
```
```ShellSession
	Создание файла-инструкции travis.yml
$ cat > .travis.yml <<EOF
language: cpp
EOF

$ cat >> .travis.yml <<EOF

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF

$ cat >> .travis.yml <<EOF

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF

$ travis login --github-token ${GITHUB_TOKEN}

$ travis lint

$ ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md
```
```ShellSession
$ git add .travis.yml
$ git add README.md
$ git commit -m"added CI"
$ git push origin master

$ travis lint - проверка ошибок
$ travis accounts - используемый аккаунт
$ travis sync - синхронизация репозиториев
$ travis repos - данные о репозиториях (подключенные к travis)
$ travis enable - добавляет проект в отслеживаемые travis
$ travis whatsup - список сборок
$ travis branches - отображает последние версии сборок для каждой ветки
$ travis history - отображает историю сборок
$ travis show - отображения задания, сборки
```
```ShellSession
Report

$ popd
$ export LAB_NUMBER=05
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```
Links

    Travis Client
    AppVeyour
    GitLab CI

Copyright (c) 2017 Братья Вершинины
