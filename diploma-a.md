# Диплом блока А "Настройки и справочники"

## Описание задачи
Создать конфигурацию "Управление ИТ-фирмой" с базовым набором справочников, поддерживающую управление пользователями ИБ в режиме Предприятия.

## Требования к результату

Конфигурация с именем "УправлениеИТФирмой", содержащая:

### Перечисление ЮридическоеФизическоеЛицо

#### Со значениями
ЮридическоеЛицо, ФизическоеЛицо

### Справочник Контрагенты

#### С реквизитами
ПолноеНаименование, ИНН, КПП, EMail, Телефон, ФактическийАдрес, ЮридическийАдрес, Тип (ПеречислениеСсылка.ЮридическоеФизическоеЛицо), Ответственный (СправочникСсылка.Сотрудники).
Типы должны быть подходящими, их длина и точность - разумно достаточными.

#### С формой элемента

КПП должен быть доступен только для контрагентов-юридических лиц. Доступность должна отрабатываться как при изменении типа, так и при перечитывании элемента справочника из ИБ.

При изменении наименования полное наименование должно заполняться с разворачиванием распространенных сокращений организационно-правовых форм (например, *АО "Вектор"* должно превращаться в *Акционерное общество "Вектор"*).

Должна быть реализована проверка корректности ИНН как для физических, так и для юридических лиц. Проверять нужно и при попытке записи (с выдачей предупреждения и отказом), и при изменении ИНН (с подсветкой поля ввода и/или выводом текста ошибки рядом с ним).

#### С формой списка

В форме списка должны присутствовать все существенные реквизиты в разумном порядке.

#### С модулем объекта

В коде которого определено заполнение по умолчанию:
* Тип - Юридическое лицо.
* Ответственный - текущий сотрудник из параметра сеанса **ТекущийСотрудник**.

### Перечисление Пол

#### Со значениями
Мужской, Женский

### Справочник Сотрудники

#### С реквизитами
EMail, ДатаРождения, ИдентификаторПользователяИБ, Оклад, Пол, СтавкаЧаса, Телефон.
Типы должны быть подходящими, их длина и точность - разумно достаточными.
Для сумм нужно использовать определяемый тип.

#### С модулем менеджера

В котором переопределено получение формы объекта в зависимости от права доступа "Администрирование".

#### С формой элемента ФормаАдминистратора

Которая открывается для пользователей с правом доступа "Администрирование".

В которой, помимо реквизитов сотрудника, есть флажок "Вход разрешен" и поля ввода "Имя для входа" и "Пароль", отражающие свойства пользователя ИБ.

При записи сотрудника из этой формы при необходимости должно выполняться:
* создание пользователя ИБ с ролью **БазовыеПрава**;
* изменение его пароля и имени для входа;
* при снятии флажка - отключение стандартной аутентификации.

Рядом с полем ввода пароля должна быть команда "Случайный пароль", генерирующая случайный пароль и показывающая его пользователю.

#### С формой элемента ФормаПользователя

Которая открывается для пользователей без права доступа "Администрирование" и содержит элементы управления для реквизитов сотрудника, упорядоченные по смыслу.

### Константу и функциональную опцию ВестиРасчетЗарплаты

Константа не должна присутствовать в командном интерфейсе сама по себе (флажок "Использовать стандартные команды" должен быть снят).
В состав ФО должны входить зарплатные реквизиты справочника **Сотрудники**, а константа должна присутствовать флажком в общей форме **НастройкаПрограммы**.

### Общую форму и общую команду **НастройкаПрограммы**

Форма должна содержать основной реквизит типа **НаборКонстант** и поле флажка для константы **ВестиРасчетЗарплаты**.
Общая команда должна открывать общую форму, принадлежать подсистеме **Настройки** и присутствовать в командном интерфейсе раздела "Настройки".

### Параметр сеанса ТекущийСотрудник

Типа **СправочникСсылка.Сотрудники**. Должен заполняться элементом справочника **Сотрудники**, идентификатор пользователя ИБ которого совпадает с идентификатором текущего пользователя ИБ.

### Роли БазовыеПрава и ПолныеПрава

Роль **ПолныеПрава** должна давать права на все, кроме:
* интерактивного удаления элементов справочников;
* пометки на удаление и удаления помеченных предопределенных элементов справочников. 

Роль **БазовыеПрава** должна давать права на чтение, просмотр и ввод по строке всех данных. Редактирование, добавление и изменение разрешается только для справочника **Контрагенты**. Роль не должна давать права на открытие настроек программы.

### Подсистему "Настройки"

Содержащую все справочники и общую команду **НастройкаПрограммы**.

## Процесс выполнения

Старайтесь использовать наработки, выполненные ранее в домашних заданиях блока А.

### Подсистема Настройки

Создайте подсистему **Настройки**, куда будете включать все добавляемые далее объекты метаданных.

### Константа и ФО

Создайте константу **ВестиРасчетЗарплаты** типа **Булево** и соответствующую ФО.
Константу включите в подсистему **Настройки**. Не забудьте снять флажок "Использовать стандартные команды", чтобы константа не появилась в командном интерфейсе сама по себе.

### Настройка программы

Создайте общую форму **НастройкаПрограммы**.
Добавьте в нее основной реквизит типа **НаборКонстант**.
Выведите константу **ВестиРасчетЗарплаты** на форму полем флажка. Заголовок флажка по общим правилам разместите справа.
Создайте общую команду **НастройкаПрограммы**, в модуле которой реализуйте открытие общей формы.
Включите команду и форму в подсистему **Настройки**.

### Справочник Сотрудники

Создйте справочник и наполните его указанным в Требованиях набором реквизитов.
Включите зарплатные реквизиты в состав ФО **ВестиРасчетЗарплаты**.

Создайте две формы элемента, **ФормаАдминистратора** и **ФормаПользователя**. Форма администратора будет основной.

В модуле менеджера определите обработчик события **ОбработкаПолученияФормы()**. В нем, в зависимости от наличия права доступа **Администирование**, открывайте форму администратора или форму пользователя. Право администрирования проверяйте так:

    Если ПравоДоступа("Администрирование", Метаданные) Тогда
		
Выбранную форму лучше возвращать не по имени, а как объект метаданных, например:

    Метаданные.Справочники.Сотрудники.Формы.ФормаАдминистратора
    

#### ФормаПользователя

Выведите в нее реквизиты сотрудника в разумном порядке.

#### ФормаАдминистратора

Можно создать копированием формы **ФормаПользователя**.
Создайте в ней две группы, левую и правую.
В левую выведите реквизиты сотрудника в разумном порядке (как в форме пользователя).
Добавьте реквизиты формы **ВходРазрешен** (Булево), **ИмяДляВхода** и **Пароль** и выведите их в правую группу флажком и двумя полями ввода. Для поля ввода **Пароль** включите режим пароля, чтобы введенное забивалось звездочками.

##### Случайный пароль
Создайте команду и кнопку **СлучайныйПароль**.
В обработчике создайте случайный пароль из 5-6 букв и цифр, отключив у поля ввода **Пароль** режим пароля, чтобы пользователь увидел его и мог скопировать.

##### ПриЧтенииНаСервере
Найдите пользователя ИБ по идентификатору (**ТекущийОбъект.ИдентификаторПользователяИБ**).
Заполните по данным пользователя ИБ реквизиты формы **ИмяДляВхода** и **Пароль**. Реквизит **ВходРазрешен** заполните по реквизиту пользователя ИБ **АутентификацияСтандартная**.
Если идентификатор не заполнен, или поиск пользователя ИБ возвращает **Неопределено**, считайте, что вход не разрешен, а имя и пароль пусты.
    
##### ПередЗаписьюНаСервере
Если идентификатор пользователя ИБ заполнен - найдите пользователя ИБ и обновите его реквизиты значениями реквизитов  **ВходРазрешен**, **ИмяДляВхода** и **Пароль**.
Если идентификатор не заполнен, а флажок **ВходРазрешен** выставлен - создайте пользователя ИБ и добавьте ему роль **Метаданные.Роли.БазовыеПрава**. 

После создания пользователя ИБ присвойте его идентификатор реквизиту **ИдентификаторПользователяИБ** записываемого объекта, чтобы найти этого пользователя ИБ при следующем открытии формы.
Обратите внимание на то, что учебная версия платформы не позволяет задать непустой пароль. Логику задания пароля, однако, все равно нужно реализовать, а в тестах задавать пустой пароль.

#### Форма списка

Создайте форму списка, добавив в нее все существенные реквизиты в разумном порядке и расположении.

### Параметр сеанса

Создайте параметр сеанса **ТекущийСотрудник** типа **СправочникСсылка.Сотрудники**.
В модуле сеанса определите процедуру **УстановкаПараметровСеанса**.
В ней, для простоты не анализируя параметр **ТребуемыеПараметры**, найдите сотрудника по значению реквизита **ИдентификаторПользователяИБ**, используя функцию менеджера:

    Справочники.Сотрудники.НайтиПоРеквизиту(<...>);

Значение идентификатора получите, обратившись к функции **ТекущийПользователь()** менеджера **ПользователиИнформационнойБазы**.

### Справочник Контрагенты

Создайте справочник и наполните его указанным в Требованиях набором реквизитов.
Не забудьте поставить флажок "Заполнять из данных заполнения" хотя бы для реквизитов **Тип** и **Ответственный**.
В модуле объекта определите обработчик **ОбработкаЗаполнения**, в котором заполните реквизит **Тип** значением по умолчанию **ЮридическоеЛицо**, а реквизит **Ответственный** - значением параметра сеанса **ТекущийСотрудник**.

#### Форма элемента

Создайте форму элемента, поместив на нее все существенные реквизиты в разумном порядке и расположении.

Реализуйте управление доступностью поля ввода **КПП** в зависимости от типа контрагента, сделав это в обработчиках событий **ПриЧтенииНаСервере** формы и **ПриИзменении** поля ввода **Тип**.

Реализуйте функцию определения корректности ИНН, которая работала бы со всеми ИНН и возвращала бы, кроме значения Булево (Истина - корректен, Ложь - некорректен), текстовое описание ошибки как неявно возвращаемое значение. 
Алгоритмы расчета:

https://www.egrul.ru/test_inn.html
https://keysystems.ru/files/fo/arm_budjet/show_docum/BKS/onlinehelp/index.html?ro_kr_algor_klyuch_inn.htm

Реализуйте вызов этой функции в двух местах:
* В обработчике события **ПередЗаписью** формы - с отказом от записи при неверном ИНН и выводом предупреждения, содержащего текстовое описание ошибки.
* В обработчике события **ПриИзменении** поля ввода ИНН - с подсветкой текста поля ввода и/или с выводом рядом с полем ввода текстового описания ошибки в виде декорации или подсказки.

Считайте, что пустой ИНН корректен.

В обработчике события **ПриИзменении** поля ввода **Наименование** реализуйте поиск распространенных сокращений организационно-правовых форм в начале строки и заполнение полного наименования по краткому с заменой сокращения ОПФ на ее полное наименование. Не изменяйте полное наименование, если оно уже было изменено пользователем вручную.

#### Форма списка

Создайте форму списка, добавив в нее все существенные реквизиты в разумном порядке и расположении.

### Роли

Создайте роли **ПолныеПрава** и **БазовыеПрава**.

В состав роли **ПолныеПрава** включите все права, сняв только:
* интерактивное удаление элементов справочников.
* пометку на удаление и удаление помеченных предопределенных элементов.

В состав роли **БазовыеПрава** включите права на чтение, просмотр и ввод по строке всех данных, включая параметры сеанса. Разрешите также изменение, добавление, редактирование и интерактивную пометку удаления справочника **Контрагенты**.

### Проверка

1. Проверьте, что подсистема **Настройки** содержит все добавленные вами объекты метаданных.
2. Создайте пользователя ИБ с полными правами.
3.  Запустив программу под ним, удостоверьтесь в видимости раздела "Настройки", всех справочников и команды "Настройка программы".
4. Включите ФО "Вести расчет зарплаты".
5. В справочнике "Сотрудники" создайте одного или нескольих сотрудников, разрешив вход.
6. Запустив программу под ними, удостоверьтесь в видимости раздела "Настройки" и всех справочников.
7. Открыв какого-нибудь сотрудника, убедитесь, что открывается именно форма пользователя.
8. Проверьте создание и редактирование контрагентов, введя реальные данные.
