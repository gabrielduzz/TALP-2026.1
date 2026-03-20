# Editorial - Aula 02: Complexidade

---

## [A - Next Round](https://codeforces.com/problemset/problem/158/A)

<details>
<summary>Solução</summary>

Para a solução dessa questão, não é necessário utilizar array. Portanto, possui complexidade espacial de $O(1)$

Como a pontuação dos participantes já esta ordenada, então todos os primeiros $k$ elementos diferentes de 0 devem ser contados. A partir dos próximos $n-k$ elementos, deve-se observar se eles são pelo menos iguais ao valor de $k$. Portanto, cria-se uma variável de referência, que será atualizada quando o valor de k for inserido.

Complexidade temporal: $O(n)$

</details>

<details>
<summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
 
const int INF = INT_MAX;
 
int main () { 
    int k, n; cin >> n >> k;
    k--;
    int contador = 0;
    int referencia = -INF;
    for (int i = 0; i < n; i++){
        int nota; cin >> nota;
        if (i == k) referencia = nota;
        if (nota >= referencia && nota != 0) contador++;
    }
    
    cout << contador;
}
```
</details>

---
[B - Divisibility Problem](https://codeforces.com/contest/1328/problem/A)
--------------------------------------------------------------------------

<details>

<summary>Dica 1</summary>

O valor de $a$ pode ser grande o suficiente para ultrapassar a capacidade de uma variável do tipo int 

</details>

<details>

<summary>Dica 2</summary>

Por que uma solução ingênua de simulação (incrementar $a$ até que seja divisível por $b$) não é viável?
Qual é a complexidade máxima que a solução pode ter para passar no tempo limite?

</details>

<details>

<summary>Solução</summary>

Uma solução ingênua seria somar $1$ a $a$ até que $a$ seja divisível por $b$. No pior caso, $a = 1$ e $b = 10^9$, seriam necessárias cerca de $10^9$ operações — o que é inviável.

Percebemos então que precisamos de um algoritmo com complexidade melhor, no máximo $O(\log b \cdot t)$ ou, idealmente, $O(t)$.

Matematicamente, a resposta é igual à distância de $a$ até o próximo múltiplo de $b$.

Se $a$ já é múltiplo de $b$, a resposta é $0$.

Caso contrário, a resposta é $b - (a \bmod b)$, pois $a \bmod b$ é o resto da divisão, ou seja, a distância até o múltiplo anterior, e $b$ é a distância entre dois múltiplos consecutivos.

Complexidade: $O(t)$, sendo $O(1)$ para cada caso teste.

Nota: usar *fast/io* acelera a entrada e saída, criando um buffer que só é descarregado ao final do programa — isso facilita o debug e melhora a eficiência.
</details>

<details>

<summary>Código (C++)</summary>


``` cpp
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;
 
int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);
 
  int t;
  cin >> t;
  while (t--) {
    long long a, b;
    cin >> a >> b;
    if (a % b == 0) {
      cout << 0 << endl;
      continue;
    }
 
    int resposta = b - a % b;
    cout << resposta << endl;
  }
}
```

</details>

* * * * *

[C - Again Twenty Five!](https://codeforces.com/problemset/problem/630/A)
-----------------------------------------------------------
<details> 

<summary>Dica 1</summary>

Por que uma solução ingênua que calcula $5^n$ diretamente não funciona?

</details>

<details>

<summary>Dica 2</summary>

Observe que o problema pede apenas os dois últimos dígitos de $5^n$.
Talvez haja um padrão ou uma propriedade matemática que simplifique o cálculo.

</details><details> <summary>Solução</summary>

Uma tentativa ingênua de calcular $5^n$ diretamente e depois pegar os dois últimos dígitos falharia devido à **integer overflow** ($5^n$ cresce muito rápido, ultrapassando rapidamente a capacidade de qualquer tipo inteiro padrão); e **time limit exceeded** (para o pior caso ($n = 2 \cdot 10^{18}$) , multiplicar $5$ $n$ vezes ( $O(n)$ ) é inviável) 

Porém, percebe-se que, para qualquer $n \ge 2$ (que é o limite minimo de $n$ para a questão), $5^n$ sempre termina com $25$.
Isso acontece porque $5^2 = 25$, e qualquer multiplicação subsequente por $5$ mantém os dois últimos dígitos como $25$.

[Prova matemática aqui](https://codeforces.com/blog/entry/24160)

Complexidade: $O(1)$


</details>

<details>

<summary>Código (C++)</summary>


``` cpp
#include <bits/stdc++.h>
using namespace std;
 
typedef long long ll;
 
int main() {
  ll n;
  cin >> n;
  cout << 25 << endl;
}
```

</details>
