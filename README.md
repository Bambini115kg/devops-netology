## Домашнее задание к занятию "5.1. Введение в виртуализацию. Типы и функции гипервизоров. 
Обзор рынка вендоров и областей применения."

### Задача 1
1. Опишите кратко, как вы поняли: в чем основное отличие полной (аппаратной) виртуализации, паравиртуализации и 
виртуализации на основе ОС.
```
Если кратко, то:
 - при полной виртуализации имеем сервер (железо)+гипервизор, который сам 
является ОС и машина виртуализируется как есть
 - в паравиртуализации железо + ОС + сверху установлен гипервизор + запускаются
 модифицированные ОС
 -  при виртуализации на основе ОС мы имеем сервер + ОС + контейнеры с ВМ, которые
 представляются пользователям как основной сервер (имеют набор таких же характеристик) 
```
2. Выберите один из вариантов использования организации физических серверов, в зависимости от условий использования.

```
2.1.Высоконагруженная база данных, чувствительная к отказу -     Физический сервер 
        требуется более высокая производительность, аппаратное размещение дает более высокий отклик, 
        и сокращает точки отказа в виде гипервизора хостовой машны.
2.2. Различные web-приложения - Виртуализация уровня ОС (контейнеры)
       требуется меньше ресурсов, выше скорость масштабирования при необходимости расширения, 
       можно запускать различные ВМ на одном хосте
2.3. Windows системы для использования бухгалтерским отделом - Паравиртуализация
       Думаю, что при такой виртуализации проще балансировать нагрузку
2.4. Системы, выполняющие высокопроизводительные расчеты на GPU -  Физические сервера
        Думаю, что при таких расчетах, необходим прямой доступ ко всем ядрам GPU    
```
3. Выберите подходящую систему управления виртуализацией для предложенного сценария. Детально опишите ваш выбор.
тестирования
```
3.1. Думаю, что подойдёт Hyper-V. Во-первых используется в основном Microsoft,
во-вторых нужна репликация, чего нет в VMware
3.2. Скоре всего KVM подойдёт для такой виртуализации. Многие хостеры её используют)
3.3. Опять же Hyper-V ?
3.4. Я выбираю VirtualBox + Vagrant. Лего и быстро разворачивается для быстрого 
```
4. Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления виртуализацией одновременно)
и что необходимо сделать для минимизации этих рисков и проблем. Если бы у вас был выбор, то создавали бы вы гетерогенную среду или нет?
Мотивируйте ваш ответ примерами.
```
Первое, что приходит в голову, проблемы с дополнительным персоналом. Второе - это возможные проблемы
"подружить" разные системы виртуализации между собой, если вдуг необходимо будет обмениваться данными. 
Для минимизации этих рисков, я бы пробовал подогнать всё под одну систему виртуализации.
Если "драки" не избежать, то облака один из вариантов избежания проблем.
Работать в одной среде или в разных, я не точно не знаю как ответить. Опыта мало.С одной стороны_
хорошо бы иметь одну среду, но что делать, если она не удовлетворяет потребностям бизнеса?
```