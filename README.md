# Лабораторная работа №10. Веб-фреймворк Flask.

## Теоретический материал 

Flask – компактный фреймворк для быстрой разработки веб-приложений. Он предоставляет минимальную необходимую функциональность и не навязывает никаких строгих правил в отношении структуры и архитектуры приложения (как это делает Django).

Flask универсален – на его основе можно создавать сложные приложения и API, и в то же время он идеально подходит для разработки небольших проектов. Самый большой плюс Flask – на нем очень просто реализовать генератор статических сайтов.

Основные преимущества Flask:

- **Минималистичность.** Flask отличается небольшим размером  в нем есть все самое необходимое и нет ничего лишнего.
- **Гибкость.** Фреймворк не диктует определенных правил и позволяет разработчику сохранить полный контроль над структурой приложения.
- **Простота в использовании.** Он имеет несколько встроенных функций, которые позволяют сразу начать создавать полноценные веб-приложения, даже если у вас нет опыта в веб-разработке на Python. Например, у Flask есть встроенный сервер, поддержка сессий, обработчик форм, шаблонизатор.
- **Интеграция с дополнительными библиотеками.** Фреймворк очень просто интегрируется с многочисленными библиотеками, которые расширяют его функциональность. Это позволяет создать гибкий, масштабируемый проект для любой сферы.
- **Простота тестирования.** У Flask есть встроенный тестовый клиент, который максимально упрощает тестирование и отладку.

### Установка
Flask лучше всего устанавливать в виртуальное окружение – это позволяет избежать появления ошибок, связанных с конфликтами версий различных библиотек и модулей. Выполните в cmd:
```bash=
python -m venv fproject\venv
cd fproject
venv\scripts\activate # активация виртуального окружения
pip install flask
```
Активировать виртуальное окружение нужно перед каждым сеансом работы с Flask.

### Простейшее приложение на Flask

Напишем приложение, которое будет выводить традиционное приветствие Hello, World! в браузере. Сохраните этот код в файле app.py в директории fproject:

```python=
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run()
```

Этот код создает объект приложения Flask с помощью класса Flask и присваивает его переменной **app**. Декоратор ```@app.route('/') ```устанавливает маршрут для главной страницы нашего приложения, а метод ```def hello() ```определяет, что будет отображаться на этой странице.
:::warning
Подробнее про декораторы [тут!](https://metanit.com/python/tutorial/2.28.php)
:::

```if __name__ == '__main__':``` проверяет, запускается ли данный файл как самостоятельное приложение, или импортируется как модуль. В нашем случае он запускается как независимое приложение, поэтому вызывается метод ```app.run()```, который запускает веб-сервер Flask.

Запустите приложение в командой строке, Откройте адрес http://localhost:5000/ в браузере:

![](https://i.imgur.com/QnxayFn.png)

Flask по умолчанию использует порт 5000. При желании его можно изменить на более привычный 8000:

```python=
app.run(port=8000)
```

Кроме того, можно включить режим отладки – тогда все возникающие ошибки будут отображаться на странице браузера, а при внесении любых изменений в файлы проекта сервер будет автоматически перезагружаться:

```python=        
app.run(debug=True)
```
Для остановки сервера нажмите **Ctrl+C**.

### Маршруты в Flask

Маршруты – это URL-адреса, по которым пользователи могут открывать определенные страницы (разделы) веб-приложения. Маршруты в Flask определяются с помощью декоратора ```@app.route()```. Для каждого маршрута можно написать отдельную функцию представления, которая будет выполнять какие-то действия при переходе по определенному адресу. Рассмотрим пример:

```python=
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return 'Это главная страница.'

@app.route('/about')
def about():
    return 'Здесь будет информация об авторе сайта.'

@app.route('/blog')
def blog():
    return 'Это блог с заметками о работе и увлечениях.'

if __name__ == '__main__':
    app.run()
```
Сохраните код, запустите приложение, последовательно откройте адреса:
- http://localhost:5000/
- http://localhost:5000/about
- http://localhost:5000/blog

![](https://i.imgur.com/vLPPPBR.png)

### Переменные в маршрутах

В URL можно передавать различные значения. Запустите этот код и перейдите по адресу, например, http://localhost:5000/user/EvilAdmin

```python=
from flask import Flask

app = Flask(__name__)

@app.route('/user/<username>')
def user_profile(username):
    return f"Это профиль пользователя {username}"

if __name__ == '__main__':
    app.run()
```

Имя пользователя, переданное в качестве переменной, будет показано на странице:

![](https://i.imgur.com/1pz7QxO.png)

А так можно передать в маршруте целое число:
```python=
from flask import Flask

app = Flask(__name__)

@app.route('/user/<int:user_id>')
def user_profile(user_id):
    return f"Это профиль пользователя с ID {user_id}"

if __name__ == '__main__':
    app.run()
```
Перейдите по адресу, например, http://localhost:5000/user/5:

![](https://i.imgur.com/ifwBh3E.png)

### GET- и POST-запросы
**GET и POST** – это HTTP-запросы, которые используются для отправки данных между клиентом и сервером.

**GET-запрос** применяют для получения данных от сервера. При выполнении GET-запроса клиент отправляет запрос на сервер, а сервер возвращает запрошенную информацию в ответ. GET-запросы могут содержать параметры в URL-адресе, которые используются для передачи дополнительных данных.

**POST-запрос** используют для отправки данных на сервер. При выполнении POST-запроса клиент отправляет данные на сервер, а сервер их обрабатывает. POST-запросы обычно применяют для отправки форм, с данными из которых нужно что-то сделать на бэкенде.

Рассмотрим простейший пример обработки формы авторизации. Базы данных для хранения учетных записей у нас пока нет, поэтому в приведенном ниже коде мы пропустим всю функциональность для проверки корректности логина и пароля (мы рассмотрим этот вопрос позже, в одном из заданий):

```python=
from flask import Flask, request, render_template

app = Flask(__name__)

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        # проверка логина и пароля
        return 'Вы вошли в систему!'
    else:
        return render_template('login.html')

if __name__ == '__main__':
    app.run()
```
Маршрут``` @app.route('/login', methods=['GET', 'POST'])``` обрабатывает и **POST**, и **GET-запросы**: в первом случае он отправит данные на сервер, во втором – просто выведет страницу с формой авторизации.

Для вывода формы на странице сделаем простейший шаблон. Этот код нужно сохранить в файле login.html, **в директории templates (в этой папке Flask по умолчанию ищет шаблоны)**:

```htmlmixed=
{% extends 'base.html' %}

{% block content %}
  <div class="container">
    <div class="row justify-content-center mt-5">
      <div class="col-md-6">
        <div class="card">
          <div class="card-header">
            <h1 class="text-center">Вход на сайт</h1>
          </div>
          <div class="card-body">
            {% with messages = get_flashed_messages() %}
              {% if messages %}
                <div class="alert alert-danger">
                  <ul>
                    {% for message in messages %}
                      <li>{{ message }}</li>
                    {% endfor %}
                  </ul>
                </div>
              {% endif %}
            {% endwith %}
            <form method="post">
              <div class="mb-3">
                <label for="username" class="form-label">Логин:</label>
                <input type="text" class="form-control" id="username" name="username" required>
              </div>
              <div class="mb-3">
                <label for="password" class="form-label">Пароль:</label>
                <input type="password" class="form-control" id="password" name="password" required>
              </div>
              <div class="text-center">
                <button type="submit" class="btn btn-primary">Войти</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
{% endblock %}
```

Никакие CSS стили к шаблону не подключены, поэтому он выглядит не слишком привлекательно. Но шаблон работает, а форма получает логин и пароль:
![](https://i.imgur.com/V615YN5.png)

### Шаблонизатор Jinja2

Шаблоны в Flask используются для динамического формирования веб-страниц. Шаблоны представляют собой HTML страницы, в которые можно передавать любые данные с бэкенда. К шаблонам можно подключать любые CSS-фреймворки типа Bootstrap и Tailwind, и любые JS-скрипты.

Поведением шаблонов управляет шаблонизатор Jinja2 – он предоставляет функциональность для создания условий, циклов, макросов, наследования и блоков. Главные преимущества шаблонизатора:

- Может проводить различные операции с контентом самостоятельно, не обращаясь к бэкенду.
- Обеспечивает наследование дизайна и стилей от базового шаблона.

Наследование работает так:

- Базовый шаблон, который обычно называется base.html, содержит общую разметку для сайта.
- В base.html подключаются локальные и CDN-фреймворки (CSS, JS), задаются фоновые изображения и фавикон.
- Дочерние шаблоны наследуют этот базовый шаблон и дополняют его своим собственным контентом.

Продемонстрируем наследование на примере. Сохраните в папке **templates** два файла. Это содержимое файла **base.html** – в нем подключается CSS-фреймворк Bootstrap, кастомные стили **custom.css** из статической папки **static**, иконки Font Awesome:

```htmlmixed=
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>{% block title %}{% endblock %}</title>
    <!-- Bootstrap стили -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- иконки fontawesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
    <!-- кастомные стили -->
    <link rel="stylesheet" href="{{ url_for('static', filename='custom.css') }}">
</head>
<body>

<!-- навигация -->
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container">
        <a class="navbar-brand" href="#">Мой личный сайт</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                data-bs-target="#navbarNav" aria-controls="navbarNav"
                aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse justify-content-end"
             id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item">
                    <a class="nav-link" href="#">Главная</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Обо мне</a>
                </li>
            </ul>
        </div>
    </div>
</nav>

<!-- контент дочерних шаблонов -->
<div class="container my-3">
    {% block content %}
    {% endblock %}
</div>

</body>
</html>
```
Шаблон base.html также содержит верхнее меню для навигации по сайту – это меню будут наследовать все дочерние шаблоны. Вот пример дочернего шаблона about.html:

```htmlmixed=
{% extends 'base.html' %}

{% block title %}Мое резюме{% endblock %}

{% block content %}
    <div class="container my-5">
        <h1 class="text-center mb-4">Мое резюме</h1>
        <div class="row">
            <div class="col-md-6">
                <h2>Образование</h2>
                <h4>Московский политехнический институт</h4>
                <p class="mb-0">Бакалавр Computer Science</p>
                <p class="text-muted">2016 - 2020</p>
            </div>
            <div class="col-md-6">
                <h2>Опыт работы</h2>
                <h4>Web Developer - XYZ компания</h4>
                <p class="mb-0">2019 - н.в.</p>
                <p class="text-muted">- Разработка и поддержка веб-приложений</p>
                <p class="text-muted">- Работа с Python, Django, HTML/CSS, JavaScript, MySQL</p>
            </div>
        </div>
        <div class="row mt-5">
            <div class="col-md-6">
                <h2>Навыки</h2>
                <ul class="list-group">
                    <li class="list-group-item border-0 py-1"><i class="fa fa-cog"></i> Python</li>
                    <li class="list-group-item border-0 py-1"><i class="fa fa-cog"></i> Django</li>
                    <li class="list-group-item border-0 py-1"><i class="fa fa-cog"></i> HTML/CSS</li>
                    <li class="list-group-item border-0 py-1"><i class="fa fa-cog"></i> JavaScript</li>
                </ul>
            </div>
            <div class="col-md-6">
                <h2>Проекты</h2>
                <h4>Сайт для продажи автомобилей</h4>
                <p class="text-muted mb-0">- Разработка сайта с использованием Django</p>
                <p class="text-muted">- Интеграция с API маркетплейса для получения данных об автомобилях</p>
                <h4>Игровой блог</h4>
                <p class="text-muted mb-0">- Разработка блога с использованием Django</p>
                <p class="text-muted">- Возможность создавать учeтные записи пользователей и писать комментарии</p>
            </div>
        </div>
        <div class="row mt-5">
            <div class="col-md-6">
                <h2>Контакты</h2>
                <p class="mb-0"><i class="fa fa-phone"></i> Телефон: +990123456789</p>
                <p class="mb-0"><i class="fa fa-envelope"></i> Email: example@example.com</p>
                <p class="mb-0"><i class="fa fa-github"> GitHub: <a href="https://github.com/example"></i>example</a></p>
            </div>
            <div class="col-md-6">
                <h2>Языки</h2>
                <ul class="list-group">
                    <li class="list-group-item border-0 py-1"><i class="fa fa-check-circle"></i> Английский (C1)</li>
                    <li class="list-group-item border-0 py-1"><i class="fa fa-check-circle"></i> Немецкий (B2)</li>
                    <li class="list-group-item border-0 py-1"><i class="fa fa-check-circle"></i> Русский (родной)</li>
                </ul>
            </div>
        </div>
    </div>
{% endblock %}
```

Фон страницы шаблонизатор берет из файла static/customs.css:

```css=
body {
    background-color: #e5e5e5;
}
```
А код для вывода страницы выглядит так:

```python=
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/about')
def about():
    return render_template('about.html')

if __name__ == '__main__':
    app.run(debug=True)
```

Запустите приложение, откройте адрес http://localhost:5000/about:

![](https://i.imgur.com/OMCX1XC.png)

:::danger
 [Про jinja2 и работу с шаблонами тут](https://proglib.io/p/rukovodstvo-dlya-nachinayushchih-po-shablonam-jinja-v-flask-2022-09-05)
:::

### Работа с базой данных

Для работы с базами данных в Flask удобно использовать ORM **SQLAlchemy**. Как уже упоминалось в предыдущей главе о **SQLite**, ORM играет роль своеобразной прослойки между приложением и СУБД SQLite, и позволяет работать с базами без использования языка SQL. Надо заметить, что работать с базами данных в SQLAlchemy немного сложнее, чем в Django ORM, но гораздо проще, чем в чистом Python.

Начнем с установки SQLAlchemy в виртуальное окружение:
```bash=
pip install flask-sqlalchemy
```

В SQLAlchemy основой для создания таблиц в базе данных служат **модели** (специальные классы). Поля классов определяют структуру таблицы, которая будет использоваться для хранения информации в базе данных. В полях классов можно задавать типы данных, которые соответствуют типам данных в БД, например, String для хранения строк, Integer для целых чисел, Float для плавающих чисел и т.д.

SQLAlchemy, как и другие ORM, очень упрощает создание связей между таблицами. В приведенном ниже примере используется связь один ко многим (ForeignKey), поскольку у одного исполнителя может быть несколько альбомов, а в одном альбоме всегда будет несколько треков:

```python=
class Album(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    year = db.Column(db.String(4), nullable=False)
    artist_id = db.Column(db.Integer, db.ForeignKey('artist.id'), nullable=False)
    artist = db.relationship('Artist', backref=db.backref('albums', lazy=True))

class Song(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    length = db.Column(db.String(4), nullable=False)
    track_number = db.Column(db.Integer, nullable=False)
    album_id = db.Column(db.Integer, db.ForeignKey('album.id'), nullable=False)
    album = db.relationship('Album', backref=db.backref('songs', lazy=True))
```

:::danger
Про методы и классы читайте  [тут!](https://metanit.com/python/tutorial/7.1.php)
:::


Сохраните этот код в файле **models.py** – мы будем импортировать модели из него в главный файл приложения app.py и в скрипт **create_db.py**, который создает базу данных и заполняет ее тестовой информацией.

Код для create_db.py будет следующим:

```python=
from flask import Flask
from models import Artist, Album, Song, db
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///music.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db.init_app(app)


if __name__ == '__main__':
    with app.app_context():
        db.create_all()

        # создаем тестовых исполнителей
        artist1 = Artist(name='The Rolling Stones')
        artist2 = Artist(name='Jefferson Airplane')
        artist3 = Artist(name='Nine Inch Nails')
        artist4 = Artist(name='Tool')
        db.session.add_all([artist1, artist2, artist3, artist4])
        db.session.commit()

        # создаем тестовые альбомы
        album1 = Album(title='Aftermath', year='1966', artist=artist1)
        album2 = Album(title='Beggars Banquet', year='1968', artist=artist1)
        album3 = Album(title='Surrealistic Pillow', year='1967', artist=artist2)
        album4 = Album(title='Broken', year='1992', artist=artist3)
        album5 = Album(title='The Fragile', year='1999', artist=artist3)
        album6 = Album(title='Lateralus', year='2001', artist=artist4) 
        album7 = Album(title='AEnima', year='1996', artist=artist4) 
        album8 = Album(title='10,000 Days', year='2006', artist=artist4) 

        # создаем тестовые песни
        song1 = Song(title='Paint it Black', length='4:20', track_number=1, album=album1)
        song2 = Song(title='Sympathy For The Devil', length='3:53', track_number=2, album=album1)
        song3 = Song(title='White Rabbit', length='3:42', track_number=5, album=album3)
        song4 = Song(title='Wish', length='3:46', track_number=6, album=album4)
        song5 = Song(title='Starfuckers, Inc.', length='5:00', track_number=1, album=album5)
        song6 = Song(title='Schism', length='6:46', track_number=7, album=album6)
        song7 = Song(title='Eulogy', length='8:29', track_number=3, album=album7)
        song8 = Song(title='Vicarious', length='7:07', track_number=5, album=album8)
        db.session.add_all([album1, album2, album3, album4, album5, album6, album7, album8, song1, song2, song3, song4, song5, song6, song7, song8])
        db.session.commit()
```

Этот код наглядно демонстрирует, как именно создаются записи, и какие между ними существуют связи. Чтобы создать и заполнить базу, запустите файл в активированном виртуальном окружении:

```python=
(venv) C:\Users\User\fproject>create_db.py
```

В реальных приложениях базу данных удобнее заполнять, например, с помощью модуля csv.

Главный файл приложения app.py выглядит так:

```python=
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///music.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

# связываем приложение и экземпляр SQLAlchemy
db.init_app(app)


@app.route('/songs')
def songs():
    songs_list = Song.query.all()
    return render_template('songs.html', songs=songs_list)

if __name__ == '__main__':
    app.run(debug=True)
```

Шаблон songs.html использует тот же самый базовый base.html, что и предыдущий пример:

```htmlmixed=
{% extends 'base.html' %}

{% block title %}
Мои любимые песни
{% endblock %}

{% block content %}
<h1 class="mb-5">Мои любимые песни</h1>

<div class="card-columns">
  <div class="row">
  {% for song in songs %}
    <div class="col-md-3">
      <div class="card mb-3">
          <div class="card-header fw-bold">{{ song.title }}</div>
          <div class="card-body">
              <p class="badge bg-primary text-wrap">{{ song.album.artist.name }}</p>
              <p class="card-text">Альбом: 
              <strong>{{ song.album.title }}</strong></p>
              <p class="card-text">Длина: {{ song.length }} минут</p>
              <p class="card-text">Номер трека: {{ song.track_number }}</p>
              <p class="card-text">Дата релиза: {{ song.album.year }}</p>
          </div>
      </div>
    </div>
  {% endfor %}
  </div>
</div>

{% endblock %}
```
После создания шаблона можно запустить приложение:

![](https://i.imgur.com/KZ6Qs8V.png)

## Практическое задание

1. Напишите Flask-приложение, которое выводит в шаблон index.html приветствие для пользователя. Приветствие зависит от времени суток:
    - С 6:00 до 12:00 – «Доброе утро»
    - С 12:00 до 18:00 – «Добрый день»
    - С 18:00 до 24:00 – «Добрый вечер»
    - С 00:00 до 6:00 – «Доброй ночи»
2. Напишите Flask-приложение, которое будет обсулживать две веб страницы ```список задача``` и ```добавить задачу```, ваше приложение доложно уметь получать новые задачи от пользовотеля и отображать новые в списке задач. 
3. Напишите app.py и шаблон welcome.html, которые выводят различный контент для пользователей с разными правами доступа: 
    - Админ имеет полный доступ.
    - Модератор может редактировать записи и комментарии.
    - Рядовой пользователь может создавать записи от своего имени и просматривать френдленту.
    Пример:
   ![](https://i.imgur.com/SUX4c1F.png)

4. Для супермаркета нужно написать веб-приложение, которое выводит список товаров на складе и позволяет добавлять новые.
    Пример:
    Список товаров
   ![](https://i.imgur.com/jdjhIDS.png)
   Добавление товара
   ![](https://i.imgur.com/91GiuiK.png)

5. Напишите сайт заметок, ваше приложение должно уметь показывать список заметок с кратким описанием и заголовком, показывать полный текст заметки при клике в новом шаблоне, и также ваше приложение должно уметь добавлять и удалять заметки, заметки должны храниться в базе данных. #   l a b 1 0  
 