Задание 4. Создание и документирование API

Создаем документацию для следующих конечных точек для приложения системы управления отоплением:
1. Получить температуру во всех комнатах дома (Получаем информацию со всех датчиков привязанных к дому)  - /get_all_temp/{home_id}
2. Получить температуру в указанной комнате - /get_temp/{home_id}/{sensor_id}
3. Установить температуру в определенной комнате - /put_temp/{home_id}/{sensor_id}
4. Включить систему отопления - /temp_is_on/{home_id}

Описание документации в файле swagger.yaml