### [A - Maximum Subarray Sum](https://cses.fi/problemset/task/1643)

<details> <summary>Dica 1</summary>
Pense em como expressar a soma de um subarray [l, r] usando prefix sums: ela é igual a prefix[r] - prefix[l-1]. Para maximizar essa diferença, o que você deve fazer com prefix[l-1]?

</details><details> <summary>Solução</summary>
Construa o array de prefix sums. Para cada posição r, a soma máxima do subarray terminando em r é prefix[r] - min(prefix[l-1]) para todo l <= r. Logo, basta iterar da esquerda para a direita mantendo o menor valor de prefix sum já visto até o momento. A resposta é o maior valor de prefix[r] - min_prefix encontrado ao longo da iteração.

Complexidade: `O(n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef longlong ll;

int main(){
    int n;
    cin >> n;
    vector<ll>a(n);
    for(ll& x : a) cin >> x;

    ll ans = LLONG_MIN, min_prefix =0, prefix =0;
    for(int i =0; i < n; i++){
        prefix += a[i];
        ans =max(ans, prefix - min_prefix);
        min_prefix =min(min_prefix, prefix);
    }
    cout << ans <<"\n";
}
```

</details>

### [B - Hoof, Paper, Scissors](https://usaco.org/index.php?page=viewproblem2&cpid=691&lang=en)

<details> <summary>Dica 1</summary>
Bessie joga o mesmo gesto até um ponto i e depois troca para outro gesto até o fim. Pense em como pré-computar, para cada posição i, quantas vitórias ela obtém jogando cada gesto no prefixo [1, i] e no sufixo [i+1, N].

</details><details> <summary>Solução</summary>
Para cada gesto g (H, P, S), construa dois arrays de prefix/suffix counts: pre[g][i] = número de vitórias jogando g nos primeiros i jogos, e suf[g][i] = número de vitórias jogando g do jogo i até o fim. A resposta é o máximo de pre[g1][i] + suf[g2][i+1] para todo ponto de troca i e todo par de gestos (g1, g2) com g1 != g2. O caso em que Bessie nunca troca também é coberto quando g1 == g2 ou testando pre[g][N] separadamente.

Complexidade: `O(n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
    int n;
    cin >> n;
    vector<char>a(n);
    for(char& c : a) cin >> c;

    string gestos ="HPS";
    vector<vector<int>>pre(3,vector<int>(n +1,0));
    vector<vector<int>>suf(3,vector<int>(n +2,0));

    auto beats =[](char g,char opp){
	    return(g =='H'&& opp =='S')||
	    (g =='P'&& opp =='H')||
	    (g =='S'&& opp =='P');
	};

    for(int i =0; i < n; i++)
	    for(int g =0; g <3; g++)
            pre[g][i +1]= pre[g][i]+beats(gestos[g], a[i]);

    for(int i = n -1; i >=0; i--)
	    for(int g =0; g <3; g++)
            suf[g][i]= suf[g][i +1]+beats(gestos[g], a[i]);

    int ans = 0;
    for(int i =0; i <= n; i++)
	    for(int g1 =0; g1 <3; g1++)
		    for(int g2 =0; g2 <3; g2++)
                ans = max(ans, pre[g1][i] + suf[g2][i]);

    cout << ans << "\n";
}
```

</details>

### [C - Subarray Sums II](https://cses.fi/problemset/task/1661)

<details> <summary>Dica 1</summary>
A soma do subarray [l, r] é prefix[r] - prefix[l-1]. Para contar os pares onde essa diferença é igual a x, pense em como usar um map de frequências dos valores de prefix sum já vistos.

</details><details> <summary>Solução</summary>
Itere pelo array mantendo o prefix sum atual. Para cada posição r, um subarray [l, r] tem soma x se e somente se prefix[r] - prefix[l-1] == x, ou seja, prefix[l-1] == prefix[r] - x. Logo, basta consultar em um map de frequências quantas vezes o valor prefix[r] - x já apareceu como prefix sum anterior, acumular esse valor na resposta e então inserir prefix[r] no map. Inicialize o map com freq[0] = 1 para cobrir subarrays que começam no índice 0.

Complexidade: `O(n log n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef longlong ll;

int main(){
    int n;
    ll x;
    cin >> n >> x;

    map<ll, ll> freq;
    freq[0]=1;
    ll prefix =0, ans =0;
    for(int i =0; i < n; i++){
        ll a;
        cin >> a;
        prefix += a;
        ans += freq[prefix - x];
        freq[prefix]++;
    }
    cout << ans <<"\n";
}
```

</details>
