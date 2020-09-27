# Net_Run_In_Memory_HostingCLR
Выполнение сборки .NET из C++ в памяти (хостинг CLR)

**Для работы нам потребуется:**\
1 - **VisualStudio 2019** и любая другая.\
2 - Установленные **C#** и **С++** инструменты (**SDK** *10.0.18362.0*)\
3 - **HxD** - это шестнадцатеричный редактор (*потребуется для конвертирования в байты*)

Видео инструкция (*кликните по картинке снизу*)\
[![C++ - Hosting CLR and Executing .NET Assembly in Memory](https://i.ytimg.com/vi/pCRBpkze1Ek/hqdefault.jpg)](https://www.youtube.com/watch?v=pCRBpkze1Ek "Выполнение сборки .NET из C++ в памяти (хостинг CLR)")

**Как работать?**\
Для начала создаём приложение в C#(Csharp), компилируем его обязательно в **Release**, проверяем что приложение работает.

Затем запускаем редактор HxD, перемещаем туда наш скомпилированный файл, выделяем весь байтовый код (ctrl + a)<br> жмём по кнопочке "**Правка**" -> "**Копировать как**" -> Выбераем язык "**С**"

После этого у нас в буфер обмена скопируется весь наш байтовый код приложения.

Следующим этапом запускаем в студии наш проект HostingCLR, переходим в класс **HostingCLR.cpp** тут нам нужно вставить наши байты за место этого кода:

````cpp
unsigned char rawData[8192] = {
  // Сюда кидаем байты!
};
````
Проверяем какой получился размер в `rawData` и запишем его размер сюда:
````cpp
constexpr auto RAW_ASSEMBLY_LENGTH = 8192;
````
Если в Main (точке входа нету никаких аргументов для передачи, оставляйте 0)
````cpp
SAFEARRAY *psaStaticMethodArgs = SafeArrayCreateVector(VT_VARIANT, 0, 0);
/* EntryPoint.Invoke(null, new object[0]) */
````
Если какие-то аргументы существуют в Main то записываете следующее: ( сколько аргументов, столько и записывайте )
````cpp
SAFEARRAY *psaStaticMethodArgs = SafeArrayCreateVector(VT_VARIANT, 0, 1);
/* EntryPoint.Invoke(null, new object[1]) */
````
Компилируем, готово!
