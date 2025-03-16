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

