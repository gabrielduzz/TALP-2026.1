# Editorial - Aula 04: Ordenação



## [A - Shopaholic](https://codeforces.com/problemset/problem/591/A)
<details> <summary>Dica 1</summary>

Perceba que, de todos os itens que Lindsay quer comprar, o item de 3º maior custo é o item de maior desconto que ela pode obter.

</details><details> <summary>Solução</summary>

A Dica 1 é verdadeira pois não é possível obter desconto nos 2 itens de maior valor da compra, já que não existem itens de maior valor que eles para formarem um trio em que ambos sejam os de menor valor. Logo, a melhor solução é sempre selecionar os 3 itens de maior custo para o caixa por vez, acumulando o custo dos itens descontados.

Para implementar isso, a melhor abordagem é ordenar os itens de forma decrescente, pois assim os trios de maior valor sempre estarão juntos, e o item descontado terá índice `i` tal que `i % 3 == 2`.

Complexidade: `O(n log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n;
  cin >> n;
  vector<int> vetor(n);
  for (int& a : vetor) cin >> a;
  sort(vetor.begin(), vetor.end(), greater<>());
  long long soma = 0;
  for (int i = 2; i < n; i += 3) {
    soma += vetor[i];
  }
  cout << soma << endl;
}
```
</details>

---

## [B - Advantage](https://codeforces.com/problemset/problem/gratitude)
<details> <summary>Dica 1</summary>

Perceba que o melhor participante medirá a sua vantagem em relação ao segundo melhor.

</details><details> <summary>Solução</summary>

Para resolver a questão, deve-se saber primeiramente qual é a força do melhor e do segundo melhor participantes. Para encontrá-los, pode-se ordenar o array de forma decrescente e isolar os valores cujos índices são `0` e `1`.

Com isso, a vantagem de todos os participantes diferentes do maior será medida em relação ao maior, e a vantagem do maior será medida em relação ao segundo maior.

Complexidade: `O(n log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);

  int t;
  cin >> t;
  while (t--) {
    int n;
    cin >> n;
    vector<int> vetor(n);
    for (int& a : vetor) cin >> a;
    vector<int> v = vetor;
    sort(vetor.begin(), vetor.end(), greater<>());
    for (int i = 0; i < n; i++) {
      if (v[i] == vetor[0])
        cout << v[i] - vetor[1] << ' ';
      else
        cout << v[i] - vetor[0] << ' ';
    }
    cout << endl;
  }
}
```
</details>

---

## [C - Polycarp Training](https://codeforces.com/problemset/problem/546/C)
<details> <summary>Dica 1</summary>

Perceba que, ao realizar um contest, Polycarp só resolverá `k` questões e descartará as demais do contest. Porém, `k` sempre aumentará nos dias seguintes, e talvez essas questões descartadas façam falta no futuro. Como é possível então diminuir a quantidade de questões descartadas por contest?

</details><details> <summary>Solução</summary>

A resposta para a Dica 1 é sempre selecionar o contest que possui o menor número possível de questões para realizar no dia `k`.

Exemplo: se Polycarp precisa resolver 2 questões hoje e há um contest de 2 questões e outro de 3 questões, é preferível escolher o de 2, pois, se escolher o de 3, no dia seguinte ele não conseguirá realizar nenhum contest.

Sabendo disso, basta ordenar o vetor de forma crescente e iterar sobre todos os contests desde o início. Se Polycarp não conseguir realizar o contest atual, continue procurando um que tenha questões suficientes. Se conseguir, incremente um contador de dias treinados e o valor de `k`, e continue a iteração.

No final, imprima a quantidade de dias treinados.

Complexidade: `O(n log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n;
  cin >> n;
  vector<int> vetor(n);
  for (int& a : vetor) cin >> a;

  sort(vetor.begin(), vetor.end());
  int contador = 0;
  int k = 1;
  for (int i = 0; i < n; i++) {
    if (vetor[i] >= k) {
      contador++;
      k++;
    }
  }
  cout << contador << endl;
}
```
</details>