## Разница между фиксированными и динамическими URL маршрутами

Фиксированные маршруты - это те, которые совпадают с URL точь-в-точь. Если у вас есть маршрут `/about/`, то он ищет точно этот URL. Однако реальный мир часто требует более гибкого подхода. Вот где пригодятся динамические маршруты.

Динамические маршруты содержат переменные, которые изменяются в зависимости от запроса.  Например, `/blog/<post_id>/` - это динамический маршрут.  Этот маршрут описывает структуру URL для страницы блога, где `post_id` - это параметр пути, представляющая уникальный идентификатор статьи в блоге, который, как правило, является целым числом. 

Вы хотите, чтобы пользователи могли просматривать каждую статью, заменяя `post_id` в URL.

- `/blog/1/`: Пользователь просматривает статью с идентификатором 1.
- `/blog/42/`: Пользователь просматривает статью с идентификатором 42.

В Django параметр пути в URL (path parameter) представляет собой динамическую часть URL-шаблона, которая используется для передачи переменных данных в ваше веб-приложение. Эти параметры динамически извлекаются из URL-пути и передаются в представление Django для обработки.

### Конверторы маршрутов

Давайте поговорим о конверторах маршрутов в Django. Звучит, конечно, как что-то сложное, но на самом деле это мощный инструмент, который делает ваши URL-адреса гибкими и интуитивно понятными.

Когда вы определяете URL-маршруты в Django, часто вам нужно работать не только с фиксированными значениями, но и с разными типами данных, такими как числа, строки и даже даты. И вот где на помощь приходят конверторы маршрутов.

Допустим уникальный идентификатор статьи `post_id` должен быть целым числом. Тогда вы можете использовать URL `/blog/<int:post_id>`. `<int:post_id>`и есть конвертор маршрута, который заключён в треугольные скобки и состоит из двух частей разделённых двоеточием:

- `int` — тип конвертора
- `post_id` — параметр пути, который вы выбираете самостоятельно и будете использовать в обработчике.

```python
# myapp/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('blog/<int:post_id>/', views.blog_post),  # <int:post_id> - динамическая часть маршурута
    ...
]
```

```python
# myapp/views.py

def blog_post(request, post_id):  # post_id будет целым числом, так это указано в конверторе маршрута
    ...
```

Теперь, если пользователь вводит URL вида `/blog/42/`, Django автоматически преобразует `42` в целое число и передаст его в ваше представление. Важно, не забыть добавить новый аргумент в функцию-обработчик с тем же названием, что и в конверторе маршрута.  

 Вы можете использовать это число, например, для извлечения соответствующей статьи. А если пользователь введёт URL вида `/blog/some_string/`, то этот URL не будет обработан маршрутом `'blog/<int:post_id>/'`, так как часть пути `some_string` не является целым числом.

### Типы конвертеров

**1. Конвертер `str`:**

```python
# myapp/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('category/<str:category_name>/', views.show_category),
    ...
]

```

В данном случае, если пользователь введет URL вида `/category/science/`, то `category_name` будет передано в представление как строка "science".

Конвертер str используется по умолчанию и запись `<str:category_name>` можно сократить до `<category_name>`. 

**2. Конвертер `int`:**

```python
# myapp/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('article/<int:article_id>/', views.show_article),
    ...
]

```

Если пользователь введет URL `/article/42/`, то `article_id` передастся в представление как целое число 42.

Этот конвертер обрабатывает только неотрицательные числа. Например, URL `/article/-101/` обработан не будет, а `/article/0/` обработается корректно. 

**3. Конвертер `slug`:**

```python
# myapp/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('post/<slug:post_slug>/', views.show_post),
    ...
]

```

При вводе URL `/post/my-first-post/`, `post_slug` передастся в представление как строка "my-first-post".

Slug - это текстовая строка, которая используется в веб-разработке для создания человеко-понятных URL-адресов. Он представляет собой обычно короткую строку, состоящую из букв, цифр и дефисов, которая отражает название или описание ресурса на веб-сайте. 

<aside>
💡 Вот несколько причин, почему slug полезен:

1. **Читаемость для человека:** Слаги улучшают читаемость URL для людей. Они содержат осмысленные слова, отражающие содержание страницы, что делает URL более дружелюбным и понятным.
2. **Улучшение SEO:** Поисковые системы, такие как Google, обращают внимание на URL-адреса при определении релевантности страницы. Использование человеко-понятных слагов может помочь в повышении SEO-рейтинга, поскольку они предоставляют дополнительную информацию о содержании страницы.
3. **Легкость в написании и запоминании:** URL-адреса с понятными слагами легче запоминаются и могут быть легче вводимыми вручную. Это упрощает обмен ссылками и повышает общую доступность вашего контента.

Пример слага: Если у вас есть статья с заголовком "10 Полезных Советов по Веб-Разработке", связанным слагом может быть что-то вроде "10-sovetov-po-veb-razrabotke".

</aside>

**4. Конвертер `uuid`:**

```python
# myapp/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('user/<uuid:user_id>/', views.show_user),
    ...
]

```

При использовании URL `/user/123e4567-e89b-12d3-a456-426614174001/`, `user_id` передастся в представление как объект UUID.

UUID, или Универсальный Уникальный Идентификатор (Universal Unique Identifier), это уникальный идентификатор, который используется для однозначной идентификации информационных объектов

<aside>
💡 Вот несколько примеров генерации UUID из официальной [документации](https://docs.python.org/3/library/uuid.html#example) Python.

</aside>

**5. Конвертер `path`:**

```python
# myapp/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('page/<path:page_path>/', views.show_page),
    ...
]

```

Если пользователь введет URL `/page/nested/structure/page123/`, то `page_path` передастся в представление как строка "nested/structure/page123".

 и их обратное разрешение

### Функция `reverse`

Когда есть имена для маршрутов, можно легко ссылаться на них в коде. Когда нужно создать URL в коде, следует использовать функцию `reverse` в Django. Например:

```python
from django.urls import reverse

app_name = 'myapp'

urlpatterns = [
    path('myview/', ..., name='my_view'),
    ...
]

url = reverse('myapp:my_view')  # "/myview/"
```

В данном случае, он формирует URL `/myview/` по URL-шаблона с именем `"myapp:my_view"`  который затем можно использовать, например, для тестирования или редиректов внутри кода Django.

Так же функция `reverse` используется для получения URL-пути на основе имени URL-шаблона содержащего параметры пути.

```python
from django.urls import reverse

app_name = 'myapp'

urlpatterns = [
    path('myview/<slug:category>/instance/<int:pk>/', ..., name='my_view_with_args'),
    ...
]

url = reverse('myapp:my_view_with_args', args=('название-категории', 1))  # myview/название-категории/instance/1/
```

Таким образом, именование маршрутов в Django - это не просто хорошая практика, но и мощный инструмент для создания чистого, читаемого и легкого для сопровождения кода.