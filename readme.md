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
Строка состояния

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
Получить список всех транзакций Вашей компании.
### Запрос:
#### cUrl:
```
curl --location --request GET 'https://app.anywash.ru/api/v2/orders/' \
--header 'Authorization: Bearer {auth_token}'
```
### Ответ:
```json
{
    "count": 66,
    "next": null,
    "previous": null,
    "results": [
        {
            "order_number": "{order_number}",       //ID заказа
            "client_price": 0.0,                    //Общая стоимость заказа
            "services": [                           //Массив услуг в рамках заказа
                {
                    "quantity": 1,                  //Количество по данной услуге
                    "service": {
                        "id": 2,                    //ID услуги
                        "name": "Кузов коврики",    //Наименование услуги
                        "category": "washing"       //Категория/тип услуги
                    },
                    "client_price_for_one": 0.0,    //Стоимость за единицу услуги
                    "wheel_size": null              //Радиус колес(для категории tires)
                }
            ],
            "plate_number": "C696TO899",            //ГРЗ
            "form_factor": 2,                       //Тип кузова а/м
            "car_class": 2,                         //Класс а/м
            "poi": {                                
                "id": 13,                           //ID точки
                "point": {
                    "lat": 55.81902894806981,       //Широта
                    "lon": 37.513913197629236       //Долгота
                         },
                "company": {
                    "id": 110,                      //ID компании-партнера
                    "name": "testwash"              //Наименование компании-партнера
                },
                "phone": "88006004159"              //Контактный телефон
            },
            "created_at": "2022-02-03T11:39:11.587062"  //Дата и время подтверждения транзакции
        }
}
```
### ***POST/orders/***
// Для компаний-партнеров. Отправить заказ на подтверждение.
### Запрос:
#### cUrl:
```
curl --location --request POST 'https://app.anywash.ru/api/v2/orders/' \
--header 'Authorization: Bearer {auth_token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "plate_number": "{plate_number}",     //ГРЗ
  "poi": 13,                            //ID ТОУ
  "services": [                         //Набор услуг в рамках заказа
    {
      "quantity": 2,                    //Количество выполненных работ по данной услуге
      "service": 4,                     //ID услуги
      "partner_price_for_one": 250,     //Стоимость услуги за единицу
      "wheel_size": "R15C"              //Радиус колеса для услуг шиномонтажа
    }
  ]
}'
```
### Ответ:
```json
{
    "status": "confirmed",
    "order_number": "{order_number}"
}
```

### ***PATCH/orders/{id}/***
Изменение статуса заказа.
### Запрос:
#### cUrl:
```
curl --location --request PATCH 'https://app.anywash.ru/api/v2/orders/ac5ed186-0ddd-4cc7-9a3e-1eef3caf7d14/' \
--header 'Authorization: Bearer {auth_token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "status": "confirmed"     //статус: init, confirmed, rejected
}'
```
### Ответ:
Строка состояния.

## Действия с ТОУ:
### ***GET/pois/***
Получить детализированный список ТОУ Anywash с ценами клиента.
### Запрос:
#### cUrl:
```
curl --location --request GET 'https://app.anywash.ru/api/v2/pois/' \
--header 'Authorization: Bearer {auth_token}' \
--data-raw ''
```
### Ответ
```json
{
    "count": 479,                                           //Общее количество ТОУ
    "next": "https://app.anywash.ru/api/v2/pois/?page=2",   //Ендпоинт следующей страницы списка
    "previous": null,
    "results": [                                            //Список ТОУ
        {
            "id": 13,                                       //ID точки                   
            "name": "testwash",                             //Наименование точки
            "info": "2 поста, высота 2,2 м. - 2,25 м.",     //Информация о точке
            "point": {
                "lat": 55.695591,                           //широта
                "lon": 37.728281                            //долгота
            },
            "phone": "88006004159",                         //Контактный телефон (телефон тех. поддержки)
            "opening_hours": "08:00:00",                    //Время открытия
            "closing_hours": "22:00:00",                    //Время закрытия
            "post_height": 2.25,                            //Высота поста
            "advance_registration": "possible",             //Предварительная запись: required, possible, no (Обязательна, возможна, нет)
            "services": [                                   //Набор услуг, доступных для оказание на точке
                {
                    "id": 1,                                //ID услуги
                    "name": "Техническая мойка",            //Наименование услуги
                    "category": "washing",                  //Категория услуги
                    "prices": [
                        {
                            "car_class": 1,                 //Класс автомобиля
                            "form_factor": 1,               //Тип кузова автомобиля(1-легковой, 2-грузовой)
                            "wheel_size": null,             //Размер колес(для услуг шиномонтажа)
                            "price": 150.0                  //Цена для клиента
                        },
                        {
                            "car_class": 2,
                            "form_factor": 1,
                            "wheel_size": null,
                            "price": 160.0
                        },
                        {
                            "car_class": 3,
                            "form_factor": 1,
                            "wheel_size": null,
                            "price": 170.0
                        },
                        {
                            "car_class": 4,
                            "form_factor": 1,
                            "wheel_size": null,
                            "price": 180.0
                        }
                    ]
                },
                {
                    "id": 2,
                    "name": "Кузов коврики",
                    "category": "washing",
                    "prices": [
                        {
                            "car_class": 1,
                            "form_factor": 1,
                            "wheel_size": null,
                            "price": 200.0
                        },
                        {
                            "car_class": 2,
                            "form_factor": 1,
                            "wheel_size": null,
                            "price": 250.0
                        },
                        {
                            "car_class": 3,
                            "form_factor": 1,
                            "wheel_size": null,
                            "price": 300.0
                        },
                        {
                            "car_class": 4,
                            "form_factor": 1,
                            "wheel_size": null,
                            "price": 400.0
                        }
                    ]
                },
                {
                    "id": 3,
                    "name": "Комплекс",
                    "category": "washing",
                    "prices": [
                        {
                            "car_class": 1,
                            "form_factor": 1,
                            "wheel_size": null,
                            "price": 500.0
                        },
                        {
                            "car_class": 2,
                            "form_factor": 1,
                            "wheel_size": null,
                            "price": 600.0
                        },
                        {
                            "car_class": 3,
                            "form_factor": 1,
                            "wheel_size": null,
                            "price": 700.0
                        },
                        {
                            "car_class": 4,
                            "form_factor": 1,
                            "wheel_size": null,
                            "price": 800.0
                        }
                    ]
                }
            ]
        },
}
```
### ***GET/pois/{id}/***
Получить информацию по ID конкретной точки.
### Запрос:
#### cUrl:
```
curl --location --request GET 'https://app.anywash.ru/api/v2/pois/13/' \
--header 'Authorization: Bearer {auth_token}' \
--data-raw ''
```
### Ответ:
```json
{
    "id": 13,                               //ID точки
    "name": "Test place",                   //Наименование точки
    "info": "some info...",                 //Информация по точке            
    "phone": "88006004159",                 //Номер телефона теъ. поддержки
    "services": [                           //Список услуг, доступных на ТОУ
        {
            "id": 1,                        //ID услуги
            "name": "Техническая мойка",    //Наименование услуги
            "category": "washing",          //Категория услуги
            "prices": [                     //Список цен для данной услуги
                {
                    "car_class": 1,         //Класс а/м
                    "form_factor": 1,       //Тип кузова а/м(1 - легковой, 2-грузовой)
                    "wheel_size": null,     //Радиус колеса(для транзакций шиномонтажа)
                    "price": 150.0          //Стоимость для клиента
                },
                {
                    "car_class": 2,
                    "form_factor": 1,
                    "wheel_size": null,
                    "price": 160.0
                },
                {
                    "car_class": 3,
                    "form_factor": 1,
                    "wheel_size": null,
                    "price": 170.0
                },
                {
                    "car_class": 4,
                    "form_factor": 1,
                    "wheel_size": null,
                    "price": 180.0
                }
            ]
        }
}
```

### ***POST/pois/***
Создать ТОУ в системе Anywash, в ответ получить ее ID.
### Запрос:
#### cUrl:
```
curl --location --request POST 'https://app.anywash.ru/api/v2/pois/' \
--header 'Authorization: Bearer {auth_token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "Москва, ул. Даниловский Вал, д. 13А",        //Наименование точки
  "info": "2 поста",                                    //Поле информации о точке
  "phone": "88006004159",                               //Телефон поддержки Anywash
  "opening_hours": "08:00",                             //Часы открытия
  "closing_hours": "22:00",                             //Часы закрытия
  "post_height": 2.5,                                   //Высота поста
  "advance_registration": "required",                   //Параметр наобходимости предварительной записи (required, possible, no)
  "services": [                                         //Список услуг, доступных на ТОУ
    {
      "id" : 1,                                         //ID услуги
      "name": "Техническая мойка",                      //Наименование услуги
      "category": "washing"                             //Категория услуги
    }
  ]
}'
```
### Ответ:




### ***GET/tickets/***
Получить список заявок на тех. поддержку.
### Запрос:
#### cUrl:
```
curl --location --request GET 'https://app.anywash.ru/api/v2/tickets/' \
--header 'Authorization: Bearer {auth_token}'
```
### Ответ:
```
{
    "count": 2,                                                     //Общее количество заявок
    "next": null,
    "previous": null,
    "results": [
        {
            "ticket_num": "c91efdd1-c597-4951-a941-313861bd2a2e",   //ID заявки
            "title": "Запрос водителя М ТАКСИ",                     //Название заявки
            "description": "Отказывают услугу на мойке",            //Описание проблемы
            "address": "Москва, даниловский вал д 13 а",            //Адрес
            "poi": 133                                              //ID точки
        },
```

### ***POST/TICKETS/***
Создать заявку на тех. поддержку.
### Запрос:
#### cUrl:
```
curl --location --request POST 'https://app.anywash.ru/api/v2/tickets/' \
--header 'Authorization: Bearer {auth_token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "title": "{title}",
  "description": "{description}",
  "address": "{adress}",
  "poi": {poi_id}
}'
```
### Ответ:
