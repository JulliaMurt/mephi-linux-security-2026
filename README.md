# mephi-linux-security-2026

## Раздел 1. Создание пользователя
- Создан пользователь `user1` с UID `1234`, входящий в группу `students`.
- Настроен срок действия пароля (90 дней) с помощью `chage -M 90 user1`.
- Доказательства: файлы `passwd.out`, `shadow.out`, `group.out`.

## Раздел 2. Мониторинг файлов и процессов
- Поиск файлов с битом set-UID: `sudo find / -perm -4000 -type f 2>/dev/null`. Результат в `setuid-files.out`.
- Поиск процессов с euid=0 и ruid>=1000: `ps -eo pid,ruid,euid,cmd | awk 'NR==1 || ($2 >= 1000 && $3 == 0)'`. Для проверки запускалась смена пароля `passwd`. Результат в `privileged-processes.out`.

## Раздел 3. Изучение механизма set-UID
- Скопирована утилита `cat` в `/home/user1/mycat`.
- Установлен владелец `root` и бит set-UID (`chmod u+s`).
- Успешно прочитан файл `/etc/shadow` от имени обычного пользователя. Доказательства в `stat.out` и `history.out`.

## Раздел 4. Изучение механизма привилегий (Capabilities)
- Скопирована утилита `chown` в `/home/user1/mychown`.
- Выдана capability `cap_chown+ep` с помощью `setcap`.
- Успешно изменен владелец файла от имени обычного пользователя. Доказательства в `getcap.out` и `stat.out`.

## Раздел 5. Изучение механизма sudo
- В `/etc/sudoers` добавлено правило: `user1 ALL=(root) /usr/bin/timedatectl`.
- Проверено выполнение `sudo timedatectl set-time ...` от имени `user1` (с предварительным отключением NTP). Доказательства в `sudoers.out` и `history.out`.

## Раздел 6. Артефакты
Все требуемые файлы (`mephi-screenshot`, `history.out`, `stat.out`, `getcap.out`, системные файлы) загружены в репозиторий. 
*Примечание: хэши паролей в `shadow.out` заменены на в целях безопасности.*
