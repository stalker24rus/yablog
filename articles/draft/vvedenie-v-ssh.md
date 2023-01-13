# [ i ] SSH


* 1. Принцип работы SSH

Протокол Secure Shell (далее сокр. SSH) обеспечивает защищенное соединение между компьютером клиента и удаленным сервером, по типу точка-точка.

В результате подключения создается тунель, через который осуществляется передача команд на удаленное управление оболочкой сервера и обмен информацией.

Для шифрования передаваемой информации используются специальные ключи.

SSH использует три метода шифрования:

симметричное;
ассиметричное;
хэширование.
1.1. Симметричное

При установке соединения специальный алгоритм обмена ключами предварительно согласовывает ключ шифрования и потом использует его на протяжении всей сессии.

1.2. Ассиметричное

В данном типе соединения используется пара ключей (приватный / публичный).

Сгенерировать пару ключей можно с помощью самой утилиты SSH (будет описано ниже). Публичный ключ хранится на удаленном сервере, его можно свободно распространять. Приватный ключ необходимо держать в секрете.

Публичным ключом данные зашифровываются, а приватным - расшифровываются.

Алгоритм работы с публичным/приватным ключем заключается в следующем:

Сервер и клиент используя свои публичные ключи генерируют общий секретный ключ шифрования текущей сессии.
Сервер шифрует данные публичным ключом и предлагает клиенту расшифровать их своим приватным ключом.
Если операция успешна, то клиент авторизуется на сервере с соответствующим именем пользователя.
Затем сессия продолжается уже с использованием симметричного шифрования.
1.3. Хеширование

В отличие от других схем шифрования, хэширование в SSH используется не для дешифровки данных, а для создания уникальных шифрованных ключей для подтверждения сообщений об аутентификации. Эти ключи (хэши) используются в механизме симметричного шифрования SSH-сессии. [1]

2. Использование SSH

2.1. Для выполнения соединения по SSH без использования ключей, пользователю Linux (MacOS) достаточно выполнить следующую команду в терминале:

$ ssh user@host

user - это имя пользователя на удаленном сервере;
host - адрес самого сервера.
2.2. После чего будет затребован пароль для входа:

user@host's password:      

2.3. После удачной аутентификации, вы увидете приглашение для ввода команд на удаленном сервере:

user ~$                               

При попытке подключения сервер запросит пароль от пользователя, после чего осуществится переход в оболочку сервера (bash, zsh и т.п.).

3. Генерация ключей

3.1.1. Простой способ, в терминале вводим ssh-keygen:

$ ssh-keygen

3.1.2. Запускается процесс генерации ключей .

Generating public/private rsa key pair.

 3.1.3. После генерирования ключей будут запрошены место размещения ключа и дополнительный пароль проверяемый перед использованием ключей (можно оставить свободным).

Enter file in which to save the key (~/.ssh/some_key):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:

3.1.4. Следующим собщением указывается куда были сохранены ключи и идентификатор ключа.

Your identification has been saved in ~/.ssh/some_key.
Your public key has been saved in ~/.ssh/some_key.pub.
The key fingerprint is:
SHA256:puyf3JstBjXWtwbMf7zqz2NiP7hu/lgBhgRAN708DiA example@example.local
The key's randomart image is:
+---[RSA 3072]----+
|      .o.+o.     |
|     E .. o..    |
|      . . .=.o   |
|         .++* o  |
|        Soo..+ + |
|     . o.  .  + +|
|      o  .   ..o.|
|     . . ooo =+= |
|      ..+.+oBBO=o|
+----[SHA256]-----+

В нашем случае, созданные пары ключей (some_key и some_key.pub) сохраняются в папку ~/.ssh, .

some_key - приватный ключ, который нужно хранить в секрете,

some_key.pub - ключ готовый к распространению.

3.1.5. Прочитать публичный ключ можно командой cat:

$cat ~/.ssh/some_key.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCrJMwk9v2eJKXND7R2GQcb31S8R0ChYmCYY5NliZ8AeBUc6Fkqye5I6kMIRyBfjI/EEJ80zvWikIcP0lJbR4Dz98+lAEF3YVl673obYbNCh/pDB9a0LIu2izoRdDNgM+N+I1vvRpkgEhRfceW3g1duGYOoBc2FzCuT/d3H0XDZMTRP2aRnaiQSfqrRyc2vBob+S8hg1f/tvA+6uUrijWTc60FjhHvZIBcyXj3w4RDtfH2TotVJSqmEfo8vWgB7C0sfFveptoQugo4Vrr8Et/gCTTHgMthCvdnSzpJDozQI6LuxDZCSHsoxpzM4fPNdEl1TnopYT3wgNCFvs7drKwn4X/cLxM28VG3mM8JORJRIX8demqrjRf7VqwjYXTQqHKI01OX7WpvMefaUrap8DNhyD3jLUpJ6GJ5JI8MeRf7YC3pDaQNpKg6X7K4JeuYad04XaZlDdgzJ+5NfufBzxU+GhSqu6tixR8uixgjninJhEVjaZAhPVXGUnP5CFTYF5Fs= example@example.local

Mac OS. В буфер обмена содержимое файла some_key.pub можно скопировать в буфер обмена командой pbcopy, а вставить - pbpaste:

$pbcopy < some_key.pub
$pbpaste
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCrJMwk9v2eJKXND7R2GQcb31S8R0ChYmCYY5NliZ8AeBUc6Fkqye5I6kMIRyBfjI/EEJ80zvWikIcP0lJbR4Dz98+lAEF3YVl673obYbNCh/pDB9a0LIu2izoRdDNgM+N+I1vvRpkgEhRfceW3g1duGYOoBc2FzCuT/d3H0XDZMTRP2aRnaiQSfqrRyc2vBob+S8hg1f/tvA+6uUrijWTc60FjhHvZIBcyXj3w4RDtfH2TotVJSqmEfo8vWgB7C0sfFveptoQugo4Vrr8Et/gCTTHgMthCvdnSzpJDozQI6LuxDZCSHsoxpzM4fPNdEl1TnopYT3wgNCFvs7drKwn4X/cLxM28VG3mM8JORJRIX8demqrjRf7VqwjYXTQqHKI01OX7WpvMefaUrap8DNhyD3jLUpJ6GJ5JI8MeRf7YC3pDaQNpKg6X7K4JeuYad04XaZlDdgzJ+5NfufBzxU+GhSqu6tixR8uixgjninJhEVjaZAhPVXGUnP5CFTYF5Fs= example@example.local

3.2. Подробный пример [2]

$ ssh-keygen \
    -m PEM \
    -t rsa \
    -b 4096 \
    -C "user@host" \
    -f ~/.ssh/mykeys/myprivatekey \
    -N mypassphrase

Описание команды:

ssh-keygen — программа, с помощью которой создаются ключи.

-m PEM — преобразование ключа в формат PEM.

-t rsa — тип создаваемого ключа; в данном случае создается ключ в формате RSA.

-b 4096 — количество битов в ключе; в данном случае ключ содержит 4096 битов.

-C "user@host" — комментарий, который будет добавлен в конец файла открытого ключа для идентификации. Обычно в качестве комментария используется адрес электронной почты, но вы можете выбрать для своей инфраструктуры любой удобный метод идентификации.

-f ~/.ssh/mykeys/myprivatekey — имя файла закрытого ключа, если вы решили не использовать имя по умолчанию. В том же каталоге будет создан соответствующий файла открытого ключа с .pub в имени. Этот каталог должен существовать.

-N mypassphrase — дополнительная парольная фраза, используемая для доступа к файлу закрытого ключа.

 

4. Загрузка публичного ключа на сервер

4.1. Через использование ssh подключение:

$ cat ~/.ssh/some_key.pub | ssh root@host 'cat >> ~/.ssh/authorized_keys'

4.2. Отредактировав на сервере файл authorized_keys при использовании vi и последующим копипастом публичного ключа:

$vi ~/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCrJMwk9v2eJKXND7R2GQcb31S8R0ChYmCYY5NliZ8AeBUc6Fkqye5I6kMIRyBfjI/EEJ80zvWikIcP0lJbR4Dz98+lAEF3YVl673obYbNCh/pDB9a0LIu2izoRdDNgM+N+I1vvRpkgEhRfceW3g1duGYOoBc2FzCuT/d3H0XDZMTRP2aRnaiQSfqrRyc2vBob+S8hg1f/tvA+6uUrijWTc60FjhHvZIBcyXj3w4RDtfH2TotVJSqmEfo8vWgB7C0sfFveptoQugo4Vrr8Et/gCTTHgMthCvdnSzpJDozQI6LuxDZCSHsoxpzM4fPNdEl1TnopYT3wgNCFvs7drKwn4X/cLxM28VG3mM8JORJRIX8demqrjRf7VqwjYXTQqHKI01OX7WpvMefaUrap8DNhyD3jLUpJ6GJ5JI8MeRf7YC3pDaQNpKg6X7K4JeuYad04XaZlDdgzJ+5NfufBzxU+GhSqu6tixR8uixgjninJhEVjaZAhPVXGUnP5CFTYF5Fs= example@example.local

4.3. Использование команды echo для дописывания ключа в конец файла authorized_keys

$ echo ssh-rsa строка-публичного-ключа >> ~/.ssh/authorized_keys

 

 5. Создание config файла для SSH

Для быстрого подключения к нужному серверу по имени используя команду ssh имя_сервера, нужно создать файл config в папке ~/.ssh.

5.1. Вызовим vim для создания файла config

$ vim ~/.ssh/config

5.2. Заполняем файл следующим содержимым:

host имя_сервера
    hostname xxx.xxx.xxx.xxx
    port 22
    user имя_пользователя
    IdentityFile ~/.ssh/some_key

host - задаем произвольное имя сервера которое будет использоваться при подключении;
hostname - доменное имя или IP-адрес;
port - номер порта ssh;
user - имя пользователя на удаленном сервере;
IdentityFile - путь к секретному ключу.
5.3. Для подключения к серверу, достаточно ввести ssh имя_сервера.

$ ssh имя_сервера

 

6. Создание на удаленном сервере non-root пользователя для входа в систему и последующей работы по SSH

 6.1. Добавляем пользователя

$ useradd username
$ passwd username
New password: 
Retype new password: 
passwd: password updated successfully

6.2. Добавляем пользователя в группу sudo (пример для Ubuntu)

$ usermod -aG sudo username

6.3. Проверяем привелегии администратора

$ su - username
$ sudo ls -l /root
total 0

6.4. Добавляем публичный ключ для вновь созданого пользователя в открывшемся редакторе vi.

$ sudo mkhomedir_helper username && mkdir ~/.ssh && touch authorized_keys && vi authorized_keys

6.5. Отключение аутентификации по паролю и логирование пользователя root

# 1 открываем файл для редактирования конфигурации демона ssh
$ vim /etc/ssh/sshd_config

# 2 Изменяем строчку PasswordAuthentication на no
PasswordAuthentication no

# 3 Для запрещения залогинивания под root, разкоментируем(или меняем) строчку 
PermitRootLogin no

# 4 Для обеспечения безопасности так же рекомендуется изменить стандартный используемый порт на иной, раскоментировав строчку и задав произвольный порт:
Port 55555

6.6. Рестартуем сервис

$ sudo service sshd restart

6.7. Проверяем подключение из другого терминала, перед тем как приступить к следующему пункту.

 

UPD130821: добавлен раздел создания non-root пользователя перед отключением root.

UPD170821: корректировка текста статьи

Discleimer Мой пост является некой компиляцией статей из интернета и не претендует на уникальность, ссылки используемых статей для подготовки публикации привожу ниже:

[1] - ru.hostings.info [2] - docs.microsoft.com [3] - firstvds.ru
