# spu_fns_report

<h1 align="center">Проект по тестированию сайта<br>"Софт Компани - цифровой системный интегратор."</h1>
> <a target="_blank" href="https://softwarecom.ru/">Ссылка на единый портал</a>

![This is an image](/design/images/main_page.PNG)

### Список проверок, реализованных в автотестах
- [x] Наличие требуемых заголовков на каждой из страниц, соответствующих пунктам в главном (верхнем) меню
- [x] Наличие текста с копирайтом "Софт Компани — цифровой интегратор © 2023" и текущего года на основной странице
- [x] Наличие сообщения 'Страница_не_найдена' при попытке перехода на несуществующую страницу
- [x] Наличие подпунктов меню главного меню (при наведении курсора мыши) и корректное открытие соответствующих страниц
- [x] Наличие блоков с логотипами организаций внизу страницы в разделе 'Клиенты и партнёры', возможность пролистывания блоков и замена логотипа на содержательную информацию при наведении курсора 
- [x] Для каждой комбинации "Отрасль" + "Услуга" отображаются отфильтрованные блоки с логотипами клиентов
- [x] Отправка сообщения из меню "Обратная Связь" / "Задать вопрос" с корректным и некорректными наборами реквизитов
- [x] Те же проверки при отправке сообщения, но реализованные с помощью параметризации теста
- [x] Отправка сообщения со страницы "О нас", кнопка "Отправить заявку", реализованная с помощью Page Object
- [x] Проверка контактных данных на странице "Контакты" (адрес, email и телефон есть и соответствуют реальным данным)
- [x] Те же проверки для сервиса REST API

## Проект реализован с использованием
Python = Pytest = Selenium = Selene = Selenoid = Allure Report = Jenkins = Telegram = Java

![](/design/icons/Python.png)&emsp;![](/design/icons/Pytest.png)&emsp;![](/design/icons/Selenium.png)&emsp;![](/design/icons/Selene.png)&emsp;![](/design/icons/Selenoid.png)&emsp;![](/design/icons/Allure_Report.png)&emsp;![](/design/icons/Jenkins.png)&emsp;![](/design/icons/Telegram.png)&emsp;![](/design/icons/Java.png)


## Запуск автотестов выполняется на сервере Jenkins
> <a target="_blank" href="http://10.155.56.61:8888/job/Demo test softwarecom/">Ссылка на проект в Jenkins</a>


### Параметры сборки

* SELECT_TESTS (default: tests -m jenkins_ok). Параметр определяет группу тестов или отдельный тест для запуска.
* browser (default: chrome). Браузер chrome или firefox.
* browser_version (default: 96.0). Версия браузера на Selenoid.
* window_size (default: 1920x1080). Размер окна браузера.


### Для запуска автотестов в Jenkins
#### 1. Открыть <a target="_blank" href="http://10.155.56.61:8888/job/Demo test softwarecom/">проект</a>

![](/design/images/jenkins1.PNG)

#### 2. Выбрать пункт "Собрать с параметрами"
#### 3. В случае необходимости изменить параметры, выбрав значения из выпадающих списков
#### 4. Нажать "Собрать"
#### 5. Результат запуска сборки можно посмотреть в отчёте Allure
![](/design/images/jenkins2.png)

## Локальный запуск автотестов
Пример командной строки:
```
pytest tests -m jenkins_ok
```

Получение отчёта:
```
allure.bat serve allure-results
```

## Настроено автоматическое оповещение о результатах сборки Jenkins в Telegram-бот
![](/design/images/telegram_bot_ru.PNG)

![](/design/images/telegram_bot_ru_green.PNG)

## Видеоотчёт теста
![](https://github.com/VladimirSedunov/softwarecom/blob/master/design/video/test_video.gif)
