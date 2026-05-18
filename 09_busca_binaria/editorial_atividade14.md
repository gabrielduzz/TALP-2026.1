# Editorial - Atividade 14



## [A - Factory Machines](https://cses.fi/problemset/task/1620/)
<details> <summary>Dica 1</summary>

Como calcular quantos produtos são produzidos após `m` segundos?

</details><details> <summary>Dica 2</summary>

Se em `m` segundos for suficiente para produzir `t` produtos, é necessário testar `m+1` segundos? E se `m` segundos não forem suficientes, `m-1` segundos serão?

</details><details> <summary>Solução</summary>

Como as respostas para a Dica 2 são verdadeiras, a função de teste para verificar se `m` segundos são suficientes é monotônica. Portanto, pode-se aplicar busca binária sobre o intervalo de possíveis respostas.

O menor tempo possível para produzir `t` produtos é `1`, enquanto o maior possível é o tempo que a máquina mais rápida demoraria para produzir todos os produtos sozinha (`mn·t`, sendo `mn` o tempo de produção dessa máquina). Para minimizar o tempo, aplica-se busca binária de minimização: diminui-se o ponteiro `r` se `m` for suficiente, e aumenta-se `l` caso contrário.

Para o teste, calcula-se quantos produtos cada máquina `i` produz em `m` segundos como `⌊m / xᵢ⌋` e soma-se todas as produções.

Complexidade: `O(n·log(mn·t))`, sendo `mn` o tempo de produção da máquina mais rápida.

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

ll n, t;
const int MAX = 2e5;
vector<ll> v(MAX);

bool f(ll m) {
  ll products = 0;
  for (int i = 0; i < n; i++) {
    products += m / v[i];
  }
  return products >= t;
}

int main() {
  cin >> n >> t;

  for (int i = 0; i < n; i++) cin >> v[i];

  ll l = 1, r = *min_element(v.begin(), v.begin() + n) * t;
  ll ans = r;

  while (l <= r) {
    ll m = l + (r - l) / 2;
    if (f(m)) {
      ans = m;
      r = m - 1;
    } else {
      l = m + 1;
    }
  }
  cout << ans << endl;
}
```
</details>

---

## [B - Building an Aquarium](https://codeforces.com/contest/1873/problem/E)
<details> <summary>Dica 1</summary>

Dada uma altura `h`, como calcular a quantidade de unidades de água no tanque?

</details><details> <summary>Dica 2</summary>

Para calcular quantas unidades de água haverá no tanque ao enchê-lo com altura `h`, deve-se calcular a contribuição de cada coluna `i` como `max(0, h - hᵢ)`, onde `hᵢ` é a altura da coluna `i`, e somar todas as contribuições.

</details><details> <summary>Dica 3</summary>

Se encher o tanque com altura `h` ultrapassa `w` unidades de água, encher com `h+1` também irá ultrapassar? E se a altura `h` preenche o tanque com `w` ou menos unidades, `h-1` também preencherá?

</details><details> <summary>Solução</summary>

Como as respostas para a Dica 3 são verdadeiras, a função de teste descrita na Dica 2 é monotônica. Portanto, pode-se aplicar busca binária sobre o intervalo de possíveis alturas.

A menor altura possível é `1` (conforme descrito pelo output da questão). Para o limite superior, observa-se que, se não houvesse nenhuma coluna no aquário, a altura máxima seria `x/n`, onde `x` é a quantidade máxima de unidades de água permitida e `n` é a quantidade de colunas. Como cada coluna possui uma altura `hᵢ` que reduz o volume de água, a altura máxima possível pode ser maior. No pior caso, todas as colunas possuem a mesma altura, então `r = x/n + mx`, onde `mx` é a maior coluna dentre as existentes.

Para maximizar a altura, aplica-se busca binária de maximização: aumenta-se o ponteiro `l` se `m` for possível, e diminui-se `r` caso contrário.

Complexidade: `O(n·log(x/n + mx))`, onde `mx` é a coluna de maior tamanho da entrada.

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int n, x;
const int MAX = 2e5;
vector<int> v(MAX);

bool f(ll m) {
  ll water = 0;
  for (int i = 0; i < n; i++) {
    water += max(0ll, m - v[i]);
  }
  return water <= x;
}

void solve() {
  cin >> n >> x;
  for (int i = 0; i < n; i++) cin >> v[i];

  ll l = 1, r = *max_element(v.begin(), v.begin() + n) + (x + n - 1) / n;
  ll ans = 1;
  while (l <= r) {
    ll m = l + (r - l) / 2;
    if (f(m)) {
      ans = m;
      l = m + 1;
    } else {
      r = m - 1;
    }
  }
  cout << ans << endl;
}

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);
  int t;
  cin >> t;
  while (t--) solve();
}
```
</details>

---

## [C - Array Division](https://cses.fi/problemset/task/1085/)
<details> <summary>Dica 1</summary>

Para testar se é possível dividir o array em `k` partes cuja soma máxima entre elas seja `m`, deve-se agrupar os elementos de forma gulosa: enquanto uma parte tiver soma menor ou igual a `m`, tenta-se adicionar mais elementos a ela. Caso contrário, inicia-se um novo grupo. Ao final, haverá `x` grupos. Se `x ≤ k`, então é possível dividir em `k` partes, pois basta separar elementos de grupos existentes para criar novos até atingir `k`.

</details><details> <summary>Dica 2</summary>

Para implementar o teste, itera-se sobre os elementos do array mantendo uma variável `curr` (soma do subarray atual) e uma variável `qtd` (quantidade de subarrays formados). Na `i`-ésima iteração, verifica-se se `curr + v[i]` supera `m`. Se não supera, adiciona-se `v[i]` ao subarray atual. Se supera, `v[i]` inicia um novo subarray (`curr = v[i]`) e `qtd` é incrementado.

</details><details> <summary>Dica 3</summary>

Se não for possível dividir o array de forma que a soma máxima seja `m`, também não será possível para valores menores. Além disso, se há uma divisão em `k` partes onde a soma máxima é no máximo `m`, então também é possível para `m+1`.

</details><details> <summary>Solução</summary>

Pela Dica 3, a função de teste descrita na Dica 1 é monotônica. Portanto, pode-se aplicar busca binária na resposta.

A menor soma máxima possível ao dividir o array em subarrays é obtida colocando cada elemento em um subarray distinto --- o maior deles será o elemento de maior valor do array. A maior soma possível é obtida colocando todos os elementos em um único subarray, ou seja, a soma de todos os elementos.

Para minimizar a soma máxima, aplica-se busca binária de minimização: diminui-se o ponteiro `r` se a soma `m` for possível pelo teste da Dica 2, e aumenta-se `l` caso contrário.

Complexidade: `O(n·log M)`, onde `M` é a soma de todos os elementos do array.

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int MAX = 2e5;
int n, k;
vector<ll> v(MAX);

bool f(ll m) {
  ll curr = v[0], qtd = 1;
  for (int i = 1; i < n; i++) {
    if (curr + v[i] > m) {
      curr = 0;
      qtd++;
    }
    curr += v[i];
  }
  return qtd <= k;
}

int main() {
  cin >> n >> k;
  for (int i = 0; i < n; i++) cin >> v[i];

  ll l = *max_element(v.begin(), v.begin() + n),
     r = reduce(v.begin(), v.begin() + n);

  ll ans = r;
  while (l <= r) {
    ll m = l + (r - l) / 2;
    if (f(m)) {
      ans = m;
      r = m - 1;
    } else {
      l = m + 1;
    }
  }
  cout << ans << endl;
}
```
</details>
