# Иструкция по установке
1. Из данного репозитория сделайте себе локальную копию.
2. Создайте и активируйте виртуальное окружение.
2. Установите необходимые для работы приложения библиотеки, введя в командную строку: pip install -r requirements.txt

# План разработки
1. Для получения информации о погоде будем обращаться на сайт https://openweathermap.org/current, предварительно
на нём будет необходимо зарегистрироваться и получить свой API_KEY. Обращаться туда будем при помощи get-запросов
из библиотеки requests.

2. Для получения информации о своём местоположении будет использована также библиотека requests.

3. В историю буду записыват только успешные запросы, это будет список. По востребованияю
из этого списка можно будет извлечь сколько-то последних записей.

4. Это будет консольное приложение.

5. Все функции будут обёрнуты в try-except данные об ошибках будут выводиться в консоль.

# Подробности реализации
Чтобы начать работу с приложением достаточно запустить файл main.py. После запуска срабатывает функция main(),
которая выводит список команд

1 - определить погоду в любом городе
2 - определить погоду в своём городе
3 - увидеть историю запросов
4 - очистить историю запросов
5 - завершить работу приложения

и ждёт дальнейших указаний. Функция main() в зависимости от выбора пользователя далее будет выполнять
команды описанные ниже для того чтоб удовлетворить запрос.

Многие функции данной программы обмениваются следующими структурами данных:

{"output": *входные данные*, "success": bool}, где *входные данные* зависят от того что делает функция,
а в формате bool передаётся информация об успешности выполнения предыдущей функции.

Все функции работают так, что если выполняются с ошибкой, она отлавливается и отправляется дальше
лишь строка сообщения об ошибке и флаг False в конструкции вывода функции скажет следующей функции
что информацию не надо обрабатывать, а просто пустить сообщение об ошибке дальше. Таким образом
на финальном выходе мы увидим сообщение об ошибке и сможем сразу понять в какой функции она возникла.

Функции:
1. get_weather_by_city({"output": *город*, "success": bool})

Работает при помощи библиотеки requests, отправляет запрос на следующий адрес:

https://api.openweathermap.org/data/2.5/weather?q={city}&appid={const.api_key}

2. get_my_ip_adress() - функция ищет ip адрес, принадлежащий пользователю с помощью библиотеки http.client,
полученные данные обрабатываются функциями библиотеки requests. Таким образом в случае успеха на выходе
имеем {"output": *ip-адрес*, "success": bool}

3. get_city_by_ip({"output": *ip-адрес*, "success": bool}) - ищет город по ip-адресу при помощи
get-запроса по адресу:

http://ip-api.com/json/{ip_adress["output"]}

4. output_constructor({"output": *данные о погоде или об ошибке*, "success": bool}) -
данная функция конструирует то, что увидит пользователь. Если все стадии до неё прошли успешно она
сформирует информацию в приятный и понятный для глаза вид, если нет - выдаст сообщение об ошибке, которая
произошла ранее. Данная функция для получения времени в нормальном виде использует функцию
datetime.datetime.fromtimestamp() из библиотеки datetime.

5. get_history(number) - во всё время выполнения программы в изначально пустой список history = []
добавлются данные об успешно обработанных запросах и при желании их можно вывести на экран. Данная Функция
для этого, на вход она принимает количество записей которые выведет, если оно окажется больше чем число
сохранённых записей, то она выведет все.
