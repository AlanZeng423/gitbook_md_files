---
description: AcWing 算法基础课程笔记
---

# 📒 Alogrithm-算法基础

## Chapter1 基础算法

### 快速排序 Quick Sort

#### 785.快速排序

```cpp
#include <iostream>
using namespace std;

const int N = 1e6 + 10;

int n;
int q[N];

void quick_sort(int q[],int l,int r)
{
    if(l>=r) return;
    int i = l-1;
    int j = r+1;
    int x = q[(l+r)/2];
    
    while(i<j){
        do i++; while(q[i]<x);
        do j--; while(q[j]>x);
        if(i<j) swap(q[i],q[j]);
    }
    
    quick_sort(q,l,j);
    quick_sort(q,j+1,r);
}


int main()
{
    scanf("%d",&n);
    for(int i=0;i<n;i++) scanf("%d",&q[i]);
    
    quick_sort(q,0,n-1);
    
    for(int i=0;i<n;i++) printf("%d ",q[i]);
    
    return 0;
}
```

注意边界问题

quick\_sort(q,l,j); quick\_sort(q,j+1,r);**//此时x不能取q\[r]**

可以更改为

quick\_sort(q,l,i-1); quick\_sort(q,i,r);**//此时x不能取q\[l]**

### 归并排序

```cpp
#include<iostream>
using namespace std;

const int N = 1e6+10;
int n;
int q[N],tmp[N];

void merge_sort(int q[],int l,int r)
{
    if(l>=r)return;
    int mid = l+r >> 1;
    merge_sort(q,l,mid);
    merge_sort(q,mid+1,r);
    int k = 0;
    int i = l,j = mid+1;
    while(i<=mid && j<=r) {
        if (q[i]<=q[j]) tmp[k++] = q[i++];
        else tmp[k++] = q[j++];
    }
    while(i<=mid) tmp[k++] = q[i++];
    while(j<=r) tmp[k++] = q[j++];
    for(i=l,j=0;i<=r;i++,j++) q[i]=tmp[j];
}

int main()
{
    scanf("%d",&n);
    for(int i=0;i<n;i++) scanf("%d",&q[i]);
    
    merge_sort(q,0,n-1);
    
    for(int i=0;i<n;i++) printf("%d ",q[i]);
    
    return 0;
}
```

### 二分排序

#### 整数二分

```cpp
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

**例题：**

给定一个按照升序排列的长度为 n 的整数数组，以及 q 个查询。

对于每个查询，返回一个元素 k 的起始位置和终止位置（位置从 00 开始计数）。

如果数组中不存在该元素，则返回 `-1 -1`。

**输入格式**

第一行包含整数 n 和 q，表示数组长度和询问个数。

第二行包含 n 个整数（均在 1∼100001∼10000 范围内），表示完整数组。

接下来 q 行，每行包含一个整数 k，表示一个询问元素。

**输出格式**

共 q 行，每行包含两个整数，表示所求元素的起始位置和终止位置。

如果数组中不存在该元素，则返回 `-1 -1`。

**数据范围**

1≤n≤100000

1≤q≤10000

1≤k≤10000

**输入样例**：

```other
6 3
1 2 2 3 3 4
3
4
5
```

**输出样例**：

```other
3 4
5 5
-1 -1
```

**答案**

```cpp
#include <iostream>
using namespace std;

const int N = 100010;
int arr[N];

int main()
{
    int n,q,k;
    scanf("%d%d",&n,&q);
    for(int i=0;i<n;i++) scanf("%d",&arr[i]);
    for(int i=0;i<q;i++) {
        scanf("%d",&k);
        int l=0,r=n-1;
        while(l<r)
        {
            int mid = l+r >> 1;
            if(arr[mid]>=k) r = mid;
            else l = mid+1;
        }
        if(arr[l]!=k) cout<<"-1 -1"<<endl;
        else{
            cout<<l<<" ";
            int l=0,r=n-1;
            while(l<r)
            {
                int mid = l+r+1 >> 1;
                if(arr[mid]<=k) l = mid;
                else r = mid-1;
            }
            cout<<r<<endl;
        }
    }
}
```

#### 浮点数二分

不考虑边界问题更简单

**例题：**

给定一个浮点数 n，求它的三次方根。

**输入格式**

共一行，包含一个浮点数 n。

**输出格式**

共一行，包含一个浮点数，表示问题的解。 注意，结果保留 66 位小数。

**数据范围**

−10000≤n≤10000

**输入样例**：

```other
1000.00
```

**输出样例**：

```other
10.000000
```

答案：

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int main()
{
    double n;
    cin>>n;
    bool isNegtive = false;
    if(n<0) {
        isNegtive = true;
        n = n*-1.0;
    }
    double l=0,r=max(1.0,n);
    while((r-l)>1e-8)
    {
        double mid = (l+r)/2;
        if(mid*mid*mid>=n){
            r = mid;
        }
        else l = mid;
    }
    if(!isNegtive) printf("%lf\n",l);
    else printf("%lf\n",-1*l);
    
    return 0;
}
```

### 高精度算法

#### 高精度加法

```cpp
#include <iostream>
#include <vector>

using namespace std;

const int N = 1e6+10;

vector<int> add(vector<int>&A,vector<int>&B)
{
    vector<int> C;
    int t = 0;
    for (int i = 0; i<A.size()||i<B.size(); i ++ ){
        if(i<A.size()) t+=A[i];
        if(i<B.size()) t+=B[i];
        C.push_back(t%10);
        t/=10;//上一位的进位
    }
    if(t) C.push_back(1);
    return C;
}

int main()
{
    string a,b;
    vector<int> A,B;
    
    cin>>a>>b;
    for(int i = a.size()-1;i>=0;i--){
        A.push_back(a[i]-'0');
    }
    for(int i = b.size()-1;i>=0;i--){
        B.push_back(b[i]-'0');
    }
    
    auto C = add(A,B);
    
    for(int i = C.size()-1;i>=0;i--){
        printf("%d",C[i]);
    }
    return 0;
}
```

#### 高精度减法

```cpp
#include <iostream>
#include <vector>

using namespace std;

bool cmp(vector<int>&A,vector<int>&B){
    if(A.size()!=B.size()) return A.size()>B.size();
    else {
        for(int i=A.size()-1;i>=0;i--){
            if(A[i]!=B[i]) return A[i]>B[i];
        }
    }
    return true;
}

vector<int> sub(vector<int>&A,vector<int>&B)
{
    //要求——A>=B
    //if A>=B 直接计算A-B
    //else 计算-(B-A)
    
    vector<int> C;
    int t = 0;
    for (int i = 0; i<A.size(); i ++ ){
        t = A[i] - t;
        if(i<B.size()) t -= B[i];
        C.push_back((t+10)%10);
        if(t<0) t = 1;
        else t = 0;
    }
    while(C.size()>1 && C.back()==0) C.pop_back();
    return C;
}

int main()
{
    string a,b;
    vector<int> A,B;
    
    cin>>a>>b;
    for(int i = a.size()-1;i>=0;i--){
        A.push_back(a[i]-'0');
    }
    for(int i = b.size()-1;i>=0;i--){
        B.push_back(b[i]-'0');
    }
    if(cmp(A,B)){
        auto C = sub(A,B);
        for(int i = C.size()-1;i>=0;i--){
            printf("%d",C[i]);
        }
    }
    else{
        auto C = sub(B,A);
        printf("-");
        for(int i = C.size()-1;i>=0;i--){
            printf("%d",C[i]);
        }
    }
    
    return 0;
}
```

#### 高精度乘法

```cpp
#include <iostream>
#include <vector>

using namespace std;

vector<int> mul(vector<int> A , int b)
{
    vector<int> C;
    int t = 0;
    for(int i=0;i < A.size() || t ; i++){
        if(i<A.size()){
            t += A[i] * b;
        }
        C.push_back(t % 10);
        t /= 10;
        // C.push_back((A[i] * b + t) % 10);
        // t = (A[i] * b + t) / 10;
    }
    while(C.size()>1 && C.back()==0) C.pop_back();
  	// 去除前导0
    return C;
}

int main()
{
    string a;
    int b;
    vector<int> A;
    cin>>a>>b;
    for(int i = a.size()-1;i>=0;i--){
        A.push_back(a[i]-'0');
    }
    
    auto C = mul(A,b);
    
    for(int i = C.size()-1;i>=0;i--){
            printf("%d",C[i]);
        }
}
```

#### 高精度除法

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> div(vector<int> &A, int b ,int &r){
    vector<int> C;// 商
    r = 0;// 余数
    // 从最高位开始算--不同于其他几种计算
    for(int i = A.size() - 1;i >= 0 ; i--){
        r = r * 10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    
    reverse(C.begin() ,C.end());
    
    while(C.size()>1 && C.back()==0) C.pop_back();
  	// 去除前导0
    return C;
}

int main()
{
    string a;
    int b;
    vector<int> A;
    cin>>a>>b;
    for(int i = a.size()-1;i>=0;i--){
        A.push_back(a[i]-'0');
    }
    
    int r;
    auto C = div(A,b,r);
    
    for(int i = C.size()-1;i>=0;i--){
        printf("%d",C[i]);
    }
    cout<<endl;
    cout<<r<<endl;
}
```

### 前缀和

#### 一维前缀和

* 更新 `S[i] = S[i-1] + a[i]`
* 计算a\[l,r] `ans = S[r] - S[l-1]`

```cpp
#include <iostream>
using namespace std;

const int N = 100010;
int a[N],S[N];

int main()
{
    int n,m;
    scanf("%d%d", &n, &m);
    
    for (int i = 1;i<=n;i++) scanf("%d", &a[i]);
    
    S[0] = 0;
    for(int i = 1;i<=n;i++){
        S[i] = S[i-1] + a[i];
    }
    for (int i = 0;i < m;i++){
        int l,r;
        scanf("%d%d", &l, &r);
        printf("%d\n",S[r]-S[l-1]);
    }
    
    return 0;
    
}
```

#### 二维前缀和(子矩阵的和)

* 更新 `S[i][j] =a[i][j] + S[i][j-1] + S[i-1][j] - S[i-1][j-1]`
* 计算(x1,y1)(x2,y2) `ans = S[x2][y2] - S[x1-1][y2] - S[x2][y1-1] + S[x1-1][y1-1]`

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

const int N = 1010;
int a[N][N],S[N][N];

int main()
{
    int n,m,q;
    scanf("%d%d%d", &n, &m, &q);
    for (int i = 1; i <= n; i++){
        for (int j = 1; j <= m; j++){
            scanf("%d",&a[i][j]);
        }
    }
    S[0][0] = 0;
    for (int i = 1; i <= n; i++){
        for (int j = 1; j <= m; j++){
            S[i][j] =a[i][j] + S[i][j-1] + S[i-1][j] - S[i-1][j-1];
        }
    }
    
    int x1,x2,y1,y2;
    while(q--){
        scanf("%d%d%d%d", &x1, &y1 , &x2, &y2);
        int ans = S[x2][y2] - S[x1-1][y2] - S[x2][y1-1] + S[x1-1][y1-1];
        printf("%d\n",ans);
    }
    
    return 0;
}

```

#### 一维差分

*   输入一个长度为 n 的整数序列。

    接下来输入 m个操作，每个操作包含三个整数 l,r,c，表示将序列中 \[l,r] 之间的每个数加上 c。

    请你输出进行完所有操作后的序列。
* 前缀和的逆运算
* 构造 等同于 操作(i,i,a\[i])
* 操作等价于
  * `b[l] += c;`
  * \`\`b\[r+1] -= c;\`

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010;
int a[N],b[N];

void op(int l,int r,int c){
    b[l] += c;
    b[r+1] -= c;
}

int main()
{
    int n,m;
    scanf("%d%d", &n, &m);
    for(int i=1;i<=n;i++){
        scanf("%d", &a[i]);
    }
    for(int i=1;i<=n;i++){
        op(i,i,a[i]);
    }
    
    /*接下来输入 m个操作，
    * 每个操作包含三个整数 l,r,c
    * 表示将序列中 [l,r]之间的每个数加上 c
    */
    while (m--) {
        int l,r,c;
        scanf("%d%d%d", &l, &r, &c);
        op(l,r,c);
    }
    
    for (int i = 1; i <= n; i++){
        b[i] += b[i-1];
    }
    for (int i = 1; i <= n; i++){
        printf("%d ",b[i]);
    }
    
    return 0;
    
    
}
```

#### 二维差分

*   输入一个 n 行 m列的整数矩阵，再输入 q 个操作，每个操作包含五个整数 x1,y1,x2,y2,c 其中 (x1,y1)和 (x2,y2)表示一个子矩阵的左上角坐标和右下角坐标。

    每个操作都要将选中的子矩阵中的每个元素的值加上 c

    请你将进行完所有操作后的矩阵输出。
* 操作等价于
  * `b[x1][y1] += c`
  * `b[x2+1][y1] -= c`
  * `b[x1][y2+1] -= c`
  * `b[x2+1][y2+1] += c`

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

const int N = 1010;
int a[N][N],b[N][N];

void op(int x1,int x2,int y1,int y2,int c){
    b[x1][y1] += c;
    b[x2+1][y1] -= c;
    b[x1][y2+1] -= c;
    b[x2+1][y2+1] += c;
}

int main()
{
    int n,m,q;
    scanf("%d%d%d", &n, &m, &q);
    
    for (int i = 1; i <= n; i++){
        for (int j = 1; j <= m; j++){
            scanf("%d",&a[i][j]);
        }
    }
    for (int i = 1; i <= n; i++){
        for (int j = 1; j <= m; j++){
            op(i,i,j,j,a[i][j]);
        }
    }
    
    int x1,x2,y1,y2,c;
    while(q--){
        scanf("%d%d%d%d%d", &x1, &y1 , &x2, &y2, &c);
        op(x1,x2,y1,y2,c);
    }
    
    for (int i = 1; i <= n; i++){
        for (int j = 1; j <= m; j++){
            b[i][j] = b[i][j] + b[i][j-1] + b[i-1][j] - b[i-1][j-1];
        }
    }
    
    for (int i = 1; i <= n; i++){
        for (int j = 1; j <= m; j++){
            printf("%d ",b[i][j]);
        }
        puts("");
    }
    
    
    return 0;
}

```

### 双指针算法

*   基本模版

    ```cpp
    for(i = 0,j = 0;i < n;i++){
    	while(j<i && check(i,j))
        j++;
      
      // ......
    }
    ```
*   核心思想

    将O(n^2^)算法优化到O(n)

#### 799.最长连续不重复子序列

给定一个长度为 n 的整数序列，请找出最长的不包含重复的数的连续区间，输出它的长度。

**输入格式**

第一行包含整数 n。 第二行包含 n 个整数（均在 0∼1050∼105 范围内），表示整数序列。

**输出格式**

共一行，包含一个整数，表示最长的不包含重复的数的连续区间的长度。

**数据范围**

1≤n≤105

**code**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>


using namespace std;

const int N = 1e5 + 10;
int n;
int a[N],s[N];


int main()
{
    scanf("%d", &n);
    for(int i = 0;i < n;i++) {
        scanf("%d", &a[i]);
    }
    int maxLen = 0;
    for (int i = 0,j = 0; i < n; i++){
        s[a[i]]++;
        while(s[a[i]] > 1){
            s[a[j]]--;
            j++;
        }
        maxLen = max(maxLen,i - j + 1);
    }
    cout << maxLen;
    return 0;
}
```

***

#### 800.数组元素的目标和

给定两个==升序排序==的有序数组 A 和 B，以及一个目标值 x。 数组下标从 0 开始。 请你求出满足 A\[i]+B\[j]=x 的数对 (i,j)。 数据保证有唯一解。

**输入格式** 第一行包含三个整数 n,m,x ，分别表示 A 的长度，B 的长度以及目标值 x 第二行包含 n 个整数，表示数组 A 第三行包含 m 个整数，表示数组 B

**输出格式** 共一行，包含两个整数 i 和 j

**数据范围** 数组长度不超过 10^5^ 同一数组内元素各不相同。 1 ≤ 数组元素 ≤ 10^9^

**code**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010;

int n,m,x;
int A[N],B[N];


int main()
{
    scanf("%d%d%d", &n, &m, &x);
    for (int i = 0; i < n; i++) {
        scanf("%d", &A[i]);
    }
    for (int i = 0; i < m; i++) {
        scanf("%d", &B[i]);
    }
    int i, j;
    for(i = 0, j = m - 1; i < n; i++) {
        while (j >= 0 && A[i] + B[j] > x) j--;
        if(j >= 0 && A[i] + B[j] == x) {
            cout << i << ' ' << j;
            return 0;
        }
    }
    return 0;
}
```

***

### 位运算

**n的二进制表示中的第k位是几？**

```cpp
int n = 10;
for(int k = 3;k >= 0;k--){
  cout << (n>>k & 1);
}
```

**`lowbit(x)` 返回x的最后一位1**

`x & -x = x & (~x + 1)`

eg:

x = 1010........==1==000......0 \~x = 0101........==0==111......1 \~x + 1 = 0101........==1==000......0 x & (\~x + 1) = 0000........==1==000......0

```cpp
int x;
cout << (x & -x);
```

#### 801.二进制中1的个数

给定一个长度为 n 的数列，请你求出数列中每个数的二进制表示中 1 的个数。

**code**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

int lowbit(int x)  // 返回末尾的1
{
    return x & -x;
}

int main()
{
    int n;
    cin >> n;
    while (n--){
        int x;
        cin>>x;
        int res = 0;
        while(x){
            x -= lowbit(x);
            res++;
        }
        cout<<res<<' ';
    }
    return 0;
}
```

### (整数)离散化

#### 模板

```cpp
vector<int> alls; // 存储所有待离散化的值
sort(alls.begin(), alls.end()); // 将所有值排序
alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素

// 二分求出x对应的离散化的值
int find(int x) // 找到第一个大于等于x的位置
{
    int l = 0, r = alls.size() - 1;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (alls[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r + 1; // 映射到1, 2, ...n
}
```

#### 802.区间和 难☹️

假定有一个无限长的数轴，数轴上每个坐标上的数都是 0 现在，我们首先进行 n 次操作，每次操作将某一位置 x 上的数加 c 接下来，进行 m 次询问，每个询问包含两个整数 l 和 r，你需要求出在区间 \[l,r] 之间的所有数的和

**输入格式**

第一行包含两个整数 n 和 m 接下来 n 行，每行包含两个整数 x 和 再接下来 m 行，每行包含两个整数 l 和 r

**输出格式**

共 m 行，每行输出一个询问中所求的区间内数字和。

**数据范围**

−10^9^≤x≤10^9^, 1≤n,m≤10^5^ −10^9^≤l≤r≤10^9^ −10000≤c≤10000

**code**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

typedef pair<int, int> PII;

const int N = 300010;
int n,m;
int a[N],s[N];

vector<int> alls;
vector<PII> add, query;

int find(int x)  // 二分
{
    int l = 0, r = alls.size() - 1;
    while(l<r){
        int mid = l+r >> 1;
        if(alls[mid] >= x){
            r = mid;
        }
        else{
            l = mid + 1;
        }
    }
    return r + 1;
}


int main()
{
    scanf("%d%d", &n, &m);
    while (n--) {
        int x,c;
        scanf("%d%d", &x, &c);
        add.push_back({x,c});
        
        alls.push_back(x);
    }
    while (m--) {
        int l,r;
        scanf("%d%d", &l, &r);
        query.push_back({l,r});
        alls.push_back(l);
        alls.push_back(r);
    }
    
    // 排序 & 去重
    sort(alls.begin(),alls.end());
    alls.erase(unique(alls.begin(), alls.end()),alls.end());
    // 处理插入
    for(auto item : add){
        int x = find(item.first);
        a[x] += item.second;
    }
    // 预处理前缀和
    for(int i = 1;i <= alls.size(); i++){
        s[i] = s[i-1] + a[i];
    }
    // 处理询问
    for(auto item : query){
        int l = find(item.first);
        int r = find(item.second);
        cout << s[r] - s[l-1] << endl;
    }
    return 0;
}
```

注意:

1. `vector<int> alls` 中存的是需要离散化的下标
2. `const int N = 300010;` 查询最多10^5^次，插入最多10^5^次，最多需要离散化2\*查询+插入 = 3\*10^5^次
3.  实现unique函数

    ```cpp
    vector<int>::iterator unique(vector<int> &a)
    {
      for(int i = 0;i < a.size(); i++){
        if(!i || a[i]!=a[i-1]){
          a[j++] = a[i];
        }
    	}
      return a.begin() + j;
    }
    ```

    `alls.erase(unique(alls.begin(), alls.end()),alls.end());`改为

    `alls.erase(unique(alls),alls.end());`

### 区间合并

1. 按区间左端点排序
2. 扫描整个区间，把所有可能有交集的区间进行合并

#### 模板

```cpp
// 将所有存在交集的区间合并
void merge(vector<PII> &segs)
{
    vector<PII> res;

    sort(segs.begin(), segs.end());

    int st = -2e9, ed = -2e9;
    for (auto seg : segs)
        if (ed < seg.first)
        {
            if (st != -2e9) res.push_back({st, ed});
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second);

    if (st != -2e9) res.push_back({st, ed});

    segs = res;
}
```

#### 803. 区间合并

给定 n 个区间 \[l~~i~~,r~~i~~]，要求合并所有有交集的区间。 注意如果在端点处相交，也算有交集。 输出合并完成后的区间个数。

Eg. \[1,3] & \[2,6] -> \[1,6]

**输入格式**

第一行包含整数 n。 接下来 n 行，每行包含两个整数 l 和 r。

**输出格式**

共一行，包含一个整数，表示合并区间完成后的区间个数。

**数据范围**

1 ≤ n ≤ 100000 −10^9^ ≤ l~~i~~ ≤ r~~i~~ ≤ 10^9^

**code**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
 
using namespace std;

const int N = 100010;
typedef pair<int,int> PII;

vector<PII> segs;

void merge(vector<PII> &segs){
    vector<PII> res;
    
    sort(segs.begin(),segs.end());
    
    int st = -2e9, ed = -2e9;
    for(auto seg : segs){
        if(ed < seg.first){
            if(st!= -2e9) res.push_back({st,ed});
            st = seg.first;
            ed = seg.second;
        }
        else ed = max(ed, seg.second);
    }
    if(st!= -2e9) res.push_back({st,ed});
    
    segs = res;
}

int main()
{
    int n;
    scanf("%d", &n);
    while (n--) {
        int l,r;
        scanf("%d%d", &l, &r);
        segs.push_back({l,r});
    }
    
    merge(segs);
    
    cout << segs.size() << endl;
  
    return 0;
    
}
```

***

## Chapter2 数据结构

### 链表

> 笔试中一般不采用动态链表

#### 单链表

**826. 单链表**

实现一个单链表，链表初始为空，支持三种操作：

1. 向链表头插入一个数；
2. 删除第 k 个插入的数后面的数；
3. 在第 k 个插入的数后插入一个数。

现在要对该链表进行 M 次操作，进行完所有操作后，从头到尾输出整个链表。

**注意**: 题目中第 k 个插入的数并不是指当前链表的第 k 个数。例如操作过程中一共插入了 n 个数，则按照插入的时间顺序，这 n 个数依次为：第 1 个插入的数，第 2 个插入的数，…第 n 个插入的数。

**输入格式**

第一行包含整数 M，表示操作次数。

接下来 M 行，每行包含一个操作命令，操作命令可能为以下几种：

1. `H x`，表示向链表头插入一个数 x。
2. `D k`，表示删除第 k 个插入的数后面的数（当 k 为 0 时，表示删除头结点）。
3. `I k x`，表示在第 k 个插入的数后面插入一个数 x（此操作中 k 均大于 00）。

**输出格式**

共一行，将整个链表从头到尾输出。

**code**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010;

/* head存储链表头，
   e[i]存储节点i的值，
   ne[i]存储节点i的next指针，
   idx表示当前用到了哪个节点
 */
int head, e[N], ne[N];
int idx;

void init() // 初始化
{
    head = -1;
    idx = 0;
}

void insert_head(int x) // 插入头节点
{
    e[idx] = x;
    ne[idx] = head;
    head = idx;
    idx++;
}

void insert(int k, int x) // 在k坐标后插入节点(值为x)
{
    e[idx] = x;
    ne[idx] = ne[k];
    ne[k] = idx;
    idx++;
}

void remove_head() // 删除头节点head
{
    head = ne[head];
}

void remove(int k) // 删除下标k后的点
{
    ne[k] = ne[ne[k]];
}

int main()
{
    int M;
    scanf("%d", &M);
    init();
    while (M--) {
        int k, x;
        char op;
        
        cin >> op;
        if (op == 'H') {
            cin >> x;
            insert_head(x);
        }
        else if (op == 'D') {
            cin >> k;
            if (k == 0) {
                remove_head();
            }
            remove(k - 1);
        }
        else {
            cin >> k >> x;
            insert(k - 1,x);
        }
    }
    
    for(int i = head; i != -1; i = ne[i]){
        cout << e[i] << ' ';
    }
    cout << endl;
    
    return 0;
}
```

***

#### 双链表

```cpp
// e[]表示节点的值，l[]表示节点的左指针，r[]表示节点的右指针，idx表示当前用到了哪个节点
int e[N], l[N], r[N], idx;

// 初始化
void init()
{
    //0是左端点，1是右端点
    r[0] = 1, l[1] = 0;
    idx = 2;
}

// 在节点a的右边插入一个数x
void insert(int a, int x)
{
    e[idx] = x;
    l[idx] = a, r[idx] = r[a];
    l[r[a]] = idx, r[a] = idx ++ ;
}

// 删除节点a
void remove(int a)
{
    l[r[a]] = l[a];
    r[l[a]] = r[a];
}
```

**827. 双链表**

实现一个双链表，双链表初始为空，支持 55 种操作：

1. 在最左侧插入一个数；
2. 在最右侧插入一个数；
3. 将第 k 个插入的数删除；
4. 在第 k 个插入的数左侧插入一个数；
5. 在第 k 个插入的数右侧插入一个数

现在要对该链表进行 M 次操作，进行完所有操作后，从左到右输出整个链表。

**注意**:题目中第 k 个插入的数并不是指当前链表的第 k 个数。例如操作过程中一共插入了 n 个数，则按照插入的时间顺序，这 n 个数依次为：第 1 个插入的数，第 2 个插入的数，…第 n 个插入的数。

**输入格式**

第一行包含整数 M，表示操作次数。

接下来 M 行，每行包含一个操作命令，操作命令可能为以下几种：

1. `L x`，表示在链表的最左端插入数 x。
2. `R x`，表示在链表的最右端插入数 x。
3. `D k`，表示将第 k 个插入的数删除。
4. `IL k x`，表示在第 k 个插入的数左侧插入一个数。
5. `IR k x`，表示在第 k 个插入的数右侧插入一个数。

**输出格式**

共一行，将整个链表从左到右输出。

**数据范围**

1 ≤ M ≤ 100000 所有操作保证合法。

**code**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010;
int m;
int e[N], l[N], r[N], idx;

//初始化 
//0号点:head pointer
//1号点:tail pointer
void init()
{
    r[0] = 1;
    l[1] = 0;
    idx = 2;
}

//在k号点右边边插入值为x的点
void insert_right(int k, int x)
{
    e[idx] = x;
    l[idx] = k;
    r[idx] = r[k];
    l[r[k]] = idx;
    r[k] = idx;
    idx++;
}

//在k号点左边插入值为x的点:直接实现
void insert_left(int k, int x)
{
    e[idx] = x;
    l[idx] = l[k];
    r[idx] = k;
    r[l[k]] = idx;
    l[k] = idx;
    idx++;
}

//在k号点左边插入值为x的点:调用法
void insert_left_call(int k, int x)
{
    insert_right(l[k], x);
}

//删除第k个点
void delete_k(int k)
{
    r[l[k]] = r[k];
    l[r[k]] = l[k];
}


int main()
{
    scanf("%d", &m);
    init();
    while (m--){
        string op;
        cin >> op;
        if(op == "L"){
            int x;
            cin >> x;
            insert_right(0, x);
        }
        else if(op == "R"){
            int x;
            cin >> x;
            insert_left(1, x);
        }
        else if(op == "D"){
            int k;
            cin >> k;
            delete_k(k + 1);
        }
        else if(op == "IL"){
            int k, x;
            cin >> k >> x;
            insert_left(k + 1, x);
        }
        else if(op == "IR"){
            int k, x;
            cin >> k >> x;
            insert_right(k + 1, x);
        }
    }
    
    for(int i = r[0]; i != 1; i = r[i]){
        cout << e[i] << " ";
    }
    cout << endl;
    
    return 0;
}
```

#### 栈

**code**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010;

int stk[N], tt;

int m;

void init()
{
    tt = 0;
}

void push(int x)
{
    stk[++tt] = x;
}

void pop()
{
    tt--;
}

bool isEmpty()
{
    if(tt > 0)
        return false;
    else 
        return true;
}

int query()
{
    return stk[tt];
}

int main()
{
    scanf("%d", &m);
    init();
    while(m--){
        string op;
        cin >> op;
        if(op == "push"){
            int x;
            cin >> x;
            push(x);
        }
        else if(op == "pop"){
            pop();
        }
        else if(op == "empty"){
            string e;
            e = isEmpty() ? "YES" : "NO";
            cout << e << endl;
        }
        else if(op == "query"){
            cout << query() << endl;
        }
    }
    return 0;
}

```

#### 队列

队尾插入，队头推出

**code**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010;

int m;
int q[N], hh, tt;

void init(){
    tt = -1;
  	hh = 0;
}

void push(int x){
    q[++tt] = x;
}

void pop(){
    hh++;
}

bool isEmpty(){
    return hh <= tt ? false : true;
}

int query(){
    return q[hh];
}

int main()
{
    scanf("%d", &m);
    init();
    while(m--){
        string op;
        cin >> op;
        if(op == "push"){
            int x;
            cin >> x;
            push(x);
        }
        else if(op == "pop"){
            pop();
        }
        else if(op == "empty"){
            string e;
            e = isEmpty() ? "YES" : "NO";
            cout << e << endl;
        }
        else if(op == "query"){
            cout << query() << endl;
        }
    }
    return 0;
}
```

#### 单调栈

**830. 单调栈**

给定一个长度为 N 的整数数列，输出每个数左边第一个比它小的数，如果不存在则输出 −1。

**输入格式**

第一行包含整数 N，表示数列长度。

第二行包含 N 个整数，表示整数数列。

**输出格式**

共一行，包含 N 个整数，其中第 i 个数表示第 i 个数的左边第一个比它小的数，如果不存在则输出 −1。

**数据范围**

1 ≤ N ≤ 10^5^ 1 ≤ 数列中元素 ≤ 10^9^

**样例**

3 4 2 7 5 -1 3 -1 2 2

**code**

```cpp
#include <iostream>

using namespace std;

const int N = 100010;

int n;
int stk[N], tt;

int main()
{
    scanf("%d", &n);
    while (n--){
        int x;
        cin >> x;
        while(tt && stk[tt] >= x) tt--;
        if(tt)
            // cout << stk[tt] << ' ';
            printf("%d ",stk[tt]);
        else
            // cout << -1 << ' ';
            printf("-1 ");
            
        stk[++tt] = x;
    }
}
```

#### 单调队列

**154. 滑动窗口——难**

给定一个大小为 n≤10^6^ 的数组。

有一个大小为 k 的滑动窗口，它从数组的最左边移动到最右边。 你只能在窗口中看到 k 个数字。 每次滑动窗口向右移动一个位置。

以下是一个例子：

该数组为 `[1 3 -1 -3 5 3 6 7]`，k 为 3。

| 窗口位置                 | 最小值 | 最大值 |
| -------------------- | --- | --- |
| \[1 3 -1] -3 5 3 6 7 | -1  | 3   |
| 1 \[3 -1 -3] 5 3 6 7 | -3  | 3   |
| 1 3 \[-1 -3 5] 3 6 7 | -3  | 5   |
| 1 3 -1 \[-3 5 3] 6 7 | -3  | 5   |
| 1 3 -1 -3 \[5 3 6] 7 | 3   | 6   |
| 1 3 -1 -3 5 \[3 6 7] | 3   | 7   |

你的任务是确定滑动窗口位于每个位置时，窗口中的最大值和最小值。

**输入格式**

输入包含两行。

第一行包含两个整数 n 和 k，分别代表数组长度和滑动窗口的长度。 第二行有 n 个整数，代表数组的具体数值。

同行数据之间用空格隔开。

**输出格式**

输出包含两个。

第一行输出，从左至右，每个位置滑动窗口中的最小值。 第二行输出，从左至右，每个位置滑动窗口中的最大值。

**输入样例**：

```
8 3
1 3 -1 -3 5 3 6 7
```

**输出样例**：

```
-1 -3 -3 -3 3 3
3 3 5 5 6 7
```

**code**

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 1000010;

int n, k;
int a[N], q[N], tt, hh;

int main()
{
    scanf("%d%d", &n, &k);
    
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
  	
  	//最小值
    hh = 0;
    tt = -1;
    for (int i = 0; i < n; i++) {
        
        // 判断队头是否已经滑出窗口
        if(hh <= tt && i - k + 1 > q[hh]){
            hh++;
        }
        while(hh <= tt && a[q[tt]] > a[i]){
            tt--;
        }
        q[++tt] = i;
        if(i >= k-1){
            printf("%d ", a[q[hh]]);
        }
    }
    printf("\n");
  	//最大值
    hh = 0;
    tt = -1;
    for (int i = 0; i < n; i++) {
        
        // 判断队头是否已经滑出窗口
        if(hh <= tt && i - k + 1 > q[hh]){
            hh++;
        }
        while(hh <= tt && a[q[tt]] <= a[i]){
            tt--;
        }
        q[++tt] = i;
        if(i >= k-1){
            printf("%d ", a[q[hh]]);
        }
    }
    return 0;
}
```

**理解**

`init() : tt = -1, hh = 0; // 此时hh>tt 队空`

窗口应该在`(i-k+1)到i`范围内

* 出队判定 : 队列不为空& 队头下标 `q[hh] ≥ i - k + 1`
* 在`(i-k+1)到i`范围内 ，如果==队尾下标对应值比i下标对应值还要大==就丢弃（此时是以(i-k+1)到i为窗口范围，但实际i还未入队）
* i下标入队 `q[++tt] = i;`

**Leetcode**

[剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode.cn/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

#### KMP算法



## Chapter4 数学知识

### 最大公约数

**最大公约数**（Greatest Common Divisor）指两个或多个整数共有约数中最大的一个。也称最大公因数、最大公因子，a， b的最大公约数记为（a，b），同样的，a，b，c的最大 公约数记为（a，b，c），多个 整数的最大公约数也有同样的记号。求最大公约数有多种 方法，常见的有 质因数分解法、 短除法、 辗转相除法、 更相减损法。

**辗转相减法**求最大公约数 用(a，b)表示a和b的最大公因数：有结论(a，b)=(a，ka+b)，其中a、b、k都为自然数。也就是说，两个数的最大公约数，将其中一个数加到另一个数上，得到的新数，其公约数不变，比如(4，6)=(4+6，6)=(4，6+2×4)=2.要证明这个原理很容易：如果p是a和ka+b的公约数，p整除a，也能整除ka+b.那么就必定要整除b，所以p又是a和b的公约数，从而证明他们的最大公约数也是相等的.

基于上面的原理，就能实现我们的迭代相减法：(78，14)=(64，14)=(50，14)=(36，14)=(22，14)=(8，14)=(8，6)=(2，6)=(2，4)=(2，2)=(0，2)=2. 基本上思路就是大数减去小数，一直减到能算出来为止，在作为练习的时候，往往进行到某一步就已经可以看出得值.

辗转相减到辗转相除 迭代相减法简单，不过步数比较多，实际上我们可以看到，在上面的过程中，由(78，14)到(8，14)完全可以一步到位，因为(78，14)=(14×5+8，14)=(8，14)，由此就诞生出我们的辗转相除法.

即：<mark style="color:red;background-color:red;">**(a， b) = (a % b， b) = （b, a %b）**</mark>

相当于每一步都把数字进行缩小，等式右边就是每一步对应的缩小结果。

对（a， b）连续使用辗转相除，直到小括号内右边数字为0，小括号内左边的数就是两数最大公约数。

**代码如下:**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

int main()
{
    int T;
    cin >> T;
    while(T--){
        int m, n;
        cin >> m >> n;
        // (m, n)
        while(n){
            int temp = m % n;
            m = n;
            n = temp;
            // (n, m%n)
        }
        cout << m << endl;
    }
    return 0;
}
```



## Chapter5 动态规划

### 整数划分

{% embed url="https://www.acwing.com/problem/content/902/" %}
题目链接
{% endembed %}

一个正整数 $n$ 可以表示成若干个正整数之和，形如：$n = n\_1 + n\_2 + … + n\_k$，其中 $n\_1 \ge n\_2 \ge … \ge n\_k, k \ge 1$。

我们将这样的一种表示称为正整数 $n$ 的一种划分。

现在给定一个正整数 $n$，请你求出 $n$ 共有多少种不同的划分方法。

**输入格式**

共一行，包含一个整数 $n$。

**输出格式**

共一行，包含一个整数，表示总划分数量。

由于答案可能很大，输出结果请对 $10^9 + 7$ 取模。

**数据范围**

$1 \le n \le 1000$

**输入样例:**

```
5
```

**输出样例：**

```
7
```

$$
f[i][j]: 前i个整数恰好拼成j的方案数 \\
 f[i][j]=f[i−1][j]+f[i][j−i]
$$





