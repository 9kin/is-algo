# Примеры решения задач

Решим задачу. Прочтите условие задачи по [ссылке](https://codeforces.com/contest/1399/problem/A).

Анализ условия
===

Переформулируем условие :

> Дан массив. Вы несколько раз (возможно ноль) выбираете два разных индекса \\( i \\) и \\( j\\) таких, что \\( |a_i - a_j| \le 1 \\), и удаляете наименьший элемент из этих двух (если они равны то удаляете любой). Определите возможно ли получить массив, состоящий только из одного элемента. (`YES`/`NO`)

Алгоритм
===

1. "На какую тему задача?". Данная задача взята не из конкретной лабы, но вчитаемся чуть внимательнее в условие. 
Учитывая, что мы удаляем из двух выбранных элементов минимальный, то логичным будет утверждение о том, что если ответ `YES`, то наш оставшийся элемент - максимальный.
Так мы пришли к тому, что задача, вероятнее всего, будет связана с сортировкой.
2. Время определить, как именно нам поможет сортировка, и какой алгоритм мы должны выстроить вокруг неё. Для этого достаточно рассмотреть последнее удаление — мы выбрали \\(x\\) и \\(y\\), где \\(y \le x\\). Следовательно максимальный элемент массива \\(a\\) невозможно удалить, а так-же \\(y=x\\) или \\(y=x-1\\). Из всего вышеперечисленного можно сделать вывод, что при оптимальном процессе удаления, числа, которые удаляются, не уменьшаются. Значит решение заключается в том, чтобы отсортировать массив и сравнить на разность соседние элементы.
3. Реализуем поэтапно наше решение, в ходе чего будем анализировать каждый шаг и возможность его оптимизации.

4. Протестируем наше решение готовыми тестами из условия, а также напишем несколько своих тестов, задумавшись о ситуациях, где входные данные могут иметь нерядовой случай, который мы могли не учесть, и который сломает наше решение.

Пункт (4) остаётся на размышление читателям.

Решение формально выглядит так :
1. считать массив
2. отсортировать его
3. сравнить соседние элементы. И если где-то разница элементов больше единицы, то ответ `NO`, иначе `YES`.

Введите код из листинга ниже в файл `main.cpp`.

```cpp
#include <iostream>

using namespace std;

int a[50];

void solve() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    sort(a, a + n);
    for (int i = 0; i + 1 < n; i++) {
        if (a[i + 1] - a[i] > 1) {
            cout << "NO\n";
            return ;
        }
    }
    cout << "YES\n";
}

int main() {
    int t;
    cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}
```

Этот код содержит много информации, поэтому рассмотрим его построчно. 

Для ввода данных и последующей печати ответов нам необходимо подключить библиотеку `iostream`. Для этого напишем `#include <iostream>`, а `using namespace std;` помогает использовать пространство имён. (не берите в голову)

Функция `main` запускается каждый раз при запуске программы. Ещё `main` называют точкой входа в программу. Функция ничего не принимает, поэтому мы написали `()`. Тело любой функции заключается в `{}`. Перед `main` написано `int` &mdash; тип результата функции, возвращаем мы 0 (`return 0`). Именно `0`, так-как это код возврата программы и чтобы не получить `RE` он должен быть `0`.

Как видно `solve` тоже функция, но тип перед ней `void` &mdash; пустой тип, поэтому мы и делаем `return ;`.

Типы данных
===

В `c++`, как и во всех языках программирования много типов данных. `int/float/string` и тп.

В задаче нам нужны типы для длины массива и количество тестов `t`. Для таких переменных подходят числа, а их тип `int`, от слова `Integer`.

Массив чисел создаётся в формате `тип НазваниеПеременной[КоличествоЭлементов]`

Ввод и вывод данных
===

Данные можно вводить и выводить различными способами, но самое простое это использовать стандартные потоки.

Так как ввод посимвольная операция, то `cin >> t;` попробует считывать символы, пока они числа и превратит результат в число типа `int`. (`>>` оператор, который лишь `перегружен` для ввода из потока)

Например, `cout << "YES\n";` &mdash; выведет три символа `Y` `E` `S`, а затем выведет `\n` &mdash; символ новой строки. 

Сортировка и циклы
===

Сортировка это лишь стандартная функция. Для сортировки по не убыванию достаточно передать в качестве аргументов два указателя на память, на начало и конец. Если проще, то просто `a` и `a+n`.

`for` и `while` &mdash; циклы.

Последние штрихи
===

Я написал отдельную функцию `solve` и запустил её `t` раз (только красиво). 

Условие в цикле для сравнения соседних элементов я написал `i + 1 < n`, чтобы не было `RE`, при чтение ненужной памяти. Собственно если разница больше единицы, то я моментально прекращаю работу функции используя `return ;`.

Следовательно если ответ на задачу должен быть `YES`, то после выполнения цикла выведется корректный ответ.

Очень многое опущено &mdash; поэтому углубитесь в язык программирования `c++` и задавайте вопросы)))