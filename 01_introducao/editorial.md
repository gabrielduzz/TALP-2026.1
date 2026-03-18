# Editorial - Aula 01: Introdução

---

## [A - Cold-puter Science](https://open.kattis.com/problems/cold)

<details>
<summary>Dica 1</summary>
Você não precisa armazenar todas as temperaturas em um vetor. Você pode processar e contar os dias negativos à medida que lê a entrada.
</details>

<details>
<summary>Solução</summary>
O problema pede apenas para contar quantos números na entrada são estritamente menores que zero.
Basta ler o número de casos N, fazer um loop iterando $N$ vezes, ler a temperatura T e verificar se T < 0. Se for, incremente uma variável de contagem. No final, imprima o contador.
</details>

<details>
<summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(0); cin.tie(0);
    int n, t, ans = 0;
    cin >> n;
    while(n--) {
        cin >> t;
        if(t < 0) ans++;
    }
    cout << ans << "\n";
    return 0;
}
```

</details>

* * * * *

[B - Soldier and Bananas](https://codeforces.com/problemset/problem/546/A)
--------------------------------------------------------------------------

<details>

<summary>Dica 1</summary>

O custo total das $W$ bananas é a soma de $1k + 2k + 3k + \dots + Wk$. Como você pode simplificar essa equação matemática?

</details>

<details>

<summary>Dica 2</summary>

Cuidado com os casos de borda. O que deve ser impresso se o soldado já possuir dinheiro suficiente para pagar a conta? A resposta não pode ser negativa.

</details>

<details>

<summary>Solução</summary>

O custo para comprar as bananas segue uma Progressão Aritmética. Fatorando $K$, temos:

$\text{Custo Total} = K \cdot (1 + 2 + \dots + W)$

A soma dos números de 1 a $W$ é dada por $\frac{W(W+1)}{2}$. Portanto, o custo total pode ser calculado em $O(1)$ como $K \cdot \frac{W(W+1)}{2}$.

O valor que ele precisa pedir emprestado é o custo total menos o que ele já tem ($N$). Se ele já tiver dinheiro suficiente, o resultado dessa subtração será negativo, mas ele deve pedir $0$ emprestado. Para tratar isso de forma limpa, usamos a função `max(0LL, custo_total - n)`.

*Nota: Declaramos as variáveis como `long long` (ll) para evitar qualquer risco de overflow na multiplicação.*

</details>

<details>

<summary>Código (C++)</summary>


``` cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main() {
    ios_base::sync_with_stdio(0); cin.tie(0);
    ll k, n, w;
    cin >> k >> n >> w;

    ll custo_total = k * (w * (w + 1)) / 2;
    ll emprestimo = max(0LL, custo_total - n);

    cout << emprestimo << "\n";
    return 0;
}

```

</details>

* * * * *

[C - Weird Algorithm](https://cses.fi/problemset/task/1068)
-----------------------------------------------------------

<details>

<summary>Dica 1</summary>

O limite de tempo é de 1.0s e $N$ começa em até $10^6$. Como o valor cai rapidamente para 1, você pode simplesmente simular o processo usando um loop `while (n != 1)`.

</details>

<details>

<summary>Dica 2</summary>

O que acontece matematicamente quando o número é ímpar e você faz $N \cdot 3 + 1$? Se $N$ for próximo de $10^9$, o novo valor cabe em um `int` de 32 bits?

</details>

<details>

<summary>Solução</summary>

Este problema é uma armadilha clássica de Integer Overflow. Embora a entrada $N$ caiba em um tipo `int` padrão, as multiplicações por 3 durante as etapas intermediárias podem fazer com que o valor ultrapasse o limite de $2 \times 10^9$ do `int` de 32 bits.

A solução é usar um loop para simular o processo de Collatz, mas obrigatoriamente declarando a variável $N$ como `long long` (64 bits). Imprima o valor atual, e então aplique a regra: se `n % 2 == 0`, faça `n /= 2`, caso contrário faça `n = n * 3 + 1`. Repita até $N = 1$ e não se esqueça de imprimir o 1 no final.

</details>

<details>

<summary>Código (C++)</summary>

``` cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main() {
    ios_base::sync_with_stdio(0); cin.tie(0);
    ll n;
    cin >> n;

    while (n != 1) {
        cout << n << " ";
        if (n % 2 == 0) {
            n /= 2;
        } else {
            n = n * 3 + 1;
        }
    }
    cout << 1 << "\n";
    return 0;
}

```

</details>