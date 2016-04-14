# API WAPCLICK.COM

**версия 1.0**

## 1. Описание

WapClick – технология мобильных подписок. Абоненту, подключенному к сети интернет по мобильной связи, достаточно нажать одну кнопку для получения доступа к платному контенту и совершения подписки. Абонент получает качественный контент, списания с его счёта происходит ежедневно!


### 1.1 Особенности:

1. Абонент соглашается на подписку на странице оператора связи (landing page).
2. Оператор связи списывает средства со счёта абонента. Списание средств и предоставление доступа прекращается при выполнении одного из следующих условий – абонент отписался, закончился срок подписки (если установлен), нет достаточного количества средств на счету. При пополнении баланса тарификация возобновляется.
3. Тарификация подписавшегося абонента происходит ежедневно.

### 1.2 Поддерживаемые операторы

Wapclick.com работает со следующими операторами мобильной связи:

- beeline (Россия)
- mts (Россия)
- tele2 (Россия)
- mf (Россия)
- one (Австрия)
- tele.ring (Австрия)
- tre_h3g (Австрия)
- t-mobile (Австрия)
- azercell (Азербайджан)
- bakcell (Азербайджан)
- azerfon (Азербайджан)
- tmobile (Германия)
- vodafone (Германия)
- eplus (Германия)
- debitel (Германия)
- o2 (Германия)
- h3g (Италия)
- it_tim (Италия)
- vodafone (Италия)
- wind (Италия)
- neo (Казахстан)

## 2. Подключение

Для подключения партнёр предоставляет:

1. Тематику и источники трафика
2. URL для уведомлений об успешных транзакциях.
2. URL для возврата абонента на сайт (может быть не задан, если партнёр будет задавать адрес возврата для каждого абонента при инициации подписки).

Менеджер сообщает партнёру:

1. Идентификатор заведённой подписки (service_id).
2. Секретный ключ (secret_key).
3. Диапазон IP-адресов, по которым партнёр может привлекать трафик.

### 2.1 Стандартная схема

Партнёр размещает на страницах сайта код загрузки скрипта

```html
<script type="text/javascript" src="//wapclick.mobi/script/[идентификатор подписки].js" async></script>
```

 Пример

```html
<script type="text/javascript" src="//wapclick.mobi/script/13511.js" async></script>
```

Партнёр размещает на страницах сайта элемент для перехода к активации подписки. Это может быть кнопка, текстовая ссылка, баннер или любой другой элемент

```html
<div class="wk__subscribe_btn"></div>
```

Так же необходимо внизу страницы разместить элемент для отображения правил подписки

```html
<div class="wk__footer_frame"></div>
```

### 2.2 Упрощённая схема

Партнёр размещает ссылку на баннере или кнопке

```
http://wapclick.mobi/init/[идентификатор подписки].html
```

Пример

```html
<a href="http://wapclick.mobi/init/13511.html">Получить услугу</a>
```

Упрощённая схема может быть использована только после согласования с менеджером.

## 3. Дополнительные параметры подключения

Если требуется предоставлять абоненту несколько видов контента или требуется большая безопасность взаимодействия, то возможно задавать дополнительные параметры при использовании как стандартной, так и упрощённой схем

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| p_data | varchar(100) | Идентификатор подписки в системе партнёра | 077dd9d0-690d-11e5
| back_url | varchar(1000) | Адрес возврата абонента | https://site.com/content
| landing[operator] | varchar(1000) | Идентификатор лэндинга на стороне оператора | landing-1322

Параметр p_data будет в дальнейшем передан партнёру в уведомлении об успешной тарификации.

back_url будет использован при возврате абонента вместо адреса, прописанного по умолчанию.

Параметр landing используется для отображения абоненту кастомизированных лэндингов при оформлении подписки на сайте оператора и представляет собой идентификатор лэндинга. Данных параметров может быть несколько, если требуется указать идентификатор лэндинга для нескольких операторов.

Данные параметры не являются обязательными.

Пример

```html
<script type="text/javascript" src="http://wapclick.mobi/script/13511.js?p_data=077dd9d0-690d-11e5-b533-0d1018f8ac82&back_url=https%3A%2F%2Fsite.com%2Fcontent&landing[mf]=landing-1322&landing[mts]=landing-11" async></script>
```

## 4. Возврат абонента на сайт партнёра

После операции подписки абонент возвращается на back_url, переданный в дополнительных параметрах кода загрузки скрипта или на адрес возврата на сайт, который прописывается при заведении подписки. При возврате к адресу добавляются следующие параметры

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| code | int | Статус подписки | 0
| error | varchar(255) | Расшифровка статуса подписки | ok

Получение успешного статуса "подписка оформлена" в адресе возврата (back_url) на сайт партнёра не является гарантией дальнейшего успешного биллинга абонента, посколько он может аннулировать подписку как после первого биллинга, так и до него.

Уведомления о тарификациях передаются партнёру отдельным запросом на скрипт (см. п. 5), а в данном методе при получении code=0 партнёр должен начать предоставлять абоненту подписную услугу (отправить в закрытый раздел сайта, сообщить логин/пароль и т.д.).

Статусы подписок

| code | error | Описание
| --- | --- | ---
| 0 | ok | Подписка оформлена
| 1 | invalid service settings (please, contact us) | Невалидные настройки сервиса, требуется их корректировка на стороне wapclick.com, просьба обратиться к нам
| 2 | operator is not supported | Оператор не поддерживается
| 3 | service not found | Сервис не найден
| 4 | already subscribed | Абонент уже подписан на данный сервис
| 5 | no money | У абонента нет денег
| 6 | session broken | Сессия абонента прервана (таймаут, некорректный запрос)
| 7 | operator internal error | Ошибка на стороне оператора связи
| 8 | service internal error | Ошибка на стороне wapclick.com
| 9 | service blacklisted | Подписка находится в чёрном списке
| 10 | phone blacklisted | Номер абонента находится в чёрном списке
| 11 | missing required parameters | Не указаны обязательные параметры (проверьте изменения докуменации)
| 12 | cancel of action | Отмена действия

## 5. Уведомления о тарификациях

wapclick.com уведомляет партнера об успешных списаниях денежных средств со счета абонента.

Уведомление - GET-запрос с параметрами на скрипт партнёра.

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| action | varchar(30) | Тип уведомления - тарификация | charge_report
| service_id | integer | Идентификатор подписки | 1234
| status | int | Статус тарификации. 0 - успешная, -1 - неуспешная | 0
| phone | bigint | Номер абонента | 79031234567
| op | varchar(30) | Оператор | beeline
| c_amount | numeric(18,2) | Сумма тарификации в валюте абонента | 100.01
| amount | numeric(18,2) | Сумма тарификации в валюте расчётов с партнёром | 100.01
| c_pay | numeric(18,2) | Сумма выплаты партнёру в валюте абонента | 50.01
| pay | numeric(18,2) | Сумма выплаты партнёру в валюте расчётов с партнёром | 50.01
| c_curr | character(3) | Буквенный код валюты абонента (ISO 4217) | RUB
| curr | character(3) | Буквенный код валюты расчётов с партнёром (ISO 4217) | RUB
| tid | varchar(100) | Уникальный идентификатор транзакции | 08057700-690d-11e5-b610-321018f8ac82
| p_data | varchar(100) | Идентификатор подписки в системе партнёра | 077dd9d0-690d-11e5-b533-0d1018f8ac82
| sign | char(64) | Подпись запроса sha256(action+service\_id+phone+tid+p\_data+secret\_key). Если p_data не использовался, то он не участвует в формировании подписи | 26d5677126775acde55a7d5d379369f840b224586063f7df1f7380b412b7a0e2


Пример уведомления

```
https://site.com/subscriptions?action=charge_report&status=0&phone=79031234567&op=beeline&c_amount=100.01&amount=100.01&c_pay=50.01&pay=50.01&c_curr=RUB&curr=RUB&tid= 08057700-690d-11e5-b610-321018f8ac82&p_data=077dd9d0-690d-11e5-b533-0d1018f8ac82&service_id=1234&sign= 26d5677126775acde55a7d5d379369f840b224586063f7df1f7380b412b7a0e2
```


## 6. Уведомления об отписках

wapclick.com уведомляет партнера об отписке абонента.

Уведомление - GET-запрос с параметрами на скрипт партнёра.

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| action | varchar(30) | Тип уведомления - отписка | unsubscribe_report
| service_id | integer | Идентификатор подписки | 1234
| phone | bigint | Номер телефона абонента | 79031234567
| tid | varchar(100) | Идентификатор транзакции | 08057700-690d-11e5-b610-321018f8ac82
| p_data | varchar(100) | Идентификатор подписки в системе партнёра | 077dd9d0-690d-11e5-b533-0d1018f8ac82
| sign | char(64) | Подпись запроса sha256(action+service_id+phone+tid+p_data+secret_key). Если p_data не использовался, то он не участвует в формировании подписи | 68e656b251e67e8358bef8483ab0d51c6619f3e7a1a9f0e75838d41ff368f728

Пример уведомления

```
https://site.com/subscriptions?action=unsubscribe_report&service_id=1234&phone=79031234567&tid=077dd9d0-690d-11e5-b533-0d1018f8ac82&p_data=077dd9d0-690d-11e5-b533-0d1018f8ac82&sign=68e656b251e67e8358bef8483ab0d51c6619f3e7a1a9f0e75838d41ff368f728
```

## 7. Оценка конверсии

Система поддерживает подключение UTM-меток к элементам активации подписки. Для их использования нужно добавить метки в виде атрибута data. Метки будут доступны для выборки в личном кабинете.

Поддерживаемые метки:

* utm_source
* utm_medium
* utm_campaign
* utm_term
* utm_content

Пример

````html
<div class="wk__subscribe_btn" data-utm_source="1" data-utm_medium="2" data-utm_campaign="3" data-utm_term="4" data-utm_content="5"></div>
````

## 8. Дополнительные возможности API

Обращение к методам API происходит по адресу:

````
https://api.wapclick.mobi/{version}/{method}?params
````
Актуальная версия API – v1.

### Пример обращения к API

```
curl https://api.wapclick.mobi/v1/operators.getList?format=json
```

### Авторизация

Некоторые методы API требуют авторизации, для доступа к ним используется basic-аутентификация.

Доступ к такиим методам можно получить с помощью логинов и паролей, в рамках механизма Basic-аутентификации протокола HTTP.

Для Basic-аутентификации каждый запрос приложения к API должен содержать заголовок Authorization следующего вида:

```
Authorization: Basic <токен доступа>
```

Токен доступа здесь — это строка вида <логин>:<пароль> в кодировке base64.

В качестве **логина** используется ID заведённой подписки (service\_id), в качестве **пароля** – cекретный ключ подписки (secret\_key).


### Получение списка операторов

**Описание:** получение списка операторов, поддерживаемых в системе.

```
GET /v1/operators.getList
```

**Параметры:**

Параметр | Тип    | Описание
---------|------- |----------
format   | string | Формат ответа, допустимые варианты: json (по умолчанию), text, xml.<br />*Необязательный параметр*.

### Получение списка IP-адресов операторов

**Описание:** получение списка IP-адресов абонентов операторов, поддерживаемых в системе.

```
GET /v1/operators.getIps
```

**Параметры:**

| Параметр | Тип    | Описание
| --- | --- | ---
| operators | string  | Один или несколько кодов операторов, перечисленных через запятую.<br />Пример: mts,beeline
| countries | string  | Один или несколько кодов стран, перечисленных через запятую.<br />Пример: ru,kz
| type | string | Формат результирующего списка.<br />Допустимые параметры:<br />* **cidr** – свёрнутый в подсети список IP-адресов (по умолчанию)<br />* **list** – развёрнутый список адресов<br />*Необязательный параметр*.
| format | string | Формат ответа, допустимые варианты: json (по умолчанию), text, xml.<br />*Необязательный параметр*.


### Получение статистики

**Описание:** получение списка транзакций за период.

```
GET /v1/subscriptions.getStats
```

**Метод требует авторизации**


**Параметры:**

Параметр | Тип    | Описание
---------|------- |----------
date_from	| string  | Начальная дата поиска транзакций.<br />Формат: YYYY-MM-DD (пример: 2016-01-01)
date_to	| string  | Конечная дата поиска транзакций.<br />Формат: YYYY-MM-DD (пример: 2017-12-31)
limit		| integer	| Количество возвращаемых записей. По умолчанию 100.<br />*Необязательный параметр*.
offset		| integer | сдвиг, необходимый для получения конкретной выборки результатов. По умолчанию 0.<br />*Необязательный параметр*.
format   	| string 	| Формат ответа, допустимые варианты: json (по умолчанию), text, xml.<br />*Необязательный параметр*.

## 9. Контакты

С вопросами обращайтесь по почте [support@wapclick.com](mailto:support@wapclick.com)

