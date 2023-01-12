docker_init

Сегодня будет затронута тема контейнеризации программного обеспечения на базе Docker. Так получилось что данный пост это частично переведеный фрагмент официальной документации с которой можно ознакомиться https://docs.docker.com/get-started/overview/.



1. Что есть Докер?

Docker это открытая платформа для разработки, доставки и запуска приложений. Docker призван разделить приложения от инфраструктуры (имеется в виду железо), что позваляет поставлять софт быстро. С докером вы сможете управлять инфраструктурой таким же путем как вы управляете своими приложениями. Принимая преимущества использования методологий Docker'а для поставки, тестирования и быстрого развертования кода, вы сможете значительно уменьшить задержку между написанием кода и его запуском в продакшн. 

В переводе автора статьи.



В обобщенном виде Docker это своего рода менеджер облегченных виртуальных машин, каждая единица которой предназначенна служить только конкретной цели (сервер баз данных, сервер приложений, и т.п.).  Docker имеет возможность организации внутренней сети между контейнерами, а также предоставляет все необходимые интерфейсы связи как внутри так и с наружи.

2. Архитектура

Docker использует клиент-серверную архитектуру.  Docker- клиент  запрашивает то, что необходимо сделать Docker- деману с контейнерами.  Docker client и daemon могут запускаться в одной системе или же Docker client может удаленно подключаться к Docker daemon. Docker client и daemon могут связываться используя REST API поверх UNIX сокетов или же по сети.

Другим же  клиентом может являться  Docker Compose, что позволяет работать с приложениями содержащими несколько контейнеров.



Docker демон

Docker демон (dockerd) прослушивает Docker API запросы и управляет Docker объектами, такими как образы, контейнеры, сети и volumes (тома). Демон может связываться с другими демонами для управления сервисами  Docker. 

Docker клиент

Docker клиент (docker) это главный инструмент взаимодействия для  пользователей с Docker. Когда вы используете такую команду как docker run, клиент отправляет эти команды в dockerd, который их выполняет. Docker команды используют Docker API. Docker клиент может коммуницировать с более чем одним демоном.

Docker реестры

Docker реестры хранят Docker-образы. Docker Hub это публичный регистр, который может использовать кто-угодно. По умолчанию Docker сконфигурирован для поискка образов на Docker Hub. Можно даже создавать свой собственный приватный регистр, если в этом есть необходимость.

Когда вы используете команды docker pull или docker run, представленные образы загружаются из сконфигурированого Вами регистра. Когда используется команда docker push, Ваш образ загружается в сконфигурированый регистр. 

Docker объекты

Когда Вы используете Docker,  вы создаете, используете образы, контейнеры, сети, тома, плагины и другие объекты. 

This section is a brief overview of some of those objects.

Images

An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

Containers

A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

By default, a container is relatively well isolated from other containers and its host machine. You can control how isolated a container’s network, storage, or other underlying subsystems are from other containers or from the host machine.

A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that are not stored in persistent storage disappear.

3. Установка и первый запуск

4. Dockerfile

5. Docker-compose

https://docs.docker.com/get-started/overview/



