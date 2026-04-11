# Editorial - Atividade 6



## [A - USB Flash Drives](https://vjudge.net/contest/803840#problem/A)
<details> <summary>Dica 1</summary>

Para minimizar o número de pendrives utilizados, deve-se utilizar os que possuem maior capacidade possível.

</details><details> <summary>Solução</summary>

Sabendo a Dica 1, é possível ordenar os pendrives pelo tamanho de forma decrescente. Nesse momento, observa-se o maior de todos (que será o primeiro elemento do array): se for possível armazenar o arquivo nele, então finaliza-se a iteração e a resposta é 1; se não, é necessário utilizar o segundo maior pendrive. Logo, deve-se guardar em uma variável a soma das capacidades de todos os pendrives utilizados no momento, até que essa capacidade seja maior ou igual ao tamanho do arquivo.

Complexidade: `O(n log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main() {
  int n;
  cin >> n;
  int m;
  cin >> m;
  vector<int> v(n);
  for (int& a : v) cin >> a;
  sort(v.begin(), v.end(), greater<int>());

  ll sum = 0;
  for (int i = 0; i < n; i++) {
    sum += v[i];
    if (sum >= m) {
      cout << i + 1 << endl;
      break;
    }
  }
}
```
</details>

---

## [B - Registration System](https://vjudge.net/contest/803840#problem/B)
<details> <summary>Dica 1</summary>

Perceba que não é necessário armazenar a variação que o sistema gera para cada nome que aparece novamente na entrada.

</details><details> <summary>Solução</summary>

A Dica 1 é verdadeira pois, para cada nome que se repete na entrada, a nova variação criada possui o nome concatenado com a frequência em que ele aparece no banco de dados. Logo, basta criar um map de frequência e, para cada string de entrada, verificar se sua frequência é 0: se sim, retorna-se `OK` e incrementa-se o valor de sua frequência no map. Se não, concatena-se o nome com sua frequência e também incrementa-se a sua frequência.

Complexidade: `O(n log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n;
  cin >> n;
  map<string, int> m;
  while (n--) {
    string s;
    cin >> s;
    if (!m.count(s)) {
      cout << "OK" << endl;
    } else {
      cout << s << m[s] << endl;
    }
    m[s]++;
  }
}
```
</details>

---

## [C - Apaxiaaaaaaaaaaaans!](https://vjudge.net/contest/803840#problem/C)
<details> <summary>Dica 1</summary>

Crie uma nova string e construa a versão compacta sobre ela, pois a solução é um algoritmo construtivo.

</details><details> <summary>Solução</summary>

Para a solução, basta iterar sobre todos os caracteres da string de entrada e ir construindo uma nova string de resposta ao mesmo tempo. Portanto, para adicionar um novo caractere à nova string, deve-se verificar se ele já não foi adicionado anteriormente, ou seja, se o último caractere adicionado é igual ao atual. Se sim, basta ignorá-lo.

Complexidade: `O(n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  string s;
  cin >> s;
  string r = "";
  for (char a : s) {
    if (a == *r.rbegin()) continue;
    r += a;
  }
  cout << r << endl;
}
```
</details>

---

## [D - Judging Troubles](https://open.kattis.com/problems/judging)
<details> <summary>Dica 1</summary>

Como há 2 listas de judging results de entrada, perceba que talvez seja necessário utilizar 2 estruturas de dados para lidar com ambas.

</details><details> <summary>Dica 2</summary>

Para facilitar a questão, utilizam-se 2 maps de frequência para contar quantas vezes cada judging result aparece em cada lista separadamente.

</details><details> <summary>Solução</summary>

Como é necessário saber quantos judging results são iguais entre cada juiz, basta contar quantas vezes cada um aparece em cada juiz e, então, para cada tipo de result, verificar quantos pares é possível formar entre os 2 juízes. Perceba que esse resultado será a menor quantidade entre ambos, pois a relação de formar pares é de 1 para 1.

Exemplo: se a palavra `"correct"` aparece 5 vezes no primeiro juiz e 2 no segundo, então só é possível formar 2 pares. Logo, o judging result `"correct"` é igual no máximo 2 vezes.

Basta repetir essa mesma lógica para todos os judging results que aparecerem e somar todos os resultados para encontrar a resposta.

Complexidade: `O(n log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main() {
  int n;
  cin >> n;
  map<string, int> m1, m2;
  for (int i = 0; i < n; i++) {
    string a;
    cin >> a;
    m1[a]++;
  }
  for (int i = 0; i < n; i++) {
    string a;
    cin >> a;
    m2[a]++;
  }
  ll ans = 0;
  for (auto [result, freq] : m1) {
    ans += min(m2[result], freq);
  }
  cout << ans << endl;
}
```
</details>