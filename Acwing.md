# 基础算法

### 快速排序

    #include<iostream>
    using namespace std;

    const int N = 1000010;

    int q[N],n;

    void quick_sort(int q[],int l,int r){
        if(l>=r)return;
        
        int i = l-1,j = r+1;
        int x=q[l+r>>1];
        while(i<j){
            do i++;while(q[i]<x);
            do j--;while(q[j]>x);
            if(i<j)swap(q[i],q[j]);
        }
        quick_sort(q,l,j);
        quick_sort(q,j+1,r);
    }

    int main(){
        scanf("%d",&n);
        for(int i = 0;i<n;i++)scanf("%d",&q[i]);
        
        quick_sort(q,0,n-1);
        
        for(int i = 0;i<n;i++)printf("%d ",q[i]);
        
        return 0;
    }

### 归并排序

    #include<iostream>
    using namespace std;

    const int N = 100010;

    int n,q[N],tmp[N];

    void merge_sort(int q[],int l,int r){
        if(l>=r)return;
        
        int mid = l+r>>1;
        merge_sort(q,l,mid),merge_sort(q,mid+1,r);
        
        int k = 0,i = l,j = mid+1;
        while(i<=mid&&j<=r)
            if(q[i]<q[j])tmp[k++] = q[ i++];
            else tmp[k++] = q[j++];
        while(i<=mid)tmp[k++] = q[i++];
        while(j<= r)tmp[k++] = q[j++];
        
        for(int i =l,j =0 ;i<=r;i++,j++)q[i] = tmp[j];
    }
    int main(){
        scanf("%d",&n);
        for(int i = 0;i<n;i++)scanf("%d",&q[i]);
        
        merge_sort(q,0,n-1);
        
        for(int i =0;i<n;i++)printf("%d ",q[i]);
        
        return 0;
    }

### 并查集大致讲解

开始时每个集合都是一个独立的集合,并且都是等于自己本身下标的数
例如:`p[5]=5,p[3]=3;`

如果是M操作的话那么就将集合进行合并,合并的操作是:
`p[3]=p[5]=5;`

所以3的祖宗节点便成为了5

此时以5为祖宗节点的集合为{5，3}

如果要将p\[9]=9插入到p\[3]当中,应该找到3的祖宗节点,

然后再把p\[9]=9插入其中,所以`p[9]=find(3)`;(find()函数用于查找祖宗节点)

也可以是p\[find(9)]=find(3),因为9的节点本身就是9
此时以5为祖宗节点的集合为{5,3,9};

如果碰到多个数的集合插入另一个集合当中其原理是相同的

例如:

上述中以5为祖宗节点的是p\[5],p\[3],p\[9];(即`p[5]=5,p[3]=5,p[9]=5`)

再构造一个以6为祖宗节点的集合为{6,4，7，10}

如果要将以6为祖宗节点的集合插入到以5为祖宗节点的集合,则该操作可以是

`p[6]=find(3)`（或者find(9),find(5)）

此时p\[6]=5

当然如果是以6为祖宗节点集合中的4,7,10则可以这样

`p[find(4)]=find(3)`
或者`p[find(7)]=find(3)`均可以

此时以6为祖宗节点的集合的祖宗节点都成为了5

       #include<iostream>
    using namespace std;

    const int N = 100010;

    int n,m;
    int p[N];

    int find(int x){    //返回x的祖宗节点 + 路径压缩
        if(p[x]!=x)p[x] = find(p[x]);
        return p[x];    //p[x]表示x的父亲节点
    }
    int main(){
        int n,m;
        scanf("%d%d",&n,&m);
        for(int i = 0;i<=n;i++)p[i] = i;
        while(m--){
            char op[2];
            int a,b;
            scanf("%s%d%d",op,&a,&b);
            if(*op == 'M')p[find(a)] = find(b);//a的祖宗节点的父亲节点为b的祖宗节点
            else{
                if(find(a) == find(b))puts("Yes");
                else puts("No");
            }
        }

        return 0;
    }

### 高精度加法

![IMG\_20221121\_203149.jpg](https://note.youdao.com/yws/res/297/WEBRESOURCEba01eb3253bc9f2f53b0c9a9998f7da1)

    #include<iostream>
    using namespace std;

    const int N = 100010;

    int A[N],B[N],C[N];

    int add(int a[],int b[],int c[],int cnt){
        int t = 0;
        for(int i = 1;i<=cnt;i++){
            t+=a[i]+b[i];
            c[i] = t%10;
            t/=10;
        }
        if(t)c[++cnt] = 1;
        
        return cnt;
    }

    int main(){
        string a,b;
        cin>>a>>b;
        
        int cnt1= 0;
        for(int i = a.size()-1;i>=0;i--){
            A[++cnt1] = a[i] - '0';
        }
        
        int cnt2 = 0;
        for(int i = b.size()-1;i>=0;i--){
            B[++cnt2] = b[i] - '0';
        }
        
        int tot = add(A,B,C,max(cnt1,cnt2));
        
        for(int i = tot;i>=1;i--){
            cout<<C[i];
        }
        
        return 0;
    }

### 前缀和

什么是前缀和？
数列的和时，Sn = a1+a2+a3+…an; Sn就是数列的前 n 项和。

前缀和就是新建一个数组，新建数组中保存原数组前 n 项的和。

***

输入一个长度为 n 的整数序列。

接下来再输入 m 个询问，每个询问输入一对 l,r。

对于每个询问，输出原序列中从第 l 个数到第 r 个数的和。

输入格式
第一行包含两个整数 n 和 m。

第二行包含 n 个整数，表示整数数列。

接下来 m 行，每行包含两个整数 l 和 r，表示一个询问的区间范围。

输出格式
共 m 行，每行输出一个询问的结果。

数据范围
1≤l≤r≤n,
1≤n,m≤100000,
−1000≤数列中元素的值≤1000
输入样例：

    5 3
    2 1 3 6 4
    1 2
    1 3
    2 4

输出样例：

    3
    6
    10



    #include<iostream>
    using namespace std;

    const int N = 100010;

    int a[N],s[N];

    int main(){

        ios::sync_with_stdio(false);

        int n,m;
        cin>>n>>m;

        for(int i = 1;i<=n;i++)cin>>a[i];

        s[0] = 0;
        for(int i = 1;i<=n;i++)s[i] = s[i-1]+a[i];
        
        while(m--){
            int l,r;
            cin>>l>>r;
            cout<<s[r] - s[l-1]<<endl;
        }
        return 0;
    }

### 二维前缀和推导

如图：
![](https://img-blog.csdnimg.cn/20201217174700577.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTYyOTI4NQ==,size_16,color_FFFFFF,t_70)

**紫色面积**是指(1,1)左上角到(i,j-1)右下角的矩形面积, **绿色面积**是指(1,1)左上角到(i-1, j )右下角的矩形面积。**每一个颜色的矩形面积都代表了它所包围元素的和**。
![](https://img-blog.csdnimg.cn/20201216215336857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTYyOTI4NQ==,size_16,color_FFFFFF,t_70)
**从图中我们很容易看出**，整个外围蓝色矩形面积s\[i]\[j] = 绿色面积s\[i-1]\[j] + 紫色面积s\[i]\[j-1] - 重复加的红色的面积s\[i-1]\[j-1]+小方块的面积a\[i]\[j];

**因此得出二维前缀和预处理公式**

s\[i] \[j] = s\[i-1]\[j] + s\[i]\[j-1 ] + a\[i] \[j] - s\[i-1]\[ j-1]

**接下来回归问题去求**以(x1,y1)为左上角和以(x2,y2)为右下角的矩阵的元素的和。

如图：
![](https://img-blog.csdnimg.cn/2020121717473287.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTYyOTI4NQ==,size_16,color_FFFFFF,t_70)

**紫色面积**是指 ( 1,1 )左上角到(x1-1,y2)右下角的矩形面积 ，**黄色面积**是指(1,1)左上角到(x2,y1-1)右下角的矩形面积；

**不难推出：**
![](https://img-blog.csdnimg.cn/20201217170800381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTYyOTI4NQ==,size_16,color_FFFFFF,t_70)

绿色矩形的面积 = 整个外围面积s\[x2, y2] - 黄色面积s\[x2, y1 - 1] - 紫色面积s\[x1 - 1, y2] + 重复减去的红色面积 s\[x1 - 1, y1 - 1]

**因此二维前缀和的结论为：**

以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：
s\[x2, y2] - s\[x1 - 1, y2] - s\[x2, y1 - 1] + s\[x1 - 1, y1 - 1]

    #include<iostream>
    using namespace std;

    const int N = 1010;
    int n,m,q;
    int s[N][N];

    int main(){
        cin>>n>>m>>q;
        for(int i = 1;i<=n;i++){
            for(int j = 1;j<=m;j++){
                cin>>s[i][j];
            }
        }
        for(int i = 1;i<=n;i++){
            for(int j = 1;j<=m;j++){
                s[i][j] += s[i-1][j] + s[i][j-1] - s[i-1][j-1];
            }
        }
        while(q--){
            int x1,x2,y1,y2;
            cin>>x1>>y1>>x2>>y2;
            cout<<s[x2][y2] - s[x1-1][y2] - s[x2][y1-1] + s[x1 - 1][y1 - 1]<<endl;
        }
        
        return 0;
    }

### 模拟栈

实现一个栈，栈初始为空，支持四种操作：

`push x` – 向栈顶插入一个数 x；
`pop `– 从栈顶弹出一个数；
`empty `– 判断栈是否为空；
`query `– 查询栈顶元素。
现在要对栈进行 M 个操作，其中的每个操作 3 和操作 4 都要输出相应的结果。

输入格式
第一行包含整数 M，表示操作次数。

接下来 M 行，每行包含一个操作命令，操作命令为 push x，pop，empty，query 中的一种。

输出格式
对于每个 empty 和 query 操作都要输出一个查询结果，每个结果占一行。

其中，empty 操作的查询结果为 YES 或 NO，query 操作的查询结果为一个整数，表示栈顶元素的值。

数据范围
1≤M≤100000,
1≤x≤109
所有操作保证合法。

输入样例：

    10
    push 5
    query
    push 6
    pop
    query
    pop
    empty
    push 4
    query
    empty

输出样例：

    5
    5
    YES
    4
    NO

```
#include<iostream>

using namespace std;

const int N=100010;

int stk[N],top;//stk[]为栈，top为栈顶。

int main(){
    int m;
    cin>>m;
    while(m--){
        string op;
        int x;
        cin>>op;
        if(op=="push"){//向栈顶插入一个元素
            cin>>x;
            stk[top++]=x;//
        }
        else if(op =="pop"){//从栈顶弹出一个数
            --top;
        }else if(op == "empty")//判断栈是否为空
        if(top)printf("NO\n");//查询栈顶元素
        else printf("YES\n");
    
    else cout<<stk[top-1]<<endl;
    }
    return 0;
}

```

### 模拟队列

实现一个队列，队列初始为空，支持四种操作：

`push x` – 向队尾插入一个数 x；
`pop` – 从队头弹出一个数；
`empty` – 判断队列是否为空；
`query` – 查询队头元素。
现在要对队列进行 M 个操作，其中的每个操作 3 和操作 4 都要输出相应的结果。

输入格式
第一行包含整数 M，表示操作次数。

接下来 M 行，每行包含一个操作命令，操作命令为 push x，pop，empty，query 中的一种。

输出格式
对于每个 empty 和 query 操作都要输出一个查询结果，每个结果占一行。

其中，empty 操作的查询结果为 YES 或 NO，query 操作的查询结果为一个整数，表示队头元素的值。

数据范围
1≤M≤100000,
1≤x≤109,
所有操作保证合法。

输入样例：

    10
    push 6
    empty
    query
    pop
    empty
    push 3
    push 4
    pop
    query
    push 6

输出样例：

    NO
    6
    YES
    4



    #include<iostream>
    using namespace std;

    const int N = 1e5+10;

    int q[N];
        
    int main(){//队列的顶部在下面，底部在上面
        int m;
        int hh=0,tt=-1;//hh为队列的顶部，tt为队列的底部。
        cin>>m;
        while(m--){
            string op;
            int x;
            cin>>op;
            if(op == "push"){
                cin>>x;
                q[++tt] = x;//在队列的底部插入元素
            }else if(op == "pop"){
                hh++;//将元素从队列的底部弹出
            }else if(op == "empty"){
                if(hh<=tt)cout<<"NO"<<endl;//如果顶部指针小于等于底部指针，说明不空
                else cout<<"YES"<<endl;
            }else {
                cout<<q[hh]<<endl;//查询操作，输出队头元素。
            }
        }
        return 0;
    }

### 单调栈

算法原理:
用单调递增栈，当该元素可以入栈的时候，栈顶元素就是它左侧第一个比它小的元素。
以：3 4 2 7 5 为例，过程如下：
![](https://img-blog.csdnimg.cn/20201211221031165.gif#pic_center)

给定一个长度为 N 的整数数列，输出每个数左边第一个比它小的数，如果不存在则输出 −1。

输入格式
第一行包含整数 N，表示数列长度。

第二行包含 N 个整数，表示整数数列。

输出格式
共一行，包含 N 个整数，其中第 i 个数表示第 i 个数的左边第一个比它小的数，如果不存在则输出 −1。

数据范围
1≤N≤105
1≤数列中元素≤109
输入样例：

    5
    3 4 2 7 5

输出样例：

    -1 3 -1 2 2



    #include<iostream>
    using namespace std;

    const int N = 100010;//栈是一个上面开口的长方体，

    int stk[N],tt;//tt为栈的顶部，即开口的部分

    int main(){
        int n;
        cin>>n;
        while(n--){
            int x;
            cin>>x;
            while(tt && stk[tt]>=x)tt--;//当栈顶元素即tt存在，不为空，并且，当前栈顶元素大于等于待入栈的元素x，则当前栈顶元素出栈。
            if(!tt)cout<<"-1 ";//如果栈空，则说明没有比当前栈顶元素小的值，输出-1.
            else cout<<stk[tt]<<" ";//如果栈不空，则当前栈顶元素就是第一个比它小的元素。输出栈顶元素。
            stk[++ tt] = x;//带入元素入栈，下一轮判断中，它即为当前栈顶元素。
        }
        
        return 0;
    }

### KMP

**一、什么是KMP算法及一些基本概念**
首先，什么是KMP算法。这是一个字符串匹配算法，对暴力的那种一一比对的方法进行了优化，使时间复杂度大大降低（我不会算时间复杂度。。。，目前也只能这么理解，还有KMP是取的三个发明人的名字首字母组成的名字）。

​ 然后是一些基本概念：

1、s\[ ]是模式串，即比较长的字符串。
2、p\[ ]是模板串，即比较短的字符串。（这样可能不严谨。。。）
3、“非平凡前缀”：指除了最后一个字符以外，一个字符串的全部头部组合。
4、“非平凡后缀”：指除了第一个字符以外，一个字符串的全部尾部组合。（后面会有例子，均简称为前/后缀）
5、“部分匹配值”：前缀和后缀的最长共有元素的长度。
6、next\[ ]是“部分匹配值表”，即next数组，它存储的是每一个下标对应的“部分匹配值”，是KMP算法的核心。（后面作详细讲解）。

**核心思想**：在每次失配时，不是把p串往后移一位，而是把p串往后移动至下一次可以和前面部分匹配的位置，这样就可以跳过大多数的失配步骤。而每次p串移动的步数就是通过查找next\[ ]数组确定的。

**二、next数组的含义及手动模拟（具体求法和代码在后面）**
​ 然后来说明一下next数组的含义：对next\[ j ] ，是p\[ 1, j ]串中前缀和后缀相同的最大长度（部分匹配值），即 p\[ 1, next\[ j ] ] = p\[ j - next\[ j ] + 1, j ]。

如：![](https://cdn.acwing.com/media/article/image/2020/06/12/31041_6f934f82ac-next%E4%BE%8B%E5%AD%90.PNG)

手动模拟求next数组：

对 p = “abcab”

p	a	b	c	a	b
下标	1	2	3	4	5
next\[ ]	0	0	0	1	2
对next\[ 1 ] ：前缀 = 空集—————后缀 = 空集—————next\[ 1 ] = 0;

对next\[ 2 ] ：前缀 = { a }—————后缀 = { b }—————next\[ 2 ] = 0;

对next\[ 3 ] ：前缀 = { a , ab }—————后缀 = { c , bc}—————next\[ 3 ] = 0;

对next\[ 4 ] ：前缀 = { a , ab , abc }—————后缀 = { a . ca , bca }—————next\[ 4 ] = 1;

对next\[ 5 ] ：前缀 = { a , ab , abc , abca }————后缀 = { b , ab , cab , bcab}————next\[ 5 ] = 2;

**三、匹配思路和实现代码**
​ KMP主要分两步：求next数组、匹配字符串。个人觉得匹配操作容易懂一些，疑惑我一整天的是求next数组的思想。所以先把匹配字符串讲一下。

​ s串 和 p串都是从1开始的。i 从1开始，j 从0开始，每次s\[ i ] 和p\[ j + 1 ]比较

![](https://cdn.acwing.com/media/article/image/2020/06/12/31041_8e70c3eeac-%E5%8C%B9%E9%85%8D.PNG)

当匹配过程到上图所示时，

s\[ a , b ] = p\[ 1, j ] && s\[ i ] != p\[ j + 1 ] 此时要移动p串（不是移动1格，而是直接移动到下次能匹配的位置）

其中1串为\[ 1, next\[ j ] ]，3串为\[ j - next\[ j ] + 1 , j ]。由匹配可知 1串等于3串，3串等于2串。所以直接移动p串使1到3的位置即可。这个操作可由j = next\[ j ]直接完成。 如此往复下去，当 j == m时匹配成功。

代码如下

for(int i = 1, j = 0; i <= n; i++)
{
while(j && s\[i] != p\[j+1]) j = ne\[j];
//如果j有对应p串的元素， 且s\[i] != p\[j+1], 则失配， 移动p串
//用while是由于移动后可能仍然失配，所以要继续移动直到匹配或整个p串移到后面（j = 0)

    if(s[i] == p[j+1]) j++;
    //当前元素匹配，j移向p串下一位
    if(j == m)
    {
        //匹配成功，进行相关操作
        j = next[j];  //继续匹配下一个子串
    }

}
注：采用上述的匹配方法（ i 与 j+1 比较）我不清楚（其实是想不清楚）为什么要这样。。。脑子有点不好使。而不推荐下标从0开始的原因我认为是：若下标从0开始的话，next\[ ]数组的值都会相应-1，这就会导致它的实际含义与其定义的意思不符（部分匹配值和next数组值相差1），思维上有点违和，容易出错。
（看了习题课，在实际操作上下标从0开始代码会多很多东西，比从1开始复杂一些，嗯。。。确实

**四、求next数组的思路和实现代码**
​ next数组的求法是通过模板串自己与自己进行匹配操作得出来的（代码和匹配操作几乎一样）。

![](https://cdn.acwing.com/media/article/image/2020/06/12/31041_97225cdcac-next%E6%95%B0%E7%BB%84.PNG)

代码如下

    for(int i = 2, j = 0; i <= m; i++)
    {
        while(j && p[i] != p[j+1]) j = next[j];

        if(p[i] == p[j+1]) j++;

        next[i] = j;
    }

代码和匹配操作的代码几乎一样，关键在于每次移动 i 前，将 i 前面已经匹配的长度记录到next数组中。

**五、完整代码**

    // 注：这不是题目的AC代码，是一个最基本的模板代码
    #include <iostream>

    using namespace std;

    const int N = 100010, M = 10010; //N为模式串长度，M匹配串长度

    int n, m;
    int ne[M]; //next[]数组，避免和头文件next冲突
    char s[N], p[M];  //s为模式串， p为匹配串

    int main()
    {
        cin >> n >> s+1 >> m >> p+1;  //下标从1开始

        //求next[]数组
        for(int i = 2, j = 0; i <= m; i++)
        {
            while(j && p[i] != p[j+1]) j = ne[j];
            if(p[i] == p[j+1]) j++;
            ne[i] = j;
        }
        //匹配操作
        for(int i = 1, j = 0; i <= n; i++)
        {
            while(j && s[i] != p[j+1]) j = ne[j];
            if(s[i] == p[j+1]) j++;
            if(j == m)  //满足匹配条件，打印开头下标, 从0开始
            {
                //匹配完成后的具体操作
                //如：输出以0开始的匹配子串的首字母下标
                //printf("%d ", i - m); (若从1开始，加1)
                j = ne[j];            //再次继续匹配
            }
        }

        return 0;
    }

### 哈希表

1.  c++模拟散列表
    题目描述
    维护一个集合，支持如下几种操作：

“I x”，插入一个数x；
“Q x”，询问数x是否在集合中出现过；
现在要进行N次操作，对于每个询问操作输出对应的结果。

**输入格式**
第一行包含整数N，表示操作数量。

接下来N行，每行包含一个操作指令，操作指令为”I x”，”Q x”中的一种。

**输出格式**
对于每个询问指令“Q x”，输出一个询问结果，如果x在集合中出现过，则输出“Yes”，否则输出“No”。

每个结果占一行。

**数据范围**

    1≤N≤105
    −109≤x≤109

**输入样例：**

    5
    I 1
    I 2
    I 3
    Q 2
    Q 5

**输出样例：**

    Yes
    No

**拉链法**

    #include<iostream>
    #include<cstdio>
    #include<cstring>
    using namespace std;
    const int N = 100003;                    //找比题设范围大且离2的整次幂最远的质数作为最大范围，是为了减少哈希映射的冲突
    int n , h[N] , ne[N] , e[N] , idx;
    void insert(int x)
    {   int k = (x % N + N) % N;             //因为负数mod的特殊性-10 % 3 = 2，所以这样映射
        e[idx] = x;                          //将x存储在idx结点（头插法插入）
        ne[idx] = h[k];                      //存储x所在结点的指针（更改链表指向，建立链式结构）
        h[k] = idx ++;                      //h[k]储存指向新插入的数据指针（头指针指向新插入的数据）之后idx++
    }

                                             //h[0] = -1 , ne[0] = h[0] = -1 , h[0] = idx = 0;
                                             //h[0] = 0 , ne[1] = h[0] = 0 , h[0] = idx = 1;
    bool find(int x)
    {   int k = (x % N + N) % N;
        for(int i = h[k];i != -1;i = ne[i])  //h[k] 是头节点，ne[h[k]] 是头节点的前一结点
         if(e[i] == x) return true;

         return false;
    }

    int main()
    {   cin >> n;
        memset(h , -1 ,sizeof h);            //初始化每个拉链处的头结点均为-1，模拟链表的头结点指向NULL(-1)
        char op[2];
        while(n --)
        {   int x;
            scanf("%s%d",op,&x);            //巧妙地输入，避免输入的是上组数据的行末回车/空格
            if(*op == 'I') insert(x);        //插入操作
            else if(*op == 'Q')              //寻找操作
            {
                if(find(x)) cout << "Yes"<<endl;
                else cout << "No"<< endl;
            }
        }

        return 0;
    }

**开放寻址法**
思路
开放寻址法采用hash函数找到在hash数组中对应的位置，如果该位置上有值，并且这个值不是寻址的值，则出现冲突碰撞，需要解决冲突方案，该算法采用简单的向右继续寻址来解决问题。

由于思想简单，将不深入探讨，在次，我们探讨一下文中常常让人摸不着头脑的参数

让人费解的参数

    1. const int N = 200003; 
        1.1开放寻址操作过程中会出现冲突的情况，一般会开成两倍的空间，减少数据的冲突

        1.2如果使用%来计算索引， 把哈希表的长度设计为素数（质数）可以大大减小哈希冲突
        比如
        10%8 = 2      10%7 = 3
        20%8 = 4      20%7 = 6
        30%8 = 6      30%7 = 2
        40%8 = 0      40%7 = 5
        50%8 = 2      50%7 = 1
        60%8 = 4      60%7 = 4
        70%8 = 6      70%7 = 0

    这就是为什么要找第一个比空间大的质数



    2.const int null = 0x3f3f3f3f 和  memset(h, 0x3f, sizeof h)之间的关系;

        首先，必须要清楚memset函数到底是如何工作的
        先考虑一个问题，为什么memset初始化比循环更快？
        答案：memset更快，为什么？因为memset是直接对内存进行操作。memset是按字节（byte）进行复制的

        void * memset(void *_Dst,int _Val,size_t _Size);
        这是memset的函数声明
        第一个参数为一个指针，即要进行初始化的首地址
        第二个参数是初始化值，注意，并不是直接把这个值赋给一个数组单元（对int来说不是这样）
        第三个参数是要初始化首地址后多少个字节
        看到第二个参数和第三个参数，是不是又感觉了
        h是int类型，其为个字节， 第二个参数0x3f八位为一个字节，所以0x3f * 4(从高到低复制4份) = 0x3f3f3f3f

        这也说明了为什么在memset中不设置除了-1， 0以外常见的值
        比如1, 字节表示为00000001，memset(h, 1, 4)则表示为0x01010101



    3. 为什么要取0x3f3f3f,为什么不直接定义无穷大INF = 0x7fffffff,即32个1来初始化呢？

    3.1 首先，0x3f3f3f的体验感很好，0x3f3f3f3f的十进制是1061109567，也就是10^9级别的
        （和0x7fffffff一个数量级），而一般场合下的数据都是小于10^9的，所以它可以作为无穷大
        使用而不致出现数据大于无穷大的情形。
        比如0x3f3f3f3f+0x3f3f3f3f=2122219134，这非常大但却没有超过32-bit，int的表示范围，
        所以0x3f3f3f3f还满足了我们“无穷大加无穷大还是无穷大”的需求。
        但是INF不同，一旦加上某个值，很容易上溢，数值有可能转成负数，有兴趣的小伙伴可以去试一试。

    3.2 0x3f3f3f3f还能给我们带来一个意想不到的额外好处：如果我们想要将某个数组清零，
        我们通常会使用memset(a,0,sizeof(a))这样的代码来实现（方便而高效），但是当我们想
        将某个数组全部赋值为无穷大时（例如解决图论问题时邻接矩阵的初始化），就不能使用
        memset函数而得自己写循环了（写这些不重要的代码真的很痛苦），我们知道这是因为memset
        是按字节操作的，它能够对数组清零是因为0的每个字节都是0，
        现在如果我们将无穷大设为0x3f3f3f3f，那么奇迹就发生了，0x3f3f3f3f的每个字节都是0x3f！所以
        要把一段内存全部置为无穷大，我们只需要memset(a,0x3f,sizeof(a))。



    开放寻址法
    #include<iostream>
    #include<cstring>
    using namespace std;

    const int N = 200003,null = 0x3f3f3f3f;
    int h[N];

    /*
    memset是一个初始化函数，作用是将某一块内存中的全部设置为指定的值
    s指向要填充的内存块。
    c是要被设置的值。
    n是要被设置该值的字符数。
    返回类型是一个指向存储区s的指针。
    */

    int find(int x){
        int k = (x%N+N)%N;
        while(h[k]!=null&&h[k]!=x){//当前的位置不为空，并且当前位置的值不为x。
            k++;//去下一个位置
            if(k == N)k =0;//走到头了，k = 0，重新回去再找一遍。
        }
        return k;//返回找到x所在的下标。
    }
    int main(){
        int n;
        scanf("%d",&n);
        memset(h,0x3f,sizeof h);//将数组h初始化，每一个位置都设置为0x3f.
        while(n--){
            char op[2];//用op[2]来写入字符，可以直接忽略回车和空格。
            int x;
            scanf("%s%d",op,&x);//x为查询的点的值.
            int k =find(x);//k为要查询的点x的下标。
            if(*op == 'I')h[find(x)] = x;//插入操作
            else {
                if(h[k]!=null)puts("Yes");//要查询的点x存在。
                else puts("No");
            }
        }
        return 0;
    }

### Trie字符串

1.  Trie字符串统计
    维护一个字符串集合，支持两种操作：

I x 向集合中插入一个字符串 x；
Q x 询问一个字符串在集合中出现了多少次。
共有 N 个操作，输入的字符串总长度不超过 105，字符串仅包含小写英文字母。

**输入格式**
第一行包含整数 N，表示操作数。

接下来 N 行，每行包含一个操作指令，指令为 I x 或 Q x 中的一种。

**输出格式**
对于每个询问指令 Q x，都要输出一个整数作为结果，表示 x 在集合中出现的次数。

每个结果占一行。

**数据范围**
1≤N≤2∗104
**输入样例：**

    5
    I abc
    Q abc
    Q ab
    I ab
    Q ab

**输出样例：**

    1
    0
    1

Trie树又称字典树、单词查找树。是一种能够高效存储和查找字符串集合的数据结构。
![](https://cdn.acwing.com/media/article/image/2020/06/13/31041_aa11ff2cad-Trie1.PNG)
**用数组来模拟Trie树**
![](https://cdn.acwing.com/media/article/image/2020/06/13/31041_aed49a42ad-Trie2.PNG)
插入操作代码：

    void insert(char *str)
    {
        int p = 0;  //类似指针，指向当前节点
        for(int i = 0; str[i]; i++)
        {
            int u = str[i] - 'a'; //将字母转化为数字
            if(!son[p][u]) son[p][u] = ++idx;
            //该节点不存在，创建节点,其值为下一个节点位置
            p = son[p][u];  //使“p指针”指向下一个节点位置
        }
        cnt[p]++;  //结束时的标记，也是记录以此节点结束的字符串个数
    }

查找操作代码：

    int query(char *str)
    {
        int p = 0;
        for(int i = 0; str[i]; i++)
        {
            int u = str[i] - 'a';
            if(!son[p][u]) return 0;  //该节点不存在，即该字符串不存在
            p = son[p][u]; 
        }
        return cnt[p];  //返回字符串出现的次数
    }

**完整代码**

    //Trie树快速存储字符集合和快速查询字符集合
    #include <iostream>

    using namespace std;

    const int N = 100010;
    //son[][]存储子节点的位置，分支最多26条；
    //cnt[]存储以某节点结尾的字符串个数（同时也起标记作用）
    //idx表示当前要插入的节点是第几个,每创建一个节点值+1
    int son[N][26], cnt[N], idx;
    char str[N];

    void insert(char *str)
    {
        int p = 0;  //类似指针，指向当前节点
        for(int i = 0; str[i]; i++)
        {
            int u = str[i] - 'a'; //将字母转化为数字
            if(!son[p][u]) son[p][u] = ++idx;   //该节点不存在，创建节点
            p = son[p][u];  //使“p指针”指向下一个节点
        }
        cnt[p]++;  //结束时的标记，也是记录以此节点结束的字符串个数
    }

    int query(char *str)
    {
        int p = 0;
        for(int i = 0; str[i]; i++)
        {
            int u = str[i] - 'a';
            if(!son[p][u]) return 0;  //该节点不存在，即该字符串不存在
            p = son[p][u]; 
        }
        return cnt[p];  //返回字符串出现的次数
    }

    int main()
    {
        int m;
        cin >> m;

        while(m--)
        {
            char op[2];
            scanf("%s%s", op, str);

            if(*op == 'I') insert(str);
            else printf("%d\n", query(str));
        }

        return 0;
    }

### 堆排序

838.堆排序
输入一个长度为 n 的整数数列，从小到大输出前 m 小的数。

输入格式
第一行包含整数 n 和 m。

第二行包含 n 个整数，表示整数数列。

输出格式
共一行，包含 m 个整数，表示整数数列中前 m 小的数。

数据范围
1≤m≤n≤105，
1≤数列中元素≤109
输入样例：

    5 3
    4 5 1 3 2

输出样例：

    1 2 3

如何手写一个堆？完全二叉树 5个操作

1.  插入一个数         heap\[ ++ size] = x; up(size);
2.  求集合中的最小值   heap\[1]
3.  删除最小值         heap\[1] = heap\[size]; size -- ;down(1);
4.  删除任意一个元素   heap\[k] = heap\[size]; size -- ;up(k); down(k);
5.  修改任意一个元素   heap\[k] = x; up(k); down(k);



    #include<iostream>
    using namespace std;

    const int N = 100010;

    int n,m;
    int h[N],cnt;
    //h[i] 表示第i个结点存储的值，i从1开始，2*i是左子节点，2*i + 1是右子节点
    //cnt 既表示堆里存储的元素个数，又表示最后一个结点的下标

    void down(int u){
        int t = u;//t存储三个点中最小的节点的下标，初始化为当前节点u。
        if(u*2<=siz && h[u*2]<h[t])t = u*2;//当前节点的左儿子存在 并且 当前节点的左儿子的值小于当前节点。更新t的值为左儿子的下标
        if(u*2+1<=siz && h[u*2+1]<h[t])t=u*2+1;//当前节点的右儿子存在 并且 当前节点的右儿子的值小于当前节点。更新t的值为右儿子的下标
        if(u!=t){//当经过上面两个判断后 当前节点的下标u不为最小的节点下标，如果t==u意味着不用变动，u就是三个节点中拥有最小值的节点下标，否则交换数值。 
            swap(h[u],h[t]);
            down(t);//交换数值后，t这个结点存储原本u的值，u存储存储t的值（三个数中的最小值）。
                    //u不用调整了，但t情况不明，可能需要调整。直到它比左右子节点都小
        }
    }

    int main(){
        scanf("%d%d",&n,&m);
        for(int i = 1;i<=n;i++)scanf("%d",&h[i]);
        cnt = n;//初始化size,表示堆里有n 个元素
        
        for(int i = n/2 ; i ; i --)down(i);//把堆初始化成小根堆，从二叉树的倒数第二行开始，把数字大的下沉
        //我认为从n/2开始，还有一个角度可以理解，
        //因为n是最大值，n/2是n的父节点，
        //因为n是最大，所以n/2是  最大的有子节点的父节点,
        //所以从n/2往前遍历，就可以把整个数组遍历一遍.
        
        while( m-- ){
            printf("%d ",h[1]);//删除最小值
            h[1] = h[siz];//把最下面的点覆盖第一个点。
            siz--;//堆里点的个数减一。
            down(1);//此时第一个点为堆里的最后一个点（第一个点被覆盖了），把这个点再重新排序（往下走）。
        }
        
        return 0;
    }

# 搜索与图论

### dfs

如何用 dfs 解决全排列问题？

dfs 最重要的是搜索顺序。用什么顺序遍历所有方案。

对于全排列问题，以 n = 3 为例，可以这样进行搜索：
![](https://cdn.acwing.com/media/article/image/2021/02/20/55289_0cd4222d73-%E6%B7%B1%E5%BA%A6%E4%BC%98%E5%85%88%E9%81%8D%E5%8E%86.png)

假设有 3 个空位，从前往后填数字，每次填一个位置，填的数字不能和前面一样。

最开始的时候，三个空位都是空的：\_\_ \_\_ \_\_

首先填写第一个空位，第一个空位可以填 1，填写后为：1 \_\_ \_\_

填好第一个空位，填第二个空位，第二个空位可以填 2，填写后为：1 2 \_\_

填好第二个空位，填第三个空位，第三个空位可以填 3，填写后为： 1 2 3

**这时候，空位填完，无法继续填数，所以这是一种方案，输出。**

然后往后退一步，退到了状态：1 2 \_\_ 。剩余第三个空位没有填数。第三个空位上除了填过的 3 ，没有其他数字可以填。

因此再往后退一步，退到了状态：1 \_\_ \_\_。第二个空位上除了填过的 2，还可以填 3。第二个空位上填写 3，填写后为：1 3 \_\_

填好第二个空位，填第三个空位，第三个空位可以填 2，填写后为： 1 3 2

**这时候，空位填完，无法继续填数，所以这是一种方案，输出。**

然后往后退一步，退到了状态：1 3 \_\_ 。剩余第三个空位没有填数。第三个空位上除了填过的 2，没有其他数字可以填。

因此再往后退一步，退到了状态：1 \_\_ \_\_。第二个空位上除了填过的 2，3，没有其他数字可以填。

因此再往后退一步，退到了状态：\_\_ \_\_ \_\_。第一个空位上除了填过的 1，还可以填 2。第一个空位上填写 2，填写后为：2 \_\_ \_\_

填好第一个空位，填第二个空位，第二个空位可以填 1，填写后为：2 1 \_\_

填好第二个空位，填第三个空位，第三个空位可以填 3，填写后为：2 1 3

**这时候，空位填完，无法继续填数，所以这是一种方案，输出。**

然后往后退一步，退到了状态：2 1 \_\_ 。剩余第三个空位没有填数。第三个空位上除了填过的 3，没有其他数字可以填。

因此再往后退一步，退到了状态：2 \_\_ \_\_。第二个空位上除了填过的 1，还可以填 3。第二个空位上填写 3，填写后为：2 3 \_\_

填好第二个空位，填第三个空位，第三个空位可以填 1，填写后为：2 3 1

**这时候，空位填完，无法继续填数，所以这是一种方案，输出。**

然后往后退一步，退到了状态：2 3 \_\_ 。剩余第三个空位没有填数。第三个空位上除了填过的 1，没有其他数字可以填。

因此再往后退一步，退到了状态：2 \_\_ \_\_。第二个空位上除了填过的 1，3，没有其他数字可以填。

因此再往后退一步，退到了状态：\_\_ \_\_ \_\_。第一个空位上除了填过的 1，2，还可以填 3。第一个空位上填写 3，填写后为：3 \_\_ \_\_

填好第一个空位，填第二个空位，第二个空位可以填 1，填写后为：3 1 \_\_

填好第二个空位，填第三个空位，第三个空位可以填 2，填写后为：3 1 2

**这时候，空位填完，无法继续填数，所以这是一种方案，输出。**

然后往后退一步，退到了状态：3 1 \_\_ 。剩余第三个空位没有填数。第三个空位上除了填过的 2，没有其他数字可以填。

因此再往后退一步，退到了状态：3 \_\_ \_\_。第二个空位上除了填过的 1，还可以填 2。第二个空位上填写 2，填写后为：3 2 \_\_

填好第二个空位，填第三个空位，第三个空位可以填 1，填写后为：3 2 1

**这时候，空位填完，无法继续填数，所以这是一种方案，输出。**

然后往后退一步，退到了状态：3 2 \_\_ 。剩余第三个空位没有填数。第三个空位上除了填过的 1，2，没有其他数字可以填。

因此再往后退一步，退到了状态：3 \_\_ \_\_。第二个空位上除了填过的 1，2，没有其他数字可以填。

因此再往后退一步，退到了状态：\_\_ \_\_ \_\_。第一个空位上除了填过的 1，2，3，没有其他数字可以填。

**此时深度优先搜索结束，输出了所有的方案。**

**算法：**

*   用 path 数组保存排列，当排列的长度为 n 时，是一种方案，输出。
*   用 state 数组表示数字是否用过。当 state\[i] 为 1 时：i 已经被用过，state\[i] 为 0 时，i 没有被用过。
*   dfs(i) 表示的含义是：在 path\[i] 处填写数字，然后递归的在下一个位置填写数字。
*   回溯：第 i 个位置填写某个数字的所有情况都遍历后， 第 i 个位置填写下一个数字。
    **代码**



    #include<iostream>
    using namespace std;

    const int N = 10;
    int path[N];//保存序列
    int stats[N];//数字是否被用过
    int n;

    void dfs(int u){//深度优先搜索 u为当前用到的位置
        if(u > n){  //n个位置填完了
            for(int i = 1;i<=n;i++){
                cout<<path[i]<<" "; //输出这个序列
            }
            cout<<endl;
        }
        
        for(int i = 1;i<=n;i++){    //每一个空位上可以选择填写的数字大小为：1~n
            if(!stats[i]){          //数字i没有被用过
                path[u] = i;        //把数字i放到u这个空位上
                stats[i] = 1;       //数字i现在被用了
                dfs(u+1);           //递归填下一个位置
                stats[i] = 0;       //回溯（恢复现场）
            }
        }
    }
    int main(){
        cin>>n;
        
        dfs(1);
        
        return 0;
    }

![de6def188d77d9b7e0b2589138253ea.jpg](https://note.youdao.com/yws/res/284/WEBRESOURCE27ba743751990d159d37aeb63f7c38da)
