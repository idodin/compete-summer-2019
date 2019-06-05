---
layout: post
title: Problem Set 3
categories:
  - graph
---


# Week 3 Solutions

## CodeForces

### [CodeForces 580C: Kefa and Park](https://codeforces.com/problemset/problem/580/C)
{% highlight c++ %}
/*
 * To determine if Kefa can visit a restaurant, we need to keep track
 * of the number of consecutive cats on the path from root node to 
 * that restaurant. If the number on a path is already greater than
 * m, then that path is not worth exploring anymore. Otherwise keep
 * going until we reach the leaves. A leaf is a node different from
 * 1 (the root node) and has exactly 1 adjacent node.
 */

#include <bits/stdc++.h>

#define vi vector<int>
#define ii pair<int,int>
#define pb push_back
#define mp make_pair

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int n,m, src, dst;
int result = 0;
bool cat[100010];
int cats[100010];
vector<vi> graph;

int main (void) {
    ios_base::sync_with_stdio(false);

    fill(cats, cats+100010, -1);

    cin >> n >> m;
    graph.resize(n+1);
    for (int i = 1; i <= n; i++) {
        cin >> cat[i];
    }

    for (int i = 1; i < n; i++) {
        cin >> src >> dst;
        graph[src].pb(dst);
        graph[dst].pb(src);
    }

    vi st {1};
    cats[1] = cat[1];
    while (!st.empty()) {
        int u = st.back(); st.pop_back();
        if (graph[u].size() == 1 && u != 1) {
            if (cats[u] <= m) result++;
            continue;
        }

        for (int v : graph[u]) {
            if (cats[v] == -1) {
                if (cat[v]) cats[v] = cats[u] + 1;
                else cats[v] = 0;
                if (cats[v] <= m)
                    st.pb(v);
            }
        }
    }

    printf("%d\n", result);

    return 0;
}
{% endhighlight %}

### [CodeForces 1167C: News Distribution](https://codeforces.com/problemset/problem/1167/C)

### [CodeForces 1143C: Queen](https://codeforces.com/problemset/problem/1143/C)

### [CodeForces 1144F: Graph Without Long Directed Paths](https://codeforces.com/problemset/problem/1144/F)
{% highlight c++ %}
/*
 * The graph as described in the problem is a bipartite graph. There
 * are 2 types of nodes: one that has only outgoing edges and the
 * other that accepts only incoming edges. It's obvious to see that
 * there will be no path of length two or greater since a vertex
 * cannot have incoming and outgoing edges at the same time. The
 * problem asks us to print the direction with regards to the edges,
 * so we might need to keep one end of each edge in an array to 
 * print the results later.
 */

#include <bits/stdc++.h>

#define vi vector<int>
#define ii pair<int,int>
#define pb push_back
#define mp make_pair

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

const int SIZE = 200010;
vector<vi> graph;
vi src;
int color[SIZE];
int n, m;

int main (void) {
    ios_base::sync_with_stdio(false);

    cin >> n >> m;
    graph.resize(n+1);
    src.resize(m);
    for (int i = 0; i < m; i++) {
        int u,v;
        cin >> u >> v;
        graph[u].pb(v);
        graph[v].pb(u);
        src[i] = u;
    }

    fill(color, color+SIZE, -1);

    vi st {1};
    color[1] = 0;
    while (!st.empty()) {
        int u = st.back(); st.pop_back();
        for (int v : graph[u]) {
            if (color[v] == -1) {
                color[v] = 1 - color[u];
                st.pb(v);
            } else if (color[v] == color[u]) {
                printf("NO\n");
                return 0;
            }
        }
    }

    printf("YES\n");
    for (int i = 0; i < m; i++) 
        printf("%d", color[src[i]] ? 0 : 1);
    printf("\n");

    return 0;
}
{% endhighlight %}

## Kattis

### [Class Picture](https://open.kattis.com/problems/classpicture)
{% highlight c++ %}
/*
 * We can solve this problem using a stack for DFS. This stack is
 * a little bit different in that it keeps all the possible
 * permutations of traversal path at each length i. The adjacent
 * vertices are kept in lexicographically decreasing order, so that
 * the "smallest" vertex will be on top of the stack each time,
 * therefore that permutation will be tested first. If one ordering
 * does not yield a possible solution, we pop another permutation
 * off the stack and start looking from there (backtracking).
 *
 * If it is possible to construct a path of length n, then we have
 * found a feasible ordering, otherwise y'all need therapy.
 *
 * Alternatively, this problem can be solved by generating all
 * possible permutation and see if any works (Complexity is O(n!))
 */

#include <bits/stdc++.h>

#define vi vector<int>
#define ii pair<int,int>
#define vii vector<pair<int,int>>

typedef long long ll;
typedef unsigned long long ull;

#define pb push_back
#define mp make_pair
#define fi first
#define se second

using namespace std;

int n,m;
string s,t;

int main (void) {
    ios_base::sync_with_stdio(false);

    while (cin >> n) {
        set<string> names;
        map<string, set<string, greater<string>>> dislike, graph;

        while (n--) {
            cin >> s;
            names.insert(s);
        }

        cin >> m;
        while (m--) {
            cin >> s >> t;
            dislike[s].insert(t);
            dislike[t].insert(s);
        }

        for (string x : names) {
            for (string y : names) {
                if (!x.compare(y)) continue;
                if (dislike[x].find(y) == dislike[x].end()) {
                    graph[x].insert(y);
                    graph[y].insert(x);
                }
            }
        }

        // run DFS starting from all possible nodes
        bool found = false;
        for (string name: names) {
            if (found)
                break;
            vector<vector<string>> st {{name}};
            while (!st.empty()) {
                vector<string> node = st.back(); st.pop_back();
                if (node.size() == names.size()) {
                    found = true;
                    for (string r : node) cout << r << " ";
                    cout << endl;
                    break;
                }

                auto u = node[node.size()-1];
                for (auto v : graph[u]) {
                    if (find(node.begin(), node.end(), v) != node.end())
                        continue;
                    vector<string> nnode (node);
                    nnode.pb(v);
                    st.pb(nnode);
                }
            }
        }
        if (!found)
            cout << "You all need therapy." << endl;
    }

    return 0;
}
{% endhighlight %}

### [Faulty Robot](https://open.kattis.com/problems/faultyrobot)
{% highlight c++ %}
/*
 * It's possible that the robot will follow the forced edges forever,
 * we need a way to detect cycles. So we need to perform DFS starting
 * from the following vertices:
 *  - Vertex 1, which is the robot's starting point.
 *  - Any vertex v that is adjacent to a vertex u in the first DFS path,
 *  where e(u,v) is not forced. In another word, this is where the bug
 *  might have happened.
 * Start DFS from those vertices and add each ending point to a set,
 * if a cycle is detected, we immediately stop DFS as there will be no
 * stopping point. The result is the number of distinct stopping vertices.
 */

#include <bits/stdc++.h>

#define vi vector<int>
#define ii pair<int,int>
#define iii pair<int,pair<int,int>>
#define pb push_back
#define mp make_pair

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int n,m,a,b;
bool visited[1010];
vector<map<int,bool>> graph;
set<int> stop;

int main (void) {
    ios_base::sync_with_stdio(false);

    cin >> n >> m;
    graph.resize(n+1);
    while (m--) {
        cin >> a >> b;
        if (a < 0) {
            a = abs(a);
            graph[a][b] = true;
        } else {
            graph[a][b] = false;   // not forced
        }
    }

    int result = 0, last = -1;
    vi to_visit;
    vi st {1};
    visited[1] = true;
    bool cycle = false;
    while (!st.empty() && !cycle) {
        int u = st.back(); st.pop_back();
        last = u;
        for (auto e : graph[u]) {
            if (visited[e.first]) {
                cycle = true;
            } else {
                if (e.second == true) {
                    visited[e.first] = true;
                    st.pb(e.first);
                } else {
                    to_visit.pb(e.first);
                }
            }
        }
    }

    if (!cycle) stop.insert(last);
    for (int i : to_visit) {
        st.clear();
        fill(visited, visited+n, false);
        st.pb(i);
        cycle = false;
        while (!st.empty() && !cycle) {
            int u = st.back(); st.pop_back();
            last = u;
            for (auto e : graph[u]) {
                if (visited[e.first]) {
                    cycle = true;
                } else if (e.second == true) {
                    visited[e.first] = true;
                    st.pb(e.first);
                }
            }
        }
        if (!cycle) stop.insert(last);
    }

    printf("%d\n", (int)stop.size());

    return 0;
}
{% endhighlight %}

### [Breaking Bad](https://open.kattis.com/problems/breakingbad)
{% highlight c++ %}
/*
 * This problem can be solved using bipartite graph check, each pair
 * of items that is considered "suspicious" denotes an edge in the
 * graph, which means those items cannot belong to the same set.
 * At the end, if the graph is bipartite, simply traverse the vertices
 * and print the elements in each color set. Since the vertices ID
 * are given as strings, we can assign a unique integer for faster
 * lookups.
 */

#include <bits/stdc++.h>

#define vi vector<int>
#define ii pair<int,int>
#define pb push_back
#define mp make_pair

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int N, M;
string s, t;
map<string, int> m;
vector<string> names (100100);
vector<vi> graph;
int colors[100100];
deque<int> q;
vector<vi> items (2);

int main (void) {
    ios_base::sync_with_stdio(false);

    cin >> N;
    graph.resize(N+1);
    for (int i = 1; i <= N; i++) {
        cin >> s;
        names[i] = s;
        m[s] = i;
    }

    cin >> M;
    while (M--) {
        cin >> s >> t;
        graph[m[s]].pb(m[t]);
        graph[m[t]].pb(m[s]);
    }

    bool possible = true;

    fill(colors, colors+N+10, -1);

    for (int i = 1; i <= N; i++) {
        if (colors[i] != -1) continue;

        q.pb(i);
        colors[i] = 0;
        items[0].pb(i);
        while (!q.empty() && possible) {
            int u = q.front(); q.pop_front();
            for (int v : graph[u]) {
                if (colors[v] == -1) {
                    colors[v] = 1 - colors[u];
                    items[colors[v]].pb(v);
                    q.pb(v);
                } else if (colors[v] == colors[u]) {
                    possible = false;
                    break;
                }
            }
        }
    }


    if (!possible) {
        cout << "impossible" << endl;
        return 0;
    } else {
        for (int i : items[0]) cout << names[i] << " ";
        cout << endl;
        for (int i : items[1]) cout << names[i] << " ";
        cout << endl;
    }

    return 0;
}
{% endhighlight %}
