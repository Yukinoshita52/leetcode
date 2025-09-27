# ACM模式刷题

## [卡码网：57. 爬楼梯](https://kamacoder.com/problempage.php?pid=1067)

这居然变成了一个完全背包问题……不可思议……（m如果为2就是普通的爬楼梯）

```java
import java.util.*;

class Main{

    public int solve(int n,int m){
        int[] dp = new int[n+1];
        dp[0] = 1;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m && j<=i;j++){
                dp[i] += dp[i-j];
            }
        }
        return dp[n];
    }

    public static void main(String[] args){
        Scanner sc  = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();

        int res = new Main().solve(n,m);

        System.out.println(res);

        sc.close();
    }
}


```