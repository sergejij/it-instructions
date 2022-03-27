# Установка pass на Windows
1. Уставаниваем gpg [gpg4win.org](https://gpg4win.org/download.html)
2. Импортируем/генерируем gpg ключи
- Импорт. Выполняем команду `gpg --import key` для публичного и приватного ключа
- Генерация `gpg --gen-key`
3. Делаем ключ доверенным `gpg --edit-key {username/mail}`
    1. trust
    2. 5
4. Скачиваем QtPass `winget install qtpass`
5. В настройках прописываем путь до gpg `C:/Program Files (x86)/gnupg/bin/gpg.exe` и до гита `C:/Program Files/Git/cmd/git.exe`
6. Если в профиле не появился нужный ключ, то копируем всё из папки `C:\Users\ushak\.gnupg` в папку `C:\Users\ushak\AppData\Roaming\gnupg`