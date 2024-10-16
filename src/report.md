## Part 1. Готовый докер

Скачаем докер, но для этого потребуется зарагестрироваться на сайте https://hub.docker.com
		
		вводим команду sudo docker login используя ВПН (тк без него не скачивается) 
НО ничего не работало и не скачивалось, поэтому качаем с левого сайта) этой командой 

	sudo docker pull dockerhub.timeweb.cloud/library/docker:latest

![alt text](img/image-1.png)

Взять докер-образ с **nginx** и выкачать его при помощи `docker pull`.

	sudo docker pull dockerhub.timeweb.cloud/library/nginx:latest

![alt text](img/image-0.png)

Проверить наличие докер-образа через 

	sudo docker images.

![alt text](img/image-2.png)

Запустить докер-образ через `docker run -d [image_id|repository]`.

	sudo docker run -d 4f67c83422ec

	-d: это флаг, который указывает Docker на запуск контейнера в фоновом режиме (detached mode). Это означает, что контейнер будет работать в фоновом режиме, и командная строка будет освобождена для дальнейшего использования.

![alt text](img/image-3.png)

Проверить, что образ запустился через `docker ps`

![alt text](img/image-4.png)

Посмотри информацию о контейнере через `docker inspect [container_id|container_name]`.

	sudo docker inspect 60db395296d7

![alt text](img/image-5.png)

Остановить докер образ через `docker stop [container_id|container_name]`.

	sudo docker stop  60db395296d7

и проверить, что образ остановился через `docker ps`.

![alt text](img/image-7.png)

Запустить докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду *run*.

По выводу команды определить размер контейнера, список замапленных портов и ip контейнера.

![alt text](img/image-8.png)

Где первые 80 и 443 - это локальные порты, а вторые 80 и 443 - порты контейнера.



	размер контейнера:
	sudo docker inspect unruffled_pike --size | grep -i -e Size

	замапленные порты:
	sudo docker inspect unruffled_pike --size | grep -A 2 -i -e Exposed

	IP контейнера
	sudo docker inspect unruffled_pike --size | grep -i -e IPAddress

Проверить, что в браузере по адресу *localhost:80* доступна стартовая страница **nginx**.

![alt text](img/image-9.png)

Перезапусти докер контейнер через `docker restart [container_id|container_name]`.

Проверить, что контейнер запустился.

![alt text](img/image-10.png)

## Part 2. Операции с контейнером

Прочитать конфигурационный файл nginx.conf внутри докер контейнера через команду exec.

![alt text](img/image-11.png)

Создать на локальной машине файл nginx.conf. Настроить в нем по пути /status отдачу страницы статуса сервера nginx.

![alt text](img/image-15.png)

Скопировать созданный файл nginx.conf внутрь докер-образа через команду docker cp.

![alt text](img/image-13.png)

Перезапустить nginx внутри докер-образа через команду exec.

![alt text](img/image-14.png)

Проверить, что по адресу localhost:80/status отдается страничка со статусом сервера nginx.

![alt text](img/image-16.png)

Экспортировать контейнер в файл container.tar через команду export Остановить контейнер.

![alt text](img/image-17.png)

Удалить образ через docker rmi [image_id|repository], не удаляя перед этим контейнеры.

![alt text](img/image-18.png)

Удали остановленный контейнер.

![alt text](img/image-19.png)

Импортировать контейнер обратно через команду import. Запустить импортированный контейнер.

![alt text](img/image-20.png)

Проверить, что по адресу localhost:80/status отдается страничка со статусом сервера nginx.

![alt text](img/image-21.png)

## Part 3. Мини веб-сервер

Написать мини-сервер на C и FastCgi, который будет возвращать простейшую страничку с надписью Hello World!.

![alt text](img/image-23.png)

![alt text](img/image-24.png)

![alt text](img/image-25.png)

![alt text](img/image-26.png)

Запустить написанный мини-сервер через spawn-fcgi на порту 8080.

![alt text](img/image-27.png)

Написать свой nginx.conf, который будет проксировать все запросы с 81 порта на 127.0.0.1:8080.

![alt text](img/image-22.png)

Проверить, что в браузере по localhost:81 отдается написанная тобой страничка.

![alt text](img/image-28.png)

Положить файл nginx.conf по пути ./nginx/nginx.conf 

![alt text](img/image-29.png)

## Part 4. Свой докер
Собрать написанный докер-образ через docker build при этом указав имя и тег.

![alt text](img/image-30.png)

![alt text](img/image-31.png)

Проверить через docker images, что все собралось корректно.

![alt text](img/image-32.png)

Запустить собранный докер-образ с маппингом 81 порта на 80 на локальной машине 

![alt text](img/image-34.png)

Проверить, что по localhost:80 доступна страничка написанного мини сервера.

![alt text](img/image-33.png)

Дописать в ./nginx/nginx.conf проксирование странички /status, по которой надо отдавать статус сервера nginx.

![alt text](img/image-35.png)

Перезапустить докер-образ.

![alt text](img/image-36.png)

После сохранения файла и перезапуска контейнера, конфигурационный файл внутри докер-образа должен обновиться самостоятельно

Проверить, что теперь по localhost:80/status отдается страничка со статусом nginx

![alt text](img/image-37.png)

## Part 5. Dockle

!!Примечание!!

	Перед выполнением данного шага необходимо установить утилиту [dockle]
	инструкция по установке [https://github.com/goodwithtech/dockle]

Просканировать образ из предыдущего задания через dockle [image_id|repository].

![alt text](img/image-38.png)

Так, чтобы при проверке через dockle не было ошибок и предупреждений.

![alt text](img/image-39.png)

## Part 6. Базовый Docker Compose

Написать файл docker-compose.yml, с помощью которого:

1) Поднять докер-контейнер из Части 5 (он должен работать в локальной сети, т.е. не нужно использовать инструкцию EXPOSE и мапить порты на локальную машину).

2) Поднять докер-контейнер с nginx, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера.

Замапить 8080 порт второго контейнера на 80 порт локальной машины.

Остановить все запущенные контейнеры.

Собрать и запустить проект с помощью команд docker-compose build и docker-compose up.

![alt text](img/image-40.png)

![alt text](img/image-41.png)

Проверить, что в браузере по localhost:80 отдается написанная тобой страничка, как и ранее.

![alt text](img/image-42.png)
