## Адрес для отправки запросов

**Базовый URL для тестовой среды:**
`https://stage.c2u.io:6901`

### Заголовки:

- `Content-Type`: `application/json`
- Авторизация: `X509 Сертификат`

---

## Создание платежа

**Эндпоинт для создания платежа:**
`POST /pos-terminal/api/v1/remote-invoices`

### Пример запроса:

```json
{
	"amount": 1000.00,
	"callback": "web-hook-url",
	"idempotencyKey": "3fa85f64-5717-4562-b3fc-2c963f66afa6",			   
	"phoneNumber": "996700005311"
}
```

### Параметры:

| Название         | Тип    | Описание                                      |
| ---------------- | ------ | --------------------------------------------- |
| `amount`         | Float  | Сумма платежа                                 |
| `callback`       | String | Адрес для отправки результата платежа         |
| `idempotencyKey` | Guid   | Случайно сгенерированный ключ идемпотентности |
| `phoneNumber`    | String | Номер телефона клиента                        |

**Примечание**:  
`callback` должен иметь структуру типа `https://site.com/webhook/some-random-string`, где `some-random-string` — это случайная строка, которая будет ассоциироваться с создаваемым платежом.

### Варианты ответов:

| Название                | HTTP Код |
| ----------------------- | -------- |
| `OK`                    | 200      |
| `BAD_REQUEST`           | 400      |
| `UNAUTHORIZED`          | 401      |
| `CONFLICT`              | 409      |
| `INTERNAL_SERVER_ERROR` | 500      |

---

## Уведомление о результате платежа

**Описание**:  
Контрагент должен реализовать эндпоинт для получения уведомлений о статусе платежа.

**Метод**:  
`POST web-hook-url`  
Авторизация не требуется.

### Пример запроса:

```json
{
	"method": "payment-status-update",	
	"data": 
        {
            "PaymentId" : "uuid",
            "InvoiceId" : "uuid",
            "IdempotencyKey" : "uuid",
            "Status" : [
                "InProcess",
                "Success",
                "InsufficientBalance", 
                "TechnicalError",
                "Timeout"
            ]
        }
}
```

### Описание объекта PaymentResultCode:

| Название              | Описание                          |
| --------------------- | --------------------------------- |
| `InProcess`           | Платеж в процессе                 |
| `Success`             | Платеж успешен                    |
| `InsufficientBalance` | Недостаточно средств              |
| `Timeout`             | Превышено время обработки запроса |
| `TechnicalError`      | Техническая ошибка                |

---

## Возврат платежа

**Эндпоинт для возврата платежа**:  
`POST /pos-terminal/api/v1/payments/{id}/refund`

### Пример запроса:

```json
{
  "idempotencyKey": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "amount": 1000000,
  "callback": "string"
}
```

### Параметры:

| Название         | Тип    | Описание                                      |
| ---------------- | ------ | --------------------------------------------- |
| `idempotencyKey` | Guid   | Случайно сгенерированный ключ идемпотентности |
| `amount`         | Float  | Сумма возврата                                |
| `callback`       | String | Адрес для отправки результата возврата        |

---

## Полезные ссылки и информация:

- [Документация Cash2U (Тестовая среда)](https://stage.c2u.io:6901)




изучить
[Использование wp\_remote\_post для взаимодействия с API](https://genius.courses/wp_remote_post/)

- [x] **Создание платежа**:
    - Реализовать функцию для отправки POST-запроса на создание платежа, используя [[X509 сертификат]] для авторизации.
- [x] **Обработка результата платежа через вебхук**:
    - Создать [[Эндпоинты]] для получения уведомлений от Cash2U о статусе платежа и реализовать логику обработки этих уведомлений.
- [ ] **Реализация возврата платежа**:
    - Создать функцию для отправки POST-запроса на возврат платежа, также используя X509 сертификат.



[Основы SSL-сертификата WordPress - SSL Dragon](https://www.ssldragon.com/ru/blog/wordpress-ssl/)
[Free SSL Certificate WordPress Plugin, HTTPS Redirect, Renewal Reminder – Автоматическая установка бесплатного SSL — Плагин для WordPress | WordPress.org Русский](https://ru.wordpress.org/plugins/auto-install-free-ssl/)



Варианты ответов

1. **`OK` (200)**  
   Это стандартный код, означающий, что запрос был успешно обработан. В контексте платежей это означает, что платеж был успешно создан или выполнен без ошибок.

2. **`BAD_REQUEST` (400)**  
   Этот код означает, что сервер не может обработать запрос из-за неверных или отсутствующих данных. Например, если переданы некорректные параметры, такие как неправильный формат суммы или отсутствует обязательное поле, сервер вернёт этот код. Необходимо проверить корректность запроса.

3. **`UNAUTHORIZED` (401)**  
   Этот код указывает на отсутствие авторизации или неверную авторизацию. В случае с Cash2U это может означать, что X509 сертификат для авторизации либо отсутствует, либо неверен. В этом случае запрос не будет обработан.

4. **`CONFLICT` (409)**  
   Этот код означает, что возник конфликт с текущим состоянием ресурса. В контексте платежей это может означать, что запрос с таким же идемпотентным ключом уже был обработан ранее. В целях идемпотентности, повторный запрос с таким же ключом не создаст новый платеж, чтобы избежать дублирования.

5. **`INTERNAL_SERVER_ERROR` (500)**  
   Этот код указывает на внутреннюю ошибку сервера. Это означает, что на стороне сервера произошла ошибка при обработке запроса. Обычно это не связано с некорректными данными в запросе, а указывает на проблему в серверной части.


вопросы
корректен ли сертификат 
какой формат должен быть сертификат pfx или pem
тестовый сервер работает ли могу ли выполнять запросы 





### Обновлённая версия функции генерации UUID v4
```php

function generateUuidV4() {
    $data = random_bytes(16);
    // Устанавливаем версию UUID на 4 (random)
    $data[6] = chr(ord($data[6]) & 0x0f | 0x40);
    // Устанавливаем вариант UUID на 8-10
    $data[8] = chr(ord($data[8]) & 0x3f | 0x80);
    // Форматируем результат в виде UUID
    return sprintf('%08x-%04x-%04x-%04x-%04x%08x',
        // 32-битное слово
        ord($data[0]) << 24 | ord($data[1]) << 16 | ord($data[2]) << 8 | ord($data[3]),
        // 16-битное слово
        ord($data[4]) << 8 | ord($data[5]),
        // 16-битное слово (включая версию)
        ord($data[6]) << 8 | ord($data[7]),
        // 16-битное слово (включая вариант)
        ord($data[8]) << 8 | ord($data[9]),
        // 16-битное слово
        ord($data[10]) << 8 | ord($data[11]),
        // оставшиеся 48 бит
        ord($data[12]) << 24 | ord($data[13]) << 16 | ord($data[14]) << 8 | ord($data[15])
    );
}

```




```php

function create_remote_invoice($amount, $callback_url, $idempotency_key, $phone_number) {
    // URL для создания платежа
    $api_url = 'https://stage.c2u.io:6901/pos-terminal/api/v1/remote-invoices';

    // Путь к сертификату и ключу
    $cert_path = get_stylesheet_directory() . '/certificates/cert.pem';
    $key_path = get_stylesheet_directory() . '/certificates/key.pem';
    
    if (!file_exists($cert_path) || !file_exists($key_path)) {
        error_log('Ошибка: Сертификат или ключ не найден.');
        return false; // Завершаем выполнение функции при отсутствии сертификатов
    }

    // Данные для отправки
    $body = array(
        'amount' => $amount,
        'callback' => $callback_url,
        'idempotencyKey' => $idempotency_key,
        'phoneNumber' => $phone_number
    );
    
    // Настройки cURL
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $api_url);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($body));
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HTTPHEADER, array(
        'Content-Type: application/json',
    ));
    curl_setopt($ch, CURLOPT_SSLCERT, $cert_path);
    curl_setopt($ch, CURLOPT_SSLKEY, $key_path);
    curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 2);
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, true);

    // Выполняем запрос
    $response = curl_exec($ch);
    $http_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);

    if ($response === false) {
        $error_msg = curl_error($ch);
        curl_close($ch);
        error_log('cURL error: ' . $error_msg); // Логируем ошибку
        return false;
    }

    // Закрываем cURL-сессию
    curl_close($ch);

    // Возвращаем результат
    return array(
        'http_code' => $http_code,
        'response' => json_decode($response, true)
    );
}

    


add_action('rest_api_init', function () {
    // Регистрируем новый REST эндпоинт для получения уведомлений
    register_rest_route('cash2u/v1', '/payment-status', array(
        'methods' => 'POST',
        'callback' => 'handle_cash2u_payment_status',
        'permission_callback' => '__return_true', // Отключаем проверку прав
    ));
});

function handle_cash2u_payment_status(WP_REST_Request $request) {
    // Получаем JSON тело запроса
    $body = json_decode($request->get_body(), true);
    
    // Проверяем, что метод и данные переданы
    if (!isset($body['method']) || $body['method'] !== 'payment-status-update') {
        return new WP_REST_Response(['error' => 'Invalid method'], 400);
    }

    // Проверяем структуру объекта data
    if (!isset($body['data']['PaymentId']) || !isset($body['data']['InvoiceId']) || !isset($body['data']['IdempotencyKey']) || !isset($body['data']['Status'])) {
        return new WP_REST_Response(['error' => 'Invalid data structure'], 400);
    }

    // Получаем данные платежа
    $payment_id = sanitize_text_field($body['data']['PaymentId']);
    $invoice_id = sanitize_text_field($body['data']['InvoiceId']);
    $idempotency_key = sanitize_text_field($body['data']['IdempotencyKey']);
    $status = sanitize_text_field($body['data']['Status']);
    

    // В зависимости от статуса, можно реализовать логику обработки
    switch ($status) {
        case 'InProcess':
            // Логика для платежей в процессе
            update_payment_status($invoice_id, 'in_process');
            break;
        case 'Success':
            // Логика для успешных платежей
            update_payment_status($invoice_id, 'success');
            break;
        case 'InsufficientBalance':
            // Логика для недостатка средств
            update_payment_status($invoice_id, 'insufficient_balance');
            break;
        case 'Timeout':
            // Логика для таймаута
            update_payment_status($invoice_id, 'timeout');
            break;
        case 'TechnicalError':
            // Логика для технической ошибки
            update_payment_status($invoice_id, 'technical_error');
            break;
        default:
            return new WP_REST_Response(['error' => 'Unknown status'], 400);
    }

    // Возвращаем успешный ответ
    return new WP_REST_Response(['message' => 'Payment status processed successfully'], 200);
}

function update_payment_status($invoice_id, $status) {
    // Здесь можно обновить статус в базе данных или выполнить другие действия
    // Например, сохранить статус в post_meta для связанного заказа
    $order_id = get_order_id_by_invoice($invoice_id); // Поиск заказа по InvoiceId
    
    if ($order_id) {
        update_post_meta($order_id, '_payment_status', $status);
    } else {
        error_log('Order not found for InvoiceId: ' . $invoice_id);
    }
}

function get_order_id_by_invoice($invoice_id) {
    // Логика для поиска заказа по InvoiceId
    // Например, если InvoiceId сохранён как мета-данные
    global $wpdb;
    $order_id = $wpdb->get_var($wpdb->prepare(
        "SELECT post_id FROM $wpdb->postmeta WHERE meta_key = '_invoice_id' AND meta_value = %s",
        $invoice_id
    ));
    return $order_id;
}

```




curl -X POST https://c2u.kg/wp-json/cash2u/v1/webhook/7956a70136522b751cc5 -H "Content-Type: application/json" -d '{   "method": "payment-status-update",
 "data": {
 "PaymentId": "b8a16d59-1c54-4c5e-82ba-1a702e61ec4c",
  "InvoiceId": "b0b13dbd-1b73-46d1-9409-2a935a88c231",
"IdempotencyKey": "c1e9b3f0-35a8-4b67-9410-911f1ecae579",
   "Status": "Success"
  }
}'

