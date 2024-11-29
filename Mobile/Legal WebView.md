Посмотрим в deeplink activity:

![image](/resourses/mobile2.png)

Видим, что есть какой то legal, который перекидывает нас на номер 3. Посмотрим на класс OpenBuckerSite

![image](/resourses/mobile7.png)

Видим, что он принимает парамет url и url должен заканчиваться на .website.yandexcloud.net

Попробуем закинуть ему сайт https://aboba.website.yandexcloud.net:

`am start -a android.intent.action.VIEW -d 'shadhw://legal?url=https://aboba.website.yandexcloud.net'`

Видим флаг в logcat'e.

Я не понимаю смысла этого задания, потому что этот сайт, очевидно, не существует. Но, видимо, можно сделать свой вредносный сайт на домене .website.yandexcloud.net и будет успех.