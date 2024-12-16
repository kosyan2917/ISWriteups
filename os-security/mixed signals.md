Что ж давайте посмотрим, что на сервере: `nc signals.is-course.ru 2412`

Напишем ls

![image](/resourses/os1.png)

Видим два файлика, один из них флаг.тхт. Так давайте выведем его содержимое!

![image](/resourses/os2.png)

Облом :(

Что ж, тогда запустим readflag:

![image](/resourses/os3.png)

Нам предлашает решить примерчик, но программа тут же завершается. Давайте посомтрим, в чем дело: `strace ./readflag`

![image](/resourses/os4.png)

Видим, что программе посылается сигнал SIGALRM, из-за чего она завершается. Давайте не допустим этого! `trap "" ./readflag`

![image](/resourses/os5.png)

Вот и все! 

![image](/resourses/kchaw.jpg)