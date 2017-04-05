# <疯狂Java讲义【第3版】>

## 8.3.3 TreeSet 类

codes\08\8.3\TreeSetTest.java
```java
public class TreeSetTest {
  public static void main(String[] args) {
     TreeSet nums = new TreeSet();
     /*向 TreeSet 中添加 4 个 Integer 对象*/
     nums.add(5);
     nums.add(2);
     nums.add(10);
     nums.add(-9);
     // 注意：一旦元素添加进TreeSet 集合中，集合底层就会对元素进行排序
     /*可以验证打印*/
     System.out.println(nums);
  }
}
```
