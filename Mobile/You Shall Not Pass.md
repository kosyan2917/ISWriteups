Глянем android manifest. Видим, что есть какая то активити, которая требует кастомный permission.

![image](/resourses/mobile1.png)

Попробовав вызвать его через adb, получим ошибку, что нам не хватает прав для успеха в жизни. Посмотрим на deeplink activity, в котором есть много всякой всяки:

![image](/resourses/mobile2.png)

Видим, что есть возможность вызвать редирект. Видим, что у него есть параметр uri. Если указать какой нибудь валидный урл, например, https://ya.ru, то нас перекинет в браузер, где мы увидим, как у нас грузится страничка с яндексом.

Давайте попробуем вызвать урл так, как написано в интенте у PrivateActivity: shadhw-secret://flag

Сделать это можно следующей командой в adb shell:

`am start -a android.intent.action.VIEW -d "shadhw://redirect?uri=shadhw-secret://flag"`

Мы увидим, как нас перекидывает на флаг.