# Editorial - Atividade 18



## [A - Building Roads](https://cses.fi/problemset/task/1666)
<details> <summary>Solução</summary>

Modelando a questão como um grafo --- sendo as cidades os vértices e as estradas as arestas --- percebe-se que o objetivo é tornar o grafo conexo e determinar a quantidade mínima de arestas necessárias para isso.

O primeiro passo é determinar quantos componentes conexos há no grafo utilizando DFS, guardando em um vetor ao menos 1 vértice de cada componente. Utilizando a propriedade da transitividade (se $A$ tem caminho com $B$, e $B$ tem caminho com $C$, então $A$ tem caminho com $C$), é possível provar que são necessárias apenas $x - 1$ arestas, sendo $x$ o número de componentes conexos. Seja $a_i$ o vértice do $i$-ésimo componente conexo: basta criar uma aresta de $a_i$ para $a_{i+1}$, e assim o grafo se tornará conexo.

Complexidade: `O(n + m)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int N = 1e5;
vector<vector<int>> adj(N);
vector<bool> visited(N);

void dfs(int i) {
  if (visited[i]) return;
  visited[i] = true;

  for (auto a : adj[i]) dfs(a);
}

int main() {
  int n, m;
  cin >> n >> m;
  for (int i = 0; i < m; i++) {
    int a, b;
    cin >> a >> b;
    a--, b--;
    adj[a].push_back(b);
    adj[b].push_back(a);
  }
  vector<int> cities;
  for (int i = 0; i < n; i++) {
    if (!visited[i]) {
      cities.push_back(i);
      dfs(i);
    }
  }
  cout << cities.size() - 1 << endl;
  for (int i = 0; i < ((int)cities.size() - 1); i++) {
    cout << cities[i] + 1 << ' ' << cities[i + 1] + 1 << endl;
  }
}
```
</details>

---

## [B - Message Route](https://cses.fi/problemset/task/1667)
<details> <summary>Solução</summary>

Modelando a questão como um grafo --- sendo os computadores os vértices e as conexões as arestas --- o objetivo é encontrar o número de vértices do menor caminho que liga o vértice `1` ao vértice `n`.

Para isso, aplica-se BFS a partir do vértice `1`, armazenando em um vetor de pais os vértices de origem de cada caminho percorrido em direção ao vértice `n`. Quando a BFS for finalizada, recupera-se o caminho a partir do vetor de pais. Se o vértice `n` não possui pai, então os vértices `1` e `n` não são conexos e é impossível. Caso contrário, guarda-se o pai de `n` em um vetor e busca-se o pai desse pai, e assim sucessivamente até encontrar o vértice `1`.

Complexidade: `O(n + m)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int N = 1e5;
vector<vector<int>> adj(N);
vector<bool> visited(N);
vector<int> pai(N, -1);
vector<int> dist(N, -1);

void bfs(int a) {
  visited[a] = true;
  pai[a] = a;
  dist[a] = 0;
  queue<int> q;
  q.push(a);
  while (q.size()) {
    int v = q.front();
    q.pop();
    for (auto u : adj[v]) {
      if (visited[u]) continue;
      visited[u] = true;
      pai[u] = v;
      dist[u] = dist[v] + 1;
      q.push(u);
    }
  }
}

vector<int> path(int a) {
  vector<int> ret;
  while (pai[a] != -1) {
    ret.push_back(a);
    if (pai[a] == a)
      break;
    else
      a = pai[a];
  }
  reverse(ret.begin(), ret.end());
  return ret;
}

int main() {
  int n, m;
  cin >> n >> m;
  for (int i = 0; i < m; i++) {
    int a, b;
    cin >> a >> b;
    a--, b--;
    adj[a].push_back(b);
    adj[b].push_back(a);
  }
  bfs(0);
  if (!visited[n - 1]) {
    cout << "IMPOSSIBLE" << endl;
    return 0;
  }

  cout << dist[n - 1] + 1 << endl;
  for (auto a : path(n - 1)) {
    cout << a + 1 << ' ';
  }
}
```
</details>

---

## [C - Building Teams](https://cses.fi/problemset/task/1668)
<details> <summary>Solução</summary>

Modelando a questão como um grafo --- sendo os alunos os vértices e as amizades as arestas --- o objetivo é verificar se o grafo é bipartido ou não.

Para isso, aplica-se o algoritmo clássico de verificação de grafos bipartidos. Cria-se um vetor de coloração, onde `color[i]` representa a cor do vértice `i`. Se `color[i] == 0`, então ele ainda não foi colorido. Ao aplicar a DFS, colore-se os vizinhos do vértice `i` com a cor oposta à sua, verificando se os vizinhos já coloridos foram coloridos corretamente. Se algum vizinho possuir a mesma cor que o vértice atual, então o grafo não é bipartido e deve-se imprimir `IMPOSSIBLE`.

Complexidade: `O(n + m)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int N = 1e5;
vector<vector<int>> adj(N);
vector<int> color(N);
bool possible = true;

void dfs(int i, int c) {
  color[i] = c;
  int newColor = (c == 1 ? 2 : 1);

  for (auto v : adj[i]) {
    if (color[v] == 0)
      dfs(v, newColor);
    else if (color[v] == newColor)
      continue;
    else {
      possible = false;
      break;
    }
  }
}

int main() {
  int n, m;
  cin >> n >> m;
  for (int i = 0; i < m; i++) {
    int a, b;
    cin >> a >> b;
    a--, b--;
    adj[a].push_back(b);
    adj[b].push_back(a);
  }

  for (int i = 0; i < n; i++) {
    if (color[i] == 0) dfs(i, 1);
    if (!possible) break;
  }

  if (!possible)
    cout << "IMPOSSIBLE" << endl;
  else {
    for (int i = 0; i < n; i++) {
      cout << color[i] << ' ';
    }
  }
}
```
</details>

---

## [D - Subordinates](https://cses.fi/problemset/task/1674)
<details> <summary>Solução</summary>

Modelando a questão como um grafo --- sendo os funcionários os vértices e as relações chefe-subordinado as arestas --- o objetivo é calcular, para cada vértice `i`, o tamanho de sua subárvore, que representa a quantidade de funcionários subordinados ao funcionário `i`.

Observe que o grafo de entrada sempre será uma árvore, pois são fornecidas $n - 1$ relações de subordinação e o grafo é sempre conexo, já que todos os funcionários são subordinados ao diretor de número `1`. Sendo assim, aplica-se DFS sobre todos os nós da árvore, calculando o tamanho da subárvore do vértice `i` a partir do tamanho das subárvores de seus vizinhos já visitados.

Complexidade: `O(n)`

</details><details> <summary>Código (C++)</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

vector<vector<int>> adj;
vector<int> subtree;

void dfs(int a) {
  subtree[a] = 1;
  for (auto u : adj[a]) {
    if (subtree[u] != -1) continue;
    dfs(u);
    subtree[a] += subtree[u];
  }
}

int main() {
  int n;
  cin >> n;
  adj = vector<vector<int>>(n);
  subtree = vector<int>(n, -1);
  for (int i = 0; i < n - 1; i++) {
    int a;
    cin >> a;
    a--;
    adj[a].push_back(i + 1);
  }

  dfs(0);
  for (int i = 0; i < n; i++) {
    cout << max(0, subtree[i] - 1) << ' ';
  }
}
```
</details>