Предыстория:
IT компания N разрабатывает программу для нахождения ошибок в Java коде и предоставления возможных вариантов для устранения этих ошибок.

Отдел, который занимается разработкой модуля для анализа кода, работающего в многопоточной среде, получил задание реализовать новый плагин для поиска избыточных блокировок(lock) и разблокировок(unlock) доступа к общему ресурсу(Смотрите java.util.concurrent.locks.ReentrantLock).

Также этот плагин должен будет подсказывать, какие из lock/unlock можно убрать, чтобы код работал правильно.

Программисты начали работать над заданием.
Один программист записал такой пример:
        ReentrantLock reentrantLock = new ReentrantLock();
        reentrantLock.lock();
        // code ...
        reentrantLock.unlock();
        // code ...
        reentrantLock.unlock();
        // code ...
        reentrantLock.lock();
        // code ...
        reentrantLock.unlock();
        // code ...
        reentrantLock.unlock();

Другой предложил свести задачу к более простой формулировке:
Будем отмечать lock как {, а unlock как }.

Тогда на основании примера приведеного выше получится такое выражение: {}}{}}.

Задача:
Вам нужно написать функцию вида:

    public static Set<String> validate(String input) {
        ...
    }

, которыя будет принимать на входе строку, например: input = "{}}{}}".
На выходе функция "validate" должна возвращать можество "правильных" строк, полученных из входной строки путем удаления МИНИМАЛЬНОГО количества избыточных lock/unlock, а именно { и }.

Пример 1:
На входе:
"{}}{}}"
На выходе:
{{}}
{}{}

Пояснение:
Чтобы получить значение {{}} из строки {}}{}} удалили символы с индексами 1 и 2(нумерация начинается с 0) "{__{}}".
Чтобы получить значение {}{} из строки {}}{}} удалили символы с индексами 2 и 5 "{}_{}_" или 1 и 4 "{_}{_}"и т.д.

Пример 2:
На входе:
"{}x}x}"
На выходе:
{x}x
{xx}
{}xx

Пояснение:
В строках могут содержаться и другие символы кроме { или }, например "x".

Пример 3:
На входе:
"{"
На выходе:

Пояснение:
Если строка не может быть преобразована в правильную путем удаления символов, то результирующий набор будет пустой.
