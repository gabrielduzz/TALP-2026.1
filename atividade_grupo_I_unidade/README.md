# Editorial - Atividade em grupo - I Unidade



## [A - Cards](https://codeforces.com/problemset/problem/1220/A)
<details> <summary>Dica 1</summary>

Sabendo que sempre é possível reorganizar as letras de forma que formem a sequência de números, quais são as letras que só aparecem na palavra "zero", e quais só na palavra "one"?

</details><details> <summary>Solução</summary>

Quando houver um `z` na lista de letras informada, haverá um `0`; quando houver um `n`, haverá um `1`. Portanto, basta iterar sobre a string, verificar se há essas letras, e adicionar os números correspondentes em um vetor, que deve ser ordenado em ordem crescente.

Complexidade: `O(n log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;

int main() {
  int n;
  cin >> n;
  string s;
  cin >> s;
  vector<int> ans;
  for (char a : s) {
    if (a == 'z') ans.push_back(0);
    if (a == 'n') ans.push_back(1);
  }
  sort(ans.begin(), ans.end(), greater<int>());

  for (int a : ans) cout << a << ' ';
  cout << endl;
}
```
</details>

---

## [B - Little Artem](https://codeforces.com/problemset/problem/1333/A)
<details> <summary>Dica 1</summary>

Uma matriz 2×2 deve conter exatamente quantos quadrados brancos?

</details><details> <summary>Dica 2</summary>

Em uma matriz 2×2, a única forma possível é colocar 1 quadrado branco e os demais pretos, pois assim `B = 2` e `W = 1`. Agora, em uma matriz 2×3 ou 3×2, é possível manter esses mesmos valores de `B` e `W`?

</details><details> <summary>Solução</summary>

Para matrizes maiores que 2×2, a solução é colocar 1 quadrado branco em algum dos cantos da matriz e preencher todos os outros com pretos. Assim, os quadrados pretos não terão como vizinhos outros quadrados brancos, garantindo que `B` seja sempre 2 e `W` seja sempre 1.

Complexidade: `O(n·m)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;

int main() {
  int t;
  cin >> t;
  int n, m;
  while (t--) {
    cin >> n >> m;
    string a(m - 1, 'B');
    cout << "W" + a << endl;
    for (int i = 1; i < n; i++) {
      cout << string(m, 'B') << endl;
    }
  }
  return 0;
}
```
</details>

---

## [C - Sorted Adjacent Differences](https://codeforces.com/problemset/problem/1339/B)
<details> <summary>Dica 1</summary>

Qual é o par de elementos que possui a maior diferença absoluta possível dentro do array?

</details><details> <summary>Dica 2</summary>

A resposta da Dica 1 é o par formado pelo maior e pelo menor elemento. Sabendo disso, é possível torná-los os últimos elementos do array em construção. Porém, qual será o par de elementos escolhidos para precedê-los?

</details><details> <summary>Solução</summary>

A resposta da Dica 2 é utilizar o par formado pelo segundo maior e o segundo menor elemento, e assim sucessivamente até finalizar o array em construção. Observe que o maior elemento de um par deve ser vizinho do menor elemento do par adjacente. Para vetores de tamanho ímpar, após a inserção dos pares pela direita, o elemento inicial deve ser a mediana do array.

**Prova:** seja `mn` o menor elemento do array, `mx` o maior, `x = |mx - mn|`, e `aᵢ` o elemento imediatamente vizinho pela esquerda desse par. Escolhendo `mx` como o último elemento do array e `mn` como o penúltimo, percebe-se que, independentemente de qual valor seja `aᵢ`, a diferença entre `aᵢ` e `mn` será sempre menor ou igual a `x`, pois:
```
mx >= aᵢ
mx - mn >= aᵢ - mn
```
Sabendo disso, `aᵢ` deve ser o elemento que forme a maior diferença possível com `mn` dentre os elementos restantes para inserção, pois está garantido que `|ai - mn|`sempre será menor ou igual à diferença dos elementos já inseridos. Dado o caso base, o passo indutivo é verdadeiro sabendo que qualquer diferença de adjacentes já existente no array é maior que qualquer outra possível de ser formada. (fim da prova)

Uma forma de implementar isso é criar 2 arrays com os elementos de entrada, ordenar um de forma crescente e outro de forma decrescente, e inserir os elementos no array final de forma alternada: primeiro do array em ordem decrescente (cujo primeiro elemento será o maior do array), depois do array em ordem crescente (cujo primeiro elemento será o menor do array). Quando `n` elementos forem inseridos, deve-se inverter o array de resposta.

Complexidade: `O(n log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int t;
  cin >> t;
  while (t--) {
    int n;
    cin >> n;
    vector<int> mx(n), mn(n);
    for (int& a : mx) cin >> a;
    mn = mx;
    sort(mx.begin(), mx.end(), greater<int>());
    sort(mn.begin(), mn.end());
    vector<int> ans;
    int id = 0;
    for (int i = 0; i < n; i++) {
      if (i % 2 == 0) {
        ans.push_back(mx[id]);
      } else {
        ans.push_back(mn[id]);
        id++;
      }
    }
    reverse(ans.begin(), ans.end());
    for (int a : ans) cout << a << ' ';
    cout << endl;
  }
}
```
</details>

---

## [D - Diversity of Scores](https://vjudge.net/problem/AtCoder-abc343_d/origin)
<details> <summary>Dica 1</summary>

Perceba que calcular quantas pontuações distintas existem entre os jogadores deve ser uma operação de complexidade `O(log n)` ou melhor.

</details><details> <summary>Dica 2</summary>

Perceba que apenas guardar na memória a contagem de pontuações distintas não é suficiente, pois, a cada soma, uma pontuação é alterada de um valor para outro. Então, qual estrutura utilizar para mapear os valores?

</details><details> <summary>Solução</summary>

A resposta para a Dica 2 é utilizar um `map` de frequência, onde a chave representa a pontuação e o valor representa a sua frequência. A quantidade de pontuações distintas existentes após cada soma será a quantidade de chaves no map. Aliado ao mapeamento das pontuações, deve-se implementar uma estrutura que mantenha a pontuação atual de todos os jogadores, como um `vector`.

Obs.: perceba que utilizar apenas um `set` não é possível, pois, apesar de ele guardar apenas os valores distintos, não será possível realizar deleções corretamente quando uma pontuação é alterada para outra --- já que os valores das pontuações podem se repetir.

Para a implementação, deve-se inicializar o `vector` com `n` posições de valor `0`, e o `map` com a chave `0` igual a `n` (pois todos estão com pontuação `0`). Após uma soma de valor `x` ao competidor `i`, devem ser realizadas as seguintes operações:

- **Map:** verificar a pontuação atual do competidor `i` no `vector` e decrementar sua frequência, pois ela será alterada. Se a frequência chegar a `0`, a chave deve ser deletada. Em seguida, calcular a nova pontuação e incrementar sua frequência.
- **Vector:** incrementar a pontuação do jogador `i` com `x` logo após a operação com o map

Após cada operação, basta imprimir a quantidade de chaves existentes no `map`.

Complexidade: `O(n log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;

int main() {
  int n, t;
  cin >> n >> t;
  vector<ll> v(n, 0);
  map<ll, ll> m;
  m[0] += n;
  for (int i = 0; i < t; i++) {
    ll id;
    cin >> id;
    id--;
    ll sum;
    cin >> sum;

    ll prev = v[id];

    m[prev]--;
    if (m[prev] == 0) m.erase(prev);

    v[id] += sum;
    m[v[id]]++;

    cout << m.size() << endl;
  }
}
```
</details>
