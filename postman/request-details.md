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

---

## 2. POST-запрос формы заявки на членство в клубе OortDepot

### Общая информация
- **Метод:** POST
- **URL:** https://oort.qahacking.ru/wp-admin/admin-ajax.php
- **Тип контента:** multipart/form-data

### Заголовки (Headers)
- `accept: application/json, text/javascript, */*; q=0.01`
- `content-type: multipart/form-data; boundary=----WebKitFormBoundary...`
- `origin: https://oort.qahacking.ru`
- `referer: https://oort.qahacking.ru/novosti-stancii/`
- `x-requested-with: XMLHttpRequest`

### Тело запроса (Body)

Поля в формате `multipart/form-data`:

| Ключ | Значение |
| :--- | :--- |
| `formData` | `first_name=&first_name=&phone_number=+79613143436&services=Просто хочу похвастаться в своей социальной сети&project_budget=17600&text_area=крутой&%5B%5D=Личный инструктор по левитации&%5B%5D=Защита от квантовых&%5B%5D=Запись вашего опыта в невесомости в формате голограммы&checkbox_field%5B%5D=Я прочитал(а) и принимаю правила клуба, осознавая риск случайной телепортации своей одежды в параллельную вселенную, превращения в квантовую пыль и/или получения счёта за разгерметизацию станции&fusion_privacy_store_ip_ua=false&fusion_privacy_expiration_interval=48&privacy_expiration_action=anonymize&fusion-form-nonce-745=40e647ece1&fusion-fields-hold-private-data=` |
| `action` | `fusion_form_submit_ajax` |
| `fusion_form_nonce` | `40e647ece1` |
| `form_id` | `745` |
| `post_id` | `698` |
| `field_labels` | `{"first_name":"","":"","phone_number":"","services":"","project_budget":"","text_area":"","checkbox_field":""}` |
| `field_types` | `{"notice_1":"notice","first_name":"text","":"checkbox","phone_number":"phone_number","services":"select","project_budget":"range","text_area":"textarea","checkbox_field":"checkbox","submit_2":"submit"}` |
| `hidden_field_names` | `[]` |

> **Важно:** Обрати внимание на поле `field_labels`. На скриншоте видно, что оно содержит пустые строки вместо нормальных названий полей, что указывает на потенциальную проблему на фронтенде.

### Примеры ответов
- **Успешный (заполненная форма):** `200 OK` с телом `{"status":"success","info":"form_submitted"}`
- **Негативный (пустое поле телефона):** `200 OK` с тем же телом `{"status":"success","info":"form_submitted"}` — **это баг!** Сервер должен был вернуть ошибку валидации.

---

## Проведённые тесты (сводная таблица по обеим формам)

| Сценарий | Что меняли | Код ответа | Тело ответа | Результат |
| :--- | :--- | :--- | :--- | :--- |
| **Поиск груза (позитивный)** | `Поиск=12` | 200 OK | `{"status":"success","info":"form_submitted"}` | ✅ PASS |
| **Поиск груза (пустое тело)** | Удалили все поля | 400 Bad Request | (пусто) | ✅ PASS (ожидаемая ошибка) |
| **Заявка (позитивный)** | Все поля заполнены корректно | 200 OK | `{"status":"success","info":"form_submitted"}` | ✅ PASS |
| **Заявка (пустое тело)** | Удалили все поля из тела запроса (none) | 400 Bad Request | (пусто) | ✅ PASS (ожидаемая ошибка) |
| **Заявка (пустой телефон)** | Удалили значение `phone_number` | 200 OK | `{"status":"success"...}` | ❌ **FAIL** (ожидалась ошибка валидации) |
| **Заявка (текст в телефоне)** | Вставили "Text" в `phone_number` | 200 OK | `{"status":"success"...}` | ❌ **FAIL** (ожидалась валидация номера) |

---

## Найденные баги

На основе проведённых тестов был выявлен следующий дефект:

### Баг: Отсутствие валидации обязательных полей в форме заявки
- **Описание:** Сервер принимает заявку на членство в клубе даже при незаполненных обязательных полях (имя, телефон) и при вводе невалидных данных (текст вместо номера телефона). В ответе всегда приходит `200 OK` и `{"status":"success"}`.
- **Серьёзность:** Критическая (Critical)
- **Приоритет:** Высокий (High)
- **Статус:** `Open`

> Подробное описание бага с шагами воспроизведения и скриншотами вынесено в отдельный файл [`bugs-found.md`](bugs-found.md).
