# String (算法中)常用API
- `char charAt(int index)`：返回指定索引处的字符。
- `String substring(int beginIndex, int endIndex)`：左闭右开。
  - `String substring(int beginIndex)`：左闭右开，至结尾。
- `String concat(String str)`：拼接末尾。
- `int indexOf(String str)`：返回指定子字符串在此字符串中第一次出现处的索引。
  - `int lastIndexOf(String str)`：返回指定子字符串在此字符串中最后一次出现处的索引。
- `String[] split(String regex)`：根据给定正则表达式的匹配拆分此字符串。
- `boolean isEmpty()`：当且仅当 length() 为 0 时返回 true。(小心NPE)
- `int length()`：返回此字符串的长度。

- `String repeat(int count)`：返回一个字符串，它是此字符串的 count 次重复。
- `String strip()`：返回一个字符串，其值为此字符串，并删除任何前导和尾随空白。
- `String stripLeading()`：返回一个字符串，其值为此字符串，并删除任何前导空白。
- `String stripTrailing()`：返回一个字符串，其值为此字符串，并删除任何尾随空白。
- `boolean isBlank()`：当且仅当字符串为空或仅包含空白字符时返回 true。
- `String indent(int n)`：返回一个字符串，其值为此字符串，并在每行前添加 n 个空格。
# HashMap 使用
- 例如`Map<Integer,Integer> num2Cnt`用于记录**数字-数字出现频率**这一关系时。初始化构造时，可以使用`merge`方法进行构造。e.g.`num2Cnt.merge(x,1,Integer::sum);`
- 需要遍历一个Map的Entry时，如下：
  ```java
  for(Map.Entry<Integer,Interget> e : num2Cnt.entrySet()){
    //todo:
    e.getKey();
    e.getValue();
  }
  ```
# Collections
- `Collections.max()`，可以用于求出一个集合中的最大值。`Collections.min()`同理。e.g.`Collections.max(num2Cnt.values())`
- `Collections.sort()`，可以用于对一个List进行排序。默认升序。`Collections.sort(list, Collections.reverseOrder())`降序。
- `Collections.reverse()`，可以用于对一个List进行反转。
# Arrays
- `Arrays.sort()`，可以用于对一个数组进行排序。默认升序。第二个参数可以写lambda，`(a,b)->b-a`降序。
- `Arrays.fill()`，可以用于将一个数组的所有元素设置为指定的值
# System
- `System.arraycopy()`，可以用于将一个数组的指定范围内的元素复制到另一个数组的指定位置。