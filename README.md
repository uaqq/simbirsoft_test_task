**Локальный запуск приложения**
1.	Клонирую проект к себе на машину.
2.	Пытаюсь запустить проект по описанию из readme.md – ставлю зависимости через `pip install -r requirements.txt` и запускаю приложение командой python3 app.py.
3.	После запуска получаю ошибку:
![](https://i.imgur.com/B8G6QGo.png)
4.	Иду в гугл с запросом `AttributeError: module 'wtforms.validators' has no attribute 'required'`. Ответ получаю по первой же ссылке на stackoverflow: `Solved - manually changed validators.required() call to validators.DataRequired() in flask-admin -> fileadmin init.py . They distinguished them in v1.0.2.`
5.	В ошибке в аутпуте консоли сказано, что проблема возникла на 6 и 7 строчке в файле `/home/citrulline/simbirsoft/flaskex/scripts/forms.py`.
6.	Открываю файл через текстовый редактор и меняю 6 и 7 строки с `validators.required()` на `validators.DataRequired()`. Сохраняю изменения.
7.	Запускаю приложение командой `python3 app.py`.
8.	В аутпуте консоли получаю информацию о порте, на котором доступно приложение.
![](https://i.imgur.com/Wp71sEa.png)
9.	Иду на 5000 порт по адресу `http://192.168.245.128:5000` и вижу запущенное приложение.
![](https://i.imgur.com/2LCRVQo.png)

**Упаковка приложения в Docker контейнер**
1.	Беру легковесный образ python alpine с docker hub.
2.	Пишу докерфайл.
 ![](https://i.imgur.com/dMEuvef.png)

Задаю рабочей директорию /app. В ней будет находиться само приложение.
Копирование зависимостей выношу в самый верх. Зависимости редко изменяются, а значит, что слой будет реже меняться и пересобираться, что впоследствии увеличит скорость сборки образа.
Устанавливаю зависимости для приложения, используя RUN.
Копирую все файлы проекта в контекст сборки.
Указываю через EXPOSE, что в образе используется порт 5000.
Через ENTRYPOINT запускаю приложение.
 
3.	Собираю образ командой `docker build -t simbirsoft:latest .` с тегом `simbirsoft:latest`.
4.	Запускаю образ командой `docker run -p 5000:5000 -d simbirsoft`, указывая через флаг –p, что надо замаппить порт 5000 контейнера на порт 5000 хоста.
5.	Через `docker ps` смотрю, что контейнер успешно запущен и не упал.
![](https://i.imgur.com/mBtXrYM.png)
6.	Перехожу по адресу хостовой машины на 5000 порт и проверяю доступность приложения.
![](https://i.imgur.com/f858eBh.png)

**Запуск приложения через docker compose**
1.	Пишу файл `docker-compose.yml` с теми же параметрами, что и раньше. В качестве образа использую образ, полученный при сборке ранее.
![](https://i.imgur.com/sAhenfl.png)
2.	Запускаю контейнер командой `docker compose up`.
![](https://i.imgur.com/K9RoT7M.png)
3.	Проверяю доступность приложения через `curl 127.0.0.1:5000` и получаю ответ.
![](https://i.imgur.com/8SxHmkv.png)
