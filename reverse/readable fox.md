Откроем в иде file_generator, видим мейн:

![image](/resourses/reverse`1.png)

В мейне видим call `github_com_bishopfox_sliver_imlant_sliver_transport_StartConnectionLoop`. Посмотрим на модуль github_com_bishopfox_sliver. Это модуль для зловредов. В функции github_com_bishopfox_sliver_implant_sliver_transports_C2Generator_func2 можно найти строку с адресом:

![image](/resourses/reverse2.png)
Вот и наш урл. Вводим его в ответ и выигрываем