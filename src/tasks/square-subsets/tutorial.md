В своё время мне понравилась эта задача.

Сделаем подсчёт каждого значения. (на это указывает ограничение в условие $a_i \le 70$).

Число $X=prime_i ^ {pow_i}$ является квадратом, если каждое $prime_i$ встречается чётное количество раз в $X$. ($pow_i \vdots 2$). 

Количество простых чисел в диапазоне $[2, 70]$ всего $\red{19}$. Значит где-то тут будут маски!!

Пронумеруем простые числа в диапазоне $[2, 70]$ числа $[2; 3; 5; \dots]$

Напишем $dp$ по маскам.

1. Состояние $dp[i][mask]$ &mdash; количество способов выбрать из массива значения $\le i$ и собрать $mask$.
Где $mask_j = 1$ &mdash; если простое $j$-ое простое число встречается нечётное количество раз в $mask$. $0$ если чётное, $0$ в противном случае.
2. $dp[0][0] = 1$ начальное значение.
3. Переход :

$NCH$ -- количество способов выбрать нечётное количество элементов из $cnt[i + 1]$

$CH$ -- аналогично чётное.

> Количество способов выбрать чётное количество элементов из множества $A$, равняется $2 ^{n - 1}$, для нечётных тоже самое.

1. $dp[i + 1][cur_{mask} \oplus nxt_{mask}] += dp[i][cur_{mask}] \times NCH$
2. $dp[i + 1][cur_{mask}] += dp[i][cur_{mask}] \times CH$

В переходе $2$ если мы берём любое чётное количество раз, мы не изменим $mask$.

В переходе $1$ логично изменим степени простых, которые пересекаются с маской от числа $(i + 1)$ на противоположные. 
4. Ответ &mdash; $dp[70][0] - 1$. ($-1$ так как мы пустое множество по условию не считаем, вроде?, мне уже лень думать)

```cpp
int P[71];
int primeOrder[71];

int get_mask(int x) {
  if (x == P[x]) return (1 << primeOrder[x]);
  int mask = 0;
  while (x != 1) {
    int pr = P[x];
    int p = 0;
    while (x % pr == 0) {
      p++;
      x /= pr;
    }
    if (p % 2 == 1) {
      mask += (1 << primeOrder[pr]);
    }
  }
  return mask;
}

void solve() {
  int n;
  cin >> n;
  for (int i = 0; i < n; i++) {
    int x;
    cin >> x;
    cnt[x]++;
  }

  for (int i = 2; i <= 70; i++) {
    if (!P[i]) {
      for (int j = 2 * i; j <= 70; j += i) {
        P[j] = i;
      }
      P[i] = i;
    }
  }

  int cur = 0;
  for (int i = 2; i <= 70; i++) {
    if (P[i] == i) {
      primeOrder[i] = cur++;
    }
  }
  dp[0][0] = 1;
  for (int i = 0; i < 70; i++) {
    int CH = ch_comb(cnt[i + 1]);
    int NCH = nch_comb(cnt[i + 1]);
    int nxt_mask = get_mask(i + 1);
    for (int cur_mask = 0; cur_mask < (1 << cur); cur_mask++) {
      dp[i + 1][cur_mask ^ nxt_mask] =
          (dp[i + 1][cur_mask ^ nxt_mask] + 1LL * dp[i][cur_mask] * NCH) % mod;
      dp[i + 1][cur_mask] =
          (dp[i + 1][cur_mask] + 1LL * dp[i][cur_mask] * CH) % mod;
    }
  }
  cout << (dp[70][0] - 1 + mod) % mod;
}
```