# Установка pass на Ubuntu

## Установка и создание ключей GPG

Утилита pass использует gpg-ключи для шифрования паролей. Поэтому перед установкой самого pass нужно настроить GPG.

Для проверки, установлен ли GPG на компьютере, нужно ввести команду `gpg --version`. Если его нет, будет выведена строка со словами "No such file or directory". Если GPG уже установлен, пропустите пункт "Установка GPG"

### Установка GPG

1. Обновляем список репозиториев: `sudo apt update`
2. Устанавливаем GPG: `sudo apt install gpg -y`

Теперь нужен ключ, которым GPG будет шифровать пароли. Его можно создать локально или импортировать с другого устройства

#### Предварительная информация

Для манипуляций с GPG ключами используется их имя. Для вывода списка ключей введите команду `gpg --list-keys`. Будет выведен список ключей следующего формата (если вы только что установили GPG и еще не выполняли команды для генерации или импорта ключей, список будет пустым):

```
pub   rsa3072 2022-03-21 [SC] [expires: 2024-03-20]
      6D9606957342E63C507D6D181741CBFA7994142B
uid           [TRUST_LEVEL] NAME <EMAIL>
sub   rsa3072 2022-03-21 [E] [expires: 2024-03-20]
```

На месте полей NAME и EMAIL будут имя и почта пользователя, которые были указаны при создании ключа. В командах, которые описаны ниже, вместо текста `<uid>` нужно будет вставлять NAME ключа.

На месте TRUST_LEVEL будет указан уровень доверия ключу. О доверии к ключам сказано ниже.

### Генерация GPG ключа

Вводим команду `gpg --gen-key`

Программа запросит некоторые данные о пользователе: имя, адрес электронной почты, и попросит подтвердить изменения. Поля можно заполнять произвольно. После подтверждения будет предложено ввести парольную фразу. Эту фразу в дальнейшем нужно будет использовать при выводе списка паролей. Поле можно оставить пустым, тогда вводить парольную фразу не придется, но рекомендуется использовать надежный пароль.

### Импорт GPG ключа с другого устройства

Для экспорта ключа на устройстве с ключами вводим команды:

1. `gpg --export-key <uid> > public_key` для экспорта публичного ключа
2. `gpg --export-secret-key <uid> > private_key` для экспорта приватного ключа

Эти команды экспортируют ключи в файлы `public_key` и `private_key` соответственно.

Далее ключи нужно передать на устройство, где мы хотим использовать pass.

> При передаче ключей между устройствами используйте каналы, которым вы полностью доверяете. Не стоит использовать публичные файлообменники и подобные сервисы

После передачи ключей на устройство их нужно импортировать. Для этого вводим команды:

1. `gpg --import public_key` для импорта публичного ключа
2. `gpg --import private_key` для импорта приватного ключа. Если при создании приватного ключа была указана парольная фраза, на этом шаге потребуется ее ввести.

По умолчанию GPG не доверяет импортированным ключам. Чтобы иметь возможность с ними работать, нужно указать, что мы им доверяем

> Не выполняйте следующие команды для ключей, которые вы получили от неизвестных людей.

1. Открываем меню GPG для управления ключом: `gpg --edit-key <uid>`
2. Вводим `trust`
3. Вводим `5`
4. Для выхода из меню GPG вводим `quit`

После этого в списке ключей на месте TRUST_LEVEL будет указано "ultimate".

Установка GPG и настройка ключей шифрования закончена.

## Устанавливаем pass

1. Установка pass:
`sudo apt install pass`

2. Создаем репозиторий pass

`pass init {username}`, где username это имя указанное при создании gpg ключа. 

Имя можно узнать командой `gpg --list-keys` 

3. Делаем из репозитория `~/.password-store`  git репозиторий, командой `pass git init`

4. Присоединяем созданный репозиторий к удаленному `git remote add origin {ssh link}`

5. Если не получается спулить `git pull origin master --allow-unrelated-histories`

### Автоматизация подгрузки паролей из удаленного репозитория с помощью Cron

1. Для вывода логов Cron в отдельный файл нужно выполнить следующие команды: 
- `vim /etc/rsyslog.d/50-default.conf`

- раскомментировать строку, начинающуюся с `#cron.*`

- сохранить файл и выполнить команду `sudo service rsyslog restart`

Теперь логи будут выводиться в файл `/var/log/cron.log`

2. Создать задачу для выполнения по расписанию
- Выполнить команду `crontab -e`

- В конце файла дописать (при необходимости изменить частоту запуска задачи) `*/5 * * * * git -C ~/.password-store pull >/dev/null 2>&1`

- Сохранить и закрыть файл. При закрытии файла должна появиться строка `crontab: installing new crontab`

В примере выше указанная частота обновления — 5 минут. Это значит, что задача будет выполняться в 00, 05, 10, 15 и тд минут


### Полезные команды pass

Посмотреть все сервисы - `pass`

Посмотреть пароль сервиса - `pass {servicename}`

Сгенерировать пароль с определенным  количеством символов для определного сервиса - `pass generate {servicename} 13` флаг  `-c` скопирует сгенерированый пароль в буфер.

Назначить пароль сервису - `pass insert {servicename}`

Назначить сервису несколько полей - `pass insert -m {servicename}`

Удалить пароль - `pass rm {servicename}`

Изменить пароль - `pass edit {servicename}`

Скопировать пароль не показывая его на экране - `pass -c {servicename}`


