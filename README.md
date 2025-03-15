# 前言

基于《代码随想录》的刷题顺序，[项目地址](https://github.com/vltown/leetcode)

[图解](./docs/图解.xlsx)

[图解样例](./docs/图解样例.xlsx)

**To-Do-List**

- [x] 做了常用的**图解样例**模板
- [ ] 做栈、队列、链表等`java.util.*`下的实现、调用方法的整理

# 数组

## [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

### 模拟思路1

解题思路在于正确模拟这个过程：

自己一开始的模拟过程（以4*4为例）

<img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637928.png" alt="image-20250122180207689" style="zoom:67%;" />

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

  <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637929.png" alt="image-20250122181138812" style="zoom: 80%;" />

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

<img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637931.png" alt="image-20250123185842861" style="zoom:80%;" />

<img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637932.png" alt="image-20250123185902223" style="zoom:80%;" />

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

![image-20250123191033243](https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637933.png)

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

## [707. 设计链表](https://leetcode.cn/problems/design-linked-list/)

考虑情况：

1. MyLinkedList的head为空
2. index小于0、index大于等于len、index在正常的值[0,len-1]

编写时注意：

1. 时刻注意“下标越界”问题
2. 时刻注意加减节点时附带len字段的变化

这道有些莫名其妙……照自己理解写能写出来，过不了样例

- [ ] 以下是官方题解，我找时间看一下java的ListNode源码是怎么写的吧

```java
/**
 * ClassName: solve3
 * Package: PACKAGE_NAME
 * Description:
 *
 * @Author YukinoshitaYukino
 * @Create 2025/1/23 19:21
 * @Version 1.0
 */
public class solve3 {
}

class MyLinkedList {
    int size;
    Node head;

    public MyLinkedList() {
        this.size = 0;
        this.head = new Node(0);//头节点是虚拟的节点
    }

    public int get(int index) {
        if (index < 0 || index >= this.size) return -1;
        Node cur = head;
        //有必要解释一下这里为什么要进行index次循环：
        //这和链表的设计有关——因为这里的单链表是从head开始的
        //而head是设定为空的头节点，不计入长度

        //??不是……为什么是小于等于index？？
        //合理解释就是……这个index的范围就是在 [0,size-1]
        //艹（来自对官方题解吐槽）
        for (int i = 0; i <= index; i++) {
            cur = cur.next;
        }
        return cur.val;

    }

    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    public void addAtTail(int val) {
        addAtIndex(this.size, val);
    }

    public void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        index = Math.max(0, index);
        size++;
        Node pred = head;
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        Node toAdd = new Node(val);
        toAdd.next = pred.next;
        pred.next = toAdd;
    }

    public void deleteAtIndex(int index) {
        if(index < 0 || index >= this.size) return;
        size--;

        Node cur = head;
        for(int i=0;i<index;i++){
            cur = cur.next;
        }
        cur.next = cur.next.next;

    }
}

class Node {
    int val;
    Node next;

    public Node(int val) {
        this.val = val;
    }
}

```

## [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

没什么好说的，很直观的解法：

<img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637934.png" alt="image-20250124124653702" style="zoom:67%;" />

<img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637935.png" alt="image-20250124124836419" style="zoom:67%;" />

- 一个节点反转后的状态如下，后面以此类推：

<img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637936.png" alt="image-20250124124925902" style="zoom:67%;" />

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
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode cur = head;
        
        ListNode tmp;
        while(cur != null){
            tmp = cur.next;
            cur.next = prev;
            prev = cur;
            cur = tmp;
        }
        return prev;
    }
}
```

- 递归写法：

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 * int val;
 * ListNode next;
 * ListNode() {}
 * ListNode(int val) { this.val = val; }
 * ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        return reverse(null,head);
    }

    public ListNode reverse(ListNode prev, ListNode cur) {
        if (cur == null) return prev;
        ListNode tmp = cur.next;
        cur.next = prev;
        return reverse(cur,tmp);
    }
}
```

## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

- 核心思路：**滑动窗口**

![image-20250124131928118](https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637937.png)

![image-20250124132002492](https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637939.png)

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        //题目意思：head不为null（1<=sz<=30）
        int size = 0;
        ListNode cur = head;
        ListNode prev = null;
        while(cur != null){
            size++;
            if(size == n + 1){
                prev = head;
                cur = cur.next;
            }
            else if(size > n + 1){
                prev = prev.next;
                cur = cur.next;
            }
            else{
                cur = cur.next;
            }
        }
        if(prev == null){//说明删除的是头节点
            return head.next;
        }else{
            prev.next = prev.next.next;
            return head;
        }
        
    }
}
```

## [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

核心考点：

1. 判断单链表有无环
2. 若有环，找出环的入口节点

### 思路一：利用HashMap记录已访问过的节点

该思路下，每访问一个就记录，然后判断“cur.next”是否已访问过，即可得到是否有环+找出入口节点

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        HashMap<ListNode,Integer> visited = new HashMap<>();
        ListNode cur = head;
        while(cur != null){
            if(visited.containsKey(cur.next)){
                return cur.next;
            }
            visited.put(cur,0);
            cur = cur.next;
        }
        return null;
    }
}
```

- 缺点：时间复杂度较高（每过一个节点就要在HashMap中匹配）

### 思路二：快慢指针（Floyd判圈算法）

> 本题的方法还有个更高大上的名字，[Floyd判圈算法](https://zh.wikipedia.org/wiki/Floyd%E5%88%A4%E5%9C%88%E7%AE%97%E6%B3%95#%E5%BA%94%E7%94%A8)，又称龟兔赛跑算法，是一个可以在[有限状态机](https://zh.wikipedia.org/wiki/有限状态机)、[迭代函数](https://zh.wikipedia.org/wiki/迭代函数)或者[链表](https://zh.wikipedia.org/wiki/链表)上判断是否存在[环](https://zh.wikipedia.org/wiki/環_(圖論))，求出该环的起点与长度的算法。

- 形象上来看分为两个过程：

  <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637940.png" alt="image-20250125204733083" style="zoom:67%;" />

  1. 快指针每次走两步，慢指针每次走一步，直到他们相遇为止（说明有环）

     <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637941.png" alt="image-20250125204745962" style="zoom:67%;" />

  2. 再定义两个指针分别从head和（fast和slow的）相遇点同时出发，直到相遇。而且相遇点必定是环的入口：

     <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637942.png" alt="image-20250125204910130" style="zoom:67%;" />

     <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637943.png" alt="image-20250125204920545" style="zoom:67%;" />

  问题来了——两个关键问题中的“如何判断有环”很好解决（fast和slow是否相遇），但是**为什么index1、index2再相遇的点一定是环的入口呢？**

  严格推导如下：

**快慢指针的基本设定**

    1. 设链表由三部分组成：
     - **起点到环入口的距离**：记为 A。
     - **环入口到快慢指针相遇点的距离**：记为 B。
     - **相遇点到环入口的剩余距离**：记为 C，环的总长度为 L，则 $L=B+C$
    2. 两个指针的移动速度：
     - **慢指针（slow）**：一次走 1 步。
     - **快指针（fast）**：一次走 2 步。
    3. 两指针初始位置为链表头节点，且都朝着链表的下一个节点移动。

**相遇点的性质**

  1. **快慢指针的相遇条件**：
     
     - 假设慢指针走了 x 步，则快指针走了 2x 步。
     
     - 快指针比慢指针多走的步数一定是环的整数倍，满足： 
     
     - $$
       2x - x = n \cdot L \quad (n \geq 1)
       $$
     
     - 即：
     - $$
       x = n \cdot L \quad (n \geq 1)	\qquad (1)
       $$
     
  2. **相遇点的推导**：
     
     - 慢指针走了 x 步，此时 x 包含起点到环入口的距离 A，以及在环内走的距离 B，故$x = A + B \qquad (2)$
     - 因此，由（1）式和（2）式可得：$A + B = n \cdot L \qquad (3)$
     - （3）式可理解为：起点到环入口的距离 A，以及在环内走的距离 B之和，相当于n倍环的长度

**环入口与相遇点的关系**

  要证明为什么从头节点到环入口的距离 A等于从相遇点沿着环走到环入口的距离 C，可以整理公式：

  1. 从（3）式，整理出：

     $A = n \cdot L - B \qquad (4)$

     因为环的长度是 L，在环内 $B + C = L$，所以$C = L - B \qquad (5)$

     假设n是1呢？也就是快指针fast多跑了1圈，那么（4）式和（5）式联立可解得$A = C$

     那么把情况考虑完全，快指针可能不止跑了一圈，那也没关系——想象一下，另设两个指针index1和index2，分别从head和相遇点出发，那么index1起码为了进入圈，要跑A的距离，也就是$n \cdot L - B$，但是index2始终在环内“绕圈圈”。对吧？所以就相当于是$A=L-B$（相当于index2在原地不动，index1在进入环前在跑掉这一圈圈的距离），最后得出的结论不变。

  2. 结论：

     - 从头节点到环入口的距离 A，等于从相遇点继续沿着环走到环入口的距离 C。（稍不严谨，但是上面已经解释清楚了原因）

> 实在不懂的话画画图理解一下还是很好理解的：
>
> <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637944.jpg" alt="89ab6f91c4ee012eeb6c68ede5102c30_720" style="zoom: 50%;" />

**算法的逻辑**

  利用上述结论，算法通过以下步骤准确找到环的入口：

  1. 两指针（快慢指针）第一次相遇后，将其中一个指针重置到链表头部，另一个指针留在相遇点。
  2. 两指针以相同速度前进（一次走一步）。
  3. 因为一个指针从头节点出发，另一个指针从相遇点出发，两者相遇时会刚好在环入口处。

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast!= null && fast.next!= null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                ListNode index1 = head;
                ListNode index2 = fast;
                while (index1 != index2) {
                    index1 = index1.next;
                    index2 = index2.next;
                }
                return index1;
            }
        }
        return null;
    }
}
```

# 哈希表

## [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

### 思路1：利用java容器的HashMap

以Character为key，Integer为value。

记录前一个字符串中每一个字符对应出现次数，再遍历第二个字符串，每遍历一个字符，便在HashMap中寻找，若返回null直接false，或者找到后，对应次数-1，若小于0，也返回false。否则返回true

```java
public boolean isAnagram(String s, String t) {
        int size = s.length();
        if(size != t.length()) return false;
        HashMap<Character,Integer> count = new HashMap<>();
        for(char c :s.toCharArray()){
            if(count.containsKey(c)){
                count.put(c,count.get(c)+1);
            }
            else {
                count.put(c, 1);
            }
        }

        for(char c :t.toCharArray()){
            if(count.get(c) == null) return false;
            count.put(c,count.get(c)-1);
        }
        return true;
    }
```

> 思路就是这样的思路，但是时间性能还能优化

### (best)思路2：自建“哈希表”

哈希表的本质是通过Hash函数，将key映射到数组中对应的存储位置上，所以不妨自己创建一个映射规则：

由于本题目中，字符范围仅仅是小写字母，可以用26个大小的数组来存储，也就是在下标为`'字符'-'a'`的位置存储`'字符'`出现的个数

例如：`arr[c - 'a']++`（c为char类型）

**最优写法**（时间耗费最少）

```java
public boolean isAnagram(String s, String t) {
        int size = s.length();
        if(size != t.length()) return false;
        int[] arr = new int[26];
        for(char c : s.toCharArray()) arr[c - 'a']++;
        for(char c : t.toCharArray()) arr[c - 'a']--;
        for(int i=0;i<arr.length;i++){
            if(arr[i] != 0) return false;
        }
        return true;
    }
```

另一种写法：

```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;

    // 26 个字母的频次数组
    int[] count = new int[26];

    // 遍历字符串 s 和 t，更新频次
    for (int i = 0; i < s.length(); i++) {
        count[s.charAt(i) - 'a']++;
        count[t.charAt(i) - 'a']--;
    }

    // 检查所有频次是否为 0
    for (int c : count) {
        if (c != 0) return false;
    }

    return true;
}

```



分析：时间复杂度为`O(n)`，空间复杂度为`O(n)`。

### 思路3：

本题还可以先对两个字符串排序后再比较也行

```java
public boolean isAnagram(String s, String t) {
        // 如果字符串长度不同，直接返回 false
        if (s.length() != t.length()) return false;

        // 将字符串转换为字符数组
        char[] charArray = s.toCharArray();
        char[] charArray1 = t.toCharArray();

        // 排序两个字符数组
        Arrays.sort(charArray);
        Arrays.sort(charArray1);

        // 比较排序后的字符数组是否相同
        return Arrays.equals(charArray, charArray1);
}
```

## [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

### 利用HashSet可解

唯一难点是java中容器的使用

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        // 使用 HashSet 存储 nums1 中的唯一元素
        HashSet<Integer> set1 = new HashSet<>();
        for (int num : nums1) {
            set1.add(num);
        }

        // 用另一个 HashSet 存储交集结果
        HashSet<Integer> intersectionSet = new HashSet<>();
        for (int num : nums2) {
            if (set1.contains(num)) {
                intersectionSet.add(num);
            }
        }

        // 将交集结果转换为数组
        int[] resultArray = new int[intersectionSet.size()];
        int index = 0;
        for (int num : intersectionSet) {
            resultArray[index++] = num;
        }

        //  // 或使用迭代器将交集结果转换为数组
        // int[] resultArray = new int[intersectionSet.size()];
        // Iterator<Integer> iterator = intersectionSet.iterator();
        // int index = 0;
        // while (iterator.hasNext()) {
        //     resultArray[index++] = iterator.next();
        // }

        return resultArray;
    }
}
```

## [1. 两数之和](https://leetcode.cn/problems/two-sum/)

- 题目要求找到两个数，相加为target，返回这两个数在nums中的下标。
- 利用HashMap可解，其中map存储的key：value为  值：下标。故每次往map集合中新加入数时，先判断是否已经有以target-nums[i]为key的元素，若有，返回答案即可。

```java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[2];

        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(set.get(target-nums[i]) != null){
                res[0] = set.get(target-nums[i]);
                res[1] = i;
                return res;
            }
            set.put(nums[i],i);
        }
        return null;
    }
}
```

## [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)

显然，拿到题目，最最直观的想法——暴力法如下：

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int res = 0;
        
        //暴力法：
        for(int i=0;i<nums1.length;i++){
            int sum = nums1[i];
            for(int j=0;j<nums2.length;j++){
                sum+=nums2[j];
                for(int k=0;k<nums3.length;k++){
                    sum+=nums3[k];
                    for(int l=0;l<nums4.length;l++){
                        sum+=nums4[l];
                        if(sum == 0){
                            res++;
                        }
                        sum -= nums4[l];
                    }
                    sum -= nums3[k];
                }
                sum -=nums2[j];
            }
        }
        return res;
    }
}
```

- 显然这也是行不通的，时间超时了

稍作改进，用四个map进行存储（key：value对应 **值：出现次数**），虽然查找效率变高了，但是还是四重循环，最终还是超时

```java
public class Solution {

    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int res = 0;

        HashMap<Integer, Integer> map1 = new HashMap<>();
        HashMap<Integer, Integer> map2 = new HashMap<>();
        HashMap<Integer, Integer> map3 = new HashMap<>();
        HashMap<Integer, Integer> map4 = new HashMap<>();
        for (int i = 0; i < nums1.length; i++) {
            if (map1.get(nums1[i]) == null) {
                map1.put(nums1[i], 1);
            } else {
                map1.put(nums1[i], map1.get(nums1[i]) + 1);
            }

            if (map2.get(nums2[i]) == null) {
                map2.put(nums2[i], 1);
            } else {
                map2.put(nums2[i], map2.get(nums2[i]) + 1);
            }

            if (map3.get(nums3[i]) == null) {
                map3.put(nums3[i], 1);
            } else {
                map3.put(nums3[i], map3.get(nums3[i]) + 1);
            }

            if (map4.get(nums4[i]) == null) {
                map4.put(nums4[i], 1);
            } else {
                map4.put(nums4[i], map4.get(nums4[i]) + 1);
            }
        }

        int sum;
        Set<Map.Entry<Integer, Integer>> entries1 = map1.entrySet();
        Set<Map.Entry<Integer, Integer>> entries2 = map2.entrySet();
        Set<Map.Entry<Integer, Integer>> entries3 = map3.entrySet();
        Set<Map.Entry<Integer, Integer>> entries4 = map4.entrySet();

        for (Map.Entry<Integer, Integer> entry1 : entries1) {
            sum = entry1.getKey();
            for (Map.Entry<Integer, Integer> entry2 : entries2) {
                sum += entry2.getKey();
                for (Map.Entry<Integer, Integer> entry3 : entries3) {
                    sum += entry3.getKey();
                    for (Map.Entry<Integer, Integer> entry4 : entries4) {
                        sum += entry4.getKey();
                        if (sum == 0) {
                            res += entry1.getValue() * entry2.getValue() * entry3.getValue() * entry4.getValue();
                        }
                        sum -= entry4.getKey();
                    }
                    sum -= entry3.getKey();
                }
                sum -= entry2.getKey();
            }
        }

        return res;
    }
}

```

- 此处需要一点小巧思，那就是map的key和值的设计。考虑一下“两数之和”的写法，只使用了一个map解决，那个map存储的是key（nums1的数值）：value（nums1[i]的下标i）
- 此处可修改为key（**num1[i]+nums2[j]**）：value（**出现次数**）
- 这样一来，我们再对nums3和num4进行一个双重循环：对于每一个nums3[i]和nums4[j]，如果存在有map(-(nums3[i] + nums4[j]) )，说明原本的四个数组相加可以为0，则此时进行res+=map.getValue操作。
- 最终返回res即可

`清晰的代码思路`

```java
public class Solution {

    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int res = 0;

        HashMap<Integer, Integer> map = new HashMap<>();

        for (int num1 : nums1) {
            for(int num2 : nums2){
                if(map.get(num1+num2) == null) map.put(num1+num2,1);
                else map.put(num1+num2,map.get(num1+num2)+1);
            }
        }

        for(int num3 : nums3){
            for(int num4:nums4){
                if(map.get(-(num3+num4)) != null) res+=map.get(-(num3+num4));
            }
        }
        
        return res;
    }
}

```

`优化的代码写法`

- **`map.merge()`**：用来简化 `if` 逻辑，它会尝试将 键`num1 + num2`对应的值与1相加，若不存在该键则插入该键值对（num1+num2:1）。
- **`map.getOrDefault()`**：在第二部分遍历时，使用 `map.getOrDefault()` 避免了 `null` 的检查，若没有找到匹配的值，直接返回 0。

```java
public class Solution {

    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int res = 0;

        // 使用合适的初始容量，减少哈希表扩容的次数
        HashMap<Integer, Integer> map = new HashMap<>(nums1.length * nums2.length);

        // 将 nums1 和 nums2 的所有和存入 HashMap
        for (int num1 : nums1) {
            for (int num2 : nums2) {
                // 使用 merge 方法避免 if 判断，提高可读性
                map.merge(num1 + num2, 1, Integer::sum);
            }
        }

        // 查找 nums3 和 nums4 的和的负数是否存在于 map 中
        for (int num3 : nums3) {
            for (int num4 : nums4) {
                res += map.getOrDefault(-(num3 + num4), 0);
            }
        }

        return res;
    }
}
```

> `map.merge(num1 + num2, 1, Integer::sum)` 这一行代码的作用是将 `num1 + num2` 作为键，`1` 作为值，尝试将该键值对插入到 `map` 中，或者在该键已存在的情况下，更新它的值。具体来说，`merge` 方法的三个参数含义如下：
>
> 1. **key** (`num1 + num2`)：我们希望在 `map` 中使用的键，这里是 `num1 + num2` 的结果。
> 2. **value** (`1`)：要插入的值，若该键还未存在于 `map` 中，直接插入 `1`。
> 3. **remappingFunction** (`Integer::sum`)：当 `map` 中已经存在该键时，会使用这个函数来计算新的值。`Integer::sum` 是一个方法引用，表示两个整数的求和操作，也就是将已有的值和新的 `1` 相加。
>
> **解释流程：**
>
> - **如果 `num1 + num2` 不在 `map` 中**，那么 `merge` 方法会直接将这个键 `num1 + num2` 和对应的值 `1` 插入到 `map` 中。
> - **如果 `num1 + num2` 已经在 `map` 中**，那么 `merge` 方法会调用 `Integer::sum` 函数，将现有值与新的 `1` 相加，更新 `map` 中对应键的值。例如，如果该键的当前值是 `3`，那么更新后会变为 `3 + 1 = 4`。
>
> **例子：**
>
> 假设我们有 `nums1 = [1, 2]` 和 `nums2 = [3, 4]`，那么：
>
> - 第一次遍历时，`num1 = 1` 和 `num2 = 3`，`num1 + num2 = 4`，在 `map` 中没有 4，因此会插入 `(4, 1)`。
> - 下一次，`num1 = 1` 和 `num2 = 4`，`num1 + num2 = 5`，在 `map` 中没有 5，因此会插入 `(5, 1)`。
> - 接着，`num1 = 2` 和 `num2 = 3`，`num1 + num2 = 5`，此时 `map` 中已经有键 5，因此会将原来的值 `1` 与新的 `1` 相加，变为 `(5, 2)`。
> - 最后，`num1 = 2` 和 `num2 = 4`，`num1 + num2 = 6`，在 `map` 中没有 6，因此会插入 `(6, 1)`。

## [15. 三数之和](https://leetcode.cn/problems/3sum/)

### 哈希解法

- 题目要求是三数之和为0，并且有**不得重复**这一条件。

- 如果暂时不考虑**去重**，可以考虑用双重循环的i、j，`nums[i]`是第一个数，`nums[j]`是第二个数，那么就需要判断在nums中是否有第三个数`-(nums[i]+nums[j])`。可以按如下步骤：

  1. 可以使用HashSet来记录。对于nums[i]固定时，j向前遍历。
  2. 若`set.contains(-(nums[i]+nums[j]))`，则将`nums[i]、nums[j]、-(nums[i]+nums[j])`作为一个结果保存到`List<List<Integer>> res`中

- 但是要考虑去重的问题，比如一个数组`nums = {-3,3,0,-3,3,0}`很明显的，按照上面方式，`{-3,3,0}`会被记录两次，所以先对nums数组进行**排序**操作，并在遍历i、j变量时加入去重判断：

  1. `if(i>0 && nums[i] == nums[i-1]) continue;`该语句保证了最小的数`nums[i]`不重复。注意：不能是`if(nums[i]==nums[i+1])`，这样会使情况减少（反例:`nums={-1,-1,2}`，此时i直接跳过第一个-1到了第二个-1，j又是从i+1开始的，所以就少了）
  2. `if(j>i+2 && nums[j]== nums[j-1] && nums[j-1]==nums[j-2]) continue;`。对于第二个数`nums[j]`的去重，只有连续三个相同时才要去重。（见代码思路）

- 图解：

  <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637945.png" alt="image-20250201143255895" style="zoom:67%;" />

  <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637946.png" alt="image-20250201143320855" style="zoom:67%;" />

```java
package yu5;

import java.util.*;

/**
 * ClassName: Solution
 * Package: yu5
 * Description:
 *
 * @Author YukinoshitaYukino
 * @Create 2025/2/1 12:41
 * @Version 1.0
 */
public class Solution {
    public static void main(String[] args) {
        int[] nums = {-2,0,1,1,2};
        List<List<Integer>> lists = threeSum(nums);
        for(var list : lists){
            System.out.println(list);
        }
    }

    public static List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);

        for(int i=0;i<nums.length;i++){
            if(nums[i] > 0) break;//如果最小的nums[i]都大于0了，说明没有三元组了
            if(i >0 && nums[i] == nums[i-1]) continue;//去重

            HashSet<Integer> set = new HashSet<>();
            for(int j=i+1;j<nums.length;j++){
                //此处的去重条件较特殊（反例：-4 …… 2 2 ）
                if(j>i+2&& nums[j] == nums[j-1] && nums[j-1] ==nums[j-2]) continue;
                if(set.contains(-(nums[i]+nums[j]))){
                    List<Integer> list = new ArrayList<>();
                    list.add(nums[i]);
                    list.add(nums[j]);
                    list.add(-(nums[i]+nums[j]));
                    res.add(list);
                    set.remove(-(nums[i]+nums[j]));
                }else{
                    set.add(nums[j]);
                }
            }
        }
        return res;
    }
}

```

### 指针解法

- 这种思路更简单、直观，代码实现容易，且效率更高，适合本题

- 看一个图就能理解：

  ​	<img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637947.png" alt="image-20250201145737223" style="zoom:80%;" />

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2; i++) {
            if (nums[i] > 0)
                break;// 如果最小的nums[i]都大于0了，说明没有三元组了
            if (i > 0 && nums[i] == nums[i - 1])
                continue;// 去重
            int left = i + 1, right = nums.length - 1;
            while (left < right) {
                if (nums[i] + nums[left] + nums[right] < 0) {
                    left++;
                } else if (nums[i] + nums[left] + nums[right] > 0) {
                    right--;
                } else {
                    res.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    //找到一个解后，为了找下一个，同时保证解不重复，也就是nums[left]要不一样
                    //既然nums[left]都不一样了，right必定也要换个位置（才能满足三元组）
                    while (left < right && nums[left] == nums[left + 1])
                        left++;
                    while (left < right && nums[right] == nums[right - 1])
                        right--;
                    left++;
                    right--;
                }
            }
        }
        return res;
    }
}
```

## [18. 四数之和](https://leetcode.cn/problems/4sum/)

### 哈希解法

- 核心思路：四数（`nums[i]、nums[j]、nums[k]、target-(nums[i]+nums[j]+nums[k])`）之和为`target`
- 先固定`nums[i]`和`nums[j]`，然后对于每一个k，判断set中是否contains(target-nums[i]-nums[j]-nums[k])
  - 若contains，则remove，set中已有的这个值，然后往res中添加这个四元组
  - 若不contains，则往set中add这个nums[k]
- （详见代码）

> 样例1：`nums =[1,-2,-5,-4,-3,3,3,5]`，`target=-11`
>
> 预期：`[[-5,-4,-3,1]]`
>
> 样例2：`nums=[-2,-1,-1,1,1,2,2]`，`target=0`
>
> 预期：`[[-2,-1,1,2],[-1,-1,1,1]]`
>
> 样例3：`nums=[1000000000,1000000000,1000000000,1000000000]`，`target=-294967296`
>
> 预期：`[]`
>
> 样例4：`nums=[-1000000000,-1000000000,1000000000,-1000000000,-1000000000]`，`target=294967296`
>
> 预期：`[]`

- 哈希解法显然时间复杂度较高，为`O(n^3)`

  <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637948.png" alt="image-20250203170351373" style="zoom:67%;" />

```java
package yu6;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.List;

/**
 * ClassName: Solution
 * Package: yu6
 * Description:
 *
 * @Author YukinoshitaYukino
 * @Create 2025/2/3 16:12
 * @Version 1.0
 */
public class Solution {
    public static void main(String[] args) {
        List res = fourSum(new int[]{-1000000000,-1000000000,1000000000,-1000000000,-1000000000}, 294967296);
        System.out.println(res);
    }

    public static List<List<Integer>> fourSum(int[] nums, int target) {
        // nums[i]、nums[j]、nums[k]、target-(nums[i]+nums[j]+nums[k])
        List<List<Integer>> res = new ArrayList<>();

        Arrays.sort(nums);// 排序后可方便解题
        for (int i = 0; i < nums.length; i++) {
            // 若第一个便大于target，说明不存在这种四元组
            // 注：只有nums[i]大于等于零，且nums[i]若大于0则说明可结束循环
            if (nums[i] >= 0 && nums[i] > target)
                break;
            // 去重，保证nums[i]不重复
            if (i > 0 && nums[i] == nums[i - 1])
                continue;


            for (int j = i + 1; j < nums.length; j++) {
                // 去重，保证nums[j]不重复
                if (j > i + 1 && nums[j] == nums[j - 1])
                    continue;
                HashSet<Integer> set = new HashSet<>();
                for (int k = j + 1; k < nums.length; k++) {
                    if (k > j + 2 && nums[k] == nums[k - 1] && nums[k - 1] == nums[k - 2])
                        continue;
                    long sumThree = (long) nums[i] + (long) nums[j] + (long) nums[k];
                    long required = (long) target - sumThree;
                    if (required < Integer.MIN_VALUE || required > Integer.MAX_VALUE) {
                        continue;
                    }
                    int requiredInt = (int) required;
                    if (set.contains(requiredInt)) {
                        res.add(Arrays.asList(nums[i], nums[j], requiredInt, nums[k]));
                        set.remove(requiredInt);
                    } else {
                        set.add(nums[k]);
                    }
                }
            }
        }
        return res;
    }
}

```

### 指针解法（better）

- 显然更推荐这种解法：

  <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637949.png" alt="image-20250203171636733" style="zoom:80%;" />

  <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/newImages120250304122637950.png" alt="image-20250203171448443" style="zoom:67%;" />

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        // nums[i]、nums[j]、nums[k]、target-(nums[i]+nums[j]+nums[k])
        List<List<Integer>> res = new ArrayList<>();

        Arrays.sort(nums);// 排序后可方便解题
        for (int i = 0; i < nums.length - 3; i++) {
            // 若第一个便大于target，说明不存在这种四元组
            // 注：只有nums[i]大于等于零，且nums[i]若大于0则说明可结束循环
            if (nums[i] >= 0 && nums[i] > target)
                break;
            // 去重，保证nums[i]不重复
            if (i > 0 && nums[i] == nums[i - 1])
                continue;


            for (int j = i + 1; j < nums.length - 2; j++) {
                // 去重，保证nums[j]不重复
                if (j > i + 1 && nums[j] == nums[j - 1])
                    continue;
                int left = j + 1, right = nums.length - 1;
                while (left < right) {
                    long sum = (long) nums[i] + nums[j] + nums[left] + nums[right];
                    if (sum > target) {
                        right--;
                    }else if(sum < target){
                        left++;
                    }else{
                        res.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        while(left<right && nums[left]==nums[left+1]){//去重
                            left++;
                        }
                        while(left < right && nums[right]==nums[right-1]){//去重
                            right--;
                        }
                        //真正对left和right同时做移动，以找下一个四元组：
                        left++;
                        right--;
                    }
                }
            }
        }
        return res;
    }
}
```

# 字符串

## [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

- 两个指针，一头一尾，向中逼近，依次交换，即可反转。

```java
class Solution {
    public void reverseString(char[] s) {
        int i=0,j=s.length-1;
        char temp;
        while(i<j){
            temp = s[i];
            s[i]=s[j];
            s[j]=temp;
            i++;
            j--;
        }
    }
}
```

## [541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/)

- 要求是对特定段的字符串进行反转，那就让指针“跳跃式”前进

```java
class Solution {
    public String reverseStr(String s, int k) {
        int i, j;
        char[] charArray = s.toCharArray();
        for (i = 0, j = k - 1; j < s.length(); i += 2 * k, j += 2 * k) {
            for (int left = i, right = j; left < right; left++, right--) {
                char tmp = charArray[left];
                charArray[left] = charArray[right];
                charArray[right] = tmp;
            }
        }
        if(i < s.length()){
            j = s.length()-1;
            while(i<j){
                char tmp = charArray[i];
                charArray[i] = charArray[j];
                charArray[j] = tmp;
                i++;
                j--;
            }
        }
        s = new String(charArray);
        return s;
    }
}
```

`简化：`

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] charArray = s.toCharArray();
        for (int i = 0; i < charArray.length; i += 2 * k) {
            int left = i;
            int right = Math.min(i + k - 1, charArray.length - 1);
            while (left < right) {
                char tmp = charArray[left];
                charArray[left] = charArray[right];
                charArray[right] = tmp;
                left++;
                right--;
            }
        }
        return new String(charArray);
    }
}
```

## [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

### 思路一

- 思路：先把原`String s`转换为`char[] charArray`
- 将这个charArray分解成一个个单词，以`String[] words`形式存储
- 最后将words反向拼接起来。

```java
class Solution {
    public String reverseWords(String s) {
        char[] charArray = s.toCharArray();

        String[] words = new String[charArray.length];
        int index = 0;

        int i = 0;
        StringBuilder word = new StringBuilder();
        while (i < charArray.length) {
            if (charArray[i] == ' ' && word.isEmpty()) {//跳过所有空格
                i++;
            } else if (charArray[i] == ' ' && !word.isEmpty()) {//说明已存在一个单词
                words[index++] = word.toString();
                i++;
                word = new StringBuilder();
            } else {
                word.append(charArray[i]);
                i++;
            }
        }

        if(!word.isEmpty()) {
            words[index++] = word.toString();
        }

        StringBuilder strBuilder = new StringBuilder();
        for (int j = index - 1; j > 0; j--) {
            strBuilder.append(words[j]);
            strBuilder.append(" ");
        }
        strBuilder.append(words[0]);

        String res = strBuilder.toString();
        return res;
    }
}
```

- 相同思路下的简洁写法（但是时间性能差一些）
- 略有不同，直接调用库函数的trim去除前后空格+split分割，但是split分割后的words有一定问题（因为没有事先对String s的中间空格作处理）

```JAVA
class Solution {
    public String reverseWords(String s) {
        String[] words = s.trim().split(" ");

        StringBuilder strBuilder = new StringBuilder();
        for (int j = words.length - 1; j > 0; j--) {
            if(words[j].equals("")){
                continue;
            }
            strBuilder.append(words[j]);
            strBuilder.append(" ");
        }
        strBuilder.append(words[0]);

        return strBuilder.toString();
    }
}
```

### 思路二

- 删除原字符串前后空格
- 对字符串中间空格删除到只剩一个
- 反转整个字符串
- 再对每个单词分别反转

> 可以实现一个函数——对指定字符串进行反转，且可以指定begin和end的index
>
> ps：性能略快一筹

```java
class Solution {
    public String reverseWords(String s) {
        char[] charArray = s.toCharArray();

        //1. 处理字符串开头、中间、结尾处的空格：
        StringBuilder sb = new StringBuilder();
        int i=0;
        boolean flag = false;//表示是否新加了一个单词
        while(i<charArray.length){
            if(charArray[i]==' ' && sb.isEmpty()){//处理开头空格
                i++;
                continue;
            }else if(charArray[i]!=' '){
                sb.append(charArray[i]);
                flag = true;
            }else if(charArray[i]==' ' && flag){
                sb.append(' ');
                flag = false;
            }
            i++;
        }
        if(sb.lastIndexOf(" ")==sb.length()-1){
            sb.deleteCharAt(sb.length()-1);//删除最后结尾的空格
        }
        charArray = sb.toString().toCharArray();//此时的charArray已处理完空格
        //处理后的charArray格式必定为：一个单词一个空格，且开头结尾无空格
        //2. 反转字符串
        reverseCharArray(charArray,0,charArray.length-1);
        //3. 再反转每个单词
        for(int begin=0;begin<charArray.length;){
            int end = begin+1;
            while(end<charArray.length && charArray[end]!=' '){
                end++;
            }
            reverseCharArray(charArray,begin,end-1);
            begin=end+1;
        }
        s = new String(charArray);
        return s;
    }
    /**
     * 反转闭区间[begin,end]内的字符
     * @param charArray
     * @param begin
     * @param end
     */
    public void reverseCharArray(char[] charArray,int begin,int end){
        for(int left = begin,right = end;left<right;left++,right--){
            char temp = charArray[left];
            charArray[left] = charArray[right];
            charArray[right] = temp;
        }
    }
}
```

## [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

kmp算法经典步骤：

1. 求得next数组（可以是部分匹配表，也可以是部分匹配表整体减一）
2. 基于next数组进行kmp匹配

**构造next数组**

- 方法为`int[] getNext(String pattern)`，传入参数为`String pattern`模式串，返回结果为`int[] next`next数组（或称部分匹配表【以下java代码实现中——next数组 == 部分匹配表】）

```java
public int[] getNext(String pattern){
    int[] next = new int[pattern.length()];
    next[0] = 0;
    for(int i=1,j=0;i<pattern.length();i++){
        while(j>0 && pattern.charAt(i)!=pattern.charAt(j)){
            j = next[j-1];
        }
        if(pattern.charAt(i) == pattern.charAt(j)){
            j++;
        }
        next[i] = j;
    }
    return next;
}
```

**基于next数组进行kmp字符串匹配**

- 方法为`int kmp(String str,String pattern)`，传入参数为`String str`原字符串、`String pattern`模式串，返回结果为匹配成功时模式串在原字符串中首次出现的位置（若无返回-1）

```java
public int kmp(String str,String pattern){
    int[] next = getNext(pattern);
    for(int i=0,j=0;i<str.length();i++){
        while(j>0 && str.charAt(i) != pattern.charAt(j)){
			j = next[j-1];
        }
        if(str.charAt(i) == pattern.charAt(j)){
            j++;
        }
        if(j == pattern.length()){
            return i-j+1;
        }
    }
    return -1;
}
```

### 本题解题代码

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int[] next = getNext(needle);
        for (int i = 0, j = 0; i < haystack.length(); i++) {
            while (j > 0 && haystack.charAt(i) != needle.charAt(j)) {
                j = next[j - 1];
            }
            if (haystack.charAt(i) == needle.charAt(j)) {
                j++;
            }
            if (j == needle.length()) {
                return i - j + 1;
            }
        }
        return -1;
    }

    public int[] getNext(String str) {
        int[] next = new int[str.length()];
        next[0] = 0;
        for (int i = 1, j = 0; i < next.length; i++) {
            while (j > 0 && str.charAt(i) != str.charAt(j)) {
                j = next[j - 1];
            }
            if (str.charAt(i) == str.charAt(j)) {
                j++;
            }
            next[i] = j;
        }
        return next;
    }
}
```

## [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

- 记字符串长度为`len`，`next`为其部分匹配表，如果满足`len % (len - next[len-1]) == 0`说明该字符串为重复的（当然实际条件要写成：`next[s.length()-1] == 0 ? false :s.length()% (s.length()-next[s.length()-1]) == 0`.if）

> 假设整个字符串都是有重复的子字符串构成，那么构造成的next数组的最后一位的数值，一定是整个字符串s的长度 减去 “最短公共重复子串的长度”
>
> 举个例子：abcdefabcdefabcdef是由abcdef不断重复组成的，且重复了3次。整个字符串长度为18，“最短公共重复子串的长度”为6（abcdef长度），我们（肉眼看出）next[len-1] = 12。（对吧！），然后代入上述公式是不是就能检验呢？
>
> > 当然上述公式 实际判断形式要稍微细心一些（排除`next[len-1] == 0`的情况）

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int[] next = getNext(s);
        //如果next[s.length()-1] == 0说明没形成重复字符串
        return next[s.length()-1] == 0 ? false :s.length()% (s.length()-next[s.length()-1]) == 0;
    }

    public int[] getNext(String s){
        int[] next = new int[s.length()];
        int j = 0;
        for(int i=1;i<s.length();i++){
            while(j>0 && s.charAt(i)!=s.charAt(j)){
                j = next[j-1];
            }
            if(s.charAt(i)==s.charAt(j)){
                j++;
            }
            next[i] = j;
        }
        return next;
    }   
}
```

# 栈与队列

## [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

略……

题目本身不重要，

**原理**：重要的是了解栈、队列数据结构的底层实现原理

**应用**：懂了原理后，掌握对应语言的的栈、队列的实现以及如何调用

**关于本题原意**：使用两个栈来**模拟**队列——

1. 我们人为给两个栈分为“输入栈”和“输出栈”
2. 当要求模拟队列“push”时，往“输入栈”存入元素即可
3. 当要求模拟队列“pop”时
   - 先把所有“输入栈”的元素逐个弹出，按弹出顺序压入“输出栈”
   - 若“输出栈”此时不为空，则弹出“输出栈”中一个元素（这就模拟了队列最前的元素的“出队”操作）
   - 最后把“输出栈”中元素依次弹出，存回“输入栈”中即可

4. “判断是否为空”，直接对“输入栈”判断即可（其实上述第三步的“判断操作”也可以略作修改）

> 代码实现略

## [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

- 同上、重要的是**原理** + **应用**
- 关于本题：

1. 由于两个队列来回“倒腾”元素最后顺序还是不变（队列的性质），所以其中一个队列只是用来临时存储的
2. 毕竟队列是先进先出，那么遇到栈的出栈操作时，我们需要取到队列的“最后一个元素”
3. 此时就需要将前面元素全部暂存到另一个栈，等输出了最后一个元素后，再把元素存回去即可

## [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

经典的栈的应用，想清楚了很简单，我这里给出高度提炼性的总结（能悟到就觉得“括号匹配”就该用栈解决）

括号匹配的特点：

1. **就近匹配**（也就是判断栈顶）
2. **成对出现**（也就是出栈条件）
3. **完美匹配**（栈为空则？）
4. **不能嵌套**（不能是`([)]`）

> emmm思路是简单的，但是实际写代码小错误还是很多的（每次看到有新的样例错了……再加上一点条件判断，最后变成了下面一坨代码）
>
> （可能可以优化吧）

### 丑陋的判断条件版

```java
class Solution {
    public boolean isValid(String s) {
        if(s!=null && (s.charAt(0) == ')' || s.charAt(0) == ']' || s.charAt(0) == '}'))
            return false;

        Deque<Character> stack = new ArrayDeque<>();
        char[] charList = s.toCharArray();
        for(int i=0;i<charList.length;i++){
            if(charList[i] == '(' || charList[i] == '[' || charList[i] == '{'){
                stack.offerLast(charList[i]);
            }else if(!stack.isEmpty()){
                if(charList[i] == ')'){
                    if(stack.getLast() == '('){
                        stack.removeLast();
                    }else {
                        return false;
                    }
                    
                }
                else if(charList[i] == ']'){
                    if(stack.getLast() == '['){
                        stack.removeLast();
                    }else{
                        return false;
                    }
                    
                }else if(charList[i] == '}'){
                    if(stack.getLast() == '{'){
                        stack.removeLast();
                    }else{
                        return false;
                    }
                }
            }else{
                stack.offerLast(charList[i]);
            }
        }    
        return stack.isEmpty();
    }

}
```

### 优化版

当“左括号需要入栈”时，可以用他对应的右括号入栈来代替，这样的好处是——当遇到右括号需要判断时，只需判断它与栈顶是否相同即可（简化了代码写法、清晰）

```java
class Solution {
    public boolean isValid(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        char[] charList = s.toCharArray();
        for (int i = 0; i < charList.length; i++) {
            // 左括号想要入栈时，我们实际存他对应的右括号
            if (charList[i] == '(')
                stack.offerLast(')');
            else if (charList[i] == '[')
                stack.offerLast(']');
            else if (charList[i] == '{')
                stack.offerLast('}');
            else {
                // 这样的话，当遇到右括号时，只需判断是否与栈顶相同即可
                if (stack.isEmpty() || stack.getLast() != charList[i]) {
                    return false;
                } else {
                    // 栈不为空 且 匹配成功的情况：
                    stack.removeLast();
                }
            }
        }
        return stack.isEmpty();
    }
}
```

## [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

经典的关于栈的运用，核心套路就这样：

1. 遇到数就入栈
2. 遇到符号就从栈中取出两个做运算

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<String> stack = new ArrayDeque<>();
        for(String s : tokens){
            if(s.equals("+")){
                String[] tks = getTokens(stack);
                Integer num2  = Integer.parseInt(tks[0]);
                Integer num1  = Integer.parseInt(tks[1]);
                stack.offerLast(Integer.toString(num1+num2));
            }else if(s.equals("-")){
                String[] tks = getTokens(stack);
                Integer num2  = Integer.parseInt(tks[0]);
                Integer num1  = Integer.parseInt(tks[1]);
                stack.offerLast(Integer.toString(num1-num2));
            }else if(s.equals("*")){
                String[] tks = getTokens(stack);
                Integer num2  = Integer.parseInt(tks[0]);
                Integer num1  = Integer.parseInt(tks[1]);
                stack.offerLast(Integer.toString(num1*num2));
            }else if(s.equals("/")){
                String[] tks = getTokens(stack);
                Integer num2  = Integer.parseInt(tks[0]);
                Integer num1  = Integer.parseInt(tks[1]);
                stack.offerLast(Integer.toString(num1/num2));
            }else{
                stack.offerLast(s);
            }
        }
        return Integer.parseInt(stack.removeLast());
    }

    public String[] getTokens(Deque<String> stack){
        String[] tokens = new String[2];
        tokens[0] = stack.removeLast();
        tokens[1] = stack.removeLast();
        return tokens;
    }
}
```



## [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

- 核心思路：

  1. 使用队列来模拟这个“滑动窗口”

  2. 每次滑动窗口时，需要弹出一个已有元素，并加入一个新的元素

     > 但是这样的想法很简单、不能满足题目需求，比如nums = [1,3,-1,-3,5,3,6,7], k = 3
     >
     > 初始滑动窗口为[1,3,-1]时，最大值为3（这一点可以做到）
     >
     > 后续开始“滑动”，当3要被弹出时，这时候更新最大值就成了问题，因为我们此时的队列中存储的k个数据与nums数组中的数据完全无异，还是要从k个元素中再次找最大值
     >

  3. 其实我们没必要维护窗口中所有元素，我们只关心最大的那个元素；在维护时，我们只需要维护**有可能成为窗口中最大值的元素**即可

- 先解释一下什么叫“有可能成为窗口中最大值的元素”：

  ![image-20250305161320609](https://raw.gitmirror.com/Yukinoshita52/images/main/imgs/20250305161429270.png)

  ![image-20250305161113455](https://raw.gitmirror.com/Yukinoshita52/images/main/imgs/20250305161127329.png)

  ![image-20250305161449054](https://raw.gitmirror.com/Yukinoshita52/images/main/imgs/20250305161459371.png)

- 那么这是怎么做到的呢？

- 需要自己设计一个队列，规则如下：

  - push时，（ps：先判断dui'lie）每次新加入的值如果比 入口元素（即队尾） 大，则不断将 入口元素（即队尾） 弹出，直到小于等于入口元素，再入队。
  -  pop时，要求队列非空 且 窗口移除的元素 等于 出口元素时，弹出出口元素（即队首），否则不进行任何操作。

- 特别注意！MyQueue 类中 pop 方法里用 `==` 比较 Integer 对象，导致大数值比较出错。在 Java 中，使用 `==` 比较 Integer 时，对于范围在 [-128,127] 之外的数值可能得不到正确结果（因为它们不是同一对象），所以需要使用 `.equals()`方法进行数值比较。

```java
public void pop(Integer num){
    //这里判断条件一定要写.equals() !!!
    if(!queue.isEmpty() && queue.getFirst().equals(num)){
        queue.removeFirst();
    }
}

```

`完整代码解答`

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int[] ans = new int[nums.length - k + 1];

        MyQueue queue = new MyQueue();

        for (int i = 0; i < k; i++) {
            queue.push(nums[i]);
        }
        ans[0] = queue.front();

        // 2. 不断向后滑动，直到最后一个元素入队
        for (int i = k; i < nums.length; i++) {
            queue.pop(nums[i - k]);
            queue.push(nums[i]);
            ans[i - k + 1] = queue.front();
        }
        return ans;
    }
}

class MyQueue {
    // 自己维护一个队列
    // 规则：
    // 1. push时，每次新加入的值如果比 入口元素（即队尾） 大
    // （非空）则不断将 入口元素（即队尾） 弹出，直到小于等于，入队
    // 2. pop时，要求队列非空 且 窗口移除的元素 等于 出口元素时，弹出出口元素（即队首）
    Deque<Integer> queue = new ArrayDeque<>();

    public void push(Integer num) {
        while (!queue.isEmpty() && num > queue.getLast()) {
            queue.removeLast();
        }
        queue.offerLast(num);
    }

    public void pop(Integer num) {
        if (!queue.isEmpty() && queue.getFirst().equals(num)) {
            queue.removeFirst();
        }
    }

    public Integer front() {
        return queue.getFirst();
    }
}
```

## [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

1. 用HashMap记录每个值及其出现次数
2. 用PriorityQueue，且自定义排序规则，来作为优先队列——保证出现次数少的数排在前面
3. 若priorityQueue中元素个数大于k，则需要弹出最前元素（因为是已排序+弹出最小的）
4. 最后输出这k个元素的key值。

> `PriorityQueue<Map.Entry<Integer,Integer>> prioriyQueue = new PriorityQueue<>();`
>
> 注意！如果没有import java.util.Map.Entry，是不能直接写Entry类的！！（包括后续用Entry遍历Map也是一样，要写Map.Entry）

```java
import java.util.*;

class Solution {
    /**
     * 求数组中出现频率最高的 K 个元素
     *
     * @param nums 给定的整数数组
     * @param k 需要找出的高频元素个数
     * @return 出现频率最高的 K 个元素
     */
    public int[] topKFrequent(int[] nums, int k) {
        // 1. 统计每个数字的出现次数
        Map<Integer, Integer> frequencyMap = new HashMap<>();
        for (int num : nums) {
            frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
        }

        // 2. 维护一个小顶堆，按出现次数排序（最小的在堆顶）
        PriorityQueue<Map.Entry<Integer, Integer>> minHeap =
            new PriorityQueue<>(Comparator.comparingInt(Map.Entry::getValue));

        // 3. 遍历频率映射表，将元素加入堆中
        for (Map.Entry<Integer, Integer> entry : frequencyMap.entrySet()) {
            minHeap.offer(entry);
            if (minHeap.size() > k) {
                minHeap.poll(); // 保持堆的大小为 K
            }
        }

        // 4. 取出堆中的 K 个元素
        int[] result = new int[k];
        for (int i = k - 1; i >= 0; i--) {
            result[i] = minHeap.poll().getKey();
        }

        return result;
    }
}

```

## [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

### 最直观——双指针法

时间复杂度：$O(n^2)$

采用了“以列来接雨水”思路：

对于每一列，分别向左右各自寻找最高的柱子，则当前柱子能存的雨水量为`当前柱子的左边最高柱子以及右边最高柱子的较矮的柱子高度 - 当前柱子高度`，看图直观理解：

- 假设有以下柱子列

  <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/imgs/20250306162901644.png" alt="image-20250306162845770" style="zoom:50%;" />

  <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/imgs/20250306163227970.png" alt="image-20250306163216844" style="zoom:50%;" />

```java
class Solution {
    public int trap(int[] height) {
        //确定接水的“起始点”、“终止点”（两个边缘点）
        //算是小优化吧……
        int start = 0;
        while(start+1 < height.length && height[start] <= height[start+1]){
            start++;
        }
        int end = height.length-1;
        while(end-1 > 0 && height[end] <= height[end-1]){
            end--;
        }
        if(start == height.length-1 || end == 0) return 0;

        int rain = 0;
        //对于每一个柱子i，用双指针寻找 它左边最高的柱子、右边最高的柱子
        for(int i=start+1;i<end;i++){
            int leftHeight = height[i];
            int rightHeight = height[i];
            for(int l = i-1;l>=start;l--){
                if(height[l] > leftHeight) leftHeight = height[l];
            }
            for(int r = i+1;r<=end;r++){
                if(height[r] > rightHeight) rightHeight = height[r];
            }
            rain += Integer.min(leftHeight,rightHeight) - height[i];
        }
        return rain;
    }
}
```

### 动态规划解法

时间复杂度：$O(n)$

- 在上述的双指针解法中，时间复杂度为$O(n^2)$的原因——对于每一个柱子，我们都要向左、向右再次找“最高柱”，导致了时间复杂度较高

- 其实在求“左边最高柱”、“右边最高柱”时，存在重复计算，可以被优化：

- 用两个数组maxLeft、maxRight分别记录第i号柱子的左、右最高柱信息

  > maxLeft[i]表示从左数起，到第i号柱子的最高柱子高度
  >
  > maxRight[i]表示从右数起，到第i号柱子的最高柱子高度
  >
  
- `maxLeft[i] = max(maxLeft[i-1],height[i]);`
- `maxRight[i] = max(maxRight[i+1],height[i]);`

```java
class Solution {
    public int trap(int[] height) {
        int[] maxLeft = new int[height.length];
        int[] maxRight = new int[height.length];

        maxLeft[0] = height[0];
        for (int i = 1; i < height.length; i++) {
            maxLeft[i] = Integer.max(maxLeft[i - 1], height[i]);
        }

        maxRight[height.length - 1] = height[height.length - 1];
        for (int i = height.length - 2; i >= 0; i--) {
            maxRight[i] = Integer.max(maxRight[i + 1], height[i]);
        }

        int rain = 0;
        // 第一格和最后一格不接水
        for (int i = 1; i < height.length - 1; i++) {
            rain += Integer.min(maxLeft[i], maxRight[i]) - height[i];
        }

        return rain;
    }
}
```

### 单调栈解法

思路见代码部分

- 下图所用测试样例：

```java
{2,3,2,1,1,0,1,0,4,0,3}
//预期结果
16
```

![image-20250309123924772](https://raw.gitmirror.com/Yukinoshita52/images/main/imgs/20250309123935213.png)

```java
class Solution {
    public int trap(int[] height) {
        //思路：“横向”接
        //计算公式为：长 × 宽，其中长为柱子高度，
        //      长 = 具体为min(左柱子，右柱子)
        //      宽  =当前下标 i - 栈顶的下一个元素
        // 采用单调栈解法，其中存储的为柱子的下标。（保持单调递增【栈顶到栈底】）
        int rain = 0;
        MyStack stack = new MyStack(height);
        for(int i=0;i<height.length;i++){
            rain += stack.push(i);
        }
        return rain;
    }
}

class MyStack{
    Deque<Integer> stack = new ArrayDeque<>();
    int[] height;

    MyStack(int[] height){
        this.height = height;
    }

    public Integer push(int i){
        
        if(stack.isEmpty()){
            stack.offerLast(i);
            return 0;
        }
        int rain = 0;
        //若新入栈的柱子高度递减，则直接入栈
        if(height[i] < height[stack.getLast()]) stack.offerLast(i);
        else if(height[i] == height[stack.getLast()]) {
            stack.removeLast();
            stack.offerLast(i);
        }else{
            //若新加入的柱子高度高于栈顶
            //说明出现“凹槽”
            //执行下列步骤：
            
            while(!stack.isEmpty() && height[i] > height[stack.getLast()]){
                //  1. 弹出栈顶元素(下标)
                int mid = stack.removeLast();
                if(!stack.isEmpty()){
                    //  2. 若栈还不为空，则可计算这个凹槽雨水量：
                    // (“两边高柱子”中短的柱子高度 - 低点高度) * 宽度（即:右边下标 - 左边下标 - 1）
                    rain += (Integer.min(height[stack.getLast()] , height[i]) - height[mid]) * (i - stack.getLast()-1);
                }
            }
            // 3. 最后将右边柱子入栈
            stack.offerLast(i);
        }
        return rain;
    }

    public Boolean isEmpty(){
        return stack.isEmpty();
    }
}
```



# 二叉树

- 注意：二叉树的前序、中序、后续、层次遍历，都有2种形式的实现——递归、迭代
- 换个角度思考的话，就是遍历方式有**四种**；实现形式有**两种**。

## [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        preOrder(res,root);
        return res;
    }

    public void preOrder(List<Integer> list,TreeNode node){
        if(node == null) return;
        list.add(node.val);
        preOrder(list,node.left);
        preOrder(list,node.right);
    }
}
```

## [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        inOrder(res,root);
        return res;
    }

    public void inOrder(List<Integer> list,TreeNode node){
        if(node == null) return;
        inOrder(list,node.left);
        list.add(node.val);
        inOrder(list,node.right);
    }
}
```

## [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode() {}
 * TreeNode(int val) { this.val = val; }
 * TreeNode(int val, TreeNode left, TreeNode right) {
 * this.val = val;
 * this.left = left;
 * this.right = right;
 * }
 * }
 */
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        postOrder(res, root);
        return res;
    }

    public void postOrder(List<Integer> list, TreeNode node) {
        if (node == null)  return;
        postOrder(list, node.left);
        postOrder(list, node.right);
        list.add(node.val);
    }
}
```



## [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

- 题目思路是很简单的：看见层次遍历，立马想到用队列
- 在遍历当前层的同时，往队列中增加下一层元素（(#`O′)……我的代码好像可以改进）
- 下面第一版代码是我下意识想到的写法，然后出乎意料的在某处报错了，特地保留这个报错来加深这个知识点的印象
- 改进版代码简洁许多（因为压根用不到两个方法）

<img src="https://raw.gitmirror.com/Yukinoshita52/images/main/imgs/20250311204329863.png" alt="image-20250311204319399" style="zoom:67%;" />

<img src="https://raw.gitmirror.com/Yukinoshita52/images/main/imgs/20250311204425505.png" alt="image-20250311204359920" style="zoom:67%;" />

<img src="https://raw.gitmirror.com/Yukinoshita52/images/main/imgs/20250311204436611.png" alt="image-20250311204415217" style="zoom:67%;" />

> 可见Java的基础知识不扎实——在这里就成了我的报错原因

### 第一反应代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode() {}
 * TreeNode(int val) { this.val = val; }
 * TreeNode(int val, TreeNode left, TreeNode right) {
 * this.val = val;
 * this.left = left;
 * this.right = right;
 * }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        Deque<TreeNode> deque = new ArrayDeque<>();
        if (root != null) {
            deque.offerLast(root);// 根节点入队列
        }
        while (!deque.isEmpty()) {
            bfs(res, deque);
        }

        return res;
    }

    public void bfs(List<List<Integer>> res, Deque<TreeNode> deque) {
        List<Integer> list = new LinkedList<>();//用于存放本层遍历的数值
        Deque<TreeNode> newDeque = new ArrayDeque<>();//用于记录下一层的node

        while (!deque.isEmpty()) {
            TreeNode node = deque.removeFirst();
            list.add(node.val);
            if (node.left != null)
                newDeque.offerLast(node.left);
            if (node.right != null)
                newDeque.offerLast(node.right);
        }
        deque.addAll(newDeque);//注意这里不能写deque = newDeque!!
        //原因是：Java中，方法的参数都是——按值传递的！
        //虽然传递的deque是个引用数据类型，但这个参数本身（deque）是-
        //（levelOrder中deque的）副本
        //因此在方法bfs中即使修改了deque的指向，也只是将这个“副本”的指向更改
        //并不会影响到外部原来deque的指向！！！
        //因此要调用方法.addAll方法！（地址.方法-->>正确修改）
        res.add(list);
    }
}
```

### 优化版

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        Deque<TreeNode> deque = new ArrayDeque<>();
        if (root != null) {
            deque.offerLast(root);// 根节点入队列
        }
        while (!deque.isEmpty()) {
            int num = deque.size();//获取本层的元素个数
            List<Integer> list = new LinkedList<>();//用于存放本层遍历的数值
            while(num-- > 0){
                TreeNode node = deque.removeFirst();
                list.add(node.val);
                if (node.left != null)  deque.offerLast(node.left);
                if (node.right != null)  deque.offerLast(node.right);
            }
            res.add(list);//本层元素加入
        }
        return res;
    }
}
```

## [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

- 统一思路：对于每个节点，反转其左右；
- 题虽简，其法繁

> 作为一个典型例子，介绍解二叉树的题目的常见方法

### 递归法

- 明确自己使用的是前序、中序、后序遍历的其中一种

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        reverse(root);
        return root;
    }
    public void reverse(TreeNode node){
        if(node == null) return;
        TreeNode tmp = node.left;
        node.left = node.right;
        node.right = tmp;

        reverse(node.left);
        reverse(node.right);
    }
}
```

### 迭代法

- 用栈来模拟递归调用

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        Deque<TreeNode> stack = new ArrayDeque<>();//迭代法（用栈）

        if(root != null) stack.addLast(root);
        while(!stack.isEmpty()){
            //弹出栈顶元素
            TreeNode node = stack.removeLast();
            //翻转该元素的左、右子树
            TreeNode tmp = node.left;
            node.left = node.right;
            node.right = tmp;
            //将该元素的左、右子树入栈
            if(node.left != null) stack.addLast(node.left);
            if(node.right != null) stack.addLast(node.right);
        }
        return root;
    }
}
```



### 层次遍历法

- 本题可以用“逐层翻转其左右子树”的思路来解题

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        Deque<TreeNode> deque = new ArrayDeque<>();
        if(root != null) deque.addLast(root);
        while(!deque.isEmpty()){
            int num = deque.size();
            for(int i=0;i<num;i++){
                TreeNode node = deque.removeFirst();
                
                TreeNode tmp = node.left;
                node.left = node.right;
                node.right = tmp;
                if(node.left != null) deque.addLast(node.left);
                if(node.right != null) deque.addLast(node.right);
            }
        }
        return root;
    }
}
```



## [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

### 递归

设计一个递归函数`boolean isSymmetric(TreeNode leftNode,TreeNode rightNode)`

1. 确定参数、返回值——左、右节点作为参数；boolean作为返回值
2. 确定终止条件——
   - 左、右同时为null，返回true（对称）
   - 左或右一个null，返回false（不对称）
   - 左、右值不等，返回false
   - 左、右值相等——向下判断：左节点的左与右节点的右，左节点的右与右节点的左

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        // 递归：
        if (root == null)
            return true;
        // 若为"前序"遍历：
        // 2 3 4（右递归）（当前节点、左子节点、右子节点）
        // 2 3 4（左递归）（当前节点、右子节点、左子节点）
        return isSymmetric(root.left, root.right);
    }

    public boolean isSymmetric(TreeNode leftNode, TreeNode rightNode) {
        // 递归参数：左、右节点
        if (leftNode == null && rightNode == null)
            return true;
        else if (leftNode == null || rightNode == null)
            return false;
        else if (leftNode.val != rightNode.val)
            return false;
        return isSymmetric(leftNode.left, rightNode.right)
                && isSymmetric(leftNode.right, rightNode.left);
    }
}
```

### 迭代

- `ArrayDeque`不允许存入null！！！

  <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/imgs/20250312121833254.png" alt="image-20250312121820441" style="zoom:67%;" />

- 但是Deque的另一个实现类`LinkedList`可以存入任何值（include null）

  <img src="https://raw.gitmirror.com/Yukinoshita52/images/main/imgs/20250312121945465.png" alt="image-20250312121929857" style="zoom:80%;" />

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        // 迭代：
        if (root == null)
            return true;
        // 若为"前序"遍历：
        // 2 3 4（右递归）（当前节点、左子节点、右子节点）
        // 2 3 4（左递归）（当前节点、右子节点、左子节点）
        // Deque<TreeNode> leftStack = new ArrayDeque<>();  //ArrayDeque不允许存入null！
        // Deque<TreeNode> rightStack = new ArrayDeque<>();
        Deque<TreeNode> leftStack = new LinkedList<>();
        Deque<TreeNode> rightStack = new LinkedList<>();
        leftStack.addLast(root.left);
        rightStack.addLast(root.right);

        while (!leftStack.isEmpty() && !rightStack.isEmpty()) {
            TreeNode leftNode = leftStack.removeLast();
            TreeNode rightNode = rightStack.removeLast();
            if (leftNode == null && rightNode == null)
                continue;
            else if (leftNode == null || rightNode == null
                    || leftNode.val != rightNode.val) {
                return false;
            }

            leftStack.addLast(leftNode.left);
            leftStack.addLast(leftNode.right);
            rightStack.addLast(rightNode.right);
            rightStack.addLast(rightNode.left);
            
        }
        return true;
    }

}
```

## [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null)
            return 0;
        return getDepth(root);// 确保了root不为null
    }

    public int getDepth(TreeNode node) {
        if(node == null) return 0;
        // 1. 确定递归参数、返回值
        // 2. 确定终止条件——遇到下一层为null值
        // 3. 确定单层递归的逻辑——将已有的深度 与 向左、右子树的较大者相加
        return Math.max(getDepth(node.left), getDepth(node.right)) + 1;
    }
}
```



## [111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

### 方式Ⅰ——设置终止条件为“遇到叶子节点”

```JAVA
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        return getMinDepth(root);
    }
    public int getMinDepth(TreeNode node){
        if(node.left == null && node.right == null){//当前节点为叶子节点
            return 1;
        }
        int leftDepth = Integer.MAX_VALUE;
        int rightDepth = Integer.MAX_VALUE;
        if(node.left != null){
            leftDepth = minDepth(node.left);//不断向左递归，直到遇到叶子节点
        }
        if(node.right != null){
            rightDepth = minDepth(node.right);//不断向右递归，直到遇到叶子节点
        }
        int minDe = Math.min(leftDepth, rightDepth) + 1;//选择从当前节点到叶子节点的节点个数少的作为minDe
        return minDe;
    }
}
```

### 方式Ⅱ——设置终止条件为遇到null值（但是返回深度时处理注意细节）

```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        int leftDepth = minDepth(root.left);	//左
        int rightDepth = minDepth(root.right);	//右
        										//中
        //始终注意：“最小深度”要遇到叶子节点		
        //若左子树为空，右子树不为空，那么计算“最小深度”，应该来自于右子树
        if(root.left == null && root.right != null){
            return rightDepth+1;
        }
        //若右子树为空，左子树不为空，那么计算“最小深度”，应该来自于左子树
        if(root.right == null && root.left != null){
            return leftDepth+1;
        }
        return Math.min(leftDepth,rightDepth)+1;
        
    }
}
```

## [110. 平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)

### 方式1

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;
        //左、右、中
        //左右子树都平衡、左右子树深度之差不超过1
        return isBalanced(root.left) && isBalanced(root.right) 
        && Math.abs(getDepth(root.left) - getDepth(root.right))<=1;
    }
    public int getDepth(TreeNode node){
        if(node == null) return 0;
        return Math.max(getDepth(node.left),getDepth(node.right))+1;
    }
}
```

### 方式2（优化）

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;
        //左、右、中
        return getDepth(root) != -1;
    }
    public int getDepth(TreeNode node){
        if(node == null) return 0;
        //返回-1则表示已经不平衡
        int leftDepth = getDepth(node.left);
        if(leftDepth == -1) return -1;
        int rightDepth = getDepth(node.right);
        if(rightDepth == -1) return -1;
        //若左右高度之差小于1，则正常返回值（当前节点高度），否则返回-1（表示不平衡）
        return (Math.abs(leftDepth - rightDepth) <= 1) ? Math.max(leftDepth,rightDepth)+1 : -1;
    }
}
```



## [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

### 回溯

```java
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        sb.append(root.val);
        treePaths(root,sb);
        return res;
    }
    public void treePaths(TreeNode root,StringBuilder sb){
        if(root.left == null && root.right == null){
            res.add(sb.toString());
        }
        if(root.left!=null) {
            treePaths(root.left,sb.append("->"+root.left.val));
            int len = new String("->"+root.left.val).length();
            sb.setLength(sb.length()-len);//回溯，删除最后添加进的字符
        }
        if(root.right!=null) {
            treePaths(root.right,sb.append("->"+root.right.val));
            int len = new String("->"+root.right.val).length();
            sb.setLength(sb.length()-len);//回溯，删除最后添加进的字符
        }
    }
}
```

### 回溯法优化

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        treePaths(root,new StringBuilder(),res);
        return res;
    }
    public void treePaths(TreeNode root,StringBuilder path,List<String> res){
        path.append(root.val);
        int len = path.length();//记录到达该层的路径长度（开始前记录状态）
        

        //叶子节点，记录到结果集
        if(root.left == null && root.right == null){
            res.add(path.toString());
        }
        
        if(root.left!=null) {
            treePaths(root.left,path.append("->"),res);
            path.setLength(len);//回溯
        }
        if(root.right!=null) {
            treePaths(root.right,path.append("->"),res);
            path.setLength(len);//回溯
        }
    }
}
```





# 力扣每日一题打卡

**分类表**（ps：题面难度 ≠ 实际难度）

| 简单                                                         | 中等                                                         | 困难 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| [3110. 字符串的分数](https://leetcode.cn/problems/score-of-a-string/) | [63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/) |      |
|                                                              | [80. 删除有序数组中的重复项 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/) |      |

**暂时不会**：

- [ ] [913. 猫和老鼠](https://leetcode.cn/problems/cat-and-mouse/)
- [ ] 

## [63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/)

```java
class Solution {

    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int[][] dp = new int[obstacleGrid.length][obstacleGrid[0].length];
        
        // 初始化第一列
        if (obstacleGrid[0][0] == 0) {//为了解决某个特例……
                dp[0][0] = 1;
        }
        for (int i = 1; i < obstacleGrid.length; i++) {
            if (obstacleGrid[i][0] == 1) {
                continue;
            } else {
                dp[i][0] = dp[i-1][0];
            }
        }
        // 初始化第一行
        for (int j = 1; j < obstacleGrid[0].length; j++) {
            if (obstacleGrid[0][j] == 1) {
                continue;
            } else {
                dp[0][j] = dp[0][j-1];
            }
        }

        for (int i = 1; i < obstacleGrid.length; i++) {
            for (int j = 1; j < obstacleGrid[i].length; j++) {

                if (obstacleGrid[i][j] == 1) {
                    continue;
                } else {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }

            }
        }
        return dp[obstacleGrid.length - 1][obstacleGrid[0].length - 1];
    }

}
```

## [80. 删除有序数组中的重复项 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/)

- 思路：双指针……想到是很快，但是实际条件判断非常多，常常会出现“意外”的运行情况……然后发现按照自己的代码没有按照自己的想法在跑……或者说情况没考虑完全。
- 说明：我这里的i始终处在一个“随时准备被覆盖”的位置上，也就是执行`nums[i] = nums[j]`语句前世满足的。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int cnt = 0;
        int i = 0, j = 0;// i是慢指针、j是快指针
        // 使用“滑动窗口”，j比i快
        while (j < nums.length) {
            int last_num = nums[j];
            int cnt_tmp = 0;
            // [1,1,1,2,2,3]
            // [0,0,1,1,1,1,2,3,3]
            // 直到找到不相同的
            // 要重新开始cnt_tmp的计数（从0开始）
            while (j < nums.length && nums[j] == last_num) {
                if (cnt_tmp < 2) {
                    nums[i] = nums[j];// 移动nums[j]的数到nums[i]上
                    i++;
                }
                cnt_tmp++;
                j++;
            }
            if (cnt_tmp > 2) {
                cnt += 2;
            } else {
                cnt += cnt_tmp;
            }

        }
        return cnt;
    }
}
```

## [913. 猫和老鼠](https://leetcode.cn/problems/cat-and-mouse/)

**思路分析**

- 利用搜索进行解题、记忆化、剪枝
- 关于题目本身：
  1. 每次猫和老鼠同时走一步
  2. 两者都要以最佳方案进行走，也就是说，只要所有方案中有平局的，那么必定平局。
  3. 大致使用搜索解题
  4. 用`catVis`记录猫走过节点、用`catNext`记录猫下一个可走的位置
  5. 用`mouseVis`记录鼠走过节点、用`mouseNext`记录鼠下一个可走的位置
  6. 其中：无论是`catVis`还是`mouseVis`，都只能出现一次，而且`mouseNext`不能在`catNext`中，也就是说，虽然是猫与鼠“同时”，但是代码实现上，要先更新猫的`catVis`和`catNext`，再更新`mouseNext`，这种实现方式相当于鼠已经是最优走法
  7. 关于剪枝：如果`mouseNext`全都在`catNext`中，则说明老鼠已经输了，不用再走下一层。
  8. 如果回到上一层，相当于“悔棋”，鼠走上一步，要走不同的走法（直到鼠能赢）
- 可不可能出现“某些情况鼠赢、某些情况猫赢、而没有平局”呢？我感觉单纯搜索的话肯定会出现……

## [3110. 字符串的分数](https://leetcode.cn/problems/score-of-a-string/)

```java
class Solution {
    public int scoreOfString(String s) {
        int res = 0;
        for(int i=0;i<s.length()-1;i++){
            res += Math.abs((int)(s.charAt(i)-s.charAt(i+1)));
        }
        return res;
    }
}
```

