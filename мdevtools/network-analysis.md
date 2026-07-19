# Анализ сетевых запросов через DevTools

## Цель
Показать, как я использую DevTools для перехвата и анализа HTTP-запросов при тестировании веб-приложений.

## Что было сделано
1. Открыта страница `https://oort.qahacking.ru/novosti-stancii/`.
2. В DevTools (F12) → вкладка **Network** перехвачен POST-запрос к `admin-ajax.php`.
3. Проанализированы заголовки запроса, тело, куки и ответ сервера.
4. Проведены тесты с изменением параметров (позитивные и негативные сценарии).

## Результаты анализа

| Что проверяли | Результат |
| :--- | :--- |
| URL запроса | `https://oort.qahacking.ru/wp-admin/admin-ajax.php` |
| Метод | `POST` |
| Тип контента | `multipart/form-data` |
| Ключевые поля | `Поиск`, `action`, `fusion_form_nonce`, `form_id` |
| Успешный ответ | `200 OK` с телом `{"status":"success","info":"form_submitted"}` |
| Ошибка (пустое тело) | `400 Bad Request` |

## Скриншоты
- [Успешный ответ](screenshots/postman-success.png)
- [Ошибка 400](screenshots/postman-error-400.png)

## Инструменты
- **Google Chrome DevTools** (вкладки Network, Console, Elements)
- **Postman** для повторной отправки запросов
