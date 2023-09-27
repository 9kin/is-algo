# Динамические массивы

Массивы бывают двух типов: [статические](./c-array.md) и [динамические](dynamic-array.md). Давайте, рассмотрим динамические.

> Главная особенность динамических массивов - изменяемый размер. Размер динамического массива может быть любим неотрицательным числом. 

Например, вам надо создать массив на `n` элементов (`n` вы ввели).

Для этого можно воспользоваться `new[]`.

```cpp
int* foo = new int[10];
```

> `new T` возвращает указатель на начало переменной памяти типа `T *`. 

Далее, мы используем массив так-же как и статический массив, но после работы нам стоит очистить выделенную `new` память, чтобы не получать `ML`.

> Чтобы очистить массив после использования надо выполнить `delete []foo`.

После прочтения главы, рекомендую прочитать про [устройство памяти](./memory.md).