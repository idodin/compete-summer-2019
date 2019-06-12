---
layout: post
title: Problem Set 2
categories: 
  - dynamic programming
---


# Week 2 Solutions

## CodeForces

### [CodeForces 961B: Lecture Sleep](https://codeforces.com/contest/961/problem/B)
{% highlight c++ %}
/*
 * If Mishka is awake, he always writes down the theorems
 * Find the maximum sum of k contiguous elements only when Mishka is asleep
 */

#include <bits/stdc++.h>

#define vi vector<int>
#define ii pair<int,int>
#define pb push_back
#define mp make_pair

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int main (void) {
    ios_base::sync_with_stdio(false);
    // your code here

    int n, k, a;
    vi v {0};
    vector<long> in, vk;
    long result = 0, incl = 0;
    cin >> n >> k;

    vk.assign(n+2, 0);

    for (int i = 1; i <= n; i++) {
        cin >> a;
        v.pb(a);
    }

    for (int i = 1; i <= n; i++) {
        cin >> a;
        if (a) {
            result += v[i];
            v[i] = 0;
        }

        if (i > k) { vk[i] -= v[i-k]; }
        vk[i] += vk[i-1] + v[i];
        incl = max(incl, vk[i]);
    }

    cout << result+incl << endl;

    return 0;
}

{% endhighlight %}

### [CodeForces 698A: Vacations](https://codeforces.com/contest/698/problem/A)
{% highlight c++%}
/*
 * If both gym and contest are closed, we have a rest day
 * If only gym is open, we can either do gym or rest, depending on the prev day
 * If only contest is open, we can either do contest or rest
 * If both gym and contest are open, we have 3 options: gym, contest, or rest
 */

#include <bits/stdc++.h>

#define vi vector<int>
#define ii pair<int,int>
#define pb push_back
#define mp make_pair

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int days[101];
int dp[101][3];
int n;

int main (void) {
    ios_base::sync_with_stdio(false);

    scanf("%d\n", &n);
    for (int i = 1; i <= n; i++) scanf("%d", &days[i]);

    for (int i = 1; i <= n; i++)
        fill(dp[i], dp[i]+3, 9999);

    for (int i = 1; i <= n; i++) {
        if (days[i] == 1) {
            dp[i][1] = min(dp[i-1][0], dp[i-1][2]);
        } else if (days[i] == 2) {
            dp[i][2] = min(dp[i-1][0], dp[i-1][1]);
        } else if (days[i] == 3) {
            dp[i][1] = min(dp[i-1][0], dp[i-1][2]);
            dp[i][2] = min(dp[i-1][0], dp[i-1][1]);
        }
        dp[i][0] = *min_element(dp[i-1], dp[i-1]+3) + 1;
    }

    printf("%d\n", *min_element(dp[n], dp[n]+3));

    return 0;
}

{% endhighlight %}

### [CodeForces 855B: Marvolo Gaunt's Ring](https://codeforces.com/contest/855/problem/B)
{% highlight c++%}
/*
 * Find the best i, j, k as we go, note the constraint that i <= j <= k
 */

#include <bits/stdc++.h>

#define vi vector<int>
#define ii pair<int,int>
#define pb push_back
#define mp make_pair

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

ll arr[100100][3];

int main (void) {
    ios_base::sync_with_stdio(false);

    ll n, p, q, r, tmp;
    scanf("%lld %lld %lld %lld\n", &n , &p, &q, &r);

    fill(arr[0], arr[0]+3, 1e18*-9);
    for (int i = 1; i <= n; i++) {
        scanf("%lld", &tmp);
        arr[i][0] = max(arr[i-1][0], p*tmp);
        arr[i][1] = max(arr[i-1][1], arr[i][0] + q*tmp);
        arr[i][2] = max(arr[i-1][2], arr[i][1] + r*tmp);
    }

    printf("%lld\n", arr[n][2]);

    return 0;
}

{% endhighlight %}

### [CodeForces 573B: Bear and Blocks](https://codeforces.com/contest/573/problem/B)
{% highlight c++%}
/*
 * Solved using two linear scans and a little bit of weird crap
 *
 * Lemma: A square will last exactly the height of the "pyramid" surrounding it
 e.g.
       *
  *   ***
 *#* **$**
# will last 2 passes
$ will last 3 passes
 *
 * Basic argument (the full proof is left as an exercise to the reader):
 * A full pyramid of size n is reduced to size n-1 after each step
 * If there is a square missing from a pyramid of size n, after a step at least one square is missing from the remaining pyramid of size n-1
 * Thus, a square surrounded by a size n (but not n+1) pyramid lasts at least n steps but not n+1 steps (i.e. n steps)
 *
 * The question is thus equivalent to finding max pyramid size
 *
 * Do 2 linear scans forwards & backwards to calculate:
 * Largest triangle from the left side, then largest triangle from the right side
 * The max pyramid size is the lesser of the left & right halves
e.g.

	        * 
	        * 
 	      * * 
 	      * #   
 	  *   # # # *
	  * # # # # # 
	   
Left: 1 1 2 3 2 2
Right:2 1 4 3 2 1
Size: 1 1 2 3 2 1
 * Return max pyramid size (highlighted with #)
 * Final complexity O(n)
 */
#include <bits/stdc++.h>
using namespace std;
int v[100100];
int a[100100][2];
int main() {
	int n;
	scanf("%d ",&n);
	memset(a,0,sizeof(a));
	for(int i=1;i<=n;i++) {
		int t;
		scanf("%d ",&t);
		v[i] = t;
	}
	n++;
	for(int i=1;i<n;i++) {
		a[i][0] = min(a[i-1][0]+1,v[i]);
	}
	for(int i=n-1;i>=0;i--) {
		a[i][1] = min(a[i+1][1]+1,v[i]);
	}
	int ma = 0;
	for(int i=1;i<n;i++) {
		ma = max(ma,min(a[i][0],a[i][1]));
	}
	printf("%d\n",ma);
}
{% endhighlight %}

## Kattis

### [Knapsack](https://open.kattis.com/problems/knapsack)
{% highlight c++%}
/*
 * This is the classic knapsack problem with an additional task of
 * constructing the optimal solution. Once we have built the look up
 * table, start from table[N][C], where N is the number of items, and
 * C is the capacity. At capacity c <= C, if adding item i or i-1 yield
 * the same best value, we choose the item with lower index.
 */

#include <bits/stdc++.h>

#define vi vector<int>
#define ii pair<int,int>
#define pb push_back
#define mp make_pair

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int main (void) {
    ios_base::sync_with_stdio(false);

    int c, n;
    vi v, w;

    while (cin >> c >> n) {
        v.resize(n+1);
        w.resize(n+1);
        for (int i = 1; i <= n; i++) {
            cin >> v[i] >> w[i];
        }

        vector<vi> table (n+1);
        for (int i = 0; i <= n; i++) table[i].assign(c+1, 0);

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= c; j++) {
                if (w[i] > j)
                    table[i][j] = table[i-1][j];
                else
                    table[i][j] = max( table[i-1][j], table[i-1][j-w[i]] + v[i] );
            }
        }

        int best_val = table[n][c];
        vi result;
        for (int i = n; i > 0 && best_val > 0; i--) {
            if (best_val == table[i-1][c])
                continue;
            else {
                result.pb(i-1);
                c -= w[i];
                best_val -= v[i];
            }
        }

        printf("%lu\n", result.size());
        for (int idx : result) printf("%d ", idx);
        putchar('\n');

    }

    return 0;
}

{% endhighlight %}

### [Restaurant Orders](https://open.kattis.com/problems/orders)
{% highlight c++%}
/*
 * Given that the largest order is 30000, we can fill an array
 * to count how many ways a specific order can be interpreted.
 * If no combination in the menu add up to a value, answer impossible,
 * if there's more than one, answer ambiguous, otherwise iterate
 * through the menu to include items in the order.
 */

#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main (void) {
    int n, m, q;
    cin >> n;
    vector<int> menu (n);
    vector<int> arr (30001, -1);

    for (int i = 0; i < n; i++) cin >> menu[i];

    arr[0] = 1;
    for (int p : menu) {
        for (int j = p; j < arr.size(); j++) {
            if (arr[j - p] == -1) continue;
            arr[j] = (arr[j] == -1) ? arr[j-p]: arr[j-p] + 1;
        }
    }

    cin >> m;
    while (m--) {
        cin >> q;
        if (arr[q] > 1 ) {
            cout << "Ambiguous" << endl;
        } else if (arr[q] == 1) {
            for (int i = 1; i <= menu.size(); i++) {
                while (q >= menu[i-1] && arr[q-menu[i-1]] == 1) {
                    q -= menu[i-1];
                    printf("%d ", i);
                }
            }
            putchar('\n');
        } else {
            cout << "Impossible" << endl;
        }
    }

    return 0;
}

{% endhighlight %}

### [Longest Increasing Subsequence](https://open.kattis.com/problems/longincsubseq)
{% highlight c++%}
/*
 * As the input size can be up to 10^5, a normal O(n^2) dynamic programming
 * approach will get TLE. Instead, we should use the O(nlogk) solution,
 * where k is the length of the longest increasing subsequence.
 * The textbook has a section about LIS in chapter 3.5
 */

#include <bits/stdc++.h>

#define vi vector<int>
#define ii pair<int,int>
#define pb push_back
#define mp make_pair

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int N = 100100;

void print_result (int end, int p[]) {
    int x = end;
    stack<int> st;
    for (; p[x] >= 0; x = p[x]) st.push(x);
    st.push(x);
    for (; !st.empty(); st.pop()) printf("%d ", st.top());
    printf("\n");
}

int main (void) {
    ios_base::sync_with_stdio(false);

    int n;
    int v[N], L[N], L_idx[N], P[N];
    int lis, lis_end;
    while (cin >> n) {
        memset(L, 0, sizeof(L));
        memset(L_idx, 0, sizeof(L));
        memset(P, 0, sizeof(P));
        lis = lis_end = 0;

        for (int i = 0; i < n; i++) {
            cin >> v[i];
            int pos = lower_bound(L, L+lis, v[i]) - L;
            L[pos] = v[i];
            L_idx[pos] = i;
            P[i] = pos ? L_idx[pos-1] : -1;

            if (pos+1 > lis) {
                lis = pos+1;
                lis_end = i;
            }
        }

        printf("%d\n", lis);
        print_result(lis_end, P);
    }

    return 0;
}

{% endhighlight %}

### [Spiderman's Workout](https://open.kattis.com/problems/spiderman)
{% highlight c++%}
/*
 * Solved with bottom-up DP
 *
 * We want to both minimize the height spoderman goes up while making sure he gets to the destination
 * To do this we look at all 14000605+ possible ways that spoderman can ascend/descend and keep track of the minimum height of a building that would allow spoderman to get there
 * This gives us m*S states, where S is the sum of exercise numbers (S <= 1000)
 * Then, when spoderman does a new exercise, we can update the DP array:
 * dp[x][height] = H (we need a building of height H to get to that height at exercise #x
 * If spoderman goes up on exercise #x, the new height might be higher than H, so 
 * We also keep pointers from all the DP states to keep track of which previous state led to it
 * dp[x+1][new height] = minimum of itself and max(dp[x][height],new height)
 * If it improved, update the pointer from the new state to the old one
 *
 * Spoderman can only go down if his current height is at least the climbing disstance, but the max height clearly does not increase, so
 * dp[x+1][new height] = minimum of itself and dp[x][height]
 *
 * The thing we are looking for is the chain of pointers from dp[m][0]
 * (The minimum building height required to end up at height 0 after all the exercises) 
 * We can detect impossible sequences either by initializing the DP states to infinity or by checking the chain of pointers to see if it breaks
 *
 * Reconstruct the jumps from the pointer chain & print
 * 
 * Complexity: O(m*S*t)
 * (m <= 40, S <= 1000, t <= 100)
 */
#include <bits/stdc++.h>
using namespace std;
int dp[50][2100];
int p[50][2100];
int v[50];
int main() {
	int n;
	scanf("%d ",&n);
	while(n--) {
		int m;
		scanf("%d ",&m);
		for(int i=0;i<=m;i++) {
			for(int j=0;j<=1000;j++) {
				dp[i][j] = 999999;
			}
		}
		memset(p,0,sizeof(p));
		dp[0][0] = 0;
		for(int i=0;i<m;i++) {
			scanf("%d ",&v[i]);
		}
		for(int i=0;i<m;i++) {
			for(int j=0;j<=1000;j++) {
				int h = j+v[i];
				int hv = max(dp[i][j],j+v[i]);
				if(hv < dp[i+1][h]) {dp[i+1][h] = hv;p[i+1][h] = 1;}
				h = j-v[i];
				if(h >= 0) {
					if(dp[i][j] < dp[i+1][h]) {
						dp[i+1][h] = dp[i][j];
						p[i+1][h] = -1;
					}
				}
			}
		}
		if(dp[m][0] > 1000) {
			printf("IMPOSSIBLE\n");
		} else {
			int res[50];
			int h = 0;
			for(int i=m;i>=1;i--) {
				res[i-1] = p[i][h];
				//printf("%d <-",h);
				h -= p[i][h]*v[i-1];
				//printf("%d\n",h);
			}
			for(int i=0;i<m;i++) {
				printf("%c",res[i]>0?'U':'D');
			}
			printf("\n");
		}
	}
}
{% endhighlight %}
