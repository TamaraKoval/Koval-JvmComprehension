## Домашнее задание "Осмысление JVM" ##
### (Выполнено для netology.ru) ###

1. ```int i = 1;```
Создана и инициализирована примитивная переменная типа int. Она хранится в фрейме функции main() в StackMemory.
2. ```Object o = new Object();```
Создана переменная типа Object, которая содержит ссылку на объект. Сама переменная находится в фрейме функции main() в стеке. 
Также создан новый объект типа Object, который помещен  в heap (кучу). Новый объект создан с помощью ClassLoader`а.
3. ```Integer ii = 2;``` Создана и инициализирована ссылочная переменная типа Integer (это "класс-обертка" для примитивного типа int). 
Сама переменная ii, хранящая ссылку на объект, хранится в фрейме функции main() в стеке. 
Новый объект типа Integer помещен в кучу.
4. ```printAll(o, i, ii);``` Вызов функции printAll. Создается фрейм функции printAll() в стеке. 
Внутри фрейма создаются 3 переменные для параметров: o, i и ii. 
Примитивная переменная i помещается в стек, ссылочные переменные o и ii устанавливают связь с переданными в функцию объектами из кучи.
5. ```Integer uselessVar = 700;``` Создана ссылочная переменная типа Integer. 
Сама переменная uselessVar, хранящая ссылку на объект, хранится в фрейме функции printAll() в стеке. 
Новый объект типа Integer помещен в кучу. 
Эта переменная нигде не зайдействована, поэтому подсвечена серым цветом.
6. ```System.out.println(o.toString() + i + ii);``` Создается фрейм метода to.String в стеке, в куче создается новый объект типа String. 
После этого создается еще один фрейм для метода println(), где хранятся: переменная i, а также ссылки на ii и на ранее созданный объект String.
7. ```System.out.println("finished");``` В куче создается строковый литерал "finished". В стеке создается фрейм функции println(), храняящий ссылку на этот строковый литерал.

### О том, что происходит до пронумерованных строк ###
* В момент запуска программы запускается ClassLoader.
* Информация o JvmComprehension.class и системных классах загружается в Metaspace.
* На строке ```public static void main(String[] args)``` в стеке создается фрейм функции main().
* После этого код отрабатывается построчно, как описано в пунктах выше. 

### О сборщиках мусора ###
* После выполнения строк с функцией println() (6 и 7), сборщик мусора удаляет созданные для них фреймы.
* Фрейм метода toString() (строка 6) удаляется сразу после отработки кода метода println() и удаления его фрейма.
* После выполнения кода функции printAll() на 4 строке, сборщик мусора удаляет фрейм этой функции.
* После выполнения кода функции main() на 7 строке, сборщик мусора удаляет созданный для нее фрейм из стека. Программа выполнена, стек пуст.
