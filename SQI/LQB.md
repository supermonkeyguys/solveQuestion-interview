# 3/16 25

## [K倍区间(前缀和 + 同余定理:可整除性)](https://www.lanqiao.cn/problems/97/learning/?page=1&first_category_id=1&second_category_id=3&difficulty=20)

```c++
#include <iostream>
using namespace std;
typedef long long ll;
const int N = 1e5 + 10;

ll qur[N];
ll a[N];

int main()
{
  int n,k;
  cin >> n >> k;

  qur[0] = 1;
  
  ll ans = 0;
  for(int i = 1 ; i <= n ; i ++ ){
    cin >> a[i];
    a[i] += a[i - 1];
	
    //对于同一个除数，如果有两个整数同余，那么它们的差就一定能被这个除数整除。
    //例如519和399对于一个除数同余，那么这个除数一定是519与399的差的因数，
    //即519与399的差一定能被这个除数整除。
    ans += qur[a[i] % k];
    qur[a[i] % k] ++ ;
  }

  cout << ans << endl;
  
  return 0;
}
```

## [分巧克力(二分)](https://www.lanqiao.cn/problems/99/learning/?page=1&first_category_id=1&second_category_id=3&difficulty=20)

```c++
#include <iostream>
using namespace std;
typedef pair<int,int>pp;
const int N = 1e5 + 10;

pp a[N];

int main()
{
  
  int n,k;
  cin >> n >> k;
  for(int i = 1 ; i <= n ; i ++ )cin >> a[i].first >> a[i].second;

  int left = 1 , right = 1e5;
  int res = 0;
  while(left <= right){
    int mid = (left + right) >> 1;

    int ans = 0;
    for(int i = 1 ; i <= n ; i ++ ){
        ans += (a[i].first / mid) * (a[i].second / mid);
    }

    if(ans < k){
      // res = mid;
      right = mid - 1;
    }
    else {
      res = mid;
      left = mid + 1;
    }
    
  }
  cout << res << endl;


  return 0;
}
```

## [拉马车(模拟)](https://www.lanqiao.cn/problems/101/learning/?page=1&first_category_id=1&second_category_id=3&difficulty=20)

```c++
#include<bits/stdc++.h>
using namespace std;

bool cnt[128];

int main()
{
	string a,b;
	cin >> a >> b;
	stack<char>pvp;
	pvp.push(a[0]) , cnt[a[0] - 0] = true , a.erase(0,1);
	//true ÎªA , false ÎªB 
	bool flag = false;
	int times = 1;
	
	while(a.length() && b.length() && times < 100000){
		
		string* now = flag ? &a : &b;
		char temp = (*now)[0];
		
		pvp.push(temp);
		now->erase(0,1);
		
		if(!cnt[temp - 0]){
			cnt[temp - 0] = true;
			flag = !flag;
		}
		else {
			*now += pvp.top();
			pvp.pop();
			while(pvp.top() != temp){
				*now += pvp.top();
				cnt[pvp.top() - 0] = false;
				pvp.pop();
			}
			*now += pvp.top();
			cnt[pvp.top() - 0] = false;
			pvp.pop();
		}
		times ++ ;
	} 
	
	if(times > 100000)cout << -1 << endl;
	else if(a.length())cout << a << endl;
	else cout << b << endl;
	
	return 0;
 } 
```

## [分考场(dfs)](https://www.lanqiao.cn/problems/109/learning/?page=1&first_category_id=1&second_category_id=3&difficulty=20)

```c++
// #include<bits/stdc++.h>
#include<iostream>
using namespace std;
const int N = 110;

int n,m;
int relation[N][N];
int room[N][N];
int minRes = N;

//第roomNum间教室 , 第x个人
void dfs(int x,int roomNum){
	//当前搜索到的房间大于等于答案时返回
  	if(roomNum >= minRes)return;
	if(x > n){
		minRes = min(minRes,roomNum);
		return;
	}
	int j;
    //对每个教室进行搜索
	for(int i = 1 ; i <= roomNum ; i ++ ){
		j = 1;
        //当前位置有人 且 两人互不相识
		while(room[i][j] && !relation[x][room[i][j]]) j ++ ;
		
		if(room[i][j] == 0){
			room[i][j] = x;
			dfs(x + 1,roomNum);
			room[i][j] = 0;//回溯
		}
	}
	//当超出能做位置 设置新考场
	room[roomNum + 1][1] = x;
	dfs(x + 1,roomNum + 1);
	room[roomNum + 1][1] = 0;
}


int main()
{
	scanf("%d %d",&n,&m);
	for(int i = 1 ; i <= m ; i ++ ){
		int u,v;
		scanf("%d %d",&u,&v);
		relation[u][v] = 1;
		relation[v][u] = 1;
	}
	
	dfs(1,1);
	
	cout << minRes << endl;
	
	return 0;
 } 
```

## [小紫的优势博弈]([D-小紫的优势博弈_牛客周赛 Round 85](https://ac.nowcoder.com/acm/contest/103948/D))

```c++
#include<iostream>
using namespace std;

int main()
{
	int n;
	string a;
    cin >> n;
	cin >> a;
	
	double win = 0;
	for(int i = 1 ; i < n + 1  ; i ++ ){
		int cnt0 = 0;
		int cnt1 = 0;
		for(int j = i ; j < n ; j ++ ){
			if(a[j] == '0')cnt0 ++ ;
			else cnt1 ++ ;
			if(!(cnt0 % 2 + cnt1 % 2)){
				win ++ ;
				break;
			}
		}
	} 
	
	printf("%lf", win / n);
		
	return 0;
 } 
```

## [B. Having Been a Treasurer in the Past, I Help Goblins Deceive]([Problem - 2072B - Codeforces](https://codeforces.com/problemset/problem/2072/B))

```c++
#include<iostream>
using namespace std;

int main()
{
	int T;
	cin >> T;
	while(T -- ){
		int n;
		cin >> n;
		string a;
		cin >> a;
		long long cnt0 = 0 , cnt1 = 0;
		for(int i = 0 ; i < a.size() ; i ++ ){
			if(a[i] == '-')cnt0 ++ ;
			else if(a[i] == '_')cnt1 ++ ;	
		}
		long long ans = (cnt0 / 2) * (cnt0 - cnt0 / 2) * cnt1;
		cout << ans << endl;
	} 
	
	return 0;
 } 
```

# 3/17 25

## [砍柴(动态+博弈)]([12.砍柴 - 蓝桥云课](https://www.lanqiao.cn/problems/19722/learning/?page=22&first_category_id=1&second_category_id=3&difficulty=20&status=1))

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;

vector<int>prime;

bool  isprime(int x){
	for(int i = 2 ; i * i <= x ; i ++ ){
		if(x % i == 0)return false;
	}
	return true;
} 

void getPrime(){
	for(int i = 2 ; i <= N ; i ++ ){
		if(isprime(i))prime.push_back(i);
	}
}

int main()
{
	int T;
	cin >> T;
	vector<int>dp(N,0);
	getPrime();
	
	for(int i = 1 ; i <= N ; i ++ ){
		for(int j = 0 ; j < prime.size() && i - prime[j] >= 0 ; j ++ ){
			//如果减了一个质数后是必输局(说明小蓝选a[j]这个质数留给小桥的是必输的局面，小蓝赢)
			if(dp[i - prime[j]] == 0){
				dp[i] = 1;
				break; 
			}
		}
	}
	
	while(T -- ){
		//小蓝(1) 小乔(0) 
		int x;
		cin >> x;
		cout << dp[x] << endl;
	}
	
	return 0;
}
```

## [回文数组(贪心+思维)]([5.回文数组 - 蓝桥云课](https://www.lanqiao.cn/problems/19715/learning/?page=22&first_category_id=1&second_category_id=3&difficulty=20&status=1))

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 1e6 + 10;
//记得开longlong
long long a[N] , b[N] , ans;

int main()
{
	int n;
	cin >> n;
	for(int i = 1 ; i <= n ; i ++ )cin >> a[i];
	
	for(int i = 1 ; i <= n / 2 ; i ++ )b[i] = a[n - i + 1] - a[i]; 
	
	for(int i = 1 ; i <= n / 2 ; i ++ ){
		if(b[i] > 0 && b[i + 1] > 0){
			if(b[i + 1] > b[i])ans += min(b[i],b[i + 1]) , b[i + 1] -= b[i];
			else ans += max(b[i],b[i + 1]) , i ++ ;
		}
		else if(b[i] < 0 && b[i + 1] < 0){
			if(b[i + 1] < b[i])ans += abs(max(b[i],b[i + 1])) , b[i + 1] -= b[i];
			else ans += abs(min(b[i],b[i + 1])), i ++ ;
		}
		else ans += abs(b[i]);
	}
	
	cout << ans << endl;
	
	return 0;
}
```

## [D. Test of Love]([Problem - D - Codeforces](https://codeforces.com/contest/1992/problem/D))

```c++
#include<bits/stdc++.h>
using namespace std;


int main()
{
	int T;
	cin >> T;
	while(T -- ){
		int n,m,k;
		cin >> n >> m >> k;
		string a;
		cin >> a;
		string way = 'S' + a;
		bool flag = true;
		for(int i = 0 ; i < way.size() ;){
//			while(way[i + 1] == 'L') ++ i;
			if(way[i] == 'W'){
				k -- ;
				i ++ ;
				if(k == -1){
					flag = false;
					break;
				}
				else continue;
			} 
			if(way[i] == 'C'){
				flag = false;
				break;
			}
			
			if(i + m > n)break;
			
			bool isJump = false;
			int  path = -1;
			int j;
			for(j = m ; j >= 1 && i + j <= n ; j -- ){
				if(way[i + j] == 'L'){
					isJump = true;
					break;
				}
				if(way[i + j] == 'W'){
					if(path == -1)path = j;
					else path = max(path,j);	
				}
			}
			
			if(isJump)i += j;
			else if(path != -1)i += path;
			else {
				flag = false;
				break;
			}
			
			if(k == -1){
				flag = false;
				break;
			}
			
		}
		if(flag)cout << "YES" << endl;
		else cout << "NO" << endl;	
	}
	
	return 0;
}
```

# 3/18 25

## [E. Novice's Mistake]([Problem - E - Codeforces](https://codeforces.com/contest/1992/problem/E))

```c++
#include<bits/stdc++.h>
using namespace std;

int weishu(int x){
	int t = 0;
	while(x){
		x /= 10;
		t ++ ;
	}	
	return t;
}

int res[7];
//获取 用n拼接出来的答案(最大6位)
void getRes(int n){
	string N = to_string(n);
	int lenN = N.length();
			
	int j = 0;
	for(int i = 1 ; i <= 6 ; i ++ ){
        //n = 12
        // 1
        // 12
        // 121 
        // 1212
        // 12121
        // 121212
		res[i] = res[i - 1] * 10 + (N[j] - '0');
		j = (j + 1) % lenN;
	} 
}

int main()
{
	int T;
	cin >> T;
	vector<pair<int,int> >ans;
	while(T -- ){
		int n;
		cin >> n;
		getRes(n);
		
		ans.clear();
		for(int a = 1 ; a <= 10000 ; a ++ ){
			for(int i = 1 ; i <= 6 ; i ++ ){
				int b = a * weishu(n) - i;
				if(b >= 1 && b <= min(10000,a * n) && b == n * a - res[i]){
					ans.push_back({a,b});
				}
			}
		}
		cout << ans.size() << endl;
		for(auto p : ans){
			printf("%d %d\n",p.first,p.second);
		}
	}
	
	return 0;
}
```

# 3/19 25

## [C. Numeric String Template]([Problem - C - Codeforces](https://codeforces.com/contest/2000/problem/C))

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 2 * 1e5 + 10;

vector<int>a(N);

int main()
{
	int T;
	cin >> T;
	while(T -- ){
		int n;
		cin >> n;
		string mod;
		for(int i = 0 ; i < n ; i ++ )cin >> a[i];
		int m;
		cin >> m;
		while(m -- ){
			
			cin >> mod;
			
			if(mod.size() != n){
				cout << "NO" << endl;
				continue;
			}
			
			map<char,int>mp;
			map<int,char>mp1;
			bool flag = true;
			
			for(int i = 0 ; i < n ; i ++ ){
				int num = a[i];
				char s = mod[i];
				if(mp.count(s) && mp[s] != num){
					flag = false;
					break;
				}
				if(mp1.count(num) && mp1[num] != s){
					flag = false;
					break;
				}
				mp[s] = num , mp1[num] = s;
			}
			
			if(flag)cout << "YES" << endl;
			else cout << "NO" << endl;
		}
			
	}
	
	return 0;
}
```

![](C:\Users\30292\Desktop\SQI\SQI\lqb_img\Snipaste_2025-03-19_16-20-06.png)

![](C:\Users\30292\Desktop\SQI\SQI\lqb_img\Snipaste_2025-03-19_16-20-45.png)

## [E. Photoshoot for Gorillas]([Problem - E - Codeforces](https://codeforces.com/contest/2000/problem/E))

```c++
 #include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 2 * 1e5 + 10;

bool cmp(ll a,ll b){
	return a > b;
}

int main()
{
	int T;
	cin >> T;
	while(T -- ){
		int n,m,k,w;
		cin >> n >> m >> k >> w;
		vector<ll>mk(w);
		for(int i = 0; i < w ; i ++ ){
			cin >> mk[i];
		}
		
		vector<vector<ll> >sp(n + 2,vector<ll>(m + 2,0));
		for(int i = 1 ; i <= n - k + 1 ; i ++ ){
			for(int j = 1 ; j <= m - k + 1 ; j ++ ){
				int x = i , y = j , X = min(n,i + k - 1) , Y = min(m,j + k - 1);
				sp[x][y] ++ ;
				sp[X + 1][y] -- ;
				sp[x][Y + 1] -- ;
				sp[X + 1][Y + 1] ++ ;
//				cout << sp[i][j] << ' ';
			}
//			cout << endl;
		}
		vector<ll>maxCnt;
		for(int i = 1 ; i <= n ; i ++ ){
			for(int j = 1 ; j <= m ; j ++ ){
				sp[i][j] += sp[i - 1][j] + sp[i][j - 1] - sp[i - 1][j - 1];
//				cout << sp[i][j] << ' ';
				maxCnt.push_back(sp[i][j]);
			}
		}
		sort(maxCnt.begin(),maxCnt.end(),cmp);
		sort(mk.begin(),mk.end(),cmp);
		ll ans = 0;
		for(int i = 0 ; i < min(mk.size(),maxCnt.size()) ; i ++ ){
//			cout << maxCnt[i] << endl;
			ans += mk[i] * maxCnt[i];
		}

		cout << ans << endl;

	}
	
	return 0;
}
```

## [冶炼金属](https://www.lanqiao.cn/problems/3510/learning/?page=2&first_category_id=1&second_category_id=3&status=1&tags=2023&tag_relation=union)

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 1e4 + 10;

int get(int a,int b){
	int l = 0 , r = 1e9;
	while(l < r){
		int mid = (l + r) >> 1;
		if(a / mid > b)l = mid + 1;
		else r = mid;
	}
	return l;
}

int main()
{
	int n;
	cin >> n;
	int minV = 0 , maxV = 1e9;
	int a,b;
	for(int i = 1 ; i <= n ; i ++ ){
		cin >> a >> b;
		minV = max(minV,get(a,b));
		maxV = min(maxV,get(a,b - 1) - 1);
	}
	
	cout << minV << ' ' << maxV << endl;
	
	return 0;
}
```

## [训练士兵](https://www.lanqiao.cn/problems/19703/learning/?page=1&first_category_id=1&second_category_id=3&status=1&tag_relation=union&name=%E8%AE%AD%E7%BB%83)

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e6 + 10;

ll cost[N];

int main()
{
	ll n,s;
	cin >> n >> s;
	ll sumCost = 0;
	int maxT = 0;
	for(int i = 1 ; i <= n ; i ++ ){
		int p ,c;
		cin >> p >> c;
		sumCost += p;
		cost[c] += p;
		maxT = max(maxT,c);
	}	
	
	ll ans = 0;
	for(int i = 1 ; i <= maxT ; i ++ ){
		if(sumCost > s){
			ans += s;
		}
		else ans += sumCost;
		sumCost -= cost[i];
	}
	
	cout << ans << endl;
	
	return 0;
}
```

# 3/20 25

## [C. Anya and 1100](https://codeforces.com/contest/2036/problem/C)

```c++
#include<bits/stdc++.h>
using namespace std;

int len;
string mod;

bool check(int i){
	if(i < 0)return false;
	if(i > len - 3)return false;
	if(mod[i] == '1' && mod[i + 1] == '1' && mod [i + 2] == '0' && mod[i + 3] == '0')return true;
	return false; 
}

int main()
{
	int T;
	cin >> T;
	while(T -- ){
		cin >> mod;
		len = mod.size();
		int count = 0;
		for(int i = 0 ; i < len ; i ++ ){
			if(check(i))count ++ ;	
		}
		int q;
		cin >> q;
		while(q -- ){
			int i,v;
			cin >> i >> v;
			i -- ;
			if(mod[i] != '0' + v){
				bool prev = check(i - 3) || check(i - 2) || check(i - 1) || check(i);
				mod[i] = '0' + v;
				bool next = check(i - 3) || check(i - 2) || check(i - 1) || check(i);
				count += next - prev;
			}
			if(count)cout << "YES" << endl;
			else cout << "NO" << endl;
		}
		
	}
	
	return 0;
}
```

## [E. Reverse the Rivers](https://codeforces.com/contest/2036/problem/E)

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;

int main()
{
	int n,k,q;
	cin >> n >> k >> q;
	vector<vector<int> >a(k,vector<int>(n,0));
	
	for(int i = 0 ; i < n ; i ++ ){
		for(int j = 0 ; j < k ; j ++ ){
			cin >> a[j][i];
            //经过 或运算 后 单组数组内的数据将会是单调递增的（或运算性质）
			if(i >= 1)a[j][i] |= a[j][i - 1];
		}
	}
	
	while(q -- ){
		int m;
		cin >> m;
		int sta = 0 , end = 2e9 + 10;
        //寻找答案区间
        //答案要求编号最小（答案为sta）
		while(m -- ){
			int p1,p2;
			char op;
			cin >> p1 >> op >> p2;
			p1 -- ;
            //约束答案区间末尾
			if(op == '>'){
				int differ = upper_bound(a[p1].begin(),a[p1].end(),p2) - a[p1].begin();
				sta = max(differ,sta);
			}
            //约束答案区间起点
			else {
				int differ = lower_bound(a[p1].begin(),a[p1].end(),p2) - a[p1].begin();
				end = min(differ - 1,end);
			}
		}
        //数组起始点为0 +1
		if(sta <= end && sta < n)cout << sta + 1 << endl;
		else cout << -1 << endl;
	}
	
	
	return 0;
}
```

## 爆气球

![](C:\Users\30292\Desktop\SQI\SQI\lqb_img\Snipaste_2025-03-20_20-13-46.png)

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;

int a[N];

int main()
{
	int n,h;
	cin >> n >> h;
	vector<int>p(n);
	for(int i = 0 ; i < n ; i ++ )cin >> p[i];
	
	int ans = 0;
	int pos = 0;
	for(int i = 0 ; i < n ; i ++ ){
		int limit = p[i] + h;
		int temp = 0;
		int sta = i;
		while(sta < n && p[sta] <= limit){
			if(p[sta] <= limit)temp ++ ;
			sta ++ ;
		}
		if(temp > ans){
			ans = temp;
			pos = sta;
		}
	}
	
	cout << p[pos - 1] - h << ' ' << ans << endl;
	
	return 0;
}
```

# 3/21 25

## [路径之谜(DFS+剪枝)](https://www.lanqiao.cn/problems/89/learning/?page=1&first_category_id=1&second_category_id=3&tag_relation=union&tags=DFS,BFS,%E5%89%AA%E6%9E%9D,%E6%90%9C%E7%B4%A2,%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2,%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92,%E9%80%92%E6%8E%A8,01%E8%83%8C%E5%8C%85,%E5%8C%BA%E9%97%B4DP,%E6%A0%91%E5%BD%A2DP,%E7%8A%B6%E5%8E%8BDP,%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98,%E6%A0%91,%E5%AD%97%E7%AC%A6%E4%B8%B2,%E8%B4%AA%E5%BF%83,%E4%BA%8C%E5%88%86,%E5%8F%8C%E6%8C%87%E9%92%88,%E6%9A%B4%E5%8A%9B,%E6%9E%84%E9%80%A0,%E8%A7%84%E5%BE%8B,%E6%80%9D%E7%BB%B4)

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 30;

int dx[4] = {1,0,-1,0};
int dy[4] = {0,-1,0,1};

int p1[N];
int p2[N];

int k[N];
int j[N];

int n;
int q[N * N];
bool isPass[N][N];
bool isFind = false;

void dfs(int x,int y,int index){
	
	if(x == n - 1 && y == n - 1){
		for(int i = 0 ; i < n ; i ++ ){
			if(p1[i] != k[i])return;
			if(p2[i] != j[i])return;
//			cout << p1[i] << ' ' << k[i] << endl;
		}
//		cout << index << endl;
//		for(int i = 0 ; i < n ; i ++ )cout << k[i] << ' ' << p1[i] << endl;
		
		for(int i = 0 ; i < index ; i ++ )cout << q[i] << ' ';
		isFind = true;
		return;
	}	
	
	for(int i = 0 ; i < 4 ; i ++ ){
		int x1 = dx[i] + x;
		int y1 = dy[i] + y;
		if(x1 < 0 || y1 < 0 || x1 >= n || y1 >= n || isPass[x1][y1])continue;
//		cout << x1 << ' ' << y1 << endl;
		if(j[x1] + 1 > p2[x1] || k[y1] + 1 > p1[y1])continue;
		j[x1] ++ , k[y1] ++ ;
		q[index] = x1 * n  + y1;
		isPass[x1][y1] = true;
		dfs(x1,y1,index + 1);
		if(isFind)return;
		isPass[x1][y1] = false;
		q[index] = 0;
		j[x1] -- , k[y1] -- ;
	}
}

int main()
{
	cin >> n;
	for(int i = 0 ; i < n ; i ++ )cin >> p1[i];
	for(int i = 0 ; i < n ; i ++ )cin >> p2[i];
	k[0] ++ ;
	j[0] ++ ;
	isPass[0][0] = true;
	dfs(0,0,1);
	
	return 0;
}
```

## [火车运输(DP)](https://www.lanqiao.cn/problems/17117/learning/?page=14&first_category_id=1&second_category_id=3&tag_relation=union&tags=DFS,BFS,%E5%89%AA%E6%9E%9D,%E6%90%9C%E7%B4%A2,%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2,%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92,%E9%80%92%E6%8E%A8,01%E8%83%8C%E5%8C%85,%E5%8C%BA%E9%97%B4DP,%E6%A0%91%E5%BD%A2DP,%E7%8A%B6%E5%8E%8BDP,%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98,%E6%A0%91,%E5%AD%97%E7%AC%A6%E4%B8%B2,%E8%B4%AA%E5%BF%83,%E4%BA%8C%E5%88%86,%E5%8F%8C%E6%8C%87%E9%92%88,%E6%9A%B4%E5%8A%9B,%E6%9E%84%E9%80%A0,%E8%A7%84%E5%BE%8B,%E6%80%9D%E7%BB%B4)

```c++
#include<bits/stdc++.h>
using namespace std;

int main()
{
	int n,a,b;
	cin >> n >> a >> b;
	vector<int>w(n);
	int sum = 0;
	for(int i = 0 ; i < n ; i ++ )cin >> w[i] , sum += w[i];
	
	vector<vector<int> >dp(1010,vector<int>(1010,0));

	for(int i = 0 ; i < n ; i ++ ){
		for(int j = a ; j >= 0 ; j -- ){
			for(int k = b ; k >= 0 ; k -- ){
				if(j >= w[i])dp[j][k] = max(dp[j][k],dp[j - w[i]][k] + w[i]);
				if(k >= w[i])dp[j][k] = max(dp[j][k],dp[j][k - w[i]] + w[i]);
			}
		}
	}

	cout << dp[a][b] << endl;
	
	return 0;
}
```

## [日期问题(暴力+枚举)](https://www.lanqiao.cn/problems/103/learning/?page=1&first_category_id=1&second_category_id=3&tag_relation=union&tags=DFS,BFS,%E5%89%AA%E6%9E%9D,%E6%90%9C%E7%B4%A2,%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2,%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92,%E9%80%92%E6%8E%A8,01%E8%83%8C%E5%8C%85,%E5%8C%BA%E9%97%B4DP,%E6%A0%91%E5%BD%A2DP,%E7%8A%B6%E5%8E%8BDP,%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98,%E6%A0%91,%E5%AD%97%E7%AC%A6%E4%B8%B2,%E8%B4%AA%E5%BF%83,%E4%BA%8C%E5%88%86,%E5%8F%8C%E6%8C%87%E9%92%88,%E6%9A%B4%E5%8A%9B,%E6%9E%84%E9%80%A0,%E8%A7%84%E5%BE%8B,%E6%80%9D%E7%BB%B4)

```c++
#include<bits/stdc++.h>
using namespace std;

int mon[13] = {0,31,28,31,30,31,30,31,31,30,31,30,31};

bool ru(int x){
	if(x % 4 == 0 && x % 100 != 0)return true;
	if(x % 400 == 0)return true;
	return false;
}

int main()
{
	int a,b,c;
	scanf("%02d/%02d/%02d",&a,&b,&c);
	
	for(int i = 1960 ; i <= 2059 ; i ++ ){
		for(int j = 1 ; j <= 12 ; j ++ ){
			int date = mon[j];
			if(j == 2 && ru(i))date ++ ;
			for(int k = 1 ; k <= date ; k++ ){
				if(i % 100 == a && b == j && c == k)printf("%d-%02d-%02d\n",i,b,c);
				else if(i % 100 == c && b == j && a == k)printf("%d-%02d-%02d\n",i,b,a);
				else if(i % 100 == c && a == j && b == k)printf("%d-%02d-%02d\n",i,a,b);
			}
		}
	}
	
	return 0;
}
```

## [好数(暴力+枚举)](https://www.lanqiao.cn/problems/19709/learning/?page=16&first_category_id=1&second_category_id=3&tag_relation=union&tags=DFS,BFS,%E5%89%AA%E6%9E%9D,%E6%90%9C%E7%B4%A2,%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2,%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92,%E9%80%92%E6%8E%A8,01%E8%83%8C%E5%8C%85,%E5%8C%BA%E9%97%B4DP,%E6%A0%91%E5%BD%A2DP,%E7%8A%B6%E5%8E%8BDP,%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98,%E6%A0%91,%E5%AD%97%E7%AC%A6%E4%B8%B2,%E8%B4%AA%E5%BF%83,%E4%BA%8C%E5%88%86,%E5%8F%8C%E6%8C%87%E9%92%88,%E6%9A%B4%E5%8A%9B,%E6%9E%84%E9%80%A0,%E8%A7%84%E5%BE%8B,%E6%80%9D%E7%BB%B4)

```c++
#include<bits/stdc++.h>
using namespace std;

int main()
{
	int n;
	cin >> n;
	int ans = 0;
	for(int i = n ; i >= 1 ; i -- ){
		for(int j = i ; j > 0 ;){
			if(j % 2 != 0)j /= 10;
			else break;
			if(j % 2 == 0)j /= 10;
			else break;
			if(j == 0)ans ++ ;
		}
	}
	
	cout << ans << endl;
	
	return 0;
}
```

## [商品库存管理](https://www.lanqiao.cn/problems/19716/learning/?page=35&first_category_id=1&second_category_id=3&tag_relation=union&tags=DFS,BFS,%E5%89%AA%E6%9E%9D,%E6%90%9C%E7%B4%A2,%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2,%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92,%E9%80%92%E6%8E%A8,01%E8%83%8C%E5%8C%85,%E5%8C%BA%E9%97%B4DP,%E6%A0%91%E5%BD%A2DP,%E7%8A%B6%E5%8E%8BDP,%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98,%E6%A0%91,%E5%AD%97%E7%AC%A6%E4%B8%B2,%E8%B4%AA%E5%BF%83,%E4%BA%8C%E5%88%86,%E5%8F%8C%E6%8C%87%E9%92%88,%E6%9A%B4%E5%8A%9B,%E6%9E%84%E9%80%A0,%E8%A7%84%E5%BE%8B,%E6%80%9D%E7%BB%B4,%E5%9B%BE%E8%AE%BA,%E5%89%8D%E7%BC%80%E5%92%8C,%E5%B7%AE%E5%88%86,%E6%9E%9A%E4%B8%BE,%E6%A8%A1%E6%8B%9F,%E9%80%92%E5%BD%92,2020,2021,2022,2023,2024,2019,%E5%9B%BD%E8%B5%9B)

**暴力O(n * m)**

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 3 * 1e5 + 10;

int l[N] , r[N];
int pp[N];

int main()
{
	int n,m;
	cin >> n >> m;
	for(int i = 1 ; i <=  m ; i ++ ){
		cin >> l[i] >> r[i];
		pp[l[i]] ++ ;
		pp[r[i] + 1] -- ;
	}
	int ans = 0;
	for(int i = 1 ; i <= n ; i ++ ){
		pp[i] += pp[i - 1];
		if(!pp[i])ans ++ ;
	}

	for(int i = 1 ; i <= m ; i ++ ){
		int res = ans;
		for(int j = l[i] ; j <= r[i] ; j ++ ){
			if(pp[j] == 1)res ++ ;
		}
		cout << res << endl;
	}
	
	return 0;
}
```

**O(n + m)**

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 3 * 1e5 + 10;

int l[N] , r[N];
int pp[N];
int cnt[N];

int main()
{
	int n,m;
	cin >> n >> m;
	for(int i = 1 ; i <=  m ; i ++ ){
		cin >> l[i] >> r[i];
		pp[l[i]] ++ ;
		pp[r[i] + 1] -- ;
	}
	int ans = 0;
	for(int i = 1 ; i <= n ; i ++ ){
		pp[i] += pp[i - 1];
		if(!pp[i])ans ++ ;
		cnt[i] += cnt[i - 1] + (pp[i] <= 1);
	}

	for(int i = 1 ; i <= m ; i ++ ){
		cout << cnt[r[i]] - cnt[l[i] - 1] + ans << endl;
	}
	
	return 0;
}
```

## [回文字符串(思维)](https://www.lanqiao.cn/problems/19718/learning/?page=35&first_category_id=1&second_category_id=3&tag_relation=union&tags=DFS,BFS,%E5%89%AA%E6%9E%9D,%E6%90%9C%E7%B4%A2,%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2,%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92,%E9%80%92%E6%8E%A8,01%E8%83%8C%E5%8C%85,%E5%8C%BA%E9%97%B4DP,%E6%A0%91%E5%BD%A2DP,%E7%8A%B6%E5%8E%8BDP,%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98,%E6%A0%91,%E5%AD%97%E7%AC%A6%E4%B8%B2,%E8%B4%AA%E5%BF%83,%E4%BA%8C%E5%88%86,%E5%8F%8C%E6%8C%87%E9%92%88,%E6%9A%B4%E5%8A%9B,%E6%9E%84%E9%80%A0,%E8%A7%84%E5%BE%8B,%E6%80%9D%E7%BB%B4,%E5%9B%BE%E8%AE%BA,%E5%89%8D%E7%BC%80%E5%92%8C,%E5%B7%AE%E5%88%86,%E6%9E%9A%E4%B8%BE,%E6%A8%A1%E6%8B%9F,%E9%80%92%E5%BD%92,2020,2021,2022,2023,2024,2019,%E5%9B%BD%E8%B5%9B)

```c++
#include<bits/stdc++.h>
using namespace std;
//找到第一个和最后一个不是(l,q,b)的字母下标
bool solve(){
	vector<int>pp;
	string s;
	cin >> s;
	for(int i = 0 ; i < s.size() ; i ++ ){
		if(s[i] != 'l' && s[i] != 'q' && s[i] != 'b')pp.push_back(i);
	}
	
	if(!pp.size())return true;
	int l = pp[0] , r = pp[pp.size() - 1];
	while(l <= r && s[l] == s[r])l ++ , r -- ;
	int L = pp[0] , R = pp[pp.size() - 1];
	while(L >= 0 && R < s.length() && s[L] == s[R])L -- , R ++ ;
	 
	return L < 0 && l > r;
}

int main()
{
	int n;
	cin >> n;
	while(n -- ){
		if(solve())cout << "Yes" << endl;
		else cout << "No" << endl;
	}
	
	return 0;
}
```

## [翻转(线性DP)](https://www.lanqiao.cn/problems/18427/learning/?page=34&first_category_id=1&second_category_id=3&tag_relation=union&tags=DFS,BFS,%E5%89%AA%E6%9E%9D,%E6%90%9C%E7%B4%A2,%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2,%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92,%E9%80%92%E6%8E%A8,01%E8%83%8C%E5%8C%85,%E5%8C%BA%E9%97%B4DP,%E6%A0%91%E5%BD%A2DP,%E7%8A%B6%E5%8E%8BDP,%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98,%E6%A0%91,%E5%AD%97%E7%AC%A6%E4%B8%B2,%E8%B4%AA%E5%BF%83,%E4%BA%8C%E5%88%86,%E5%8F%8C%E6%8C%87%E9%92%88,%E6%9A%B4%E5%8A%9B,%E6%9E%84%E9%80%A0,%E8%A7%84%E5%BE%8B,%E6%80%9D%E7%BB%B4,%E5%9B%BE%E8%AE%BA,%E5%89%8D%E7%BC%80%E5%92%8C,%E5%B7%AE%E5%88%86,%E6%9E%9A%E4%B8%BE,%E6%A8%A1%E6%8B%9F,%E9%80%92%E5%BD%92,2020,2021,2022,2023,2024,2019,%E5%9B%BD%E8%B5%9B)

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;

//0±íÊ¾²»·­×ª,1±íÊ¾·­×ª 
int dp[N][2];

int main()
{
	int n;
	cin >> n;
	string prev;
	for(int i = 1 ; i <= n ; i ++ ){
		string now;
		cin >> now;
    	// cout << now << endl;
		if(i > 1){
			bool t1 = now[0] == prev[1];
      bool t2 = now[0] == prev[0];
      bool t3 = now[1] == prev[1];
      bool t4 = now[1] == prev[0];

			dp[i][0] = min(dp[i - 1][0] + 2 - t1 , dp[i - 1][1] + 2 - t2);
			dp[i][1] = min(dp[i - 1][0] + 2 - t3 , dp[i - 1][1] + 2 - t4);
		}
		else {
			dp[1][0] = 2;
			dp[1][1] = 2;
		}
		// cout << prev << endl;
		prev = now;
	}
	
	cout << min(dp[n][0],dp[n][1]) << endl;
	
	return 0;
}
```

## [最大阶梯(DP)](https://www.lanqiao.cn/problems/17147/learning/?page=34&first_category_id=1&second_category_id=3&tag_relation=union&tags=DFS,BFS,%E5%89%AA%E6%9E%9D,%E6%90%9C%E7%B4%A2,%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2,%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92,%E9%80%92%E6%8E%A8,01%E8%83%8C%E5%8C%85,%E5%8C%BA%E9%97%B4DP,%E6%A0%91%E5%BD%A2DP,%E7%8A%B6%E5%8E%8BDP,%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98,%E6%A0%91,%E5%AD%97%E7%AC%A6%E4%B8%B2,%E8%B4%AA%E5%BF%83,%E4%BA%8C%E5%88%86,%E5%8F%8C%E6%8C%87%E9%92%88,%E6%9A%B4%E5%8A%9B,%E6%9E%84%E9%80%A0,%E8%A7%84%E5%BE%8B,%E6%80%9D%E7%BB%B4,%E5%9B%BE%E8%AE%BA,%E5%89%8D%E7%BC%80%E5%92%8C,%E5%B7%AE%E5%88%86,%E6%9E%9A%E4%B8%BE,%E6%A8%A1%E6%8B%9F,%E9%80%92%E5%BD%92,2020,2021,2022,2023,2024,2019,%E5%9B%BD%E8%B5%9B)

**每个更大的阶梯可以看成 两个小阶梯重叠一角形成 + 当前块组成**

**把大拆小**

***DP思想：1.分解问题 2.求解子问 3.状态定义与状态转移方程 4.自底向上(迭代) 或 自顶向下求解(递归+记忆化--记录子问题)***

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 1010;

int mp[N][N];
int dp[N][N];

int n;

void initDp(){
	for(int i = 1 ; i <= n ; i ++ ){
		for(int j = 1; j <= n ; j ++ ){
			dp[i][j] = 1;
		}
	}
}


int main()
{
	cin >> n;
	for(int i = 1 ; i <= n ; i ++ )
		for(int j = 1 ; j <= n ; j ++ )
			cin >> mp[i][j];
	
	int ans = 0;
	
	initDp();
	for(int i = 1 ; i <= n ; i ++ ){
		for(int j = 1 ; j <= n ; j ++ ){
			if(mp[i][j] == mp[i][j - 1] && mp[i][j] == mp[i - 1][j])
			dp[i][j] = min(dp[i][j - 1],dp[i - 1][j]) + 1;
			ans = max(dp[i][j],ans);
		}
	}
	
	initDp();
	for(int i = 1 ; i <= n ; i ++ ){
		for(int j = n ; j >= 1 ; j -- ){
			if(mp[i][j] == mp[i][j + 1] && mp[i][j] == mp[i - 1][j]){
				dp[i][j] = min(dp[i][j + 1],dp[i - 1][j]) + 1;
				ans = max(dp[i][j],ans);
			} 
		}
	}
	
	initDp();
	for(int i = n ; i >= 1 ; i -- ){
		for(int j = 1 ; j <= n ; j ++ ){
			if(mp[i][j] == mp[i + 1][j] && mp[i][j] == mp[i][j - 1]){
				dp[i][j] = min(dp[i + 1][j],dp[i][j - 1]) + 1;
				ans = max(dp[i][j],ans);
			}
		}
	}
	
	initDp();
	for(int i = n ; i >= 1 ; i -- ){
		for(int j = n ; j >= 1 ; j -- ){
			if(mp[i][j] == mp[i + 1][j] && mp[i][j] == mp[i][j + 1]){
				dp[i][j] = min(dp[i + 1][j],dp[i][j + 1]) + 1;
				ans = max(dp[i][j],ans);
			}
		}
	}
	
	cout << ans << endl;
	
	return 0;
 } 
```

# 3/22 25

## [弹珠堆放(数学-等差公式 + 前缀和)](https://www.lanqiao.cn/problems/17142/learning/?page=34&first_category_id=1&second_category_id=3&tag_relation=union&tags=DFS,BFS,%E5%89%AA%E6%9E%9D,%E6%90%9C%E7%B4%A2,%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2,%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92,%E9%80%92%E6%8E%A8,01%E8%83%8C%E5%8C%85,%E5%8C%BA%E9%97%B4DP,%E6%A0%91%E5%BD%A2DP,%E7%8A%B6%E5%8E%8BDP,%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98,%E6%A0%91,%E5%AD%97%E7%AC%A6%E4%B8%B2,%E8%B4%AA%E5%BF%83,%E4%BA%8C%E5%88%86,%E5%8F%8C%E6%8C%87%E9%92%88,%E6%9A%B4%E5%8A%9B,%E6%9E%84%E9%80%A0,%E8%A7%84%E5%BE%8B,%E6%80%9D%E7%BB%B4,%E5%9B%BE%E8%AE%BA,%E5%89%8D%E7%BC%80%E5%92%8C,%E5%B7%AE%E5%88%86,%E6%9E%9A%E4%B8%BE,%E6%A8%A1%E6%8B%9F,%E9%80%92%E5%BD%92,2020,2021,2022,2023,2024,2019,%E5%9B%BD%E8%B5%9B)

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 1e6 + 10;

int a[N];


int main()
{
	int n = 20230610;
	
	
	a[1] = 1;
	a[2] = 4;
	a[3] = 10;
	a[4] = 20;
	
	for(int i = 5 ; i <= N ; i ++ ){
		a[i] = (a[i - 1] - a[i - 2]) + i + a[i - 1];
//		cout << a[i] << endl;
		if(n == a[i]){
			cout << i << endl;
			break;
		}
		if(n < a[i] && n >= a[i - 1]){
			cout << i - 1 << endl;
			break;
 		}
	}
	
	return 0;
}
```

## [划分(DP+背包问题)](https://www.lanqiao.cn/problems/17143/learning/?page=34&first_category_id=1&second_category_id=3&tag_relation=union&tags=DFS,BFS,%E5%89%AA%E6%9E%9D,%E6%90%9C%E7%B4%A2,%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2,%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92,%E9%80%92%E6%8E%A8,01%E8%83%8C%E5%8C%85,%E5%8C%BA%E9%97%B4DP,%E6%A0%91%E5%BD%A2DP,%E7%8A%B6%E5%8E%8BDP,%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98,%E6%A0%91,%E5%AD%97%E7%AC%A6%E4%B8%B2,%E8%B4%AA%E5%BF%83,%E4%BA%8C%E5%88%86,%E5%8F%8C%E6%8C%87%E9%92%88,%E6%9A%B4%E5%8A%9B,%E6%9E%84%E9%80%A0,%E8%A7%84%E5%BE%8B,%E6%80%9D%E7%BB%B4,%E5%9B%BE%E8%AE%BA,%E5%89%8D%E7%BC%80%E5%92%8C,%E5%B7%AE%E5%88%86,%E6%9E%9A%E4%B8%BE,%E6%A8%A1%E6%8B%9F,%E9%80%92%E5%BD%92,2020,2021,2022,2023,2024,2019,%E5%9B%BD%E8%B5%9B)

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 5e5 + 9;

ll dp[N >> 1];

int main()
{
	ll a[41] = {0,5160,9191,6410,4657,7492,1531,8854,1253,4520,9231,
	1266 ,4801 ,3484 ,4323 ,5070 ,1789 ,2744 ,5959 ,9426 ,4433,
	4404 ,5291 ,2470 ,8533 ,7608 ,2935 ,8922 ,5273 ,8364 ,8819,
	7374 ,8077 ,5336 ,8495 ,5602 ,6553 ,3548 ,5267 ,9150 ,3309};
	
	ll sum = 0;
	for(int i = 1 ; i <= 40 ; i ++ )sum += a[i];
	
	for(int i = 1 ; i <= 40 ; i ++ ){
		for(int j = sum / 2 ; j >= a[i] ; j -- ){
			dp[j] = max(dp[j],dp[j - a[i]] + a[i]);
		}
	}
	
	cout << dp[sum / 2] *  (sum - dp[sum / 2]) << endl;
	
	return 0;
}

```

# 3/23 25

## [货物摆放](https://www.lanqiao.cn/problems/1463/learning/)

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;

int main()
{
	ll p = 2021041820210418;
	ll ans = 0;
	
	vector<ll>mp;
	for(ll i = 1 ; i * i <= p ; i ++ ){
		if(p % i == 0){
      mp.push_back(i);
      if(i * i != p){
        mp.push_back(p / i);
      }
    }
	} 
	
	for(auto p1 : mp){
		for(auto p2 : mp){
			for(auto p3 : mp){
				if(p1 * p2 * p3 == p)ans ++ ;
			}
		}
	}
	
	cout << ans << endl;
	
	return 0;
}
```

## [砝码称重](https://www.lanqiao.cn/problems/1447/learning/)

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;

int w[110];
int dp[110][N];

int main()
{
	int n;
	cin >> n;
	int maxV = 0;
	for(int i = 1 ; i <= n ; i ++ )cin >> w[i] , maxV += w[i];
	
	for(int i = 1 ; i <= n ; i ++ ){
		for(int j = maxV ; j >= 1 ; j -- ){
			dp[i][j] = dp[i - 1][j];
			if(dp[i][j] == 0){
				if(w[i] == j)dp[i][j] = 1;
				//假设先前有个重量 w = a[i] + j;
				//那么j重量就可以拿 j = w - a[i] 表示 
				if(dp[i - 1][j + w[i]])dp[i][j] = 1;
				//假设先前有个重量 w = abs(j - a[i])
				//那么j重量就可以拿 j = w + a[i] 表示 
				if(dp[i - 1][abs(j - w[i])])dp[i][j] = 1;
			} 
		}
	}
	
	int ans = 0;
	for(int j = 1 ; j <= maxV ; j ++ )if(dp[n][j])ans ++ ;
	
	cout << ans << endl;
	
	return 0;
}
```

# 3/24 25

## [积木画(DP)](https://www.lanqiao.cn/problems/2110/learning/)

```c++
#include<bits/stdc++.h>
#define endl '\n'
using namespace std;
typedef long long ll;
const int N = 1e7 + 10 , mod = 1e9 + 7;

ll dp[N][3];

signed main()
{
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
  dp[0][2] = 1;
	dp[1][2] = 1;

	ll n;
	cin >> n;
	
	for(int i = 2 ; i <= n ; i ++ ){
		dp[i][0] = (dp[i - 1][1] + dp[i - 2][2]) % mod;
		dp[i][1] = (dp[i - 1][0] + dp[i - 2][2]) % mod;
		dp[i][2] = (dp[i - 1][0] + dp[i - 1][1] + dp[i - 1][2] + dp[i - 2][2]) % mod; 
	} 
	
	cout << dp[n][2] << endl;
	
	return 0;
}
```

# 3/25 25

## [统计子矩阵](https://www.lanqiao.cn/problems/2109/learning/)

```c++
#include<iostream>
#define endl '\n'
#define int long long
using namespace std;
const int N = 550;

int a[N][N];

signed main()
{
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int n,m,k;
	cin >> n >> m >> k;
	
	for(int i = 1 ; i <= n ; i ++ ){
		for(int j = 1 ; j <= m ; j ++ ){
			cin >> a[i][j];
		}
	}
	
	for(int i = 1 ; i <= n ; i ++ ){
		for(int j = 1 ; j <= m ; j ++ ){
			a[i][j] += a[i - 1][j] + a[i][j - 1] - a[i - 1][j - 1];
//			cout << a[i][j] << ' ';
		}
//		cout << endl;
	}
	
	int ans = 0;
	for(int i = 1 ; i <= n ; i ++ ){
		for(int j = i ; j <= n ; j ++ ){
        	for(int l = 1 , r = 1 ; r <= m ; r ++ ){
          		while(l <= r){
            		int s = a[j][r] - a[i - 1][r] - a[j][l - 1] + a[i - 1][l - 1];
            		if(s <= k)break;
            		l ++ ;
          		}
          		ans += r - l + 1;
        	}
		}
	}
	
	
	cout << ans << endl;
	
	return 0;
}
```

# 3/26 25

## [班级活动](https://www.lanqiao.cn/problems/17153/learning/)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 1e5 + 10;

int a[N];

signed main()
{ 
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	
	int n;
	cin >> n;
	
	for(int i = 1 ; i <= n ; i ++ ){
		int x;
		cin >> x;
		a[x] ++ ;
	}
	
	int one = 0;
	int two = 0;
	for(int i = 1 ; i <= n ; i ++ ){
		if(a[i] == 1)one ++ ;
		if(a[i] > 2)two += a[i] - 2;
	}
	
	int ans = 0;
	if(two > one){
    ans = two;
  }
  else  {
    ans = (one - two) / 2 + two;
  }
	
	cout << ans << endl;
	
	return 0;
}
```

## [管道](https://www.lanqiao.cn/problems/17153/learning/)

```c++
#include<bits/stdc++.h>
#define endl '\n'
using namespace std;
typedef long long ll;

struct pp{
	ll L;
	ll S;
};

int main()
{
//	ios::sync_with_stdio(0);
//	cin.tie(0);
//	cout.tie(0);
	
	ll n,len;
	cin >> n >> len;
	
	vector<pp>a(n + 1);
	for(int i = 1 ; i <= n ; i ++ )cin >> a[i].L >> a[i].S;
	
//	cout << n << endl;
	ll l = 0 , r = 2e9;
	while(l < r){
		ll mid = (l + r) >> 1;
//		cout << mid << endl;
		vector<pair<ll,ll>>q;
		for(int i = 1 ; i <= n ; i ++ ){
			ll L = a[i].L , S = a[i].S;
			if(a[i].S <= mid){
				ll T = mid - S;
				ll sta = max(1LL,L - T);
				ll end = min(len,L + T);
				q.push_back({sta,end});
			}
		}
		sort(q.begin(),q.end());
		ll sta = -1 , end = -1;
		for(int i = 0 ; i < q.size() ; i ++ ){
			if(end + 1 >= q[i].first){
				end = max(end,q[i].second);
			}
			else {
				sta = q[i].first;
				end = q[i].second;
			}
		}
		bool flag = sta == 1 && end == len;
		if(flag){
			r = mid;
		}
		else {
			l = mid + 1;
		}
//		cout << mid << endl;
	}
	
	cout << r << endl;
	
	return 0;
}
```

## [独木桥](https://www.luogu.com.cn/problem/P1007)

```c++
#include<bits/stdc++.h>
#define endl '\n'
using namespace std;
typedef long long ll;
const int N = 5 * 1e3 + 10;

ll a[N];

int main()
{
	int n,len;
	cin >> len >> n;
	if(n == 0){
		cout << 0 << ' ' << 0 << endl;
		return 0;
	} 
	for(int i = 1 ; i <= n ; i ++ )cin >> a[i];
	
	sort(a + 1,a + 1 + n);
	
	ll minTime = 0 , maxTime = 0 ;
	for(int i = 1 ; i <= n ; i ++ ){
		minTime = max(min(a[i],len + 1 - a[i]),minTime); 
	}
	maxTime = max(len + 1 - a[1],a[n]);
	
	cout << minTime << ' ' << maxTime << endl;
	
	return 0;
}
```

## [轰炸](https://www.luogu.com.cn/problem/P1142)

```c++
#include<bits/stdc++.h>
#define endl '\n'
using namespace std;
typedef long long ll;
const int N = 710;

struct point{
	int x,y;
}point[N];

int main()
{
	ll n;
	cin >> n;
	for(int i = 1 ; i <= n ; i ++){
		cin >> point[i].x >> point[i].y; 
	}
	
	int ans = 0;
	for(int i = 1 ; i < n ; i ++ ){
		for(int j = i + 1 ; j <= n ; j ++ ){
			int vx = point[j].x - point[i].x;
			int vy = point[j].y - point[i].y;
			int cnt = 2;
			for(int k = 1 ; k <= n ; k ++ ){
				if(k == i || k == j)continue;
				int vy1 = point[k].y - point[i].y;
				int vx1 = point[k].x - point[i].x;
				if(vx1 * vy == vy1 * vx)cnt ++ ;
				
			}
			ans = max(ans,cnt);
		}
	}

	cout << ans << endl;
	
	return 0;
}
```

# 3/27 25

## [李白打酒加强版](https://www.luogu.com.cn/problem/P8786)

```c++
#include<bits/stdc++.h>
#define endl '\n'
using namespace std;
const int N = 110 , mod = 1e9 + 7;

int dp[2 * N][N][N];

int main()
{
	int n,m;
	cin >> n >> m;
	
	dp[0][0][2] = 1;
	for(int i = 0 ; i < n + m ; i ++ ){
		for(int j = 0 ; j < m ; j ++ ){
			for(int k = 0 ; k <= m ; k ++ ){
				if(dp[i][j][k]){
					if(k > 0)dp[i + 1][j + 1][k - 1] += dp[i][j][k] % mod;
					if(k <= 50)dp[i + 1][j][2 * k] += dp[i][j][k] % mod;
				}
			}
		}
	}
	
	cout << dp[n + m][m][0] << endl;
	
	return 0;
}
```

## [画中漂流](https://www.luogu.com.cn/problem/P8725)

```c++
#include<bits/stdc++.h>
#define endl '\n'
using namespace std;
const int N = 1510 , mod = 1e9 + 7;

int dp[2 * N][N];

int main()
{
	int d,t,m;
	cin >> d >> t >> m;
	
	dp[0][m] = 1;
	for(int i = 1 ; i <= t ; i ++ ){
		for(int j = 0 ; j <= m ; j ++ ){
			int len = d + (m - j) - (i - (m - j));
			if(len)dp[i][j] = (dp[i - 1][j + 1] + dp[i - 1][j]) % mod;
		}
	}
	
	cout << dp[t][0] << endl;
	
	return 0;
}
```

## [飞机降落](https://www.luogu.com.cn/problem/P9241)

```c++
#include<bits/stdc++.h>
#define endl '\n'
#define int long long
using namespace std;
typedef long long ll;

struct pp{
	int T;
	int D;
	int L;
}a[15]; 

int n;
bool pass[15];
bool isOk;

int path[15];

void dfs(int index,int time){

	if(index > n){
		isOk = true;
		return;
	}
	
	for(int i = 1 ; i <= n ; i ++ ){
		if(index == 1 && !pass[i]){
			pass[i] = true;
//			cout << i << endl;
			int nextTime = a[i].T + a[i].L;
			dfs(index + 1,nextTime);
			pass[i] = false;
		}
		else if(!pass[i] && a[i].T + a[i].D >= time){
			pass[i] = true;
			int nextTime = max(time,a[i].T) + a[i].L;
//			cout << i << endl;
			dfs(index + 1,nextTime);
			pass[i] = false;
		}
		else if(a[i].T + a[i].D < time && !pass[i])return;
	}
	
}

signed main()
{
	int t;
	cin >> t;	
	while(t -- ){
		isOk = false;
		memset(pass,false,sizeof pass);
		
		cin >> n;
		for(int i = 1 ; i <= n ; i ++ ){
			cin >> a[i].T >> a[i].D >> a[i].L;
		}	
		
		dfs(1,0);
		
		if(isOk)cout << "YES" << endl;
		else cout << "NO" << endl;

	}
		
	return 0;
}
```

# 3/28 25

## [跳石头](https://www.luogu.com.cn/problem/P10914)

```c++
#include<bits/stdc++.h>
#define endl '\n'
using namespace std;
const int N = 40010;

bitset<N>dp[N];
int c[N];

int main()
{
	int n;
	cin >> n;
	for(int i = 1 ; i <= n ; i ++ )cin >> c[i];
	
	int ans = 0;
	for(int i = n ; i >= 1 ; i -- ){
		dp[i][c[i]] = 1;
		if(i + c[i] <= n)dp[i] |= dp[i + c[i]];
		if(2 * i <= n){
			dp[i] |= dp[2 * i];
		}
		ans = max(ans,(int)dp[i].count());
	}
	
	cout << ans << endl;
	
	return 0;
}
```

# 3/29 25

![](C:\Users\30292\Desktop\SQI\SQI\lqb_img\Snipaste_2025-03-29_22-21-27.png)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

vector<vector<int>>res;
vector<int>t;
 int n;
 int sum;
 
 void dfs(int index){
 	
 	if(index > 10){
 		if(sum == n)res.push_back(t);
		 return;	
	}
 	
 	for(int i = 1 ; i <= 3 ; i ++ ){
 		t.push_back(i);
 		sum += i;
 		dfs(index + 1);
 		sum -= i;
 		t.pop_back();
	 }
 	
 }
 
 signed main()
 {
 	cin >> n;
 	
 	dfs(1);
 	
 	cout << res.size() << endl;
 	for(int i = 0 ; i < res.size() ; i ++ ){
 		for(int j = 0 ; j < 10 ; j ++ ){
 			cout << res[i][j] << ' ';
		 }
		 cout << endl;
	 }
 	
 	return 0;
 }
```

![](C:\Users\30292\Desktop\SQI\SQI\lqb_img\Snipaste_2025-03-29_22-22-28.png)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 10010;

struct work{
	int val;
	int deadline;
};

bool cmp(struct work a,struct work b){
	if(a.val != b.val)return a.val > b.val;
	return a.deadline < b.deadline;
}

signed main(){
	
	int n;
	cin >> n;
	vector<work>pp; 
	for(int i = 1 ; i <= n ; i ++ ){
		int x,y;
		cin >> x >> y;
		pp.push_back({x,y});
	}
	sort(pp.begin(),pp.end(),cmp);
	
	vector<bool>isDo(N,false);
	int ans = 0;
	for(auto p : pp){
		for(int j = p.deadline ; j >= 1 ; j -- ){
			if(!isDo[j]){
				isDo[j] = true;
				ans += p.val;
				break;
			}
		}
	}
	
	cout << ans << endl;
	
	return 0;
}
```

# 3/30 25

## [AB路线](https://www.luogu.com.cn/problem/P9425)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 1010;

bool vis[N][N][15];
int mp[N][N];
int n,m,k;

struct pp{
	int x;
	int y;
	int cnt;
};


int dx[4] = {0,1,0,-1};
int dy[4] = {1,0,-1,0};

bool check(int x,int y,int cnt){
	bool t1 = (cnt % (2 * k) < k ? 1 : 0) == mp[x][y];
	bool t2 = (x <= n && x > 0 && y <= m && y > 0);
	bool t3 = vis[x][y][cnt % k];
	
	return t1 && t2 && !t3;
}


void bfs(){
	
	queue<pp>q;
	q.push({1,1,0});
	vis[1][1][0] = true;
	
	while(!q.empty()){
		auto t = q.front();
		q.pop();
		
		if(t.x == n && t.y == m){
			cout << t.cnt;
			return;
		}		
	
		for(int i = 0 ; i < 4 ; i ++ ){
			int x1 = t.x + dx[i];
			int y1 = t.y + dy[i];
			if(check(x1,y1,t.cnt + 1)){
				q.push({x1,y1,t.cnt + 1});
				vis[x1][y1][(t.cnt + 1) % k] = true;
			}
		}
	}	
	
	cout << -1;
} 



signed main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
	cin >> n >> m >> k;
	
	for(int i = 1 ; i <= n ; i ++ ){
		for(int j = 1 ; j <= m ; j ++ ){
			char op;
			cin >> op;
			if(op == 'A')mp[i][j] = 1;
			else mp[i][j] = 0;
		}
	}
	 
	bfs();
	
	return 0;
}
```

# 3/31 25

## [更小的数](https://www.luogu.com.cn/problem/P9232)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 5010;

int dp[N][N];


signed main()
{
	string x;
	cin >> x;
	int n = x.size();
	string s = "0";
	s += x;
	
	for(int i = 1 ; i < n ; i ++ ){
		for(int j = 1 ; i + j <= n ; j ++ ){
			if(s[j] >= s[j + i]){
				if(s[j] > s[j + i])dp[j][j + i] ++ ;
				dp[j][j + i] += dp[j + 1][j + i - 1];
			}
		}
	}	
	
	int ans = 0; 
	for(int i = 1 ; i < n ; i ++ ){
		for(int j = 1 ; j <= n ; j ++ ){
			if(dp[i][j])ans ++ ;
		}
	}
	
	cout << ans << endl;
	
	return 0;
}
```

## [签到题](https://www.luogu.com.cn/problem/P9147)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 1e6 + 10;

int a[N];
int f[N] , g[N];

signed main()
{
	int n;
	cin >> n;
	for(int i = 1 ; i <= n ; i ++ )cin >> a[i];
	
	f[1] = 1;
	for(int i = 2 ; i <= n ; i ++ ){
		f[i] = 1;
		if(a[i] > a[i - 1])f[i] += f[i - 1]; 
	}

    g[n] = 1;
	for(int i = n - 1 ; i >= 1 ; i -- ){
		g[i] = 1;
		if(a[i] < a[i + 1])g[i] += g[i + 1];
	}
	
	int ans = 0;
	for(int i = 1 ; i <= n ; i ++ ){
		if(a[i - 1] + 1 <= a[i + 1] - 1 && i + 1 <= n && i - 1 >= 1){
			int t = f[i - 1] + g[i + 1] + 1;
			ans = max(t,ans);
		}
		else {
			int t = max(f[i - 1] + 1,g[i + 1] + 1);
			ans = max(ans,t); 
		}
	}
	
	cout << ans << endl;

	return 0;
}
```

## 送水

![](C:\Users\30292\Desktop\SQI\SQI\lqb_img\Snipaste_2025-03-31_22-39-04.png)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 1e6 + 10;

int x[N] , apperTimes[N] , totalApperTimes[N];
int n;

int getValue(int y,int a,int b){
	int t1 = (a + b) * (y * apperTimes[y] - totalApperTimes[y]) + b * (totalApperTimes[N - 10] - y * n);
	return t1;
}

signed main()
{
	ios::sync_with_stdio(0) , cin.tie(0) , cout.tie(0);
	cin >> n;
	for(int i = 1 ; i <= n ; i ++ ){
		int t;
		cin >> t; 
		x[t] ++ ;
	}
	
	for(int i = 1 ; i <= 1e6 ; i ++ ){
		apperTimes[i] = apperTimes[i - 1] + x[i];
		if(apperTimes[i])totalApperTimes[i] += totalApperTimes[i - 1] + i * x[i];
		else totalApperTimes[i] += totalApperTimes[i - 1];
	}
	
	int q;
	cin >> q;
	while(q -- ){
		int a,b;
		cin >> a >> b;
		int l = 0 , r = 1e6;
		while(l < r){
			int mid = (l + r) / 2;
			int rt = mid + 1;
			
			int v1 = getValue(mid,a,b);
			int v2 = getValue(rt,a,b);
			
			if(v1 < v2)r = mid;
			else l = mid + 1;
		}
		int ans1 = getValue(l,a,b);
		int ans2 = getValue(r,a,b);
		cout << min(ans1,ans2) << endl;
	}
	
	return 0;
}
```

# 4/1 25

## [Likecloud-吃、吃、吃](https://www.luogu.com.cn/problem/P1508)

```C++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 210;

vector<vector<int>>mp(N,vector<int>(N,-9999));
int dp[N][N];

signed main(){
	ios::sync_with_stdio(0) , cin.tie(0) , cout.tie(0);
	
	int n,m;
	cin >> m >> n;
	
	for(int i = 1 ; i <= m ; i ++ ){
		for(int j = 1 ; j <= n ; j ++ ){ 
			cin >> mp[i][j];
		}
	}
	
	for(int i = 1 ; i <= m ; i ++ ){
		for(int j = 1 ; j <= n ; j ++ ){
			dp[i][j] = max(dp[i - 1][j],max(dp[i - 1][j - 1],dp[i - 1][j + 1])) + mp[i][j];
		}
	}
	
	int p = (n + 1) / 2;
	int ans = max(dp[m][p],max(dp[m][p - 1],dp[m][p + 1]));
	
	cout << ans << endl;
	
	return 0;
}

//dfs
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 210;

int mp[N][N];
int f[N][N];
int n,m;
int ans = 0;
int p; 

int dfs(int x,int y){
	
	if(f[x][y])return f[x][y];
	
	if(x <= 0 || y <= 0 || y > n || x > m)return 0;

	return f[x][y] = max(dfs(x - 1,y) + mp[x - 1][y],max(dfs(x - 1,y - 1) + mp[x - 1][y - 1],dfs(x - 1,y + 1) + mp[x - 1][y + 1]));
}

signed main(){
	ios::sync_with_stdio(0) , cin.tie(0) , cout.tie(0);
	
	cin >> m >> n;
	
	for(int i = 1 ; i <= m ; i ++ ){
		for(int j = 1 ; j <= n ; j ++ ){
			cin >> mp[i][j];
		}
	}
	
	p = (n + 1) / 2;
	
	ans = max(dfs(m,p - 1) + mp[m][p - 1],max(dfs(m,p) + mp[m][p],dfs(m,p + 1) + mp[m][p + 1]));
	
	cout << ans << endl;
	
	return 0;
}
```

## [H数](https://www.luogu.com.cn/problem/P1748)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 1e4 + 10;

vector<int>H;

int myPow(int a,int b){
	int res = 1;
	for(int i = 1 ; i <= b ; i ++ )res *= a;
	return res;
}


signed main(){
	ios::sync_with_stdio(0) , cin.tie(0) , cout.tie(0);
	
	priority_queue<int,vector<int>,greater<int>>q;
	set<int>st;
	
	int n;
	cin >> n;
	int cnt = 0;
	int num = 0;
	
	st.insert(1);
	q.push(1);
	vector<int>pp = {2,3,5,7};
	
	while(cnt < n){
		num = q.top();
		q.pop();
		cnt ++ ;
		
		if(cnt == n)break;
		
		for(auto p : pp){
			int res = p * num;
			if(st.find(res) == st.end()){
				q.push(res);
				st.insert(res);
			}
		}
		
	}
	
	cout << num << endl;
	
	return 0;
}
```

# 4/2 25

## [装备运输](https://www.luogu.com.cn/problem/P1794)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

struct pp{
	int t;
	int v;
	int g;
}pp[510];

int dp[510][510]; 

signed main()
{
	int v,g;
	cin >> v >> g;
	int n;
	cin >> n;
	for(int i = 1 ; i <= n ; i ++ )cin >> pp[i].t >> pp[i].v >> pp[i].g;
	
	for(int i = 1 ; i <= n ; i ++ ){
		for(int j = v ; j >= 1 ; j -- ){
			for(int k = g ; k >= 1 ; k -- ){
				if(j >= pp[i].v && k >= pp[i].g)
				dp[j][k] = max(dp[j - pp[i].v][k - pp[i].g] + pp[i].t,dp[j][k]);	
			}
		}
	}
	
	cout << dp[v][g] << endl;
	
	return 0;
}
```

## [A % B Problem](https://www.luogu.com.cn/problem/P1865)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 1e6 + 10;

bool noprime[N];
int cnt[N];

bool getPrime(int x){
	if(x == 1)return false;
	for(int i = 2 ; i * i <= x ; i ++ ){
		if(x % i == 0)return false;
	}
	return true;
}

void primeFilter(int l,int r){
	
	for(int i = l ; i <= r ; i ++ ){
		if(getPrime(i))
			for(int j = 2 * i ; j <= r ; j += i)
				noprime[j] = true;
	}
	
	noprime[0] = true;
	noprime[1] = true; 

	for(int i = l ; i <= r ; i ++ ){
		if(!noprime[i]){
			cnt[i] ++ ;
		}
		cnt[i] += cnt[i - 1];
	}	
	
}

signed main()
{
	int n,m;
	cin >> n >> m;
	primeFilter(1,m);
	while(n -- ){
		int l,r;
		cin >> l >> r;
		if(l < 1 || r > m){
			cout << "Crossing the line" << endl;
			continue;
		}
		
		cout << cnt[r] - cnt[l - 1] << endl;
				
	}
	
	return 0;
}
```

## [接龙数列](https://www.luogu.com.cn/problem/P9242)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

int dp[10];
string a;

signed main()
{
	int n;
	cin >> n;
	for(int i = 1 ; i <= n ; i ++ ){
		cin >> a;
		int l = a.size() - 1;
		int p = a[0] - '0';
		int q = a[l] - '0';
		dp[q] = max(dp[q],dp[p] + 1);
	}
	
	int len = 0;
	for(int i = 0 ; i <= 9 ; i ++ )len = max(len,dp[i]);
	
	cout << n - len << endl;
	
	return 0;
}
```

## [后缀式表达](https://www.luogu.com.cn/problem/P8683)

```c++
#include<bits/stdc++.h>
#define int long long 
#define endl '\n'
using namespace std;
const int N = 2 * 1e5 + 10;

vector<int>a,b;

signed main()
{
	int n,m;
	cin >> n >> m;
	for(int i = 1 ; i <= n + m + 1 ; i ++ ){
		int x;
		cin >> x;
		a.push_back(x);
	}
	
	sort(a.begin(),a.end());
	
	int sum = 0;

	if(m == 0){
		for(auto p : a)sum += p;
	}
	else {
		sum = a[(int)a.size() - 1] - a[0];
		for(int i = 1 ; i < a.size() - 1 ; i ++ ){
			sum += abs(a[i]);
		}
	}

	cout << sum << endl;
	
	return 0;
}
```

# 4/3 25

## [整数拼接](https://www.luogu.com.cn/problem/P8708)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 1e5 + 10;

int a[N];
string s[N];
string k;

int cmp(string a){
	if(a.size() > k.size())return -1;
	if(a.size() < k.size())return 1;
	
	for(int i = 0 ; i < a.size() ; i ++ ){
		if(a[i] - '0' > k[i] - '0')return -1;
		if(a[i] - '0' < k[i] - '0')return 1;
	}
	
	return 0;
}

signed main()
{
	ios::sync_with_stdio(0) , cin.tie(0) , cout.tie(0);
	int n;
	cin >> n >> k;
	for(int i = 1 ; i <= n ; i ++ )cin >> a[i];
	
	sort(a + 1 , a + 1 + n);
	for(int i = 1 ; i <= n ; i ++ ){
		s[i] = to_string(a[i]);
	}
	
	int l = 1 , r = n;
	int cnt = 0;
	while(l <= r){
		string t1 = s[l] + s[r];
		int t = cmp(t1);
		
		if(t == -1) -- r;
		else if(t == 0){
			cnt += r - l;
			-- r;
			++ l;
		}
		else {
			cnt += r - l;
			 ++ l;
		}
	}
	
	l = 1 ,r = n;
	while(l <= r){
		string t1 = s[r] + s[l];
		int t = cmp(t1);
		
		if(t == -1) -- r;
		else if(t == 0){
			cnt += r - l;
			-- r;
			++ l;
		}
		else {
			cnt += r - l;
			 ++ l;
		}
	}
	
	cout << cnt << endl;
	
	return 0;
}
```

# 4/4 25

## [灵梦的字符串问题](https://ac.nowcoder.com/acm/contest/106509/C)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n' 
using namespace std;

signed main()
{
	int n,m;
	cin >> n >> m; 
	string s;
	cin >> s;
	vector<int>a(n);
	for(int i = 0 ; i < n ; i ++ ){
		cin >> a[i];
	}
	
	vector<int>p;
	int l = 0;
	for(int i = 0 ; i < n - 1 ; i ++ ){
		if(s[i] < s[i + 1]){
			while(s[l] != s[i]) ++ l;
			for(int j = l ; j <= i ; j ++ )p.push_back(j);
			l = i + 1;
		}
	}
	
	vector<bool>isChosen(n,false);
	for(auto i : p){
		if(m >= a[i]){
			isChosen[i] = true;
			m -= a[i];
		}
	}
	
	string res;
	for(int i = 0 ; i < n ; i ++ ){
		res += s[i];
		if(isChosen[i])res += s[i];
	}
	
	cout << res << endl;
		
	return 0;
}
```

## [带分数](https://www.luogu.com.cn/problem/P8599)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

vector<int>t; 
int ans;
bool ip[10];
int n;

void isT(){

	int a = 0;
	for(int i = 1 ; i <= 7 ; i ++ ){
		a = a * 10 + t[i];
		int b = 0;
		for(int j = i + 1 ; j <= 8 ; j ++ ){
			b = b * 10 + t[j];
			int c = 0;
			for(int k = j + 1 ; k <= 9 ; k ++ ){
				c = c * 10 + t[k];
			}
			
//			cout << a << ' ' << b << ' ' << c << endl;
 			if(b % c == 0 && a + b / c == n)ans ++ ;
		}
	}
	
}

void dfs(int index){
	
	if(index == 10){
		isT();
		return;
	}
	
	for(int i = 1 ; i <= 9 ; i ++ ){
		if(!ip[i]){
			t.push_back(i);
			ip[i] = true;
			dfs(index + 1);
			t.pop_back();
			ip[i] = false;
		}
	}
	
	
}

signed main()
{
	cin >> n;
	t.push_back(0);
	
	dfs(1);
	
	cout << ans << endl;
	
	return 0;
}
```

## [蚂蚁感冒](https://www.luogu.com.cn/problem/P8611)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

signed main()
{
	int n;
	int mid;
	cin >> n;
	cin >> mid;
	int num = 1;
	
	int l = 0 , r = 0; 
	for(int i = 1 ; i < n ; i ++ ){
		int x;
		cin >> x;
		if(abs(x) > abs(mid) && x < 0)r ++ ;
		if(abs(x) < abs(mid) && x > 0)l ++ ;
	}
	
	int ans = 0;
	if(r == 0 && mid > 0)ans = 1;
	if(l == 0 && mid < 0)ans = 1;
	else ans = num + l + r;
	
	cout << ans << endl;
	
	return 0;
}
```

# 4/5 25

## [123](https://www.luogu.com.cn/problem/P8762)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 2 * 1e6 + 10;

vector<int>s(N);
vector<int>f(N);

signed main()
{
	for(int i = 1 ; i <= N ; i ++ ){
		s[i] += s[i - 1] + i; 
		f[i] += f[i - 1] + s[i];
	}
	int T;
	int l,r;
	cin >> T;
	while(T -- ){
		cin >> l >> r;
		l -- ;
		int t1 = lower_bound(s.begin(),s.end(),l) - s.begin() - 1;
		int lev_l = max(0LL,t1);
		int t2 = lower_bound(s.begin(),s.end(),r) - s.begin() - 1;
		int lev_r = max(0LL,t2);
		int sumL = f[lev_l];
		int sumR = f[lev_r];
		sumL += (l - s[lev_l] + 1) * (l - s[lev_l]) / 2;
		sumR += (r - s[lev_r] + 1) * (r - s[lev_r]) / 2;
		
		cout << sumR - sumL << endl;
	}
	
	return 0;
}
```

## [砍竹子](https://www.luogu.com.cn/problem/P8787)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 2 * 1e5 + 10;

vector<int>h(N);
vector<int>m(N);

int getNewH(int x){
	return (int)sqrt((int)(x / 2) + 1);
}

signed main()
{
	int n;
	cin >> n;
	int res = 0;
	for(int i = 1 ; i <= n ; i ++ ){
		cin >> h[i];
		int t = h[i];
		while(t != 1){
			m[i] ++ ;
			t = getNewH(t);
		}
		res = max(m[i],res);
	}
	
	int ans = 0;
	for(int i = res ; i >= 1 ; -- i){
		for(int j = 1 ; j <= n ; j ++ ){
			if(m[j] == i){
				if(h[j] != h[j + 1])ans ++ ;
				m[j] -- ;
				h[j] = getNewH(h[j]);
			}
		}
	}
	
	cout << ans << endl;
	
	return 0;
}
```

# 4/6 25

## [轨道炮](https://www.luogu.com.cn/problem/P8695)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 1010 , M = 1e6 + 10;

struct position{
	int x,y;
	int vx , vy;
		
};

position pos[N];

int nums[M];

signed main()
{
	int n;
	cin >> n;
	for(int i = 1 ; i <= n ; i ++ ){
		int v;
		char op;
		cin >> pos[i].x >> pos[i].y;
		cin >> v >> op;
		if(op == 'L')pos[i].vx = -v;
		else if(op == 'R')pos[i].vx = v;
		else if(op == 'U')pos[i].vy = v;
		else pos[i].vy = -v;
	}
	
	int ans = 0;
	for(int i = 0 ; i <= N ; i ++ ){
		map<int,int>x,y;
		for(int j = 1 ; j <= n ; j ++ ){
			x[pos[j].x + pos[j].vx * i] ++ ;
			y[pos[j].y + pos[j].vy * i] ++ ;
		}
		for(auto p : x){
			ans = max(ans,p.second);
		}
		for(auto p : y){
			ans = max(ans,p.second);
		}
		
	}
	
	cout << ans << endl;
	
	return 0;
}
```

## [卡牌](https://www.luogu.com.cn/problem/P8800)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 2 * 1e6 + 10;

int a[N] , b[N];
int n,m;

bool check(int x){
		
	int t = m;
	bool flag = true;
	
	for(int i = 1 ; i <= n ; i ++ ){
//		cout << a[i] << ' ';
		if(a[i] >= x)continue;
		else {
			if(b[i] >= x - a[i]){
				t -= x - a[i];
			}
			else {
				flag = false;
				break;
			}
			
			if(t < 0){
				flag = false;
				break;
			}
		}
	}
	
//	cout << endl;
	
	if(flag){
//		cout << 1 << endl; 
		return true;
	}
	
//	cout << 0 << endl;
	return false;
}


signed main(){
	cin >> n >> m;
	
	for(int i = 1 ; i <= n ; i ++ )cin >> a[i];
	for(int i = 1 ; i <= n ; i ++ )cin >> b[i];
	
	int l = 0 , r = N;
	while(l < r){
		int mid = (l + r + 1) / 2;
		if(check(mid)){
			l = mid;		
		}
		else {
			r = mid - 1;
		}
	}
	
	cout << l << endl;
	
	return 0;
}
```

## [答疑](https://www.luogu.com.cn/problem/P8732)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
const int N = 1010;

struct pp{
	int t;
	int e;
	int total;
}pp[N];

bool cmp(struct pp p1,struct pp p2){
	if(p1.total != p2.total)return p1.total < p2.total;
	if(p1.t != p2.t)return p1.t < p2.t;
	return p1.e < p2.e;
}


signed main()
{
	int n;
	cin >> n;
	for(int i = 1 ; i <= n ; i ++ ){
		int x,y;
		cin >> x >> y >> pp[i].e;
		pp[i].t = x + y;
		pp[i].total = x + y + pp[i].e;
	}	

	sort(pp + 1,pp + 1 + n,cmp);
	
	int ans = 0;
	int now = 0;
	for(int i = 1 ; i <= n ; i ++ ){
		now += pp[i].t;
		ans += now;
		now += pp[i].e;
	}
	
	cout << ans << endl;
	
	return 0;
}
```

## [小红杀怪](https://ac.nowcoder.com/acm/contest/90960/D)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

signed main()
{
	int a,b,x,y;
	cin >> a >> b >> x >> y;
	
	int ans = 0;
	bool f = false;
	for(int i = 1 ; ; i ++ ){
		for(int j = i ; j >= 0 ; j -- ){
			for(int k = 0 ; j + k <= i ; k ++ ){
				int h1 = a;
				int h2 = b;
				h1 -= k * y;
				h2 -= k * y;
				if(h1 > 0 && h2 <= 0){
					h1 -= j * x;
					if(h1 <= 0){
						ans = i;
						f = true;
						break;
					}	
				}
				else if(h1 <= 0 && h2 > 0){
					h2 -= j * x;
					if(h2 <= 0){
						ans = i;
						f = true;
						break;
					} 
				}
				else if(h1 <= 0 && h2 <= 0){
					ans = i;
					f = true;
					break;
				}
				else {
					
					for(int l = 1 ; l < j ; l ++ ){
						int hp1 = h1;
						int hp2 = h2;
						hp1 -= l * x;
						hp2 -= (j - l) * x;
						if(hp1 <= 0 && hp2 <= 0){
							ans = i;
							f = true;
							break;
						}
					}
				}
				if(f)break;
			}
			if(f)break;
		}
		if(f)break;
	}
	
	cout << ans << endl;
	
	return 0;
}
```

# 4/7 25

## [小梦的AB交换](https://www.luogu.com.cn/problem/U535982)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;


signed main()
{
	int T;
	cin >> T;
	string a;
	while(T -- ){
		int n;
		cin >> n;
		cin >> a;
		string s1;
		string s2;
		for(int i = 0 ; i < n ; i ++ )s1 += "AB";
		for(int i = 0 ; i < n ; i ++ )s2 += "BA";
		int ds1 = 0;
		int ds2 = 0;
		
		for(int i = 0 ; i < 2 * n ; i ++ ){
			if(a[i] != s1[i])ds1 ++ ;
			if(a[i] != s2[i])ds2 ++ ;
		}
		
		int ans = min(ds1,ds2) / 2;
		
		cout << ans << endl;
	}
	
	return 0;
}
```

## [零的数列 Zero Sum](https://www.luogu.com.cn/problem/P1473)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

int n;
vector<int>a(12,0);
vector<char>p(12);

void dfs(int index,int sum,int prev,char last){
	
	if(index == n){
		if(last == '+')sum += prev;
		else sum -= prev;
		
		if(sum == 0){
			cout << 1;
			for(int i = 1 ; i < n ; i ++ ){
				cout << p[i] << a[i];
			}
			cout << endl;
		}
	} 
	else{
		p[index] = ' ';
		dfs(index + 1,sum,prev * 10 + a[index],last);
		
		p[index] = '+';
		if(last == '+'){
			dfs(index + 1,sum + prev,a[index],'+');
		}
		else {
			dfs(index + 1,sum - prev,a[index],'+');
		}
		
		p[index] = '-';
		if(last == '+'){
			dfs(index + 1,sum + prev,a[index],'-');
		}
		else {
			dfs(index + 1,sum - prev,a[index],'-');
		}
	}
	
	
}

signed main()
{
	cin >> n;
	for(int i = 1 ; i < n ; i ++ )a[i] = i + 1;
	
	dfs(1,0,1,'+');
	
	return 0;
}
```

## [马的遍历](https://www.luogu.com.cn/problem/P1443)

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;
typedef pair<int,int>pp;
const int N = 410;

int n,m;
int x,y;
int mp[N][N];
bool p[N][N];
int dx[8] = {-2,-2,-1,-1,1,1,2,2};
int dy[8] = {-1,1,-2,2,-2,2,-1,1};

void bfs(){

	queue<pp>q;	
	q.push({x,y});
	
	while(!q.empty()){
		
		auto t = q.front();
		q.pop();
	
		int i = t.first;
		int j = t.second;
//		cout << i << ' ' << j << endl;
		
		if(p[i][j])continue; 
		p[i][j] = true; 
		
		for(int k = 0 ; k < 8 ; k ++ ){
			int x1 = dx[k] + i;
			int y1 = dy[k] + j;
			if(x1 <= 0 || x1 > n || y1 <= 0 || y1 > m)continue;
			if(p[x1][y1])continue;
			mp[x1][y1] = mp[i][j] + 1;
			q.push({x1,y1});
		}
	}
	
}

signed main()
{
	cin >> n >> m >> x >> y;
	
	bfs();
	
	for(int i = 1 ; i <= n ; i ++ ){
		for(int j = 1 ; j <= m ; j ++ ){
			if(p[i][j])cout << mp[i][j] << "   ";
			else cout << -1 << "   ";
		}
		cout << endl;
	}
	
	return 0;
}
```

