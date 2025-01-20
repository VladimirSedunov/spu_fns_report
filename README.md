<h1 align="center">Проект по тестированию процесса получения и обработки отчетности по страховым взносам, поступающей из ФНС, подсистемы СПУ"</h1>
<hr>

## Проект реализован с использованием
Python = Pytest = Selenium = Selene = Selenoid = Allure Report = Jenkins = Telegram = Java

![](/design/icons/Python.png)&emsp;![](/design/icons/Pytest.png)&emsp;![](/design/icons/Selenium.png)&emsp;![](/design/icons/Selene.png)&emsp;![](/design/icons/Selenoid.png)&emsp;![](/design/icons/Allure_Report.png)&emsp;![](/design/icons/Jenkins.png)&emsp;![](/design/icons/Telegram.png)&emsp;![](/design/icons/Java.png)
<hr>

## Основные понятия и функции тестируемого процесса

Пакет с отчетностью ФНС «за НР» состоит из документов:
* –	Документ «Расчёт по страховым взносам» – VO_RASCHOPS.
* –	Унифицированный протокол проверки - УПП.

Пакет с отчетностью ФНС для ФЛ/ИП/КФХ «за себя» состоит из документов:
* –	Документ «Расчёт по страховым взносам» – VO_RASCHOPS.
* –	Документ «Уплата страхователем страховых взносов» – VO_SVEDUPLSV.
* –	Унифицированный протокол проверки – УПП.

Основные функции:
Подсистема СПУ должна обеспечить выполнение следующих функций:
* –	Прием и обработку пакета отчетности ФНС по наемным рабочим с учетом УПП; 
* –	Прием и обработку пакета отчетности по КФХ за себя / КФХ «за члена КФХ» с учетом VO_SVEDUPLSV и УПП;
* –	Прием и обработку пакета отчетности по ИП/ФЛ «за себя» с учетом VO_SVEDUPLSV и УПП;
* –	Прием и обработку пакета отчетности по ИП АУСН с учетом УПП;
* –	Автоматическое формирование стажа по отчетности ИП/ФЛ/ИП АУСН;
* –	Автоматический расчет уплат по ИП/ФЛ/КФХ «за себя»;
* –	Формирование протокола разнесения сведений.

Документы, участвующие в технологическом процессе:
* Входящие документы по процессу:
  * Расчет по СВ в части начислений по СВ на ОПС	(VO_RASCHOPS)
  * Уплата страхователями СВ на ОПС для целей ведения ПУ	(VO_SVEDUPLSV)
  * Унифицированный протокол проверки	(УНИФИЦИРОВАННЫЙ_ПРОТОКОЛ_ПРОВЕРКИ)
* Исходящие документы по процессу:
  * Протокол разнесения сведений пакета с расчетом и уплатами	(ПРС VO_RASCHOPS)
<hr>

## Основной шаблон выполнения шагов тест-кейса:
* Подготовка База Данных (удаление сведений по Расчопс, Налпров, Сведупл по указанным организациям за все периоды, выполнение скриптов UPDATE организаций)
* Чтение файла КП (контрольные примеры) с набором тест-кейсов (чек-лист), настройками и скриптами для сверки в формате Excel.
* Укладка реестра:
  * Распаковка zip-файла во временный каталог tmp с учётом кириллицы в названиях файлов
  * -
  * подготовка файла описателя VO_SVEDUPLSV в формате json (корректировка полей) из шаблонного файла
  * отправка опияателя (POST-запрос) и получение значения id из response
  * отправка VO_SVEDUPLSV (файл в формате XML) на URL, зависящий от id, и получение значения documentId из response в формате JSON
  * -
  * подготовка файла описателя УПП в формате json (корректировка полей) из шаблонного файла
  * отправка описателя УПП (POST-запрос) и получение значения id из response
  * отправка УПП (файл в формате XML) на URL, зависящий от id, и получение значения documentId из response в формате JSON
  * -
  * подготовка файла описателя VO_RASCHOPS в формате json (корректировка полей) из шаблонного файла
  * отправка описателя VO_RASCHOPS (POST-запрос) и получение значения id из response
  * отправка VO_RASCHOPS (файл в формате XML) на URL, зависящий от id, и получение значения documentId из response в формате JSON
  * -
  * корректировка множества полей файла в формате SOAP из zip-архива ("квиток") и сохранение в папке TMP в формате XML
  * отправка квитка через MQClient с помощью приложения и библиотек JAVA
* Ожидание окончания процесса укладки реестра (выполнение в цикле пяти различных SQL-запросов к базе данных)
* Создание отчёта в Excel о расхождениях между ожидаемым и фактическим результатом
<hr>
 
## Фрагмент КП для ручного тестирования.
Тест-кейсов очень много, из них отобраны 103 теста для автоматизации.
![](/design/images/КП.PNG)
<hr>

## Файл КП для автотестов:
* /data/ФНС_отчетность/КП автотест ФНС.xlsx

<hr>

## Как реализованы проверки в автотестах


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
