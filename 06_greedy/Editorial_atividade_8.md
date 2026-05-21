# Editorial - Atividade 7

## [A - Tasks and Deadlines](https://vjudge.net/contest/805690#problem/A)
<details> <summary>Dica 1</summary>

Perceba que o tempo de término f cresce conforme cada tarafa é executada.

</details><details> <summary>Solução</summary>

Pensando na dica, a melhor estratégia está em escolher primeiro a task com menor duração, o que irá garantir a recompensa ótima.

Complexidade: `O(n lg n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define endl '/n'

int main() {
  ios_base::sync_with_stdio(0);cin.tie(0);
  int n;cin >> n;
  vector<pair<int, int>> t;
  for (int i = 0; i < n; i++) {
    int a, d;cin >> a >> d;
    t.push_back({a, d});
  }
  
  sort(t.begin(), t.end());
  
  int ans = 0, f = 0;
  for (int i = 0; i < n; i++) {
    f += t.first;
    ans += t.second - f;
  }
  
  cout << ans << endl;
  
  return 0;
}
```
</details>

---

## [B - Stick Lengths](https://vjudge.net/contest/805690#problem/B)
<details> <summary>Dica 1</summary>

Mediana.

</details><details> <summary>Solução</summary>

Encontre a mediana dos tamanhos, levando em conta as repetições, e, em seguida, calcule o custo necessário para que todos os gravatos tenham o mesmo tamanho da mediana.

Complexidade: `O(n lg n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
typedef long long ll;

int main() {
  ios_base::sync_with_stdio(0);cin.tie(0);
  int n;cin >> n;
  ll p[n];for (int i = 0; i < n; i++) cin >> p[i];
  
  sort(p, p + n);
  
  ll med = p[n / 2], ans = 0;
  for (int i = 0; i < n; i++) {
    ans += abs(med - p[i]);
  }
  
  cout << ans << endl;
  
  return 0;
}
