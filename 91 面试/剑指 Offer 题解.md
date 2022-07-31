选择（选择最大的放到前面）

```
public void sort(T[] nums) {
    int N = nums.length;
    for (int i = 0; i < N - 1; i++) {
        int min = i;
        for (int j = i + 1; j < N; j++) {
            if (less(nums[j], nums[min])) {
                min = j;
            }
        }
        swap(nums, i, min);
    }
}
```

冒泡

```
public void sort(T[] nums) {
    int N = nums.length;
    for (int i = N - 1; i > 0; i--) {
        for (int j = 0; j < i; j++) {
            if (less(nums[j + 1], nums[j])) {
                swap(nums, j, j + 1);
            }
        }
    }
}
```

插入（拿一个未排序的，放到已排序里面）

```
public void sort(T[] nums) {
    int N = nums.length;
    for (int i = 1; i < N; i++) {
        for (int j = i; j > 0 && less(nums[j], nums[j - 1]); j--) {
            swap(nums, j, j - 1);
        }
    }
}
```

快排

```
public static void quickSort(int[] arr,int low,int high){
    int i,j,temp,t;
    if(low>=high){
        return;
    }
    i=low;
    j=high;
    //temp就是基准位
    temp = arr[low];
    while (i<j) {
        while (temp<=arr[j]&&i<j) {
            j--;
        }
        while (temp>=arr[i]&&i<j) {
            i++;
        }
        //如果满足条件则交换
        if (i<j) {
            t = arr[j];
            arr[j] = arr[i];
            arr[i] = t;
        }
    }
    //最后将基准为与i和j相等位置的数字交换
    arr[low] = arr[i];
    arr[i] = temp;
    quickSort(arr, low, j-1);
    quickSort(arr, j+1, high);
}
```

归并


字符串

```
str.append("  ");
str.setCharAt(P2--, '%');
str.charAt(i)
str.toCharArray()
```

堆栈

```
push
pop
peek 最上面元素

PriorityQueue<Integer> maxHeap = new PriorityQueue<>((o1, o2) -> o2 - o1); 大顶堆（最小n个数）
heap.remove(num[i]);
heap.add(num[j]);
```

链表

```
head.next
.val
```

树


其他

```
Arrays.sort(nums, (s1, s2) -> (s1 + s2).compareTo(s2 + s1));
Math.pow(2, target - 1)
```

## 数组与矩阵

- [3. 数组中重复的数字](https://github.com/CyC2018/CS-Notes/blob/master/notes/3.%20%E6%95%B0%E7%BB%84%E4%B8%AD%E9%87%8D%E5%A4%8D%E7%9A%84%E6%95%B0%E5%AD%97.md)

   - 
```
在一个长度为 n 的数组里的所有数字都在 0 到 n-1 的范围内。
数组中某些数字是重复的，但不知道有几个数字是重复的，也不知道每个数字重复几次。
请找出数组中任意一个重复的数字。

数值对应位置一直交换
```

```
public class Solution {
    public int duplicate (int[] numbers) {
        for(int i = 0; i < numbers.length; i++){
            while(numbers[i] != i){
                if (numbers[i] == numbers[numbers[i]]){
                    return numbers[i];
                }
                swap(numbers, i, numbers[i]);
            }
        }
        return -1;
    }
    public void swap(int[] numbers, int a, int b){
        int t = numbers[a];
        numbers[a] = numbers[b];
        numbers[b] = t;
    }
}
```


- [4. 二维数组中的查找](https://github.com/CyC2018/CS-Notes/blob/master/notes/4.%20%E4%BA%8C%E7%BB%B4%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E6%9F%A5%E6%89%BE.md)

   - 
```
给定一个二维数组，其每一行从左到右递增排序，从上到下也是递增排序。
给定一个数，判断这个数是否在该二维数组中。

小于它的数一定在其左边，大于它的数一定在其下边。
```


- [5. 替换空格](https://github.com/CyC2018/CS-Notes/blob/master/notes/5.%20%E6%9B%BF%E6%8D%A2%E7%A9%BA%E6%A0%BC.md)

   - 
```
将一个字符串中的空格替换成 "%20"。

先 append，再两个指针过一遍
```

```
public class Solution {
    public String replaceSpace (String s) {
        StringBuffer str = new StringBuffer(s);
        int p1 = str.length() - 1;
        for (int i = 0; i <= p1; i++){
            if (' ' == str.charAt(i)) {
                str.append("  ");
            }
        }
        int p2 = str.length() - 1;
        while (p1 >= 0 && p1 < p2) {
            char c = str.charAt(p1);
            if (' ' == c) {
                str.setCharAt(p2--, '0');
                str.setCharAt(p2--, '2');
                str.setCharAt(p2--, '%');
            } else {
                str.setCharAt(p2--, c);
            }
            p1--;
        }
        return str.toString();
    }
}
```


- [29. 顺时针打印矩阵](https://github.com/CyC2018/CS-Notes/blob/master/notes/29.%20%E9%A1%BA%E6%97%B6%E9%92%88%E6%89%93%E5%8D%B0%E7%9F%A9%E9%98%B5.md)

   - 
```
从外向里以顺时针的顺序依次打印出每一个数字

四个位置下标，判断下标是否重叠
```

```
public class Solution {
    public ArrayList<Integer> printMatrix(int [][] matrix) {
        ArrayList<Integer> arr = new ArrayList<>();
        int x0 = 0, y1 = matrix[0].length - 1, y0 = 0, x1 = matrix.length - 1;
        while (x0 <= x1 && y0 <= y1) {
            if (x0 == x1) {
                for (int i = y0; i <= y1; i++) {
                    arr.add(matrix[x0][i]);
                }
            } else if (y0 == y1) {
                for (int i = x0; i <= x1; i++) {
                    arr.add(matrix[i][y0]);
                }  
            } else {
                for (int i = y0; i < y1; i++) {
                    arr.add(matrix[x0][i]);
                }
                for (int i = x0; i < x1; i++) {
                    arr.add(matrix[i][y1]);
                }
                for (int i = y1; i > y0; i--) {
                    arr.add(matrix[x1][i]);
                }
                for (int i = x1; i > x0; i--) {
                    arr.add(matrix[i][y0]);
                }
            }
            x0++; x1--; y0++; y1--;
        }
        return arr;
    }
}
```


- [50. 第一个只出现一次的字符位置](https://github.com/CyC2018/CS-Notes/blob/master/notes/50.%20%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%8F%AA%E5%87%BA%E7%8E%B0%E4%B8%80%E6%AC%A1%E7%9A%84%E5%AD%97%E7%AC%A6%E4%BD%8D%E7%BD%AE.md)

   - 
```
在一个字符串中找到第一个只出现一次的字符，并返回它的位置。字符串只包含 ASCII 码字符。

两次遍历，统计出现次数+找到第一个。（数组，BitSet更节约空间）
```

```
public int FirstNotRepeatingChar(String str) {
    int[] cnts = new int[128];
    for (int i = 0; i < str.length(); i++)
        cnts[str.charAt(i)]++;
    for (int i = 0; i < str.length(); i++)
        if (cnts[str.charAt(i)] == 1)
            return i;
    return -1;
}

public int FirstNotRepeatingChar2(String str) {
    BitSet bs1 = new BitSet(128);
    BitSet bs2 = new BitSet(128);
    for (char c : str.toCharArray()) {
        if (!bs1.get(c) && !bs2.get(c))
            bs1.set(c);     // 0 0 -> 0 1
        else if (bs1.get(c) && !bs2.get(c))
            bs2.set(c);     // 0 1 -> 1 1
    }
    for (int i = 0; i < str.length(); i++) {
        char c = str.charAt(i);
        if (bs1.get(c) && !bs2.get(c))  // 0 1
            return i;
    }
    return -1;
}
```



## 栈队列堆

- [9. 用两个栈实现队列](https://github.com/CyC2018/CS-Notes/blob/master/notes/9.%20%E7%94%A8%E4%B8%A4%E4%B8%AA%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.md)

   - 
```
用两个栈来实现一个队列，完成队列的 Push 和 Pop 操作。
```



- [30. 包含 min 函数的栈](https://github.com/CyC2018/CS-Notes/blob/master/notes/30.%20%E5%8C%85%E5%90%AB%20min%20%E5%87%BD%E6%95%B0%E7%9A%84%E6%A0%88.md)

   - 
```
实现一个包含 min() 函数的栈，该方法返回当前栈中最小的值。

使用一个额外的 minStack，栈顶元素为当前栈中最小的值。
在对栈进行 push 入栈和 pop 出栈操作时，同样需要对 minStack 进行入栈出栈操作，
从而使 minStack 栈顶元素一直为当前栈中最小的值。
在进行 push 操作时，需要比较入栈元素和当前栈中最小值，将值较小的元素 push 到 minStack 中。
```

```
private Stack<Integer> dataStack = new Stack<>();
private Stack<Integer> minStack = new Stack<>();

public void push(int node) {
    dataStack.push(node);
    minStack.push(minStack.isEmpty() ? node : Math.min(minStack.peek(), node));
}

public void pop() {
    dataStack.pop();
    minStack.pop();
}

public int top() {
    return dataStack.peek();
}

public int min() {
    return minStack.peek();
}
```


- [31. 栈的压入、弹出序列](https://github.com/CyC2018/CS-Notes/blob/master/notes/31.%20%E6%A0%88%E7%9A%84%E5%8E%8B%E5%85%A5%E3%80%81%E5%BC%B9%E5%87%BA%E5%BA%8F%E5%88%97.md)

   - 
```
输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。
假设压入栈的所有数字均不相等。

使用一个栈来模拟压入弹出操作。
每次入栈一个元素后，都要判断一下栈顶元素是不是当前出栈序列 popSequence 的第一个元素，
如果是的话则执行出栈操作并将 popSequence 往后移一位，继续进行判断。
```

```
public class Solution {
  public boolean IsPopOrder(int [] pushA,int [] popA) {
    int num = popA.length;
    Stack<Integer> stack = new Stack<>();
    for(int pushIndex = 0, popIndex = 0; pushIndex < num; pushIndex++){
      stack.push(pushA[pushIndex]);
      while(popIndex < num && !stack.isEmpty() && stack.peek() == popA[popIndex]){
        stack.pop();
        popIndex++;
      }
    }
    return stack.isEmpty();
  }
}
```


- [40. 最小的 K 个数](https://github.com/CyC2018/CS-Notes/blob/master/notes/40.%20%E6%9C%80%E5%B0%8F%E7%9A%84%20K%20%E4%B8%AA%E6%95%B0.md)

   - 
```
给定一个数组，找出其中最小的K个数。

大小为 K 的最小堆
使用大顶堆。在添加一个元素之后，如果大顶堆的大小大于 K，那么将大顶堆的堆顶元素去除
```

```
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        if(input.length < k || k <= 0){
            return new ArrayList();
        }
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((o1, o2) -> o2 - o1);
        for(int a : input){
            maxHeap.add(a);
            if(maxHeap.size() > k){
                maxHeap.poll();
            }
        }
        return new ArrayList(maxHeap);
    }
}
```


- [41.1 数据流中的中位数](https://github.com/CyC2018/CS-Notes/blob/master/notes/41.1%20%E6%95%B0%E6%8D%AE%E6%B5%81%E4%B8%AD%E7%9A%84%E4%B8%AD%E4%BD%8D%E6%95%B0.md)

   - 
```
得到一个数据流中的中位数

大顶堆 + 小顶堆
```

```
public class Solution {
    private PriorityQueue<Integer> small = new PriorityQueue<>((o1, o2) -> o2 - o1);
    private PriorityQueue<Integer> big = new PriorityQueue<>();
    private int n = 0;
    public void Insert(Integer num) {
        if(n % 2 == 0){
            small.add(num);
            big.add(small.poll());
        } else {
            big.add(num);
            small.add(big.poll());
        }
        n++;
    }
    public Double GetMedian() {
        if(n % 2 == 0){
            return (small.peek() + big.peek()) / 2.0;
        } else {
            return (double)big.peek();
        }
    }
}
```


- [41.2 字符流中第一个不重复的字符](https://github.com/CyC2018/CS-Notes/blob/master/notes/41.2%20%E5%AD%97%E7%AC%A6%E6%B5%81%E4%B8%AD%E7%AC%AC%E4%B8%80%E4%B8%AA%E4%B8%8D%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%97%E7%AC%A6.md)

   - 

```
public class Solution {
    private int[] a = new int[128];
    private Queue<Character> queue = new LinkedList<>();
    public void Insert(char ch) {
        queue.add(ch);
        a[ch]++;
        while (!queue.isEmpty() && a[queue.peek()] > 1 ) {
            queue.poll();
        }
    }
    public char FirstAppearingOnce() {
        return queue.isEmpty() ? '#' : queue.peek();
    }
}
```


- [59. 滑动窗口的最大值](https://github.com/CyC2018/CS-Notes/blob/master/notes/59.%20%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E7%9A%84%E6%9C%80%E5%A4%A7%E5%80%BC.md)

   - 
```
给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。

大顶推，移动时添加删除元素
```

```
public class Solution {
    public ArrayList<Integer> maxInWindows(int [] num, int size) {
        ArrayList<Integer> arr = new ArrayList<>();
        if (size > num.length || size < 1) {
            return arr;
        }
        PriorityQueue<Integer> heap = new PriorityQueue<Integer>((o1, o2) -> o2 - o1);
        for (int i = 0; i < size; i++) {
            heap.add(num[i]);
        }  
        arr.add(heap.peek());
        for (int i = 0, j = i + size; j < num.length; i++, j++) {
            heap.remove(num[i]);
            heap.add(num[j]);
            arr.add(heap.peek());
        }   
        return arr;   
    }
}
```



## 双指针

- [57.1 和为 S 的两个数字](https://github.com/CyC2018/CS-Notes/blob/master/notes/57.1%20%E5%92%8C%E4%B8%BA%20S%20%E7%9A%84%E4%B8%A4%E4%B8%AA%E6%95%B0%E5%AD%97.md)

   - 
```
在有序数组中找出两个数，使得和为给定的数 S。如果有多对数字的和等于 S，输出两个数的乘积最小的。

使用双指针，一个指针指向元素较小的值，一个指针指向元素较大的值。
指向较小元素的指针从头向尾遍历，指向较大元素的指针从尾向头遍历。
```



- [57.2 和为 S 的连续正数序列](https://github.com/CyC2018/CS-Notes/blob/master/notes/57.2%20%E5%92%8C%E4%B8%BA%20S%20%E7%9A%84%E8%BF%9E%E7%BB%AD%E6%AD%A3%E6%95%B0%E5%BA%8F%E5%88%97.md)

   - 
```
输出所有和为 S 的连续正数序列。

双指针，从最小开始移动
```

```
public ArrayList<ArrayList<Integer>> FindContinuousSequence(int sum) {
    ArrayList<ArrayList<Integer>> ret = new ArrayList<>();
    int start = 1, end = 2;
    int curSum = 3;
    while (end < sum) {
        if (curSum > sum) {
            curSum -= start;
            start++;
        } else if (curSum < sum) {
            end++;
            curSum += end;
        } else {
            ArrayList<Integer> list = new ArrayList<>();
            for (int i = start; i <= end; i++)
                list.add(i);
            ret.add(list);
            curSum -= start;
            start++;
            end++;
            curSum += end;
        }
    }
    return ret;
}
```


- [58.1 翻转单词顺序列](https://github.com/CyC2018/CS-Notes/blob/master/notes/58.1%20%E7%BF%BB%E8%BD%AC%E5%8D%95%E8%AF%8D%E9%A1%BA%E5%BA%8F%E5%88%97.md)

   - 
```
"I am a student."     "student. a am I"

先翻转每个单词，再翻转整个字符串。
```

```
public class Solution {
    public String ReverseSentence(String str) {
        int num = str.length();
        char[] ch = str.toCharArray();
        for (int i = 0, j = 0; j <= num; j++) {
            if (j == num || ch[j] == ' ') {
                swap(ch, i, j - 1);
                i = j + 1;
                j++;
            }
        }
        swap(ch, 0, num - 1);
        return new String(ch);
    }
    private void swap (char [] c, int i, int j) {
        char t;
        while (i < j) {
            t = c[i];
            c[i] = c[j];
            c[j] = t;
            i++; j--;
        }
    }
}
```


- [58.2 左旋转字符串](https://github.com/CyC2018/CS-Notes/blob/master/notes/58.2%20%E5%B7%A6%E6%97%8B%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.md)

   - 
```
"abcXYZdef"    "XYZdefabc"

先将 "abc" 和 "XYZdef" 分别翻转，得到 "cbafedZYX"，
然后再把整个字符串翻转得到 "XYZdefabc"。
```




## 链表

- [6. 从尾到头打印链表](https://github.com/CyC2018/CS-Notes/blob/master/notes/6.%20%E4%BB%8E%E5%B0%BE%E5%88%B0%E5%A4%B4%E6%89%93%E5%8D%B0%E9%93%BE%E8%A1%A8.md)

   - 
```
头插法，栈
```

```
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        Stack<Integer> stack = new Stack<>();
        ArrayList<Integer> al = new ArrayList<Integer>();
        while (listNode != null) {
            stack.add(listNode.val);
            listNode = listNode.next;
        }
        while (!stack.isEmpty()) {
            al.add(stack.pop());
        }
        return al;
    }
}
```


- [18.1 在 O(1) 时间内删除链表节点](https://github.com/CyC2018/CS-Notes/blob/master/notes/18.1%20%E5%9C%A8%20O(1)%20%E6%97%B6%E9%97%B4%E5%86%85%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E8%8A%82%E7%82%B9.md)

   - 



- [18.2 删除链表中重复的结点](https://github.com/CyC2018/CS-Notes/blob/master/notes/18.2%20%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E4%B8%AD%E9%87%8D%E5%A4%8D%E7%9A%84%E7%BB%93%E7%82%B9.md)

   - 
```
删除该链表中重复（且连续）的结点，重复的结点不保留，返回链表头指针。
```

```
public class Solution {
    public ListNode deleteDuplication(ListNode pHead) {
        if (pHead == null || pHead.next == null)
            return pHead;
        ListNode next = pHead.next;
        if (pHead.val == next.val) {
            while (next != null && pHead.val == next.val)
                next = next.next;
            return deleteDuplication(next);
        } else {
            pHead.next = deleteDuplication(pHead.next);
            return pHead;
        }
    }
}
```


- [22. 链表中倒数第 K 个结点](https://github.com/CyC2018/CS-Notes/blob/master/notes/22.%20%E9%93%BE%E8%A1%A8%E4%B8%AD%E5%80%92%E6%95%B0%E7%AC%AC%20K%20%E4%B8%AA%E7%BB%93%E7%82%B9.md)

   - 
```
双指针；
指针一计数，指针二找到位置
```

```
public class Solution {
    public ListNode FindKthToTail (ListNode pHead, int k) {
        if (pHead == null) {
            return null;
        }
        ListNode head = pHead;
        int num = 1;
        while (head.next != null) {
            head = head.next;
            num++;
        }
        if (k > num) {
            return null;
        } else {
            num -= k;
            head = pHead;
        }
        while (num > 0) {
            head = head.next;
            num--;
        }
        return head;
    }
}
```


- [23. 链表中环的入口结点](https://github.com/CyC2018/CS-Notes/blob/master/notes/23.%20%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%8E%AF%E7%9A%84%E5%85%A5%E5%8F%A3%E7%BB%93%E7%82%B9.md)

   - 

```
public class Solution {
    public ListNode EntryNodeOfLoop(ListNode pHead) {
        if (pHead == null || pHead.next == null) {
            return null;
        }
        ListNode slow = pHead, fast = pHead;
        do {
            fast = fast.next.next;
            slow = slow.next;
        } while (slow != fast);
        ListNode flag = pHead;
        while (slow != flag) {
            slow = slow.next;
            flag = flag.next;
        }
        return flag;

    }
}
```


- [24. 反转链表](https://github.com/CyC2018/CS-Notes/blob/master/notes/24.%20%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8.md)

   - 
```
头插法
```

```
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode newList = new ListNode(0);
        while (head != null) {
            ListNode temp = head.next;
            head.next = newList.next;
            newList.next = head;
            head = temp;
        }
        return newList.next;
    }
}
```


- [25. 合并两个排序的链表](https://github.com/CyC2018/CS-Notes/blob/master/notes/25.%20%E5%90%88%E5%B9%B6%E4%B8%A4%E4%B8%AA%E6%8E%92%E5%BA%8F%E7%9A%84%E9%93%BE%E8%A1%A8.md)

   - 

```
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        ListNode head = new ListNode(0);
        ListNode cur = head;
        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                cur.next = list1;
                list1 = list1.next;
            } else {
                cur.next = list2;
                list2 = list2.next;
            }
            cur = cur.next;
        }
        if (list1 != null)
            cur.next = list1;
        if (list2 != null)
            cur.next = list2;
        return head.next;
    }
}
```


- [35. 复杂链表的复制](https://github.com/CyC2018/CS-Notes/blob/master/notes/35.%20%E5%A4%8D%E6%9D%82%E9%93%BE%E8%A1%A8%E7%9A%84%E5%A4%8D%E5%88%B6.md)

   - 



- [52. 两个链表的第一个公共结点](https://github.com/CyC2018/CS-Notes/blob/master/notes/52.%20%E4%B8%A4%E4%B8%AA%E9%93%BE%E8%A1%A8%E7%9A%84%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%85%AC%E5%85%B1%E7%BB%93%E7%82%B9.md)

   - 
```
当访问链表 A 的指针访问到链表尾部时，令它从链表 B 的头部重新开始访问链表 B；
同样地，当访问链表 B 的指针访问到链表尾部时，令它从链表 A 的头部重新开始访问链表 A。
```

```
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        ListNode l1 = pHead1, l2 = pHead2;
        while (l1 != l2) {
            if (l1 == null) {
                l1 = pHead2;
            } else {
                l1 = l1.next;
            }
            if (l2 == null) {
                l2 = pHead1;
            } else {
                l2 = l2.next;
            }
        }
        return l1;
    }
}
```



## 树

- [7. 重建二叉树](https://github.com/CyC2018/CS-Notes/blob/master/notes/7.%20%E9%87%8D%E5%BB%BA%E4%BA%8C%E5%8F%89%E6%A0%91.md)

   - 
```
根据二叉树的前序遍历和中序遍历的结果，重建出该二叉树。
假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

前序遍历的第一个值为根节点的值，使用这个值将中序遍历结果分成两部分，
左部分为树的左子树中序遍历结果，右部分为树的右子树中序遍历的结果。
然后分别对左右子树递归地求解。
```



- [8. 二叉树的下一个结点](https://github.com/CyC2018/CS-Notes/blob/master/notes/8.%20%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E4%B8%8B%E4%B8%80%E4%B8%AA%E7%BB%93%E7%82%B9.md)

   - 
```
给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回 

如果一个节点的右子树不为空，那么该节点的下一个节点是右子树的最左节点；
 否则，向上找第一个左链接指向的树包含该节点的祖先节点。
```

```
public class Solution {
    public TreeLinkNode GetNext(TreeLinkNode pNode) {
        TreeLinkNode node = null;
        if (pNode.right != null) {
            node = pNode.right;
            while (node.left != null)
                node = node.left;
            return node;
        } else {
            while (pNode.next != null) {
                TreeLinkNode parent = pNode.next;
                if (parent.left == pNode)
                    return parent;
                pNode = pNode.next;
            }
        }
        return null;
    }
}
```


- [26. 树的子结构](https://github.com/CyC2018/CS-Notes/blob/master/notes/26.%20%E6%A0%91%E7%9A%84%E5%AD%90%E7%BB%93%E6%9E%84.md)

   - 
```
输入两棵二叉树A，B，判断B是不是A的子结构。

递归
```

```
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        if (root1 == null || root2 == null)
            return false;
        return judge(root1, root2) || HasSubtree(root1.left, root2) || HasSubtree(root1.right, root2);

    }

    public boolean judge(TreeNode root1,TreeNode root2) {
        if (root2 == null)
            return true;
        if (root1 == null)
            return false;
        if (root1.val != root2.val)
            return false;
        return judge(root1.left, root2.left) && judge(root1.right, root2.right);
    }
}
```


- [27. 二叉树的镜像](https://github.com/CyC2018/CS-Notes/blob/master/notes/27.%20%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%95%9C%E5%83%8F.md)

   - 
```
操作给定的二叉树，将其变换为源二叉树的镜像。

递归
```

```
public class Solution {
    public TreeNode Mirror (TreeNode pRoot) {
        if (pRoot == null) {
            return pRoot;
        }
        TreeNode temp = pRoot.left;
        pRoot.left = pRoot.right;
        pRoot.right = temp;

        Mirror(pRoot.left);
        Mirror(pRoot.right);

        return pRoot;
    }
}
```


- [28. 对称的二叉树](https://github.com/CyC2018/CS-Notes/blob/master/notes/28.%20%E5%AF%B9%E7%A7%B0%E7%9A%84%E4%BA%8C%E5%8F%89%E6%A0%91.md)

   - 
```
请实现一个函数，用来判断一棵二叉树是不是对称的。
注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。
```

```
public class Solution {
    boolean isSymmetrical(TreeNode pRoot) {
        if (pRoot == null)
            return true;
        return isSymmetrical(pRoot.left, pRoot.right);
    }

    boolean isSymmetrical(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null)
            return true;
        if (t1 == null || t2 == null)
            return false;
        if (t1.val != t2.val)
            return false;
        return isSymmetrical(t1.left, t2.right) && isSymmetrical(t1.right, t2.left);
    }
}
```


- [32.1 从上往下打印二叉树](https://github.com/CyC2018/CS-Notes/blob/master/notes/32.1%20%E4%BB%8E%E4%B8%8A%E5%BE%80%E4%B8%8B%E6%89%93%E5%8D%B0%E4%BA%8C%E5%8F%89%E6%A0%91.md)

   - 
```
队列
```

```
public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
    Queue<TreeNode> queue = new LinkedList<>();
    ArrayList<Integer> ret = new ArrayList<>();
    queue.add(root);
    while (!queue.isEmpty()) {
        int cnt = queue.size();
        while (cnt-- > 0) {
            TreeNode t = queue.poll();
            if (t == null)
                continue;
            ret.add(t.val);
            queue.add(t.left);
            queue.add(t.right);
        }
    }
    return ret;
}
```


- [32.2 把二叉树打印成多行](https://github.com/CyC2018/CS-Notes/blob/master/notes/32.2%20%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%A0%91%E6%89%93%E5%8D%B0%E6%88%90%E5%A4%9A%E8%A1%8C.md)

   - 

```
ArrayList<ArrayList<Integer>> Print(TreeNode pRoot) {
    ArrayList<ArrayList<Integer>> ret = new ArrayList<>();
    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(pRoot);
    while (!queue.isEmpty()) {
        ArrayList<Integer> list = new ArrayList<>();
        int cnt = queue.size();
        while (cnt-- > 0) {
            TreeNode node = queue.poll();
            if (node == null)
                continue;
            list.add(node.val);
            queue.add(node.left);
            queue.add(node.right);
        }
        if (list.size() != 0)
            ret.add(list);
    }
    return ret;
}
```


- [32.3 按之字形顺序打印二叉树](https://github.com/CyC2018/CS-Notes/blob/master/notes/32.3%20%E6%8C%89%E4%B9%8B%E5%AD%97%E5%BD%A2%E9%A1%BA%E5%BA%8F%E6%89%93%E5%8D%B0%E4%BA%8C%E5%8F%89%E6%A0%91.md)

   - 



- [33. 二叉搜索树的后序遍历序列](https://github.com/CyC2018/CS-Notes/blob/master/notes/33.%20%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97.md)

   - 



- [34. 二叉树中和为某一值的路径](https://github.com/CyC2018/CS-Notes/blob/master/notes/34.%20%E4%BA%8C%E5%8F%89%E6%A0%91%E4%B8%AD%E5%92%8C%E4%B8%BA%E6%9F%90%E4%B8%80%E5%80%BC%E7%9A%84%E8%B7%AF%E5%BE%84.md)

   - 

```
private ArrayList<ArrayList<Integer>> ret = new ArrayList<>();

public ArrayList<ArrayList<Integer>> FindPath(TreeNode root, int target) {
    backtracking(root, target, new ArrayList<>());
    return ret;
}

private void backtracking(TreeNode node, int target, ArrayList<Integer> path) {
    if (node == null)
        return;
    path.add(node.val);
    target -= node.val;
    if (target == 0 && node.left == null && node.right == null) {
        ret.add(new ArrayList<>(path));
    } else {
        backtracking(node.left, target, path);
        backtracking(node.right, target, path);
    }
    path.remove(path.size() - 1);
}
```


- [36. 二叉搜索树与双向链表](https://github.com/CyC2018/CS-Notes/blob/master/notes/36.%20%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%8E%E5%8F%8C%E5%90%91%E9%93%BE%E8%A1%A8.md)

   - 

```
private TreeNode pre = null;
private TreeNode head = null;

public TreeNode Convert(TreeNode root) {
    inOrder(root);
    return head;
}

private void inOrder(TreeNode node) {
    if (node == null)
        return;
    inOrder(node.left);
    node.left = pre;
    if (pre != null)
        pre.right = node;
    pre = node;
    if (head == null)
        head = node;
    inOrder(node.right);
}
```


- [37. 序列化二叉树](https://github.com/CyC2018/CS-Notes/blob/master/notes/37.%20%E5%BA%8F%E5%88%97%E5%8C%96%E4%BA%8C%E5%8F%89%E6%A0%91.md)

   - 



- [54. 二叉查找树的第 K 个结点](https://github.com/CyC2018/CS-Notes/blob/master/notes/54.%20%E4%BA%8C%E5%8F%89%E6%9F%A5%E6%89%BE%E6%A0%91%E7%9A%84%E7%AC%AC%20K%20%E4%B8%AA%E7%BB%93%E7%82%B9.md)

   - 



- [55.1 二叉树的深度](https://github.com/CyC2018/CS-Notes/blob/master/notes/55.1%20%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%B7%B1%E5%BA%A6.md)

   - 

```
public class Solution {
    public int TreeDepth(TreeNode root) {
         return root == null ? 0 : 1 + Math.max(TreeDepth(root.left), TreeDepth(root.right));
    }
}
```


- [55.2 平衡二叉树](https://github.com/CyC2018/CS-Notes/blob/master/notes/55.2%20%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.md)

   - 



- [68. 树中两个节点的最低公共祖先](https://github.com/CyC2018/CS-Notes/blob/master/notes/68.%20%E6%A0%91%E4%B8%AD%E4%B8%A4%E4%B8%AA%E8%8A%82%E7%82%B9%E7%9A%84%E6%9C%80%E4%BD%8E%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.md)

   - 

```
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null)
        return root;
    if (root.val > p.val && root.val > q.val)
        return lowestCommonAncestor(root.left, p, q);
    if (root.val < p.val && root.val < q.val)
        return lowestCommonAncestor(root.right, p, q);
    return root;
}

public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q)
        return root;
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    return left == null ? right : right == null ? left : root;
}
```



## 贪心思想

- [14. 剪绳子](https://github.com/CyC2018/CS-Notes/blob/master/notes/14.%20%E5%89%AA%E7%BB%B3%E5%AD%90.md)

   - 



- [63. 股票的最大利润](https://github.com/CyC2018/CS-Notes/blob/master/notes/63.%20%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E5%A4%A7%E5%88%A9%E6%B6%A6.md)；最大利润和最小价格不是对应的

   - 




## 二分查找

- [11. 旋转数组的最小数字](https://github.com/CyC2018/CS-Notes/blob/master/notes/11.%20%E6%97%8B%E8%BD%AC%E6%95%B0%E7%BB%84%E7%9A%84%E6%9C%80%E5%B0%8F%E6%95%B0%E5%AD%97.md)

   - 



- [53. 数字在排序数组中出现的次数](https://github.com/CyC2018/CS-Notes/blob/master/notes/53.%20%E6%95%B0%E5%AD%97%E5%9C%A8%E6%8E%92%E5%BA%8F%E6%95%B0%E7%BB%84%E4%B8%AD%E5%87%BA%E7%8E%B0%E7%9A%84%E6%AC%A1%E6%95%B0.md)

   - 




## 分治

- [16. 数值的整数次方](https://github.com/CyC2018/CS-Notes/blob/master/notes/16.%20%E6%95%B0%E5%80%BC%E7%9A%84%E6%95%B4%E6%95%B0%E6%AC%A1%E6%96%B9.md)

   - 




## 搜索

- [12. 矩阵中的路径](https://github.com/CyC2018/CS-Notes/blob/master/notes/12.%20%E7%9F%A9%E9%98%B5%E4%B8%AD%E7%9A%84%E8%B7%AF%E5%BE%84.md)

   - 



- [13. 机器人的运动范围](https://github.com/CyC2018/CS-Notes/blob/master/notes/13.%20%E6%9C%BA%E5%99%A8%E4%BA%BA%E7%9A%84%E8%BF%90%E5%8A%A8%E8%8C%83%E5%9B%B4.md)

   - 



- [38. 字符串的排列](https://github.com/CyC2018/CS-Notes/blob/master/notes/38.%20%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E6%8E%92%E5%88%97.md)

   - 




## 排序

- [21. 调整数组顺序使奇数位于偶数前面](https://github.com/CyC2018/CS-Notes/blob/master/notes/21.%20%E8%B0%83%E6%95%B4%E6%95%B0%E7%BB%84%E9%A1%BA%E5%BA%8F%E4%BD%BF%E5%A5%87%E6%95%B0%E4%BD%8D%E4%BA%8E%E5%81%B6%E6%95%B0%E5%89%8D%E9%9D%A2.md)

   - 



- [45. 把数组排成最小的数](https://github.com/CyC2018/CS-Notes/blob/master/notes/45.%20%E6%8A%8A%E6%95%B0%E7%BB%84%E6%8E%92%E6%88%90%E6%9C%80%E5%B0%8F%E7%9A%84%E6%95%B0.md)

   - 



- [51. 数组中的逆序对](https://github.com/CyC2018/CS-Notes/blob/master/notes/51.%20%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E9%80%86%E5%BA%8F%E5%AF%B9.md)

   - 




## 动态规划

- [10.1 斐波那契数列](https://github.com/CyC2018/CS-Notes/blob/master/notes/10.1%20%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E5%88%97.md)

   - 



- [10.2 矩形覆盖](https://github.com/CyC2018/CS-Notes/blob/master/notes/10.2%20%E7%9F%A9%E5%BD%A2%E8%A6%86%E7%9B%96.md)

   - 



- [10.3 跳台阶](https://github.com/CyC2018/CS-Notes/blob/master/notes/10.3%20%E8%B7%B3%E5%8F%B0%E9%98%B6.md)

   - 



- [10.4 变态跳台阶](https://github.com/CyC2018/CS-Notes/blob/master/notes/10.4%20%E5%8F%98%E6%80%81%E8%B7%B3%E5%8F%B0%E9%98%B6.md)

   - 



- [42. 连续子数组的最大和](https://github.com/CyC2018/CS-Notes/blob/master/notes/42.%20%E8%BF%9E%E7%BB%AD%E5%AD%90%E6%95%B0%E7%BB%84%E7%9A%84%E6%9C%80%E5%A4%A7%E5%92%8C.md)

   - 



- [47. 礼物的最大价值](https://github.com/CyC2018/CS-Notes/blob/master/notes/47.%20%E7%A4%BC%E7%89%A9%E7%9A%84%E6%9C%80%E5%A4%A7%E4%BB%B7%E5%80%BC.md)

   - 



- [48. 最长不含重复字符的子字符串](https://github.com/CyC2018/CS-Notes/blob/master/notes/48.%20%E6%9C%80%E9%95%BF%E4%B8%8D%E5%90%AB%E9%87%8D%E5%A4%8D%E5%AD%97%E7%AC%A6%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.md)

   - 



- [49. 丑数](https://github.com/CyC2018/CS-Notes/blob/master/notes/49.%20%E4%B8%91%E6%95%B0.md)

   - 



- [60. n 个骰子的点数](https://github.com/CyC2018/CS-Notes/blob/master/notes/60.%20n%20%E4%B8%AA%E9%AA%B0%E5%AD%90%E7%9A%84%E7%82%B9%E6%95%B0.md)

   - 



- [66. 构建乘积数组](https://github.com/CyC2018/CS-Notes/blob/master/notes/66.%20%E6%9E%84%E5%BB%BA%E4%B9%98%E7%A7%AF%E6%95%B0%E7%BB%84.md)

   - 




## 数学

- [39. 数组中出现次数超过一半的数字](https://github.com/CyC2018/CS-Notes/blob/master/notes/39.%20%E6%95%B0%E7%BB%84%E4%B8%AD%E5%87%BA%E7%8E%B0%E6%AC%A1%E6%95%B0%E8%B6%85%E8%BF%87%E4%B8%80%E5%8D%8A%E7%9A%84%E6%95%B0%E5%AD%97.md)

   - 



- [62. 圆圈中最后剩下的数](https://github.com/CyC2018/CS-Notes/blob/master/notes/62.%20%E5%9C%86%E5%9C%88%E4%B8%AD%E6%9C%80%E5%90%8E%E5%89%A9%E4%B8%8B%E7%9A%84%E6%95%B0.md)

   - 



- [43. 从 1 到 n 整数中 1 出现的次数](https://github.com/CyC2018/CS-Notes/blob/master/notes/43.%20%E4%BB%8E%201%20%E5%88%B0%20n%20%E6%95%B4%E6%95%B0%E4%B8%AD%201%20%E5%87%BA%E7%8E%B0%E7%9A%84%E6%AC%A1%E6%95%B0.md)

   - 




## 位运算

- [15. 二进制中 1 的个数](https://github.com/CyC2018/CS-Notes/blob/master/notes/15.%20%E4%BA%8C%E8%BF%9B%E5%88%B6%E4%B8%AD%201%20%E7%9A%84%E4%B8%AA%E6%95%B0.md)

   - 



- [56. 数组中只出现一次的数字](https://github.com/CyC2018/CS-Notes/blob/master/notes/56.%20%E6%95%B0%E7%BB%84%E4%B8%AD%E5%8F%AA%E5%87%BA%E7%8E%B0%E4%B8%80%E6%AC%A1%E7%9A%84%E6%95%B0%E5%AD%97.md)；位运算得到“判断用的值”，将原数组分层两堆，每堆各自异或

   - 




## 其它

- [17. 打印从 1 到最大的 n 位数](https://github.com/CyC2018/CS-Notes/blob/master/notes/17.%20%E6%89%93%E5%8D%B0%E4%BB%8E%201%20%E5%88%B0%E6%9C%80%E5%A4%A7%E7%9A%84%20n%20%E4%BD%8D%E6%95%B0.md)

   - 



- [19. 正则表达式匹配](https://github.com/CyC2018/CS-Notes/blob/master/notes/19.%20%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%8C%B9%E9%85%8D.md)

   - 



- [20. 表示数值的字符串](https://github.com/CyC2018/CS-Notes/blob/master/notes/20.%20%E8%A1%A8%E7%A4%BA%E6%95%B0%E5%80%BC%E7%9A%84%E5%AD%97%E7%AC%A6%E4%B8%B2.md)

   - 



- [44. 数字序列中的某一位数字](https://github.com/CyC2018/CS-Notes/blob/master/notes/44.%20%E6%95%B0%E5%AD%97%E5%BA%8F%E5%88%97%E4%B8%AD%E7%9A%84%E6%9F%90%E4%B8%80%E4%BD%8D%E6%95%B0%E5%AD%97.md)

   - 



- [46. 把数字翻译成字符串](https://github.com/CyC2018/CS-Notes/blob/master/notes/46.%20%E6%8A%8A%E6%95%B0%E5%AD%97%E7%BF%BB%E8%AF%91%E6%88%90%E5%AD%97%E7%AC%A6%E4%B8%B2.md)

   - 



- [61. 扑克牌顺子](https://github.com/CyC2018/CS-Notes/blob/master/notes/61.%20%E6%89%91%E5%85%8B%E7%89%8C%E9%A1%BA%E5%AD%90.md)

   - 



- [64. 求 1+2+3+...+n](https://github.com/CyC2018/CS-Notes/blob/master/notes/64.%20%E6%B1%82%201+2+3+...+n.md)

   - 



- [65. 不用加减乘除做加法](https://github.com/CyC2018/CS-Notes/blob/master/notes/65.%20%E4%B8%8D%E7%94%A8%E5%8A%A0%E5%87%8F%E4%B9%98%E9%99%A4%E5%81%9A%E5%8A%A0%E6%B3%95.md)

   - 



- [67. 把字符串转换成整数](https://github.com/CyC2018/CS-Notes/blob/master/notes/67.%20%E6%8A%8A%E5%AD%97%E7%AC%A6%E4%B8%B2%E8%BD%AC%E6%8D%A2%E6%88%90%E6%95%B4%E6%95%B0.md)

   - 



