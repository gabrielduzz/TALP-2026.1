# Editorial - Atividade 7 (Two Pointers)

## [A - Favorite Sequence](https://codeforces.com/problemset/problem/1462/A)

<details> <summary>Dica 1</summary>

A sequência original foi construída pegando alternadamente elementos das extremidades do array: o primeiro, depois o último, o segundo, o penúltimo, e assim por diante.

</details><details> <summary>Solução</summary>

Para reverter a sequência, podemos usar a técnica de dois ponteiros. Basta inicializar um ponteiro no início do vetor (`left = 0`) e outro no final (`right = n - 1`).

Em seguida, em um laço, adicionamos o elemento apontado por `left` e avançamos esse ponteiro. Se o ponteiro `left` ainda for menor ou igual a `right`, adicionamos o elemento apontado por `right` e decrementamos esse ponteiro. Fazemos isso até que os ponteiros se cruzem.

Complexidade: `O(n)`

</details><details> <summary>Código (C++)</summary>

```cpp
	#include <bits/stdc++.h>
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (int i = 0; i < n; i++) cin >> a[i];

    int left = 0, right = n - 1;
    vector<int> ans;
  
    while (left <= right) {
        ans.push_back(a[left]);
        left++;
        if (left <= right) {
            ans.push_back(a[right]);
            right--;
        }
    }

    for (int i = 0; i < n; i++) {
        cout << ans[i] << (i == n - 1 ? "" : " ");
    }
    cout << endl;
}

int main() {
    int t;
    cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}
```

---

## [B - Books](https://codeforces.com/problemset/problem/279/B)

<details> <summary>Dica 1</summary>

Perceba que Valera deve ler livros em ordem contígua a partir de algum índice **`i`**. Pense em como manter eficientemente a soma de uma janela de livros que caiba em **`t`** minutos.

</details><details> <summary>Solução</summary>

Utilize a técnica de dois ponteiros: mantenha um ponteiro esquerdo **`l`** e um direito **`r`**, ambos iniciando em 0, e uma variável **`sum`** com a soma atual da janela **`[l, r]`**. A cada iteração, avance **`r`** somando **`a[r]`** a **`sum`**. Enquanto **`sum > t`**, remova **`a[l]`** de **`sum`** e avance **`l`**. Atualize a resposta com o tamanho máximo da janela válida encontrada (**`r - l + 1`**).

Complexidade: **`O(n)`**

</details><details> <summary>Código (C++)</summary>

```#**include**<bits/stdc++.h>**
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    long long t;
    cin >> n >> t;
    vector<int> a(n);
    for (int& x : a) cin >> x;
    long long sum = 0;
    int ans = 0, l = 0;
    for (int r = 0; r < n; r++) {
        sum += a[r];
        while (sum > t) sum -= a[l++];
        ans = max(ans, r - l + 1);
    }
    cout << ans << "\n";
}
```

---

## [C - Cipher Shifer](https://codeforces.com/problemset/problem/1840/A)

<details> <summary>Dica 1</summary>

Para cada caractere da string original, a cifra garante que ele aparece duas vezes seguidas no texto cifrado: primeiro na posição original e depois ao fim do bloco. Pense em como localizar essas ocorrências usando dois ponteiros.

</details><details> <summary>Solução</summary>

Utilize dois ponteiros **`l`** e **`r`**, ambos iniciando em 0. Fixe **`l`** como a posição do próximo caractere original a ser encontrado. Avance **`r`** a partir de **`l + 1`** até encontrar uma posição onde **`s[r] == s[l]`**: nesse momento, **`s[l]`** é o caractere original. Adicione-o à resposta, e mova **`l`** para **`r + 1`** para processar o próximo bloco. Repita até percorrer toda a string.

Complexidade: **`O(n)`**

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int t;
    cin >> t;
    while (t--) {
        int n;
        string s;
        cin >> n >> s;
        string ans = "";
        int l = 0;
        while (l < n) {
            int r = l + 1;
            while (s[r] != s[l]) r++;
            ans += s[l];
            l = r + 1;
        }
        cout << ans << "\n";
    }
}

```

</details>
