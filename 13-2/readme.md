# Домашнее задание к занятию 13.2 "Защита хоста"

------

### Задание 1.

1. Установите **eCryptfs**.
2. Добавьте пользователя `sec_user`.
3. Зашифруйте домашний каталог пользователя с помощью eCryptfs.

--- 

Команды:
```sh
apt install ecryptfs-utils rsync lsof
modprobe ecryptfs
adduser sec_user    # passphrase = 1
ecryptfs-migrate-home -u sec_user
```
[Источник](https://wiki.debian.org/TransparentEncryptionForHomeFolder)

![task1 screen](https://github.com/paive-media/dz13/blob/main/13-2/dz13-2_task1.png "ecryptfs ex")

---


### Задание 2.

1. Установите поддержку **LUKS**.
2. Создайте небольшой раздел (например, 100 Мб).
3. Зашифруйте созданный раздел с помощью LUKS.

--- 

Команды

```sh
lsblk
fdisk -l

# подготовка
cryptsetup -y -v --type luks2 luksFormat /dev/sda6  # passphrase = lUks
# монтирование
cryptsetup luksOpen /dev/sda6 disk
ls /dev/mapper/disk

# форматирование
dd if=/dev/zero of=/dev/mapper/disk
mkfs.ext4 /dev/mapper/disk

# монтирование в папку
mkdir .secret_disk
mount /dev/mapper/disk .secret_disk/

# проверка доступа
cd .secret_disk
touch 123
ls

# отключение
umount .secret_disk
cryptsetup luksClose disk

# попытка монтирования БЕЗ дешифровки
mount /dev/sda6 .secret_disk/

```

Скрины:

![task2 screen1](https://github.com/paive-media/dz13/blob/main/13-2/dz13-2_task2-1.png "установка библиотеки")
![task2 screen2](https://github.com/paive-media/dz13/blob/main/13-2/dz13-2_task2-2.png "подготовка раздела")
![task2 screen3](https://github.com/paive-media/dz13/blob/main/13-2/dz13-2_task2-3.png "подключение диска")
![task2 screen4](https://github.com/paive-media/dz13/blob/main/13-2/dz13-2_task2-4.png "подключение диска")
![task2 screen5](https://github.com/paive-media/dz13/blob/main/13-2/dz13-2_task2-5.png "подключение диска")
![task2 screen6](https://github.com/paive-media/dz13/blob/main/13-2/dz13-2_task2-6.png "проверка доступа")
![task2 screen7](https://github.com/paive-media/dz13/blob/main/13-2/dz13-2_task2-7.png "отключенное состояние")



### Задание 3. *

1. Установите **apparmor**.
2. Повторите эксперимент, указанный в лекции.
3. Отключите (удалите) apparmor.

--- 


[Источник 1](https://habr.com/ru/company/ruvds/blog/532988/)
[Источник 2](https://www.simplified.guide/ubuntu/remove-apparmor)
[Источник 3](https://help.ubuntu.ru/wiki/руководство_по_ubuntu_server/безопасность/apparmor)

Команды:

```sh
# установка
sudo apt install apparmor-profiles apparmor-utils apparmor-profiles-extra

sudo apparmor_status
sudo aa-status 

# посмотрел основные профили
ls /etc/apparmor.d/

# посмотрел профиль для ping
cat /etc/apparmor.d/bin.ping
# посмотрел профиль для man
cat /etc/apparmor.d/usr.bin.man

# посмотрел доп. профили
ls /usr/share/apparmor/extra-profiles/

# сохранил исх версию man -> man0
sudo cp /usr/bin/man /usr/bin/man0

# испортил man <- ping
sudo cp /bin/ping /usr/bin/man

# проверки:
man ya.ru
man 127.0.0.1
sudo aa-complain man
man ya.ru
man 127.0.0.1
sudo aa-enforce man
man ya.ru
man 127.0.0.1

# отключение профиля
sudo aa-disable man

# вернул исх версию man <- man0
sudo rm /usr/bin/man
sudo mv /usr/bin/man0 /usr/bin/man

```


Скрины:

![task3 screen1](https://github.com/paive-media/dz13/blob/main/13-2/dz13-2_task3-1.png "aa status")
![task3 screen2](https://github.com/paive-media/dz13/blob/main/13-2/dz13-2_task3-2.png "aa enforced")
![task3 screen3](https://github.com/paive-media/dz13/blob/main/13-2/dz13-2_task3-3.png "проверка")

---



