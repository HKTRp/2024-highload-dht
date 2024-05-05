# Таразанов Максим, ИТМО, М4139, Stage 1

Работа выполнена на WSL2.

При выполнении задания использовалась референсный **dao**.
В первую очередь было решено проверить точку разладки. В результате было получено, что разладка для **put** наступает при 4000rps, а для **get** -- при 3500rps.
Далее тестирование проводилось на 3500rps при замере времени в 120 секунд.
## GET-запросы
- Alloc
![getmany_3500_alloc.png](images%2Fgetmany_3500_alloc.png)
Около 60% выделяется на handleRequest, из которых 20% на создание реквеста, 40% на сервер. Стоит заметить, что больша'я часть памяти в совокупности нужна для создания memorySegment'ов.
Также около 20% аллокаций происходит на парсинг реквеста.

- CPU
![getmany_3500_cpu.png](images%2Fgetmany_3500_cpu.png)
Больше половины времени уходит на запись в сокет.

## PUT-запросы
- Alloc
![putmany_3500_alloc.png](images%2Fputmany_3500_alloc.png)
Около 60% также уходит на handleRequest, но на этот раз на сервер уходит ме'ньшая часть (порядка 35%). В совокупности опять же бо'льшая часть памяти уходит также на создание memorySegment'ов.

- CPU
![putmany_3500_cpu.png](images%2Fputmany_3500_cpu.png)
Больше половины времени уходит на запись в сокет.

## Улучшения
Предлагается сделать меньше операций со String путём работы с байтами запроса.