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



## Дополнительные задания (со звездочкой*)

Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.

### Задание 3. *

1. Установите **apparmor**.
2. Повторите эксперимент, указанный в лекции.
3. Отключите (удалите) apparmor.


*В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.*



