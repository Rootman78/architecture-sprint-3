Задание 2. Проектирование микросервисной архитектуры

Как понял из технического задания, наиболее значимые критерии при создании нового приложения это сроки (необходимо сделать MVP продукт) и ограниченность в ресурсах (небольшая компания). В системе не планируется сверх большое количество пользователей (при 10 кратном масштабировании их будет около 1000), но необходимо осуществлять надежную связь. Все решения принимаются исходя из этого.

После анализа монолита было принято решение использовать микросервисы при разработке нового функционала.
Программное решение будет содержать следующие микросервисы:
1. Микросервис управления отоплением
2. Микросервис управления светом
3. Микросервис управления воротами
4. Микросервис управления видеонаблюдением
5. Микросервис подключения устройств умного дома
6. Микросервис аутентификации и авторизации


Пользователь может управлять устройствами при помощи мобильного приложения, либо через web браузер
У мобильного приложения web интерфейса будут одинаковые конечные точки. Поэтому у них будут общий бэк-энд. Различие только во фронтэнд частях. В исходном задании не сказано на чем написан фронтэнд веб интерфейса, поэтому предлагаем оставить его том же технологическом стеке для минимизации доработок.
Про мобильные приложения, если оно было используем тот же технологический стек. Если его не было до этого, то необходимо его разработать, так как без этого невозможно управлять системами находясь вне дома, и без него тяжело продвигать наше программное решение на высококонкурентном рынке. Так как нужен достаточно стандартный функционал, предлагаю для экономии писать кроссплатформенное приложение и использовать React Native. Для экономии, в мобильном приложении будет использоваться только основной функционал, связанный с управлением и мониторингом. Функционал настройки и подключения датчиков будет доступен только через веб приложение.

Так как бэк-энд функционал систем отопления написан на Java, следовательно в компании есть опыт написания на этом языке, и разработчики нужной квалификации, поэтому планируем продолжать использовать этот язык для написания дополнительного функционала микросервисов. 

Для связи с внешними датчиками и реле будим использовать асинхронное взаимодействие, так как предполагаем, что ряд домов будет находится за городом, где возможны более частые проблемы с интернетом, и нам важно чтобы команды отправленные пользователем обязательно отработали. Предполагаю использовать Apache Kafka для связи с датчиками, реле, камерами, умными устройствами. Хранение видео не предусмотрено.

В качестве баз данных, планирую использовать PostgreSQL как и в первоначальном решении.
Предлагаю создать общую базу для каждого микросервиса, так как не планируются большие объемы информации (только состояния устройств и их настройки, видео не храним в базе) и большое количество пользователей. 

Микросервисы в системе будут развернуты с использованием контейнеров в Kubernatis (а компании есть специалисты подходящего уровня). Это решение положительно скажется на отказоустойчивости.

Для настройки взаимодействия между сервисами организуем слой Service Mesh и будем использовать Istio, установив его в контейнеры Kubernatis. Для авторизации и аутентификации так же используем Istio. Пользователь не может самостоятельно регистрироваться в системе. Сотрудники компании его регистрируют и выдают логини и пароль. Логин и пароль общие для всех жителей дома, использующих приложение.

Для статических данных и как общую точку входа будем использовать Nginx. После него дальнейшую маршрутизацию возьмет на себя Istio 

Пользователь может подключать к системе любые умные устройства, работу с которыми поддерживает наша система. В случае если для добавленного устройства поддерживается алгоритм взаимодействия, мы перенаправляем команды управления на сервер внешнего производителя через наш брокер сообщений. Данный функционал имеет низший приоритет разработки в случае выпуска MVP приложения

Пояснение по диаграмме кода:
Диаграмму кода делаем на английском языке для более точной передачи нейминга
Связь микросервисов с именами классов:
1. Микросервис управления отоплением - class ControlTempApp
2. Микросервис управления светом - class ControlLigthApp
3. Микросервис управления воротами - class ControlGateApp
4. Микросервис управления видеонаблюдением - class ControlVideoApp
5. Микросервис подключения устройств умного дома - class ControlDevApp
6. Микросервис аутентификации и авторизации - class Home

Более подробно можно посмотреть:
 1. Диаграмму контейнеров файле task_2_container.puml
 2. Диаграмму 2-х компонентов файле task_2_components.puml task_2_components2.puml 
 3. Диаграмму кода task_2_code.puml
