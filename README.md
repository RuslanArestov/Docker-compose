# **Задача 1**

Сценарий выполнения задачи:
+ Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
+ Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: {"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}
+ Зарегистрируйтесь и создайте публичный репозиторий с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
+ скачайте образ nginx:1.21.1;
+ Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:

<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>

+ Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП).
+ Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .


## **Решение**


<https://hub.docker.com/repository/docker/ruslanar/custom-nginx/general/>


# **Задача 2**

1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
+ имя контейнера "ФИО-custom-nginx-t2"
+ контейнер работает в фоне
+ контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080 ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## **Решение**

![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/1.png)\

![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/2.png)

# **Задача 3**
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните docker ps -a и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду nginx -s reload, а затем внутри контейнера curl http://127.0.0.1:80 ; curl http://127.0.0.1:81.
9. Выйдите из контейнера, набрав в консоли exit или Ctrl-D.
10. Проверьте вывод команд: ss -tlpn | grep 127.0.0.1:8080 , docker port custom-nginx-t2, curl http://127.0.0.1:8080. Кратко объясните суть возникшей проблемы.
11. Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. пример источника
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## **Решение**

![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/3.png)\

Docker отправляет сигнал SIGINT контейнеру, что приводит к завершению процессов.

![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/4.png)\

![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/5.png)\

![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/6.png)\

Проблема заключается в том, что после изменения порта в конфигурационном файле Nginx, Nginx теперь слушает на порту 81 внутри контейнера, но проброс портов остался на порту 80. Это приводит к тому, что запросы на порт 8080 на хост-системе не достигают Nginx, так как Nginx больше не слушает на порту 80 внутри контейнера.

Команда для принудительного удаления, запущенного контейнера
docker rm -f custom-nginx-t2

Не приложил скриншот к команде docker rm -f custom-nginx-t2, потому что пропустил случайно это задание, а все контейнеры и образы я уже удалил.

# **Задача 4**

+ Запустите первый контейнер из образа centos c любым тегом в фоновом режиме, подключив папку текущий рабочий каталог $(pwd) на хостовой машине в /data контейнера, используя ключ -v.
+ Запустите второй контейнер из образа debian в фоновом режиме, подключив текущий рабочий каталог $(pwd) в /data контейнера.
+ Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data.
+ Добавьте ещё один файл в текущий каталог $(pwd) на хостовой машине.
+ Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## **Решение**

![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/8.png)\
![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/9.png)\

# **Задача 5**
1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него. "compose.yaml" с содержимым:
version: "3"
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock

"docker-compose.yaml" с содержимым:
version: "3"
services:
  registry:
    image: registry:2

    ports:
    - "5000:5000"

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )
2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)
3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"

Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".
Удалите любой из манифестов компоуза(например compose.yaml). Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.
В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

## **Решение**
Запустился compose.yaml. Если существуют оба файла, Compose предпочитает канонический compose.yaml.

![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/10.png)\
![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/11.png)\
![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/12.png)\
![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/13.png)\
![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/14.png)\

Первое предупреждение указывает на то, что атрибут version в файле docker-compose.yaml устарел и будет игнорироваться.
Второе предупреждение указывает на то, что Docker Compose нашел "сиротские" контейнеры, которые больше не описаны в файле docker-compose.yaml.

![alt text](https://github.com/RuslanArestov/Docker-compose/blob/main/images/15.png)\
