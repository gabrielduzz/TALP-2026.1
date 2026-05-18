# Editorial - Atividade 13



## [A - Counting Haybales](https://usaco.org/index.php?page=viewproblem2&cpid=666&lang=en)
<details> <summary>Solução</summary>

Uma solução ingênua seria verificar, para cada feno, se sua posição está entre `A` e `B`, incrementando um contador. Porém, esse algoritmo possui complexidade linear por consulta e `O(n·q)` por teste, o que não é suficiente dados os valores de entrada.

Para otimizar, percebe-se que, ao ordenar o vetor de fenos, todos os fenos dentro do intervalo `[A, B]` estarão contíguos. Assim, sua quantidade será a distância entre o `lower_bound` de `A` e o `upper_bound` de `B` no vetor ordenado. Como cada busca possui complexidade logarítmica, a complexidade total por teste será `O(q·log n)`.

Complexidade: `O(q·log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);

  freopen("haybales.in", "r", stdin);
  freopen("haybales.out", "w", stdout);

  int n, q;
  cin >> n >> q;
  vector<int> v(n);
  for (int& a : v) cin >> a;

  sort(v.begin(), v.end());

  while (q--) {
    int a, b;
    cin >> a >> b;

    auto l = lower_bound(v.begin(), v.end(), a);
    auto r = upper_bound(v.begin(), v.end(), b);

    cout << r - l << endl;
  }
}
```
</details>

---

## [B - Concert Tickets](https://cses.fi/problemset/task/1091/)
<details> <summary>Dica 1</summary>

Perceba que quando um ingresso é comprado, ele não pode ser comprado novamente. Qual é a melhor estrutura de dados para armazenar os ingressos disponíveis, buscá-los e então removê-los em tempo de execução?

</details><details> <summary>Solução</summary>

A resposta para a Dica 1 é o `multiset`, pois é uma estrutura que permite a busca e a remoção dos ingressos em `O(log n)`.

Utilizando o `multiset`, deve-se buscar, para cada comprador, qual é o ingresso de maior preço disponível que seja menor ou igual ao preço máximo `x` que ele está disposto a pagar. Para isso, pode-se utilizar o `upper_bound` de `x` no `multiset`. Se todos os ingressos forem maiores que `x`, o `upper_bound` retornará o iterador `begin()` do `multiset` e deve-se retornar `-1`. Caso contrário, deve-se decrementar o iterador retornado pelo `upper_bound`, imprimir o elemento e então removê-lo do `multiset`.

Complexidade: `O(m·log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);

  int n, m;
  cin >> n >> m;

  multiset<int> multi;
  for (int i = 0; i < n; i++) {
    int a;
    cin >> a;
    multi.insert(a);
  }

  while (m--) {
    int a;
    cin >> a;
    auto it = multi.upper_bound(a);
    if (it == multi.begin())
      cout << -1 << endl;
    else {
      it--;
      cout << *it << endl;
      multi.erase(it);
    }
  }
}
```
</details>

---

## [C - Towers](https://cses.fi/problemset/task/1073/)
<details> <summary>Dica 1</summary>

Perceba que, ao processar um cubo, a estratégia gulosa mais eficiente é tentar colocá-lo em uma torre já existente. Caso haja múltiplas opções, deve-se escolher a torre cujo topo possui o menor tamanho possível (ainda maior que o cubo atual). Isso porque uma torre de topo maior possui mais possibilidades de cubos que podem ser inseridos no futuro e, portanto, deve ser preservada.

</details><details> <summary>Dica 2</summary>

A única informação necessária manter por torre é o cubo que está em seu topo. Qual é a melhor estrutura para representá-las?

</details><details> <summary>Solução</summary>

A melhor estrutura para representar as torres é o `multiset`, pois, para cada inserção de cubo, deve-se substituir o topo da torre correspondente --- o que implica muitas remoções e inserções durante a execução.

Para cada cubo `x`, deve-se procurar o menor topo de torre que seja estritamente maior que `x`. Para isso, utiliza-se o `upper_bound` de `x` no `multiset`. Caso não exista nenhum topo maior que `x`, o `upper_bound` retornará o iterador `end()` e deve-se criar uma nova torre, inserindo `x` no `multiset`. Caso contrário, remove-se o valor retornado pelo `upper_bound` e insere-se `x` em seu lugar, representando a colocação do cubo no topo da torre.

Ao final, o tamanho do `multiset` é a resposta.

Complexidade: `O(n·log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);

  int n;
  cin >> n;
  vector<int> v(n);
  for (int& a : v) cin >> a;

  multiset<int> m;

  for (int a : v) {
    auto it = m.upper_bound(a);
    if (it == m.end()) {
      m.insert(a);
    } else {
      m.erase(it);
      m.insert(a);
    }
  }
  cout << m.size() << endl;
}
```
</details>
