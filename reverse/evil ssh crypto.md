Откроем наш lib в иде. Видим в мейне call на sys_connect, а сверху в ргеистр `rсx` записывается какая то 16-ная строка.

![image](/resourses/reverse3.png)

Откроем презентацию по реверсу. Видим, что это вот такая структура:

![image](/resourses/reverse4.png)

2 байта под AF_INET, 2 байта под Port number, 2 байта под айпи. Но стоит учитывать, что это little endian (кажется :) ) и читать надо слева направо, но побайтово. Буква h На конце нас не интересует, она указывает, что это хекс (опять же вроде бы, но точно знаю, что это не данные). Получается, что AF_INET - это 0200, порт 0035, ip - 5bcae98d. Осталось все перевести из 16-й системы в 10-ю. Питон в помощь:

```python
ip = "5b ca e9 8d"
ip = ip.split()
port = "35"
for h in ip:
    print(int(h, 16), end=".")
print()
print(int(port, 16))
```

Получаем:

![image](/resourses/reverse5.png)