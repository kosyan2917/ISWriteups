Для этой категории вам нужены следующие инструменты:
- Android studio
- ADB
- JADX

Большой минус данной домашки в том, что я смог ее сделать, практически не понимая, что происходит. Поэтому объяснений, как та или иная вещь работает, будет немного. Но я постараюсь.

Так как я флаги уже сдал и я не помню, чей флаг чей, поэтому я надеюсь, что расставлю все правильно.

По факту все, что нужно делать - это смотреть android-manifest.xml на предмет полезных активити.

Много задач решается через deeplink activity, вот так он выглядит в манифесте:

![image](/resourses/mobile3.png)

Тут часто возникает логкат, его можно вывести через adb или перейти во вкладку logcat в adnroid studio