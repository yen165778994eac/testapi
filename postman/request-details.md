# POST-запрос формы поиска груза

## Общая информация
- **Метод:** POST
- **URL:** https://oort.qahacking.ru/wp-admin/admin-ajax.php
- **Тип контента:** multipart/form-data

## Заголовки (Headers)
- accept: application/json, text/javascript, */*; q=0.01
- accept-language: ru,en;q=0.9
- content-type: multipart/form-data; boundary=----WebKitFormBoundary...
- origin: https://oort.qahacking.ru
- referer: https://oort.qahacking.ru/novosti-stancii/
- user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) ...
- x-requested-with: XMLHttpRequest

## Тело запроса (Body)
Поля в формате multipart/form-data:

| Ключ | Значение |
| :--- | :--- |
| formData | Поиск=12&fusion_privacy_store_ip_ua=false&fusion_privacy_expiration_interval=48&privacy_expiration_action=anonymize&fusion-form-nonce-710=abf1bb2bdc&fusion-fields-hold-private-data= |
| action | fusion_form_submit_ajax |
| fusion_form_nonce | abf1bb2bdc |
| form_id | 710 |
| post_id | 698 |
| field_labels | {"Поиск":""} |
| field_types | {"fusion_builder_container_3-1":"fusion_builder_container","Поиск":"text","submit_1":"submit"} |
| hidden_field_names | [] |

## Примеры ответов
- **Успешный:** `200 OK` с телом `{"status":"success","info":"form_submitted"}`
- **Ошибка:** `400 Bad Request` при пустом теле запроса
