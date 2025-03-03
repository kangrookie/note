# G. Penacony
## 题目理解
首先我们规定1到n，顺时针排列，i与i+1之间的边为i-1
我们思考对于一对朋友a，b，要有路径联通，又因为是一个环形，因此我们只有两种可能路径连接这两点，即顺时针方向a->b或者是b->a,我们利用哈希思想，设置一个随机数$x_i$赋予这对关系,$x_i$表示第i对关系从min(a,b)逆时针走到另一个点的路径，这样对于m对关系我们得到了m个$x_i$，我们设$f_i$表示第$i$边可以被删除时$x_i$序列异或的值。
这时我们先证明当边i可以被删除时对应的$x_i$序列唯一，对于一对关系，有两条路径当我们不选$x_i$时一定会选另一条，而当其中一条包含第i条边时，另外一条一定不包括，对于每一对都如此，因此我们对于一个边i确定时我们可以唯一确定x序列唯一。
之后我们去思考状态转移，我们从i小的边开始向前转移状态，当i-1的x序列已知，i有两种情况
1. 当边i-1在这个情况下可以被删除，但是i不能被删除时，点i+1一定与某一个点之间是朋友关系（也可能是多个点），在i-1中一定是没选择这对关系对应的x（也可能是多对关系），而$f_i$要将$f_{i-1}$中没选择的这个x给异或上
2. 比较简单就是i-1在这种情况可以被删除，且i也能被删除，这时点i+1一定不与任何点之间是朋友关系，这时$f_i$等于$f_{i-1}$ 
## 代码：
粘贴一下jiangly的代码
```
#include <bits/stdc++.h>
 
using u32 = unsigned;
using i64 = long long;
using u64 = unsigned long long;
 
std::mt19937_64 rng {std::chrono::steady_clock::now().time_since_epoch().count()};
 
void solve() {
    int n, m;
    std::cin >> n >> m;
    
    std::vector<u64> f(n);
    for (int i = 0; i < m; i++) {
        int a, b;
        std::cin >> a >> b;
        a--;
        b--;
        u64 x = rng();
        f[a] ^= x;
        f[b] ^= x;
    }
    for (int i = 1; i < n; i++) {
        f[i] ^= f[i - 1];
    }
    std::map<u64, int> cnt;
    int ans = 0;
    for (int i = 0; i < n; i++) {
        ans = std::max(ans, ++cnt[f[i]]);
    }
    std::cout << n - ans << "\n";
}
 
int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
 
    int t;
    std::cin >> t;
    
    while (t--) {
        solve();
    }
    
    return 0;
}
```