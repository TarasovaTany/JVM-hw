# Задача "Понимание JVM"

## Описание
Просмотрите код ниже и опишите (текстово или с картинками) каждую строку с точки зрения происходящего в JVM

Не забудьте упомянуть про:
- ClassLoader'ы,
- области памяти (стэк (и его фреймы), хип, метаспейс)
- сборщик мусора

## Код для исследования
```java

public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
} 
```

# Ответ

1. `int i = 1;`:
    * ClassLoader:  `ClassLoader` загружает класс `JvmComprehension` в память.
    * Method Area (metaspace): Здесь хранится информация о классе `JvmComprehension`, включая определение метода `main`.
    * Stack:  JVM создает фрейм стека для метода `main`. Внутри фрейма выделяется место для примитивной переменной `i`  и ей присваивается значение `1`.

2. `Object o = new Object();`:
    * Heap: Создается новый экземпляр класса `Object`.  Этот объект размещается в heap.
    * Stack: Внутри фрейма стека `main`  переменной `o` присваивается ссылка на объект `Object` в heap.

3. `Integer ii = 2;`:
    * Heap: Создается объект-обертка `Integer`  с значением `2` и размещается в heap.
    * Stack:  Переменной `ii` присваивается ссылка на объект `Integer` в heap.

4. `printAll(o, i, ii);`:
    * Stack:
        * Создается новый фрейм стека для метода `printAll`.
        * В этот фрейм копируются значения `i` (1),  ссылка на объект `o` из heap и ссылка на объект `ii` из heap.

5. `Integer uselessVar = 700;`:
    * Heap:  Создается объект `Integer` со значением `700` в heap.
    * Stack:  Переменной `uselessVar` в фрейме `printAll` присваивается ссылка на объект `Integer`.

6. `System.out.println(o.toString() + i + ii);`:
    * Выполняется метод `toString()` объекта `o`.
    * Происходит конкатенация строк.
    * Результат выводится на консоль.
    * Garbage Collector: Объект `Integer` (uselessVar), созданный внутри `printAll`, становится недостижимым после завершения метода.  Сборщик мусора может удалить его из heap.

7. `System.out.println("finished");`:
    * Строка "finished" выводится на консоль.
    * После завершения метода `main`,  фрейм стека `main` уничтожается. Все локальные переменные  (`i`, `o`, `ii`)  становятся недоступными.
    * Garbage Collector: Сборщик мусора может удалить объекты, на которые ссылались эти переменные, если на них больше нет ссылок из других частей программы.
