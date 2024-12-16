Откроем презу по реверсу. Там есть такой слайд:

![image](/resourses/reverse6.png)

Проделаем то же самое у себя с этим файлом

![image](/resourses/reverse7.png)

![image](/resourses/reverse8.png)

Получили зашифрованные данные, ключ и инструкции. По лекции я предположил, что в r1 кладется указатель на первый символ зашифрованных данных. Дальнейший мой анализ привел меня к тому, что скорее всего, эти инструкции просто перепорачивают шифротекст задом наперед, но оставляя байты на месте, как это было в предыдущей задаче, а потом ксорит с ключом. Проверим. Питон в помощь!

```python
s = "5b0b11114a120e0c08151d115d160e0705085b17070108071604190417"
result = []
for i in range(len(s), 0, -2):
    result.append(s[i-2:i])
print("".join(result))
```

Если возникает вопрос, откуда s - это хексы зашифрованного сообщения из первого скрина.

Скрипт выдает нам это `170419041607080107175b0805070e165d111d15080c0e124a11110b5b`. Заксорим его с ключом:

![image](/resourses/reverse9.png)

Забавно, но путь получился наоборот. Я не знаю с чем это связано. После переворота получаем `/etc/thelast/shall/bedefeated`.