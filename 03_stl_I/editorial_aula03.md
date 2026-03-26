# Editorial - Aula 03: STL I



## [A - String Task](https://codeforces.com/problemset/problem/118/A)

<details>
<summary>Dica 1</summary>

Cuidado com a definição de vogais que a questão cria.

</details>

<details>
<summary>Solução</summary>

A resolução da questão é construir exatamente o que está sendo pedido.

Para isso, pode-se criar uma string auxiliar, que será construída com base nos requisitos pedidos. Abaixo está uma das várias formas de se realizar cada um:

- **"deletes all the vowels"**
  - Durante a iteração dos caracteres da string de entrada, basta ignorar as vogais a partir de um `if`.

- **"inserts a character "." before each consonant"**
  - Quando não for vogal, basta concatenar na string auxiliar a string `"."` antes da consoante lida.

- **"replaces all uppercase consonants with corresponding lowercase ones"**
  - Pode-se utilizar manipulação de caracteres ASCII, onde cada caractere da string que não está no intervalo de `['a', 'z']` deve ser subtraído por 32.
  - Ou pode-se utilizar a função `tolower(c)`, onde `c` é o caractere sendo lido.

Complexidade: `O(s.length())`

</details>

<details>
<summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);

  auto ehVogal = [](char a) {
    return a == 'a' || a == 'o' || a == 'y' || a == 'e' || a == 'u' || a == 'i';
  };

  string s;
  cin >> s;

  string resposta = "";
  for (char& c : s) {
    c = tolower(c);
    if (!ehVogal(c)) {
      resposta += ".";
      resposta += c;
    }
  }

  cout << resposta << endl;
}
```
</details>

---

## [B - Three Strings](https://codeforces.com/problemset/problem/1301/A)
<details> <summary>Dica 1</summary>

Observe que todos os caracteres da string `c` serão incorporados ou pela string `a` ou pela string `b`, pois as trocas são obrigatórias.

</details><details> <summary>Solução</summary>

Levando em consideração a dica 1, percebe-se que `a[i]` e `b[i]` só serão iguais se `c[i]` possuir um caractere igual a algum dos 2. Levando isso em consideração, basta iterar por todos os caracteres (`1 <= i <= n`) e verificar se a condição é verdadeira.

Complexidade: `O(s.length())`

</details><details> <summary>Código (C++)</summary>


```cpp
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;

void solve() {
  string a, b, c;
  cin >> a >> b >> c;
  int n = a.length();

  for (int i = 0; i < n; i++) {
    if (a[i] != c[i] && b[i] != c[i]) {
      cout << "NO" << endl;
      return;
    }
  }
  cout << "YES" << endl;
}

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);

  int t;
  cin >> t;
  while (t--) {
    solve();
  }
}
```
</details>

---

## [C - Vanya and Fence](https://codeforces.com/problemset/problem/677/A)
<details> <summary>Dica 1</summary>

Para essa questão, não é necessário utilizar vector.

</details><details> <summary>Solução</summary>

Primeiro, cria-se um contador representando o tamanho da estrada. Para cada valor lido, basta verificar se é menor ou igual à altura `h`. Se sim, soma-se 1 ao contador; se não, soma-se 2.

Complexidade: `O(n)`

</details><details> <summary>Código (C++)</summary>


```cpp
#include <bits/stdc++.h>
#define endl '\n'
typedef long long ll;
using namespace std;
 
int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);
 
  int n, h;
  cin >> n >> h;
  ll contador = 0;
  for (int i = 0; i < n; i++) {
    int a;
    cin >> a;
    contador++;
    if (a > h)
      contador++;
  }
  cout << contador << endl;
}
```
</details>