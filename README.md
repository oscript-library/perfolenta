# Библиотека для запуска кода Перфоленты из скриптов OneScript.

Библиотека позволяет компилировать на лету и тут же запускать без сохранения на диск, код написанный на языке программирования Перфолента.
Возможно так же подключение откомпилированого кода в качестве внешней компоненты для работы с классами.<br>
Когда может пригодиться?<br>
* Если Вы писали-писали на OneScript и вдруг поняли, что в каком-то месте производительности не хватает;
* Если уже есть код написанный на Перфоленте и можно его просто использовать;
* Если из OneScript не получается сделать вызов какой-либо функции из библиотек написанных на .Net языках;  

## Одновременная компиляция и запуск.
Методы `КомпилироватьИВыполнить` и `КомпилироватьИВыполнитьИзФайла` подходят для однократного или редкого выполнения кода Перфоленты, 
т.к. компиляция происходит при каждом вызове метода, что занимает некоторое время.
В этом случае метод Старт модуля Программа может быть функцией или процедурой, 
но обязательно с одним параметром, имеющим тип совместимый с типами языка OneScript.
Для функции возвращаемое значение так же должно иметь тип совместимый с типами языка OneScript.
Если необходимо передать в метод Старт несколько параметров, то Вы можете поместить их в массив или в другую коллекцию.

```bsl
Скрипт = "
|Программа ЗапускКода
|    Функция Старт(Чис тип Число) тип Строка
|       ВыводСтроки Чис
|       Возврат ""ОК! Выполнено!!!""
|    КонецФункции
|КонецПрограммы 
|";
П = Новый Перфолента;
Результат = П.КомпилироватьИВыполнить(Скрипт,555));
//то же самое, но скрипт должен находиться в файле
Результат = П.КомпилироватьИВыполнитьИзФайла("C:\МойСкрипт.пфлс",555));
```

## Компиляция и многократный запуск.
Подходит для многократного выполнения кода Перфоленты, т.к. компиляция выполняется один раз перед выполнением.
В начале вызывается один из методов `Компилировать` или `КомпилироватьИзФайла`, 
а затем вызов метода `Выполнить` повторяется необходимое количество раз. 
В этом случае метод Старт модуля Программа может быть функцией или процедурой, но обязательно с одним параметром, имеющим тип Объект.
Для функции возвращаемое значение так же должно иметь тип Объект.
Если необходимо передать в метод Старт несколько параметров, то Вы можете поместить их в массив или в другую коллекцию.

```bsl
П = Новый Перфолента;
П.Текст = "
|Программа ЗапускКода
|    Функция Старт(Чис тип Объект) тип Объект
|       ВыводСтроки Чис
|       Возврат ""ОК! Выполнено!!!""
|    КонецФункции
|КонецПрограммы 
|";
//указываем имя модуля Программа, если оно задано в скрипте...
П.Компилировать("ЗапускКода"); 
//команду Выполнить можно запускать многократно с разными параметрами
Результат = П.Выполнить(333);
Результат = П.Выполнить(555);
Результат = П.Выполнить(777);
```

## Подключаем как внешнюю компоненту.
Подходит для использования классов определенных в программе написанной на языке Перфолента.

```bsl
П = Новый Перфолента;
П.Текст = "
|&ВидноВсем Класс Котик    
|    &ВидноВсем Свойство Имя тип Строка = ""Безымянный""
|    &ВидноВсем Функция Мяу() тип Строка
|       Возврат ""МЯУ!!!""
|    КонецФункции
|КонецКласса    
|";
П.Компилировать(); 
П.ПодключитьКакВнешнююКомпоненту();
Котик = Новый COMОбъект("Котик");
Сообщить("Было имя: "+Котик.Имя);
Котик.Имя="Васька";
Сообщить("Стало имя: "+Котик.Имя);
Сообщить(Котик.Мяу());
```

## Требования
OneScript **1.4.0** и выше. <br>
Перфолента **0.4.13.0** и выше.

## Установка

Для установки необходимо:
* Скачать файл perfolenta-*.ospx из раздела [releases](https://github.com/perfolenta/PerfolentaForOneScript/releases)
* Воспользоваться командой:
```
opm install -f <ПутьКФайлу>
```
или установить с хаба пакетов
```
opm install perfolenta
```
Для установки языка Перфолента скачайте и запустите инсталлятор с официального сайта: <br>
[Загрузка файлов и документации языка Перфолента](http://promcod.com.ua/subcat.asp?cat=perfolenta-programmig-language&subcat=perfolenta-download)

## Дополнительная информация

[Статья об интеграции языков OneScript и Перфолента](http://promcod.com.ua/Article.asp?code=20211210023810008537)

## Сайт языка программирования Перфолента

[Переход на сайт языка Перфолента.Net](http://promcod.com.ua/cat.asp?cat=perfolenta-programmig-language)
  
## Лицензия

Смотри файл [LICENSE](https://github.com/perfolenta/PerfolentaForOneScript/blob/main/LICENSE).
 
 
