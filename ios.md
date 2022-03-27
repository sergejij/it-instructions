# Настройка приложения pass на ios

1. Устанавливаем приложение Pass из стора

2. Добавляем репозиторий во вкладке "Password Repository"

url - ssh ссылка для клонирования репы

username - git

auth method - ssh key -> Load from file, надо установить файл с приватным ssh ключом, который установлен на гитхабе 

Удалям ssh ключ из файлов и других мест с телефона.

3. Добавляем GPG ключи с помощью файлов. 

Узнаем ключ pub командой `gpg --list-keys`

Записываем приватный и публичный ключ в файлы:

`gpg --export -a {pub} > key.pub`

`gpg --export-secret-keys -a {pub} > key`

Выбираем эти файлы в приложении.

### Полезные ссылки

[Home · mssun/passforios Wiki · GitHub](https://github.com/mssun/passforios/wiki)


