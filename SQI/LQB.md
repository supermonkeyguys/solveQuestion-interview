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

