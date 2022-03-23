# Установка pass на Ubuntu

1. Должен быть установлен gpg.

2. Устанавливаем pass

`sudo apt install pass`

3. Создаем репозиторий pass

`pass init {username}`, где username это имя указанное при создании gpg ключа. 

Имя можно узнать командой `gpg --list-keys` 

4. Делаем из репозитория `~/.password-store`  git репозиторий, командой `pass git init`

5. Присоединяем созданный репозиторий к удаленному `git remote add origin {ssh link}`



### Полезные команды pass

Посмотреть все сервисы - `pass`

Посмотреть пароль сервиса - `pass {servicename}`

Сгенерировать пароль с определенным  количеством символов для определного сервиса - `pass generate {servicename} 13` флаг  `-c` скопирует сгенерированый пароль в буфер.

Назначить пароль сервису - `pass insert {servicename}`

Назначить сервису несколько полей - `pass insert -m {servicename}`

Удалить пароль - `pass rm {servicename}`

Изменить пароль - `pass edit {servicename}`

Скопировать пароль не показывая его на экране - `pass -c {servicename}`


