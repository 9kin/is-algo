 Решение 2 указателя. Будем передвигать вторую границу, пока условие выполняется. 


https://atcoder.jp/contests/abc129/tasks/abc129_e

$a + b \le L; a \oplus b = a + b$ $L \le 2^{100 \\ 001}$

[//]: <> (https://codeforces.com/blog/entry/67558?#comment-517438)

Ограничения на длину строки и отет по модулю, наталкивает на dp по цифрам.
То, что $L$ дана в виде бинарной строке, позволяет нам думать над решением побитово.

$a \oplus b = a + b$ записывается как $a \& b = 0$ Или просто расмотрим все пары битом $(0, 0), (0, 1), (1, 0), (1, 1)$ из это всего явно не подходит пара $(1, 1)$.


$dp1$ - количетво пар $(a, b)$, таких что $a + b < L; a \& b = 0$ 

$dp2$ - количетво пар $(a, b)$, таких что $a + b = L; a \& b = 0$

Проходясь от самоего левого бита (начало строки) до самомго последнего.

!! Под $L$ будет пониматься, число собраное на префиксе от левого до текущего $i$-того бита из $L$.


1. $s_i = 0$ 

для $dp_2$ $i$-тый бит можно собрать только, если ко всем последовательностям до $i$ мы припишем всего $(0, 0)$, значит $dp2$ не как не изменится

для $dp_1$ $i$-тый бит можно собрать любыми $3$ комбинациями битов. $dp_1 = 3 \times dp_1$

2. $s_i = 0$

для $dp_2$ $i$-тый бит можно собрать только, если ко всем последовательностям до $i$ мы припишем всего $(0, 1); (1, 0)$. $dp_2 = 2 \times dp_2$

для $dp_1$ $i$-тый бит можно собрать любыми $3$ комбинациями из $dp_1$, а также $(0, 1); (1, 0)$ приписываея к $dp_2$, так как сумма новых будет строго меньше $L$. $dp_1 = 3 \times dp_1 + dp_2$

```cpp
void solve() {
  string s;
  cin >> s;
  int dp1 = 0;
  int dp2 = 1;
  for (int i = 0; i < s.size(); i++) {
    if (s[i] == '0') {
      dp1 = mult(3, dp1);
      // dp2 = mult(1, dp2)
    } else {
      dp1 = add(dp2, mult(3, dp1));
      dp2 = mult(2, dp2);
    }
  }
  cout << add(dp1, dp2);
}
```