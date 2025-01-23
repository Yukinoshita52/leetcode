# 前言

基于《代码随想录》的刷题顺序，[项目地址](https://github.com/vltown/leetcode)

[图解](./docs/图解.xlsx)

# 数组

## [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

### 模拟思路1

解题思路在于正确模拟这个过程：

自己一开始的模拟过程（以4*4为例）

<img src="./imgs/image-20250122180207689.png" alt="image-20250122180207689" style="zoom:67%;" />

对应的代码如下：

```java
import java.util.Arrays;
import java.util.Scanner;

/**
 * ClassName: solve1
 * Package: PACKAGE_NAME
 * Description:
 *
 * @Author YukinoshitaYukino
 * @Create 2025/1/22 18:03
 * @Version 1.0
 */
public class solve1 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int[][] arrs = generateMatrix(n);
        for (int i = 0; i < arrs.length; i++) {
            System.out.println(Arrays.toString(arrs[i]));
        }
    }

    public static int[][] generateMatrix(int n) {
        int[] dx = {0, 1, 0, -1};
        int[] dy = {1, 0, -1, 0};
        int[][] arr = new int[n][n];

        int num = 1;

        //假设是4*4的矩阵
        //先生成最上方的一条边
        for (int i = 0; i < n; i++) {
            arr[0][i] = num++;
        }
        int x = 0, y = n - 1;

        //还需生成3组：3 3 2 2 1 1
        //所以i还要遍历n-1次
        int k = 1;
        for (int i = 1; i < n; i++) {
            //对于每一“组”，有两个for语句（比如3 3……）
            for (int j = 0; j < n - i; j++) {
                x = (x + dx[k % 4]);
                y = (y + dy[k % 4]);
                arr[x][y] = num++;
            }
            k++;
            for (int j = 0; j < n - i; j++) {
                x = (x + dx[k % 4]);
                y = (y + dy[k % 4]);
                arr[x][y] = num++;
            }
            k++;
        }

        return arr;
    }
}

```

### 其他模拟思路

- 一圈一圈地进行，并且都是“左闭右开”式的，特别注意n为奇数时，要补全中间的位置。这个代码实现起来更简单。

  <img src="./imgs/image-20250122181138812.png" alt="image-20250122181138812" style="zoom: 80%;" />

```java
public static int[][] generateMatrix(int n) {
        int[][] arr = new int[n][n];

        int t = n / 2;
        int x = 0, y = 0;
        int num = 1;
        for (int i = 0; i < t; i++) {
            for (int j = 0; j < n - 2 * i - 1; j++) {
                arr[x][y++] = num++;
            }
            for (int j = 0; j < n - 2 * i - 1; j++) {
                arr[x++][y] = num++;
            }
            for (int j = 0; j < n - 2 * i - 1; j++) {
                arr[x][y--] = num++;
            }
            for (int j = 0; j < n - 2 * i - 1; j++) {
                arr[x--][y] = num++;
            }
            //走完“一圈”后，更新为下一圈的“起点”
            x++;
            y++;
        }
        if (n % 2 == 1) {
            arr[n / 2][n / 2] = num;
        }

        return arr;
    }
}
```

# 链表

## [203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

### 利用prev

题目一眼看上去很简单（实际上也确实），不过由于有段时间没接触算法，很多知识都忘完了——这题只要想起来几个关键词，就能做出：

**单链表**，外加一点“经验”，该结构遍历时有一定“局限性”，比如无法知道上一个节点信息。可以加入prev、next（有些题会用到）来进行补足，方便解题，本题只要加入了prev指针，就很简单能解出来了。

图解：

<img src="./imgs/image-20250123185842861.png" alt="image-20250123185842861" style="zoom:80%;" />

<img src="./imgs/image-20250123185902223.png" alt="image-20250123185902223" style="zoom:80%;" />

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
       while (head != null && head.val == val) {
            head = head.next;
        }

        ListNode prev = head;//涉及单链表的删除操作——记住加入prev节点！
        if(head == null) return null;//特殊：可能已经把所有节点删除完
        ListNode p = head.next;
        while (p != null) {
            if(p.val == val) {
                prev.next = p.next;//删除节点p
            }
            else{
                prev = p;//若不删除，更新prev
            }
            p=p.next;
        }
        return head;
    }
}
```

### 不利用prev

- 也是可以做的，稍微注意一下条件判断，避免出现`NullPointerException`报错

![image-20250123191033243](./imgs/image-20250123191033243.png)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
       while (head != null && head.val == val) {
            head = head.next;
        }

        ListNode cur = head;
        while (cur != null) {
            while(cur.next != null && cur.next.val == val) {
                cur.next = cur.next.next;
            }
            cur = cur.next;
           
        }
        return head;
    }
}
```

