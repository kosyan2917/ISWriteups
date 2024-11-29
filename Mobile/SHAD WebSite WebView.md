Посмотрим в deeplink activity:

![image](/resourses/mobile2.png)

Видим, что есть host.equals("shad-site")
Давайте заставим приложение перейти по дргуой ссылке. Для этого запустим в adb shell 

`am start -a android.intent.action.VIEW -d 'shadhw://shad-site?url=https://ya.ru'`

Нас перекидыает на сайт яндекса, а в логкате появляется флаг.