# План автоматизации тестирования сервиса

## Задача:
Автоматизировать сценарии тестирования покупки тура "Путешествие дня"

## Артефакты:
SUT aqa-shop.jar

## Предусловия:
Тестируемое приложение запускается по инструкции, указанной в файле README.md

## Карты, представленные для тестирования (файл data.json):

* 4444 4444 4444 4441, status APPROVED
* 4444 4444 4444 4442, status DECLINED

## Тестируемые Базы данных:

* MySql
* PostgreSQL

## Перечень автоматизируемых сценариев:

### Тестирование UI
1. Открыть в браузере главную страницы приложения, нажать кнопку "Купить"
**Ожидаемый результат:** появляется форма для ввода данных карты с заголовком "Оплата по карте"
2. Открыть в браузере главную страницы приложения, нажать кнопку "Купить в кредит"
**Ожидаемый результат:** появляется форма для ввода данных карты с заголовком "Кредит по данным карты"

### Тестирование валидации полей формы ввода данных карты

#### Валидными данными при заполнении полей ввода следует считать:

1. Номер карты: цифровые символы, 16 шт.,в формате **** **** **** ****
1. Месяц: 01-12, но не ранее текущего месяца в случае, если указан текущий год
1. Год: последние две цифры порядкового номера года, не ранее текущего года
1. Владелец: буквенные символы латинского алфавита
1. CVC: цифровые символы, 3 шт.

### Позитивные сценарии:

1. Оплата по дебетовой карте APPROVED, ввод валидных значений:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" латиницей;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:** 
* Кнопка "Продолжить" меняется на кнопку "Отправляем запрос в Банк...".
* Появляется всплывающее окно с сообщением "Операция одобрена Банком".
* В БД в payment_entity появилась запись со статусом APPROVED

2. Оплата по дебетовой карте DECLINED, ввод валидных значений:

* В поле "Номер карты" ввести "4444 4444 4444 4442";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:** 
* Кнопка "Продолжить" меняется на кнопку "Отправляем запрос в Банк...".
* Появляется всплывающее окно с сообщением "Ошибка! Банк отказал в проведении операции".
* В БД в payment_entity появилась запись со статусом DECLINED

3. Оплата в кредит по карте APPROVED, ввод валидных значений:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:** 
* Кнопка "Продолжить" меняется на кнопку "Отправляем запрос в Банк...".
* Появляется всплывающее окно с сообщением "Операция одобрена Банком".
* В БД в credit_request_entity появилась запись со статусом APPROVED

4. Оплата в кредит по карте DECLINED, ввод валидных значений:

* В поле "Номер карты" ввести "4444 4444 4444 4442";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Кнопка "Продолжить" меняется на кнопку "Отправляем запрос в Банк..." с крутящимся индикатором загрузки;
* Появляется всплывающее окно с сообщением "Ошибка! Банк отказал в проведении операции".
* В БД в credit_request_entity появилась запись со статусом DECLINED

### Негативные сценарии:

1. Пустое поле номера карты:

* Поле "Номер карты" оставить пустым;
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Номер карты" появляется сообщение красными буквами "Поле обязательно для заполнения".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

2. "Нулевая" карта:
* В поле "Номер карты" ввести "0000 0000 0000 0000";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Кнопка "Продолжить" меняется на кнопку "Отправляем запрос в Банк..." с крутящимся индикатором загрузки;
* Появляется всплывающее окно с сообщением "Ошибка! Банк отказал в проведении операции".
* В БД в payment_entity новая запись не появилась

3. Несуществующий номер карты:
* В поле "Номер карты" ввести "0000 0000 0000 0002";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Кнопка "Продолжить" меняется на кнопку "Отправляем запрос в Банк..." с крутящимся индикатором загрузки;
* Появляется всплывающее окно с сообщением "Ошибка! Банк отказал в проведении операции".
* В БД в payment_entity новая запись не появилась

4. Номер карты, превышающий допустимые значения:

* В поле "Номер карты" ввести "1111 2222 3333 4444 5555 6666";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Номер карты" появляется сообщение красными буквами "Неверный формат".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

5. Поле номера заполнено символами:

* В поле "Номер карты" ввести "@#$% %$#@ ^%$$ @#%^";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Номер карты" появляется сообщение красными буквами "Неверный формат".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

6. Поле номера заполнено текстовыми символами:

* В поле "Номер карты" ввести "fnst ktif mmsi mrew";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Номер карты" появляется сообщение красными буквами "Неверный формат".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

7. Короткий номер карты:

* В поле "Номер карты" ввести "1111 2222 3333";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Номер карты" появляется сообщение красными буквами "Неверный формат".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

8. В поле "Месяц" введено значение предыдущего месяца:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "09";
* В поле "Год" ввести "22";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Месяц" появляется сообщение красными буквами "Истек срок действия карты".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

9. В поле "Месяц" введено "00":

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "00";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Месяц" появляется сообщение красными буквами "Неверно указан срок действия карты".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

10. В поле "Месяц" введено "13":

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "13";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Месяц" появляется сообщение красными буквами "Неверно указан срок действия карты".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

11. В поле "Месяц" введено одно число:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "1";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Месяц" появляется сообщение красными буквами "Неверно указан срок действия карты".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

12. В поле "Месяц" введен текст:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "qw";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Месяц" появляется сообщение красными буквами "Неверно указан срок действия карты".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

13. В поле "Месяц" введены символы:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "#$";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Месяц" появляется сообщение красными буквами "Неверно указан срок действия карты".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

14. Поле "Месяц" остается пустым:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* Поле "Месяц" не заполнено";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Месяц" появляется сообщение красными буквами "Поле обязательно для заполнения".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась


15. В поле "Год" введено значение, меньшее текущего года:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "10";
* В поле "Год" ввести "21";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Год" появляется сообщение красными буквами "Истек срок действия карты".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

16. В поле "Год" введено значение "00":

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "10";
* В поле "Год" ввести "00";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Год" появляется сообщение красными буквами "Неверно указан срок действия карты".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

17. Поле "Год" не заполнено:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "10";
* Поле "Год" оставляем пустым;
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Год" появляется сообщение красными буквами "Поле обязательно для заполнения".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

18. В поле "Год" введен текст:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "10";
* В поле "Год" ввести "qw";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Год" появляется сообщение красными буквами "Неверно указан срок действия карты".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

19. В поле "Год" введены символы:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "10";
* В поле "Год" ввести "@#";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Год" появляется сообщение красными буквами "Неверно указан срок действия карты".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

20. В поле "Год" введено одно число:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "10";
* В поле "Год" ввести "2";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Год" появляется сообщение красными буквами "Неверно указан срок действия карты".
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

21. В поле "Владелец" введены кириллические буквы:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на кириллице;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Владелец" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

22. В поле "Владелец" введены символы:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено "№%?% "@#$%&";
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Владелец" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

23. В поле "Владелец" введены символы:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено "1234 56789";
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Владелец" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

24. В поле "Владелец" введена фамилия:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено "Ivanov";
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Владелец" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

25. Поле "Владелец" оставляем пустым:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" пустое;
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Владелец" появляется сообщение «Поле обязательно для заполнения».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

26. В поле "Владелец" введена одна буква:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено "I";
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Владелец" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

27. В поле "Владелец" введена большое число букв:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено "Iiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiii";
* В поле "CVC/CVV" ввести "123";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "Владелец" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

28. Поле "CVC/CVV" оставляем пустым:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* Поле "CVC/CVV" оставляем пустым;
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "CVC/CVV" появляется сообщение «Поле обязательно для заполнения».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

29. В поле "CVC/CVV" вводим одно число:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" вводим "1";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "CVC/CVV" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

30.В поле "CVC/CVV" вводим два числа:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" вводим "12";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "CVC/CVV" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

31. В поле "CVC/CVV" вводим четыре числа:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" вводим "1234";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "CVC/CVV" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

32. В поле "CVC/CVV" вводим символы:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" вводим "@#$";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "CVC/CVV" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

33. Нулевое "CVC/CVV":

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* В поле "CVC/CVV" вводим "000";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "CVC/CVV" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась

34. В поле "CVC/CVV" вводим текст:

* В поле "Номер карты" ввести "4444 4444 4444 4441";
* В поле "Месяц" ввести "02";
* В поле "Год" ввести "24";
* Поле "Владелец" заполнено с помощью "Faker" на латинице;
* Поле "CVC/CVV" вводим "qwe";
* Нажать на кнопку "Продолжить".

**Ожидаемый результат:**
* Под полем "CVC/CVV" появляется сообщение «Неверный формат».
* Отправка формы не выполняется.
* В БД в payment_entity новая запись не появилась


### Перечень используемых инструментов:
**1. IntelliJ IDEA** - интегрированная среда разработки программного обеспечения, в которой будет выполняться написание автоматизированных сценариев;

Преимущество:
* Подсветка кода;
* Автоматическое завершение строки;
* Настраиваемые горячие клавиши;
* Настройка шрифтов;
* Выбор цветовой схемы редактора кода.

**2. Java (JDK 11)** - объектно-ориентированный язык программирования, на котором будет выполняться написание автоматизированных сценариев;
 
Преимущество:
* Простота;
* Объектно-ориентированный подход (ООП);
* Безопасность;
* Производительность;
* Надёжность;
* Независимость от аппаратной части и ОС;
* Динамичность и адаптируемость;
* Удобные и эффективные сетевые возможности;

**3. Gradle** - система автоматической сборки;_

Преимущество:
* Версия нового поколения, основанная на инструменте сборки JVM, который сочетает в себе преимущества Ant и Maven;
* Использует основанный на Groovy DSL, который отказывается от XML и является более мощным и выразительным;
* Предоставляет набор методов управления зависимостями, которые не только хорошо настраиваются, но и совместимы с Maven и Ivy;
* Поддерживает плавную миграцию проектов сборки Ant и Maven.

**4. JUnit5** - библиотека для тестирования программного обеспечения;

Преимущество:
* Использует функционал Java 8 или более поздних релизов;
* Добавлено функций для описания, организации и выполнения тестов;
* Возможность использовать более одного процесса тестирования за раз, чего нельзя сделать в JUnit4.

**5. Lombok** - библиотека генерации кода;

Преимущество:
* Просто генерирует код;
* Нет семантики;
* Работает для любой версии, начиная с Java 6

**6. Selenide** - инструмент для тестирования Web-приложений;

Преимущество:
* Встроенное неявное ожидание
* Автоматически закрытие браузера после запуска варианта использования
* Код прост и удобен для чтения
* Автоматически делать скриншоты неудачных тестов

**7. AppVeyor** - веб-сервис непрерывной интеграции, предназначенный для сборки и тестирования;

Преимущество:
* Поддерживается несколько систем управления версиями:
* GitHub.com и GitHub Enterprise
* Bitbucket.com и сервер Bitbucket
* GitLab.com и GitLab Enterprise
* Azure DevOps (репозитории Git и TFVC)
* Настройка CI проектов через пользовательский интерфейс или в точечном файле appveyor.yml.
* Полный sud доступ к виртуальной машине под управлением сборки
* SQL Server 2017 для Linux.
* Bash и ядро PowerShell для управления потоком сборки.
* Параллельная конфигурация как для Windows, так и для Linux-сборок.
* Предустановленный сервис Docker.
* Встроенный сервер NuGet.
* Единый кросс-платформенный Build Worker API как в Windows (сообщения, результаты тестов).

**8. Allure** - инструмент построения отчетов автотестов;

Преимущество:
* Гибкий легкий многоязычный инструмент отчета о тестировании;
* Сокращает общий жизненный цикл дефектов;
* Модульность и расширяемость.
  
**9. Faker** - библиотека генерации данных, с помощью которой, будут созданы данные пользователя (клиента банка);

Преимущество:
* Faker позволяет с легкостью генерировать данные и организовывать работу через API.
  
**10. Git** - инструмент, позволяющий реализовать распределённую систему контроля версий;

Преимущество:
* Откат к определенной точке в разработке; 
* Логирование (ведение журнала) изменений в проекте;
* Быстрое и удобное обновление файлов на сервере (сравнивал на личном опыте, это удобней FTP загрузки);
* Возможность командной разработки.

**11. GitHub** - сервис онлайн-хостинга репозиториев, обладающий всеми функциями распределённого контроля версий и функциональностью управления исходным кодом.

Преимущество:
* Распределенная VCS, которую можно использовать в автономном режиме.
* Git настолько вездесущ, что большинство других разработчиков знакомы с ним, многие проекты с открытым исходным кодом размещаются на GitHub, поэтому это отличное место, чтобы найти и внести свой вклад в эти проекты.
* GitHub также включает сторонние интеграции, чтобы упростить рабочие процессы.
---

### Перечень и описание возможных рисков при автоматизации:
* Возможны ошибки при написании автотестов;
* Возможны ошибки в автотестах, связанные с техническим сбоем;
* Возможны ошибки в автотестах, в связи с изменением кода;
* Неоправданная стоимость и затраты на автоматизацию тестирования;
* Возможен некорректный запуск симулятора банковских карт;
* Возможны сложности при настройке двух СУБД ("MySQL" и "PostgreSQL"), и корректном подключении к каждой из них.

### Интервальная оценка с учётом рисков (в часах):
**Интервальная оценка составляет 75 часов.**
1. Планирование автоматизации тестирования: 7 часов;
2. Автоматизация тестирования: 50 часов;
3. Отчетные документы по итогам автоматизированного тестирования: 8 часов;
4. Отчетные документы по итогам автоматизации: 10 часов;
5. Дополнительно на устранение последствий наступления рисков для реализации проекта ~15 часов (+20% от общего времени).

### План сдачи работы:
1. Планирование автоматизации тестирования: до 01.10.2022
2. Автоматизация тестирования: до 10.10.2022
3. Отчетные документы по итогам автоматизированного тестирования: до 12.10.2022
4. Отчетные документы по итогам автоматизации: до 14.07.2022