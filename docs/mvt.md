## MVT (Model-View-Template)

Django използва архитектурния модел **MVT (Model-View-Template)**, който е подобен на популярния **MVC (Model-View-Controller)**, но с малка разлика.

* В MVT моделът, контролерът е частично интегриран във View, което опростява работата на разработчика.
* Django автоматично обработва част от логиката, което прави разработката по-бърза и ефективна.

---

## Как работи MVT?

Когато потребителят направи заявка към Django приложение, тя преминава през следните стъпки:

1. **User** (Потребител) – Изпраща HTTP заявка (напр. отваря уеб страница).
2. **Django** – Получава заявката и я препраща към URL маршрутизацията.
3. **URL** (urls.py) – Насочва заявката към съответната View функция.
4. **View** (views.py) – Основната бизнес логика. Обработва заявката, комуникира с Model (ако е необходимо) и подготвя отговора.
5. **Model** (models.py) – Комуникира с базата данни и връща нужните данни към View.
6. **View** (views.py) – Подготвя отговора и избира Template за рендиране.
7. **Template** (HTML шаблон) – Представя данните на потребителя във визуален формат.

---

## Обяснение на всяка част от MVT

## **Model** – Работа с данни

* Model е частта, която работи с базата данни.
* Django използва ORM (Object-Relational Mapping), което означава, че можете да работите с базата без да пишете SQL.

**Примерен Model в Django (models.py):**

```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=255)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    description = models.TextField()
```

* `name` – Име на продукта (текстово поле с ограничена дължина).
* `price` – Цена на продукта (десетично число).
* `description` – Описание на продукта.

**Използване на Model в View:**

```python
products = Product.objects.all()  # Взима всички продукти от базата
```

---

## **View** – Бизнес логика

* View обработва логиката на приложението.
* Получава данни от Model и избира кой Template да се използва.

**Примерен View в Django (views.py):**

```python
from django.shortcuts import render
from .models import Product

def product_list(request):
    products = Product.objects.all()  # Взимаме всички продукти от базата
    return render(request, 'product_list.html', {'products': products})
```

* `request` – HTTP заявка от потребителя.
* `Product.objects.all()` – Взимаме всички продукти от базата данни.
* `render()` – Използваме Template (HTML файл) за показване на данните.

---

## **URL** – Свързване на View с URL

* Django използва urls.py, за да свърже URL пътища с View функции.

**Примерен URL в Django (urls.py):**

```python
from django.urls import path
from .views import product_list

urlpatterns = [
    path('products/', product_list, name='product_list'),
]
```

* `/products/` – URL, който потребителят ще отвори.
* `product_list` – Функция, която ще се изпълни, когато потребителят отвори страницата.

---

## **Template** – Представяне на данните

* Template е HTML файл, който визуализира данните, подготвени от View.
* Django използва шаблонен език (Django Template Language – DTL) за динамично вмъкване на данни.

**Примерен Template (product_list.html):**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Списък с продукти</title>
</head>
<body>
    <h1>Продукти</h1>
    <ul>
        {% for product in products %}
            <li>{{ product.name }} - {{ product.price }} лв.</li>
        {% endfor %}
    </ul>
</body>
</html>
```

* `{% for product in products %}` – Цикъл, който изписва всеки продукт от списъка.
* `{{ product.name }}` – Вмъква името на продукта в HTML.

---

## Целият процес на работа в Django MVT

1. Потребителят отваря https://example.com/products/.
2. Django търси маршрута /products/ в urls.py.
3. Django изпълнява product_list() от views.py.
4. View взема всички продукти от Model (базата данни).
5. View предава списъка с продукти към Template (HTML файл).
6. Template рендерира списъка като HTML страница.
7. Браузърът на потребителя показва крайния резултат.

---

## Разлика между MVT и MVC

| Характеристика  | MVC (Model-View-Controller)                   | MVT (Model-View-Template)         |
|-----------------|-----------------------------------------------|-----------------------------------|
| **Model**       | Управлява базата данни                        | Същото                            |
| **View**        | Показва информацията (шаблони)                | Обработва бизнес логиката         |
| **Controller**  | Управлява взаимодействието между Model и View | Django автоматично поема тази роля|
| **Template**    | Обикновено част от View                       | Самостоятелен компонент           |


* В **MVC**, контролерът (Controller) управлява логиката между моделите и изгледите.
* В **MVT**, Django автоматично се грижи за част от контролирането, което улеснява разработката.