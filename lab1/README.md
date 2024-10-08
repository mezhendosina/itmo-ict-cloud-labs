# lab 1

Итак, лаба просит поднять nginx, который умеет держать 2 домена на одном сервере. Ну ок, погнали сварим nginx на сервере за дешево и не запарно. Без докеров и виртуализаций, потому что лаба этого не просит, а поднимать докеры и виртуализаци запарно 

Для приготовления сервера с nginx понадобится:
Самый дешевый сервер на timeweb с двумя публичным ip
2 простейших HTML файла
Конфиг для nginx
SSL сертификат
Базовые знания линух

Чтож, начнем

## Сделаем сервер

Логинимся на timeweb.cloud(ни в коем случае не реклама, просто денюжки там были) и конфигурируем себе сервер. Если вы богатый, можно и полный фарш себе взять. Но для нас подойдет самый слабый, потому что самый дешевый
![](/lab1/report_img/1.png)
Здесь важно не забыть взять себе еще публичный ip, позже он нам еще понадобится
![](/lab1/report_img/2.png)

Жмем кнопку заказать и ждем, когда наш сервер подымется
![](/lab1/report_img/3.png)

Когда он все таки поднялся, идем в сеть и добавляем себе еще один ip. Это понадобится, чтобы сделать себе 2 домена
![](/lab1/report_img/4.png) 

Ну и все. Подрубаемся по ssh к нему. Можно даже под рутом. Надо быть чуть осторожнее, зато не надо запариваться насчет нового пользователя. К тому же, лаба не просит делать нового юзера на сервере

Пишем в консольку
```
apt update && apt upgrade -y && apt install nginx
```
Теперь в одну строку мы обновили все, что не обновлено, и поставили nginx. Осталось настроить его, сделать html файлы и ssl сертификат

## Делаем HTML файлы

Ну тут все совсем просто.

Берем базированный Hello World, меняем его на Первый. Потом делаем копию и в ней меняем Первый на Второй. 

Вот и все, что хостить у нас есть. Положим их в /root/lab1/first/index.html и /root/lab1/second/index.html соответсвенно. 


## Варим сертификат

Так как мы профессиональные серверовары, то и сертификат мы сможем сварить сами. Берем консольку и пишем

```
mkdir -p /root/lab1/cert
openssl genpkey -algorithm RSA -out  /root/lab1/cert/cert_key.key -aes128 -pass pass:yourpassword
openssl req -new -key  /root/lab1/cert/cert_key.key -out  /root/lab1/cert/cert.csr -passin pass:yourpassword
openssl x509 -req -days 365 -in  /root/lab1/cert/cert.csr -signkey /root/lab1/cert/cert_key.key -out /root/lab1/cert/cert.crt -passin pass:yourpassword
```

Теперь нам нужно только настроить сам nginx.  

## Делаем nginx.conf
Берем базированный nginx.conf и, путем небольшого гуглежа, модифицируем его под нужды тз лабы. Посмотреть на него можно [вот тут](/lab1/nginx.conf)

Все в одном файле, потому что тут совсем немного, да и лаба иного не требует. По хорошему, базированный конфиг надо не трогать, а новые добавлять в специальную папочку /etc/nginx/sites-enabled. Но опять же, тз лабы этого не требует

## Проверяем
Вводим ``nginx -t && nginx -s reload``, убеждаемся, что все ок, переходим на наши ip адреса и видим заветные
![](/lab1/report_img/5.png)
![](/lab1/report_img/6.png)

Вот и все. Сервер работает, а мы - счастливы!

## Вывод

А вывод такой, что ничего тут сложно нет. Вспомнил, как подымал в свои 16 лет сервер, а в 17 - сайт :)
