# [ Linux ] Развертывание удаленного сервера

В данном посте содержится информация по развертыванию удаленного сервера. За основу взят материал Алексея Голобурдина. 

## 0. Настройки безопасности подключения по SSH:

### 0.1. Добавляем пользователя
> $ useradd username  
> $ passwd username  
> New password:   
> Retype new password:   
> passwd: password updated successfully  

### 0.2. Добавляем пользователя в группу sudo (пример для Ubuntu)
> $ usermod -aG sudo username  

### 0.3. Проверяем привелегии администратора
> $ su - username  
> $ sudo ls -l /root  
> total 0  

### 0.4. Добавляем публичный ключ для вновь созданого пользователя в открывшемся редакторе vi.
> $ sudo mkhomedir_helper username && mkdir ~/.ssh && touch authorized_keys && vi authorized_keys  

Проверяем подключение из другого терминала, перед тем как приступить к следующему пункту.

### 0.5. Отключение аутентификации по паролю и логирование пользователя root

*1 открываем файл для редактирования конфигурации демона ssh  
> $vim /etc/ssh/sshd_config  

*2 Изменяем строчку PasswordAuthentication на no  
> PasswordAuthentication no  

*3 Для запрещения залогинивания под root, разкоментируем(или меняем) строчку 
> PermitRootLogin no  

*4 Для обеспечения безопасности так же рекомендуется изменить стандартный используемый порт на иной, раскоментировав строчку и задав произвольный порт:
Port 55555

### 0.6. Рестартуем сервис
> $ sudo service sshd restart  

 
## 1. Общие настройки:

*1 Обновляем информацию об пакетах  
> sudo apt-get update  

*2 Устанавливаем необходимые пакеты для работы. 
> sudo apt-get install -y vim bash tmux htop git curl wget unzip zip gcc build-essential make  

*3 Устанавливаем дополнительные пакеты:
> sudo apt-get install -y zsh tree redis-server nginx zlib1g-dev libbz2-dev libreadline-dev llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev liblzma-dev python3-dev python-imaging python3-lxml libxslt-dev python-libxml2 python-libxslt1 libffi-dev libssl-dev python-dev gnumeric libsqlite3-dev libpq-dev libxml2-dev libxslt1-dev libjpeg-dev libfreetype6-dev libcurl4-openssl-dev supervisor  

*4 Устанавливаем oh-my-zsh:
> sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"  

*5 Создаем алиасы:  
> vim ~/.zshrc  
>    alias cls="clear". 

*6 Устанавливаем zsh как shell по умолчанию:  
> chsh -s $(witch zsh)  

## 2. Установка Python из исходников
*1 создаем директорию, где будут храниться исходники
> mkdir ~/code  
>  
> wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz ; \  
> tar xvf Python-3.7.* ; \  
> cd Python-3.7.3 ; \  
> mkdir ~/.python ; \  
> ./configure --enable-optimizations --prefix=/home/www/.python ; \  
> make -j8 ; \  
> sudo make altinstall  

Теперь Python в директории ~/.python/bin/python3.7, обновляем pip
> sudo ~/.python/bin/python3.7 -m pip install -U pip  

Сейчас мы можем загрузить наш проект из Git репозитория (или создать собственный), создадим виртуальное окружение python и запустим его:  
> cd code  
> git pull project_git  
> cd project_dir  
> python3.7 -m venv env  
> . ./env/bin/activate  
