# Editorial - Atividade 7

## [A - Studying Algorithms](https://vjudge.net/contest/805402#problem/A)
<details> <summary>Dica 1</summary>

Para maximizar a quantidade de algoritmos aprendidos, deve-se começar por quais?

</details><details> <summary>Solução</summary>

Pensando na dica 1, deve-se escolher sempre o algoritmo que leve menos tempo. Para isso é preciso ordenar o array e checar se há tempo o suficiente para aprender o algoritmo em questão.

Complexidade: `O(n lg n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'

int main() {
  int n, x;cin >> n >> x;
  int a[n];for(int i = 0; i < n; i++) cin >> a[i];

  sort(a, a + n);

  int ans = 0;
  
  for(int i = 0; i < n; i++) {
    if (x >= a[i]) {
      x -= a[i];
      ans++;
    } else break;
  }

  cout << ans << endl;
  
  return 0;
}
```
</details>

---

## [B - Movie Festival](https://vjudge.net/contest/805402#problem/B)
<details> <summary>Dica 1</summary>

Para maximizar a quantidade de filmes assistidos, pense no horário de término.

</details><details> <summary>Solução</summary>

Pensando na dica 1, deve-se ordenar os filmes pelo horário de término, pois, escolhendo sempre aquele que termina primeiro e é possível ser assitido, resta mais tempo para assistir os proximos.

Complexidade: `O(n lg n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'

int main() {
  int n;cin >> n;
  vector<pair<int, int>> m;
  for(int i = 0; i < n; i++) {
    int a, b;cin >> a >> b;
    m.push_back({b, a}); // inverte-se a ordem nos pares devido a ordenacao dessas estruturas
  }

  sort(m.begin(), m.end());

  int ans = 1, prev = m[0].first;
  
  for(int i = 1; i < n; i++) {
    if (m[i].second >= prev) {
      ans++;
      prev = m[i].first;
    }
  }

  cout << ans << endl;
  
  return 0;
}
