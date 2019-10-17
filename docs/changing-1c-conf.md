# Порядок внесения изменений в конфигурацию

## Именование добавляемых объектов конфигурации

Все добавляемые объекты конфигурации должны именоваться начиная с префикса «бит_», в случае проектной разработки с последующим сопровождением силами заказчика, рекомендуется использовать собственный префикс заказчика. К таким объектам относятся:

* Объекты метаданных (общие модули, справочники, документы, подсистемы и т.п.)

* Реквизиты объектов метаданных (включая: реквизиты, табличные части, измерения регистров, команды и т.п.)

* Предопределенные значения (включая: значения перечислений, предопределенные значения справочников и т.п.)

* Реквизиты форм
_Если добавляется реквизит формы с колонками префикс устанавливается для самого реквизита формы, но не для его колонок._

* Элементы форм
_Если добавляется группа формы, то префикс устанавливается для группы, для добавляемых элементов формы, входящих в группу префикс не указывается_

  **При добавлении полей формы рекомендуется добавлять сначала группу формы и для нее устанавливать префикс «бит_», а затем уже добавлять элементы в группу без префикса – это позволит уменьшить количество изменений ряда программных алгоритмов.**

* Процедуры и функции
_Если процедура/функция добавляется в модуль объекта/формы (или общий модуль), который уже имеет префикс «бит_», то в имени такой процедуры/функции префикс указывать не нужно._

## Включение объектов в состав типовых подсистем командного интерфейса

Если необходимо включить объект в состав типовой подсистемы командного интерфейса, то необходимо добавить в состав типовой подсистемы новую (если отсутствует) подсистему с префиксом  **«бит_»**. Дальнейшая настройка размещения объекта в командном интерфейсе должна выполняться в рамках добавленной подсистемы.

## Комментирование изменений в коде

Все изменения вносимые в код модулей конфигурации должны обрамляться комментарием вида:

```bsl
//бит_<Первая буква имени автора><Фамилия автора> - начало изменения [(IR/CR <номер запроса>)] <дата изменения> {{
КОД
//}} бит_<Первая буква имени автора><Фамилия автора> - конец изменения
```

[(IR/CR <номер запроса>)] – в случае, когда изменение в модуле связано с исправлением ошибки или с реализацией запроса на изменение указывается номер ошибки/изменения с префиксом IR для ошибки или CR для запроса на изменение.

Например:

```bsl
//бит_АКузнецов - начало изменения 11.01.2014 {{
//Выборка = Результат.Выбрать();
//Пока Выборка.Следующий() Цикл
ТаблицаСтрок = Результат.Выгрузить();
Для Каждого ТекСтрока Из ТаблицаСтрок Цикл
//}} бит_АКузнецов - конец изменения
```

Описания добавляемых процедур должны начинаться с:

```bsl
//бит_<Первая буква имени автора><Фамилия автора> - <описание назначения процедуры/функции>
//
```

Например:

```bsl
//бит_АКузнецов – Функция преобразует строку в массив подстрок
//
// Параметры:
// Стр  - Строка - Строка для преобразования
// Разделитель - Строка - Разделитель подстрок (по умолчанию «;»)
//
// Возвращаемое значение - Массив  - Результат преобразования
//
Функция РазложитьСтроку(Знач Стр, Разделитель = «;»)
```

## Изменение ролей

Недопустимо изменение типовых ролей базовой конфигурации, при необходимости добавления/изменения настроек прав доступа, необходимо создать дополнительную роль с префиксом «бит_» и выполнить необходимые настройки прав для нее.

Если функционал, для которого настраиваются права доступа, имеет аналоги в базовой конфигурации, то рекомендуется создавать новые роли путем копирования существующих и затем настраивать в них права по аналогии с существующим функционалом.

В дальнейшем допускается модификация созданных ролей при необходимости.
Также при создании ролей необходимо придерживаться принципа «атомарности» ролей, т.е. не рекомендуется смешивать в одной роли права на не связанный друг с другом по смыслу функционал.

Назначение прав пользователей должно выполняться с использованием функционала профилей доступа из библиотеки стандартных подсистем.

## Внесение изменений в общие модули

Если необходимо внести изменения в общие модули, то по возможности следует вносить изменения только в модули с суффиксом «*Переопределяемый».

## Внесение изменений в обработчики событий форм

При необходимости внесения изменений в поведение обработчиков событий типовой формы, такие изменения необходимо вносить в общих модулях:

* МодификацияКонфигурацииВызовСервераПереопределяемый

* МодификацияКонфигурацииКлиентПереопределяемый

* МодификацияКонфигурацииКлиентСерверПереопределяемый

* МодификацияКонфигурацииПереопределяемый

в соответствующих процедурах-обработчиках.

При этом рекомендуется в процедурах обработчиках прописывать только вызовы процедур-обработчиков, добавленных в новые общие модули.

## Внесение изменений в обработчики событий объектов и менеджеров

В общем случае не допускается внесение изменений в типовые обработчики событий объектов и менеджеров.
Для таких изменений необходимо создавать соответствующие подписки на события.

## Внесение изменений в состав источников типовых подписок на события

В не допускается внесение изменений в состав источников типовых подписок на события.
Вместо изменения типовой подписки необходимо:

1. Создать новую подписку (можно скопировать существующую) с наименованием «бит_<ИмяТиповойПодписки>»
2. Указать в качестве источника те объекты, которые необходимо добавить
3. В качестве обработчика указать ту же процедуру, что и в типовой подписке.

## Комментарии к изменениям в хранилище

Рекомендуется при помещении измененных объектов в хранилище в соответствующем диалоге конфигуратора указывать в комментарии краткое описание характера изменений с точки зрения пользователя.
Это в дальнейшем облегчит сборку общего описания изменений к релизу.

## Описание изменений к релизу

Описание изменений к релизу формируется по разделам, каждый раздел соответствует разделу учета (проектному решению) (например: Интеграция, Снабжение, Склад и т.п.). Для изменений которые не могут быть отнесены ни к одному из существующих разделов можно использовать раздел «Прочие изменения».
Каждый раздел описания изменений должен включать подразделы:

* Изменения в системе – содержит описания отдельных изменений с точки зрения пользователя (см. ниже)

* Изменения в инструкциях пользователя – содержит список измененных разделов инструкции пользователя

* Исправленные ошибки – содержит номера исправленных ошибок, перечисленные через запятую.

Описание каждого отдельного изменения должно включать:

* Порядковый номер в рамках текущего описания изменений

* Наименование объекта системы, с которым изменение связано в первую очередь (если применимо)

* Краткое описание характера внесенных изменений с точки зрения пользователя

* Номер исправленной ошибки или реализованного запроса на изменение (если такой существует)

  _Если изменение однотипно для нескольких объектов системы, то такое изменение указывается один раз, а все объекты перечисляются списком._

### Рекомендации

* Описание изменений необходимо делать при каждом помещении изменений в хранилище;

* Не рекомендуется накапливать мало связанные между собой изменения до помещения их в хранилище. Версии в хранилище должны быть максимально "атомарны;

* Для сбора отчета по изменениям можно воспользоваться стандартной функцией конфигуратора "отчет по версиям хранилища";