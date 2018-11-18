
## 第一场网测

### 题目列表

[Problem A](https://github.com/Beyyes/IndeedTokyoWebTest2018/blob/master/A.png)

[Problem B](https://github.com/Beyyes/IndeedTokyoWebTest2018/blob/master/B.png)

[problem C](https://github.com/Beyyes/IndeedTokyoWebTest2018/blob/master/C.png)

[problem D](https://github.com/Beyyes/IndeedTokyoWebTest2018/blob/master/D.png)


### Solution

A、B不提

C. 优先队列Dijsta算法，加入多个起点即可

D. dp。dp i j表示t扫描的i位置, 此时t的后缀与s的前缀有j位是一样的, dp i j能转移到dp i+1 j+1 和 dp i+1 0

附一个代码
```java
public class D_New {

    static long[][] dp = new long[50 + 1][27];
    public static void main(String[] args) throws FileNotFoundException {

        FileInputStream fis = new FileInputStream("input.txt");
        System.setIn(fis);

        int N;

        Scanner cin = new Scanner(System.in);

        while (cin.hasNext()) {
            dp = new long[50 + 1][27];
            N = cin.nextInt();
            String str = cin.next();

            long mod = 1000000000 + 7;
            dp[1][1] = 1;
            dp[1][0] = 25;
            for (int i = 1; i <= N-1; i++) {
                for (int j = 0; j < str.length(); j++) {
                    dp[i+1][j+1] = (dp[i+1][j+1] + dp[i][j]) % mod;

                    if (str.charAt(0) != str.charAt(j))
                        dp[i+1][1] = (dp[i+1][1] + dp[i][j]) % mod;

                    if (j == 0)
                        dp[i+1][0] = (dp[i][0] * 25) % mod;
                    else {
                        if (str.charAt(0) != str.charAt(j))
                            dp[i + 1][0] = (dp[i+1][0] + dp[i][j]*24) % mod;
                        else
                            dp[i + 1][0] = (dp[i+1][0] + dp[i][j]*25) % mod;
                    }
                }
            }

            long ans = 0;
            for (int i = 0; i < str.length(); i++) {
                ans = (ans + dp[N][i]) % mod;
            }

            System.out.println(ans);
        }
    }
}
```

## 第二场网测

[Problem A] 水题

[Problem B](https://github.com/Beyyes/IndeedTokyoWebTest2018/blob/master/B.pdf)

[problem C](https://github.com/Beyyes/IndeedTokyoWebTest2018/blob/master/C.pdf)

[problem D](https://github.com/Beyyes/IndeedTokyoWebTest2018/blob/master/D.pdf)

### Solution

A、B、C不提

D. 线段树维护区间最大最小值

附一个代码

```java
public class D {

    static int N = 200005;
    static int n, m;

    static long a[] = new long[N];
    static long sum[] = new long[N];

    static class node {
        int l, r;
        long min, max;
    }

    static int L(int x) {
        return (x << 1);
    }

    static int R(int x) {
        return ((x << 1) | 1);
    }

    static node[] f = new node[N];

    static void build(int l, int r, int i) {
        f[i].l = l;
        f[i].r = r;
        if (l == r) {
            f[i].min = f[i].max = sum[l];
            return;
        }
        int mid = (l + r) / 2;
        build(l, mid, L(i));
        build(mid + 1, r, R(i));
        f[i].min = Math.min(f[L(i)].min, f[R(i)].min);
        f[i].max = Math.max(f[L(i)].max, f[R(i)].max);
    }

    static long qmin(int l, int r, int i) {
        if (l == f[i].l && r == f[i].r) {
            return f[i].min;
        }
        int mid = (f[i].l + f[i].r) / 2;
        if (r <= mid) return qmin(l, r, L(i));
        else if (l > mid) return qmin(l, r, R(i));
        else return Math.min(qmin(l, mid, L(i)), qmin(mid + 1, r, R(i)));
    }

    static long qmax(int l, int r, int i) {
        if (l == f[i].l && r == f[i].r) {
            return f[i].max;
        }
        int mid = (f[i].l + f[i].r) / 2;
        if (r <= mid) return qmax(l, r, L(i));
        else if (l > mid) return qmax(l, r, R(i));
        else return Math.max(qmax(l, mid, L(i)), qmax(mid + 1, r, R(i)));
    }


    public static void main(String[] args) throws FileNotFoundException {
        Scanner cin = new Scanner(System.in);
        n = cin.nextInt();
        m = cin.nextInt();

        for (int i = 0; i < N; i++) {
            f[i] = new node();
        }

        for (int i = 1; i <= n; ++i) {
            a[i] = cin.nextInt();
            sum[i] = sum[i - 1] + a[i];
        }

        build(0, n, 1);
        while (m-- > 0) {
            int l, r;
            l = cin.nextInt();
            r = cin.nextInt();
            long ans = qmax(l - 1, r, 1) - qmin(l - 1, r, 1);
            if (ans < 0) {
                ans = 0;
            }
            System.out.println(ans);
        }
    }
}
```
