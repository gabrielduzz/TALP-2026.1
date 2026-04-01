# Editorial - Aula 03: STL I (Parte 2)



## [A - Grasshopper and the String](https://codeforces.com/problemset/problem/676/B)
<details> <summary>Dica 1</summary>

Tome cuidado com a definição de vogal que a questão informa.

</details><details> <summary>Solução</summary>

A solução da questão é o tamanho da maior sequência de consoantes somado com 1. Para calculá-la, é possível percorrer a string como uma máquina de estado. Se verificar uma consoante, incremente uma variável que guarde o tamanho da sequência atual de consoantes. Se verificar uma vogal, guarde em uma variável de resposta o maior dos valores (o valor que já estava em resposta ou o tamanho da sequência atual de consoantes), e zere o valor da variável com o tamanho da sequência atual.

Complexidade: `O(s.size())`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;
typedef long long ll;

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);

  auto ehVogal = [](char a) {
    return (a == 'A' || a == 'E' || a == 'I' || a == 'O' || a == 'U' ||
            a == 'Y');
  };

  string s;
  cin >> s;
  int n = s.size(), mx = 1, atual = 1;
  for (int i = 0; i < n; i++) {
    if (ehVogal(s[i])) {
      mx = max(mx, atual);
      atual = 1;
      continue;
    }
    atual++;
  }
  cout << max(mx, atual) << endl;
}
```
</details>

---

## [B - Game Outcome](https://codeforces.com/problemset/problem/1033B)
<details> <summary>Solução</summary>

Para essa solução, basta iterar sobre todas as posições da matriz e verificar se ela é um "winning square" ou não (ou seja, para cada termo `a[i][j]`, verificar se o somatório dos termos `a[k][j]` (com `k -> [0, n)`) é maior que o somatório dos termos `a[i][k]` (com `k -> [0, n)`)). Há 2 formas de realizar essa verificação:

**Primeira solução `O(n³)`:** para a verificação do termo `a[i][j]`, deve-se criar uma sub-rotina capaz de somar todos os termos da linha `i` e todos os termos da coluna `j`. Daí, comparam-se os valores retornados e incrementa-se um contador toda vez que o termo for um winning square. Como a sub-rotina possui complexidade `O(n)`, e é necessário percorrer todos os elementos da matriz (`O(n²)`), então a complexidade é `O(n³)`.

**Segunda solução `O(n²)`:** percebe-se que, na solução anterior, cada linha e cada coluna possui o valor da soma total calculada `n` vezes, já que há `n` termos participantes em cada linha/coluna. Porém, como o valor da soma sempre será constante, basta calculá-lo uma única vez no início do processamento e então reutilizá-lo posteriormente.

Complexidade: `O(n²)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n;
  cin >> n;
  int matriz[n][n], somaColunas[n], somaLinhas[n];

  fill(somaColunas, somaColunas + n, 0);
  fill(somaLinhas, somaLinhas + n, 0);

  for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
      cin >> matriz[i][j];
      somaColunas[j] += matriz[i][j];
      somaLinhas[i] += matriz[i][j];
    }
  }

  int contador = 0;
  for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
      if (somaColunas[j] > somaLinhas[i]) contador++;
    }
  }
  cout << contador;
}
```
</details>

---

## [C - Odd Man Out](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1387)
<details> <summary>Dica 1</summary>

Perceba que a quantidade máxima de termos por caso de teste é pequena (`G < 1000`).

</details><details> <summary>Dica 2</summary>

O pior algoritmo para resolver o problema pode ter complexidade temporal `O(n²)`.

</details><details> <summary>Dica 3</summary>

Uma outra forma de ler o problema é: "como eu posso evitar que valores duplicados sejam a saída?"

</details><details> <summary>Solução</summary>

Esse é uma representação do problema clássico do LeetCode ["Single Number"](https://leetcode.com/problems/single-number/description/). Nesse problema, dada uma lista de `n` números, deve-se encontrar qual deles aparece uma única vez.

Aqui será apresentada a solução e o código esperado para a atividade dentro dos conhecimentos adquiridos de STL na última aula (Solução 1). Porém, são apresentadas 3 soluções adicionais mais eficientes para quem deseja se aprofundar. O código apresentado será apenas da Solução 1. Qualquer dúvida da implementação das outras soluções é só entrar em contato.
<details> 
<summary> Solução 1:
</summary>

- Complexidade temporal: `O(n²)`
- Complexidade espacial: `O(n)`

Como dito na Dica 3, uma forma de realizar a questão é evitar que o código tenha como saída um valor duplicado. Portanto, para cada termo `a[i]` (`0 <= i < n`), deve-se verificar se existe algum termo `a[j]` (`0 <= j < n` e `j != i`) que seja igual a `a[i]`. Se sim, basta criar uma flag para marcar que esse valor possui uma dupla e, portanto, não deve ser imprimido. Se não, essa é a resposta.

</details><details> <summary>Código (C++) (Solução 1)</summary>

```cpp
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;
typedef long long ll;

void solve(int caso) {
  int g;
  cin >> g;
  vector<int> convites(g);
  for (int& a : convites) cin >> a;

  for (int i = 0; i < g; i++) {
    bool ehSolitario = true;

    for (int j = 0; j < g; j++) {
      if (i == j) continue;
      if (convites[i] == convites[j]) ehSolitario = false;
    }

    if (ehSolitario) {
      cout << "Case #" << caso << ": " << convites[i] << endl;
      return;
    }
  }
}

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);

  int n;
  cin >> n;
  for (int i = 1; i <= n; i++) solve(i);
}
```
</details>

<details> 
<summary> Solução 2:
</summary>

- Complexidade temporal: `O(n log n)`
- Complexidade espacial: `O(n)`

Nessa solução, deve-se primeiramente ordenar o array, pois assim todos os termos duplicados estarão juntos. Assim, para cada termo `a[i]` cujo `i` é par (`0 <= i < n-1`), deve-se verificar se `a[i+1]` é exatamente igual a `a[i]`. Se não for, então ele é a resposta esperada.
</details>

<details> 
<summary> Solução 3:
</summary>

- Complexidade temporal: `O(n)`
- Complexidade espacial: `O(n)`

Nessa solução, utiliza-se uma hash table. Para cada termo da lista, deve-se verificar se ele está na hash table ou não. Se sim, remove-se o termo da tabela (`O(1)`). Se não, insere-se o termo na tabela (`O(1)`). Observe que, ao final da iteração, o único termo da tabela será o termo esperado (tome um tempo para verificar que isso é verdade).
</details>

<details> 
<summary> Solução 4:
</summary>

- Complexidade temporal: `O(n)`
- Complexidade espacial: `O(1)`

Essa é a solução esperada em uma possível entrevista onde essa questão possa aparecer. Percebe-se que todas as soluções anteriores são possíveis para a questão, porém conhecer novas técnicas sempre será útil para outras questões no futuro.

Para essa solução, utiliza-se a operação lógica XOR (cujo símbolo é `^`), pois ela possui a capacidade de neutralizar termos iguais em sucessivas operações. Isto é, se `A = B`, então `A ^ B = 0`.

**Prova:** na tabela verdade do XOR, quando os valores-verdade são iguais entre si, o resultado do XOR será 0. Logo, considerando que os números são longas sequências de bits, 2 números iguais possuem sequências de bits iguais. Logo, `A ^ A = 0`. (prova finalizada)

Além dessa propriedade, o XOR é comutativo, associativo, e seu elemento neutro é o 0. Logo, pode-se resolver a questão simplesmente realizando o XOR entre todos os termos à medida que forem sendo lidos, sem necessidade de armazenamento. O resultado da expressão será o termo único.

Exemplo: dada a lista `[A, C, D, B, C, D, A]`:
```
A ^ C ^ D ^ B ^ C ^ D ^ A =
A ^ A ^ D ^ D ^ C ^ C ^ B =  (comutatividade)
(A ^ A) ^ (D ^ D) ^ (C ^ C) ^ B =  (associatividade)
(0) ^ (0) ^ (0) ^ B =  (operação com elementos iguais)
B  (operação com elemento neutro)
```
</details>
</details>

---

## [D - Jolly Jumpers](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=459)
<details> <summary>Dica 1</summary>

Perceba que, se existir qualquer diferença entre 2 números sucessivos menor que 1 ou maior que `n-1`, então com certeza a sequência é Not jolly. Porém, essa não é a única forma de a sequência ser Not jolly.

</details><details> <summary>Dica 2</summary>

Perceba que a questão quer que **todos** os valores entre `1` e `n-1` estejam presentes nas diferenças. Porém, existem exatamente `n-1` intervalos dentro da lista. Portanto, pelo princípio da casa dos pombos, não podem haver repetições de termos nessa lista, já que, se houver repetição, algum dos termos entre `1` e `n-1` não estará presente (tome um tempo para perceber que isso é verdade).

</details><details> <summary>Solução</summary>

Como dito nas dicas, há 2 condições para o array de entrada ser Jolly. Suponha que `d[i] = |a[i] - a[i+1]|`, então:
- `1 <= d[i] <= n-1`
- não pode existir nenhum `d[j]` (`0 <= j < n` e `j != i`) onde `d[j] = d[i]`.

Para a primeira condição, basta criar um `if` após calcular o valor de `d[i]`.

Para a segunda condição, percebe-se que o problema é muito semelhante ao da questão anterior, porém com a diferença de que todos os termos `d[i]` devem ser solitários. Para resolver esse problema, pode-se utilizar um vetor de frequência dos termos `d[i]`. Nesse vetor, o termo de índice `i` representa a quantidade de vezes que a diferença de valor `i` aparece na lista de termos de entrada. Essa estratégia é muito utilizada dentro da programação competitiva.

Exemplo 1: para o array de entrada `1 4 2 3`, o vetor de frequência é:
```
valor da diferença: 1 2 3
frequência:         1 1 1
```

Exemplo 2: para o array de entrada `1 4 2 4`, o vetor de frequência é:
```
valor da diferença: 1 2 3
frequência:         0 2 1
```
Isso ocorre porque `d[0] = 3`, `d[1] = 2` e `d[2] = 2`. Perceba que essa sequência é Not jolly.

Sabendo disso, basta montar o vetor de frequência e verificar se todos os termos são iguais a 1. Se sim, é Jolly. Se não, é Not jolly.

Complexidade: `O(n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;
typedef long long ll;

void solve(int n) {
  vector<int> freq(3e5 + 10), valores(n);
  for (int& a : valores) cin >> a;
  for (int i = 1; i < n; i++) {
    freq[abs(valores[i] - valores[i - 1])]++;
  }

  for (int i = 1; i < n; i++) {
    if (freq[i] == 1) continue;
    cout << "Not jolly" << endl;
    return;
  }
  cout << "Jolly" << endl;
}

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);

  int n;
  while (cin >> n) solve(n);
}
```
</details>
