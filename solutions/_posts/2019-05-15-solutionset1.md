---
layout: post
title: Problem Set 1
categories: 
  - linear data structures
  - non-linear data structures
---


# Week 1 Solutions

## CodeForces

### [CodeForces 1104B: Game with String](https://codeforces.com/contest/1104/problem/B)
{% highlight c++ %}
#include <bits/stdc++.h>
using namespace std;
stack<char> s;
char c[100100];
int main() {
	scanf("%s ",c);
	int l = strlen(c);
	int ctr=0;
	for(int i=0;i<l;i++) {
		char t = c[i];
		if(!s.empty() && s.top() == t) {s.pop();ctr++;} else {
			s.push(t);
		}
	}
	printf("%s\n",(ctr%2)?"Yes":"No");
}
{% endhighlight %}

### [CodeForces 1100B: Building a Contest](https://codeforces.com/contest/1100/problem/B)
{% highlight c++ %}
#include <bits/stdc++.h>
using namespace std;
int main() {
	int v[100100],cr[100100];
	int n,m;
	int th=1;
	scanf("%d %d ",&n,&m);
	memset(cr,0,sizeof(cr));
	for(int i=0;i<m;i++) {
		int t;
		scanf("%d ",&t);
		v[t]++;
		cr[v[t]]++;
		if(cr[th] == n) {th++;printf("1");} else {
			printf("0");
		}
	}
	printf("\n");
}
{% endhighlight %}

### [CodeForces 702B: Powers of Two](https://codeforces.com/contest/702/problem/B)
{% highlight c++ %}
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unordered_map<ll,ll> ml;
ml m;
void inc(int n) {
	if(m.find(n) == m.end()) {m[n] = 0;}
	m[n]++;
}
int main() {
	int n;
	scanf("%d ",&n);
	while(n--) {
		int t;
		scanf("%d ",&t);
		inc(t);
	}
	ll mx = 1LL<<31;
	ll tot = 0;
	for(ll i = 2;i<=mx;i<<=1) 
		for(auto& it: m) {
			ll val = i-it.first;
			if(val < 0) continue;
			if(m.find(val) == m.end()) continue;
			tot += m[val]*it.second;
			if(val == it.first) tot -= it.second;
		}
	cout << tot/2 << '\n';
}
{% endhighlight %}

### [CodeForces 1007A: Reorder the Array](https://codeforces.com/contest/1007/problem/A)
{% highlight c++ %}
#include <bits/stdc++.h>
using namespace std;
map<int,int> m;
int main() {
	int n;
	scanf("%d ",&n);
	int mf = 0;
	for(int i=0;i<n;i++) {
		int t;
		scanf("%d ",&t);
		m[t]++;
		mf = max(m[t],mf);
	}
	printf("%d\n",n-mf);
}
{% endhighlight %}

### [CodeForces 1006C: Three Parts of the Array](https://codeforces.com/contest/1006/problem/C)
{% highlight c++ %}
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int v[200200];
int main() {
	int n;
	scanf("%d ",&n);
	for(int i=0;i<n;i++) {
		int t;
		scanf("%d ",&t);
		v[i] = t;
	}
	int s = 0,e = n;
	ll ma = 0,ss = 0,es = 0;
	while(s < e) {
		if(ss < es) {ss += v[s++];} else {
			es += v[--e];
		}
		if(es == ss) {ma = ss;}
	}
	cout << ma << '\n';
}
{% endhighlight %}

### [CodeForces 1041C: Coffee Break](https://codeforces.com/contest/1041/problem/C)
{% highlight c++ %}
#include <bits/stdc++.h>
using namespace std;
typedef pair<int,int> ii;
typedef pair<int,ii> ti;
typedef vector<ii> vii;
int res[200200];
int main() {
	priority_queue<ii,vector<ii>,greater<ii>> pq;
	vii v;
	int n,m,d;
	scanf("%d %d %d ",&n,&m,&d);
	for(int i=0;i<n;i++) {
		int t;
		scanf("%d ",&t);
		v.push_back({t,i});
	}
	queue<int> av;
	sort(v.begin(),v.end());
	int ma = 0;
	for(int i=0;i<n;i++) {
		int t = v[i].first;
		while(!pq.empty() && pq.top().first+d < t) {
			ii co = pq.top();pq.pop();
			av.push(co.second);
		}
		int id;
		if(av.empty()) {
			id = pq.size();
		} else {
			id = av.front();av.pop();
		}
		ma = max(ma,id);
		res[v[i].second] = id;
		pq.push({t,id});
	}
	printf("%d\n",ma+1);
	for(int i=0;i<n;i++) {
		if(i > 0) printf(" ");
		printf("%d",res[i]+1);
	}
	printf("\n");
}
{% endhighlight %}

### [CodeForces 18C: Stripe](https://codeforces.com/contest/18/problem/C)
{% highlight c++ %}
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int v[100100];
int main() {
	int n;
	scanf("%d ",&n);
	ll sm = 0;
	for(int i=0;i<n;i++) {
		int t;
		scanf("%d ",&t);
		v[i] = t;
		sm += t;
	}
	ll rs = sm;
	int ctr = 0;
	for(int i=0;i<n-1;i++) {
		rs -= v[i];
		if(rs*2 == sm) {ctr++;}
	}
	printf("%d\n",ctr);
}
{% endhighlight %}

### [CodeForces 650A: Watchmen](https://codeforces.com/contest/650/problem/A)
{% highlight c++ %}
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int,int> ii;
map<int,int> mp[2];
map<ii,int> co;
int main() {
	int n;
	scanf("%d ",&n);
	while(n--) {
		int a[2];
		scanf("%d %d ",a,a+1);
		ii c = {a[0],a[1]};
		co[c]++;
		mp[0][a[0]]++;
		mp[1][a[1]]++;
	}
	ll tot = 0;
	for(int i=0;i<2;i++) {
		for(auto& it: mp[i]) {
			ll val = it.second;
			tot += val*(val-1)/2;
		}
	}
	for(auto& it: co) {
		ll val = it.second;
		tot -= val*(val-1)/2;
	}
	cout << tot << '\n';
}
{% endhighlight %}

## Kattis

### [Baloni](https://open.kattis.com/problems/baloni)
{% highlight c++ %}
/*
 * Problem: https://open.kattis.com/problems/baloni
 *
 * Solution by: Trung
 *
 * Since 1 <= H_i <= 1,000,000, we can initialize an array that contains 1M ints.
 * We then get the input one by one to update the number of arrows through each
 * height. At a height h, we check the number of arrow at height h+1, if the value
 * is 0, it means there's no available arrow to pop the current balloon, we need
 * to add a new arrow. Otherwise, we shift an arrow from h+1 to h, to represent
 * an arrow popping the current balloon after going through a balloon of height
 * h+1 earlier in the sequence.
 *
 * Complexity: O(n)
 * A brute force approach also works in C++. I haven't tested in Java.
 */

#include <bits/stdc++.h>

#define ii pair<int,int>
#define pb push_back

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int main (void) {
    ios_base::sync_with_stdio(false);

    int n, result = 0, height;
    vector<int> v (1000010, 0);

    cin >> n;
    while (n--) {
        cin >> height;

        if (v[height] == 0) {   // technically we have to check height+1, but we can save some operations doing this
            v[height-1]++;
        } else {
            v[height]--;
            v[height-1]++;
        }
    }

    for (int i : v) result += i;

    cout << result << endl;

    return 0;
}
{% endhighlight %}

### [JollyJumpers](https://open.kattis.com/problems/jollyjumpers)
{% highlight c++ %}
#include <bits/stdc++.h>
using namespace std;
bitset<3300> bs;
int v[3300];
int main() {
	int n;
	while(scanf("%d ",&n) > 0) {
		for(int i=0;i<n;i++) {
			int t;
			scanf("%d ",&t);
			v[i] = t;
		}
		bs.reset();
		for(int i=0;i<n-1;i++) {
			int df = abs(v[i+1]-v[i]);
			if(df < n && df >= 1) {bs.set(df);}
		}
		//printf("%solly\n",bs.count() == n-1?"J":"Not j");
		if(bs.count() == n-1) {
			printf("Jolly\n");
		} else {
			printf("Not jolly\n");
		}
	}
}
{% endhighlight %}

### [Pivot](https://open.kattis.com/problems/pivot)
{% highlight c++ %}
/*
 * Problem: https://open.kattis.com/problems/pivot
 * Solution by: Trung
 *
 * Given that all n values in the array are distinct.
 * An element at index k (call it a_k) is a pivot if:
 *  a_k > a_i for i from 0 to k
 *  a_k < a_i for i from k to n-1
 * We need to iterate through the array from both ends to find
 * these max/min values at every index.
 *
 * Complexity: O(n)
 */


#include <bits/stdc++.h>

#define ii pair<int,int>
#define pb push_back

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int main (void) {
    ios_base::sync_with_stdio(false);

    int n, result = 0;
    vector<int> arr, left, right;
    cin >> n;
    arr.resize(n);
    left.resize(n, 0);
    right.resize(n, 1e6);

    for (int i = 0; i < n; i++) cin >> arr[i];

    left[0] = arr[0];
    right[n-1] = arr[n-1];

    for (int i = 1; i < n; i++) {
        left[i] = max(arr[i], left[i-1]);
        right[n-i-1] = min(arr[n-i-1], right[n-i]);
    }

    for (int i = 0; i < n; i++) {
        if (arr[i] == left[i] && arr[i] == right[i]) result++;
    }

    cout << result << endl;

    return 0;
}
{% endhighlight %}

### [Backspace](https://open.kattis.com/problems/backspace)
{% highlight c++ %}
/*
 * Problem: https://open.kattis.com/problems/backspace
 * Solution by: Trung
 *
 * Use a stack to simulate, pushing normal characters and popping
 * stack when we encounter a '<'
 * In order to print the result, we need to use vector, or deque.
 * Both container types support stack-like operations (push and pop)
 *
 * Complexity: O(n)
 */

#include <bits/stdc++.h>

#define ii pair<int,int>
#define pb push_back

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int main (void) {
    ios_base::sync_with_stdio(false);

    vector<char> st;
    char c;
    while (cin >> c) {
        if (c == '<') st.pop_back();
        else st.pb(c);
    }

    for (char ch : st) cout << ch;
    cout << endl;

    return 0;
}
{% endhighlight %}

### [Distributing Ballot Boxes](https://open.kattis.com/problems/ballotboxes)
{% highlight c++ %}
/*
 * Problem: https://open.kattis.com/problems/ballotboxes
 * Solution by: Trung
 *
 * This problem can be solved using a greedy approach with max
 * priority queue. First of all, we need to allocate one box to
 * each city. Nodes are compared using the maximum number of people
 * assigned to one box in a city. While there are still boxes,
 * we need to add one box to the city with the current max number
 * of people per box. Our desired result will be stored on top of
 * the priority queue at the end.
 */

#include <bits/stdc++.h>

#define ii pair<int,int>
#define pb push_back
#define iii pair<int, pair<int, int>>

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int main (void) {
    ios_base::sync_with_stdio(false);

    int n, b, i;
    while (cin >> n >> b, n != -1) {
        priority_queue<iii> pq;
        while (n--) {
            cin >> i;
            pq.push({i, {i,1}});
            b--;
        }

        while (b--) {
            ii tmp = pq.top().second;
            int b = tmp.first;
            int c = tmp.second+1;
            int a = ceil(float(b)/c);
            pq.pop();
            pq.push({a, {b,c}});
        }

        int result = pq.top().first;
        cout << result << endl;
    }

    return 0;
}
{% endhighlight %}

### [Candy Division](https://open.kattis.com/problems/candydivision)
{% highlight c++ %}
/*
 * Problem: https://open.kattis.com/problems/candydivision
 * Solution by: Trung
 *
 * Use a set to keep all the factors unique. Set is a binary search
 * tree under the hood. And an iterator will go through elements
 * in an increasing order.
 */

#include <bits/stdc++.h>
#include <cmath>

#define ii pair<int,int>
#define pb push_back

typedef long long ll;
typedef unsigned long long ull;

using namespace std;

int main (void) {
    ios_base::sync_with_stdio(false);

    set<ll> s;
    ll n, div = 1;
    cin >> n;
    ll lim = sqrt(n) + 1;

    while (div < lim) {
        if (n % div == 0) {
            s.insert(div);
            s.insert(n/div);
        }
        div++;
    }

    for (auto it = s.begin(); it != s.end(); it++) cout << *it-1 << " ";

    cout << endl;

    return 0;
}
{% endhighlight %}

### [Compound Words](https://open.kattis.com/problems/compoundwords)
{% highlight c++ %}
#include <bits/stdc++.h>
using namespace std;
vector<string> v;
set<string> bs;
int main() {
	string s;
	while(cin >> s) {
		v.push_back(s);
	}
	for(int i=0;i<v.size();i++) {
		for(int j=0;j<v.size();j++) {
			if(i == j) continue;
			bs.insert(v[i]+v[j]);
		}
	}
	for(auto& it: bs) {
		cout << it << '\n';
	}
}
{% endhighlight %}

## UVa
- [Army Buddies](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=3778)

