# Краткое описание возможностей API Anywash
## Swagger: https://app.anywash.ru/swagger/
## Base url: https://app.anywash.ru/api/v2/


## Действия с автомобилями:
### ***GET/cars/***
Получить список автомобилей, закрепленных за Вашей компанией.

### Запрос:
#### cUrl:
```
curl --location --request GET 'https://app.anywash.ru/api/v2/cars/' \
--header 'Authorization: Bearer {auth_token}'
```

### Ответ:
#### Json:
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

### ***GET/cars/{id}/***
Получить список установленных доступных услуг для конкретного автомобиля Вашей компании.

### Запрос:
{id} - ГРЗ автомобиля
#### cUrl:
```
curl --location --request GET 'https://app.anywash.ru/api/v2/cars/{plate_number}/' \
--header 'Authorization: Bearer {auth_token}'
```

### Ответ:
#### Json:
```json
{
    "plate_number": "string",               //ГРЗ автомобиля
    "allowed_services": [                   //Массив услуг, доступных автомобилю
        {
            "id": 1,                        //ID услуги
            "name": "Техническая мойка",    //Наименование услуги
            "category": "washing"           //Категория услуги ("washing" - автомойка; "tires" - шиномонтаж)
        },
        {
            "id": 2,
            "name": "Кузов коврики",
            "category": "washing"
        },
        {
            "id": 3,
            "name": "Комплекс",
            "category": "washing"
        }
    ]
}
```

### ***POST/cars/***
Добавить новый автомобиль Вашей компании в список а\м.

### Запрос:
#### cUrl:
```
curl --location --request POST 'https://app.anywash.ru/api/v2/cars/' \
--header 'Authorization: Bearer {auth_token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "plate_number": "{plate_number}"    ///ГРЗ автомобиля
}'
```
### Ответ:
Строка состояния(http статус)



## Действия с компанией:
### ***GET/companies/{id}/***
Получить список доступных услуг для всех автомобилей Вашей компании.

### Запрос:
{id} - ID Вашей компании в системе ©Anywash.
#### cUrl:
```
curl --location --request GET 'https://app.anywash.ru/api/v2/companies/{company_id}/' \
--header 'Authorization: Bearer {auth_token}'
```
### Ответ:
#### Json:
```json
{
    "id": 551,                          //ID компании
    "name": "ООО \"Test Company\"",     //Наименование компании
    "allowed_services": [               //Массив доступных услуг
        {
            "id": 2,                    //ID услуги
            "name": "Кузов коврики",    //Наименование услуги
                "category": "washing"   //Категория услуги ("washing" - автомойка; "tires" - шиномонтаж)
            }
    ]
}
```

## Действия с заказами\транзакциями:
### ***GET/orders/***
### Запрос:
#### cUrl:
### Ответ:
```json
{
    "count": 66,
    "next": null,
    "previous": null,
    "results": [
        {
            "order_number": "d4c185fa-6d5a-4216-97ea-3ab2f01e0d7d",
            "client_price": 0.0,
            "services": [
                {
                    "quantity": 1,
                    "service": {
                        "id": 2,
                        "name": "Кузов коврики",
                        "category": "washing"
                    },
                    "client_price_for_one": 0.0,
                    "wheel_size": null
                }
            ],
            "plate_number": "C696TO899",
            "form_factor": 2,
            "car_class": 2,
            "poi": {
                "id": 13,
                "point": null,
                "company": {
                    "id": 110,
                    "name": "testwash"
                },
                "phone": "1"
            },
            "created_at": "2022-02-03T11:39:11.587062"
        }
}
```




### ***POST/cars/***
### Запрос:
#### cUrl:
### Ответ:
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
