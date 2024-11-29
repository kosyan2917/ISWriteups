В нашем приложении есть классная страничка с профилем, в которой буквально ничего нет... или есть? Давайте посмотрим на эту штуку в deeplink activity:

![image](/resourses/mobile2.png)

Видим, что profile под номером 2. Перейдем в класс OpenProfilePage и глянем на него.

![image](/resourses/mobile5.png)

Видим, что есть функция getFlag(), проброшенная в JS через интерфейс shad.
Давайте теперь перейдем в профиль и посмотрим Logcat (вкладка в AndroidStudio). Видим, что осуществляется переход по ссылке https://shad-homework-android.website.yandexcloud.net/profile.html?username=John%20Doe. Кажется, что поле username уязвимо для xss. Проверим:

В adb shell пропишем `am start -a android.intent.action.VIEW -d 'shadhw://profile?username=<img%20src=x%20onerror="alert(123)"/>'`. Видим, что алерт открылся:

![image](/resourses/mobile6.png)

Значит xss действительно есть. Тогда давайте попробуем вызвать функцию getFlag():

`am start -a android.intent.action.VIEW -d 'shadhw://profile?username=<img%20src=x%20onerror="shad.getFlag()"/>'`

Смотрим в logcat и видим флаг.