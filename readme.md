# Краткое описание возможностей API Anywash
## Base url: https://app.anywash.ru/api/v2/
## Swagger: https://app.anywash.ru/swagger/


### Действия с автомобилями:
GET/cars/   -     Получить список автомобилей, закрепленных за Вашей компанией.

### Запрос:
#### cUrl:
```
curl --location --request GET 'https://app.anywash.ru/api/v2/cars/' \
--header 'Authorization: Bearer _{auth_token}_'
```

### Ответ:
```json
{
    "count": 986,                                           //Общее количество автомобилей, закрепленных за компанией
    "next": "https://app.anywash.ru/api/v2/cars/?page=2",   //Эндпоинт следующей страницы
    "previous": null,                                       //Эндпоинт предыдущей страницы
    "results": [
        {
            "plate_number": "string"                        //ГРЗ автомобиля
        }
    ]
}
```





1. Подтверждение заказов через API 
   1. нужно подписаться на получение данных о создании новых заказов с помощью обращения в техническую поддержку
   2. в данный хук будет приходить объект вида:
```json
{
   "this-json": "looks awesome..."
}
```
   На что хук может возвращать ответ:
```json
   {"status": "confirmed"}
```
   что будет означать подтверждение заказа
   
2. 
