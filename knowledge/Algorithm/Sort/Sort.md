### 一、概述
1. **分类**
> 分为5大类：
	1. 插入排序（直接插入排序、希尔排序）。
	2. 交换排序（冒泡排序、快速排序）。
	3. 选择排序（直接选择排序、堆排序）。
	4. 归并排序。
	5. 分配排序（箱排序、基数排序）。
2. **优点和缺点**
>所需辅助空间最多：归并排序
所需辅助空间最少：堆排序
平均速度最快：快速排序
不稳定：快速排序，希尔排序，堆排序。

![](images/Pasted%20image%2020230114162140.png)
### 二、插入排序
#### 直接插入排序
```
直接插入排序是指将一列乱序的数字逐个插入到队列中，在插入时小的放前面，大的放后面（指正序，逆序思路相反）。
实现过程如下：
1. 建立一个临时变量（也叫哨兵），将即将参与排序的数字赋值给它。
2. 从被插入的数组后面往前比较，若大于哨兵，则记录下标，从记录处后面的元素都向后移动，直到遇到比哨兵小的数。
3. 将哨兵放置到遇到的第一个比哨兵小的数的后面。
```

![](images/Pasted%20image%2020230114163007.png)

```java
public static void main(String[] args) {  
    int[] arr = new int[]{6, 8, 9, 2, 3, 5, 4, 7, 1};  
    for (int i : arr) {  
        System.out.print(i + " ");  
    }  
    System.out.println();  
    for (int i = 1; i < arr.length; i++) {  
        int temp = arr[i];//哨兵  
        for (int j = i - 1; j >= 0; j--) {  
            if (temp < arr[j]) {  
                arr[j + 1] = arr[j];  
                arr[j] = temp;  
            }  
        }  
    }  
    for (int i : arr) {  
        System.out.print(i + " ");  
    }  
}
```

#### 希尔排序

```
希尔排序是把记录按下标的一定增量分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至1时，整个文件恰被分成一组，算法便终止
```
![](images/Pasted%20image%2020230115150021.png)
```java
public static void main(String[] args) {  
    int[] nums = {8, 9, 1, 7, 2, 3, 5, 4, 6};  
    for (int num : nums) {  
        System.out.print(num + " ");  
    }  
    System.out.println();  
    for (int step = nums.length / 2; step > 0; step /= 2) {  
        for (int start = 0; start < step; start++) {  
            for (int i = start + step; i < nums.length; i += step) {  
                int temp = nums[i];  
                for (int j = i - step; j >= 0; j -= step) {  
                    if (temp < nums[j]) {  
                        nums[j + step] = nums[j];  
                        nums[j] = temp;  
                    }  
                }  
            }  
        }  
    }  
    for (int num : nums) {  
        System.out.print(num + " ");  
    }  
}
```

### 三、交换排序
#### 冒泡排序
```
它重复地走访过要排序的元素列，依次比较两个相邻的元素，如果顺序（如从大到小、首字母从Z到A）错误就把他们交换过来。走访元素的工作是重复地进行，直到没有相邻元素需要交换，也就是说该元素列已经排序完成。
```
![](images/Pasted%20image%2020230115150640.png)
```java
public static void main(String[] args) {  
    int[] nums = new int[]{6, 8, 9, 2, 3, 5, 4, 7, 1};  
    for (int num : nums) {  
        System.out.print(num + " ");  
    }  
    System.out.println();  
    for (int i = 0; i < nums.length; i++) {  
        for (int j = 0; j < nums.length - i - 1; j++) {  
            if(nums[j] > nums[j+1]){  
                int temp = nums[j];  
                nums[j] = nums[j + 1];  
                nums[j+1] = temp;  
            }  
        }  
    }  
    for (int num : nums) {  
        System.out.print(num + " ");  
    }  
}
```

#### 快速排序

```
快速排序(Quick Sort)使用分治法策略。  
它的基本思想是：选择一个基准数，通过一趟排序将要排序的数据分割成独立的两部分；其中一部分的所有数据都比另外一部分的所有数据都要小。然后，再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

快速排序流程：  
(1) 从数列中挑出一个基准值。  
(2) 将所有比基准值小的摆放在基准前面，所有比基准值大的摆在基准的后面(相同的数可以到任一边)；在这个分区退出之后，该基准就处于数列的中间位置。  
(3) 递归地把"基准值前面的子数列"和"基准值后面的子数列"进行排序。
```
![](images/Pasted%20image%2020230115162053.png)
```java
public static void main(String[] args) {  
    int[] nums = new int[]{72, 6, 57, 88, 60, 42, 83, 73, 48, 85};  
    QuickSort quickSort = new QuickSort();  
    //quickSort.AdjustArray(nums, 0, nums.length - 1);  
    quickSort.quickSort(nums, 0, nums.length - 1);  
    for (int num : nums) {  
        System.out.print(num + " ");  
    }  
}  
  
public int AdjustArray(int nums[], int l, int r) {  
    int i = l, j = r;  
    int x = nums[i];  
    while (i < j) {  
        while (i < j && x < nums[j])  
            j--;  
        if (i < j)  
            nums[i++] = nums[j];  
        while (i < j && x > nums[i])  
            i++;  
        if (i < j)  
            nums[j--] = nums[i];  
    }  
    nums[i] = x;  
    return i;  
}  
  
public void quickSort(int nums[], int l, int r) {  
    if (l < r) {  
        int x = AdjustArray(nums, l, r);  
        quickSort(nums, l, x - 1);  
        quickSort(nums, x + 1, r);  
    }  
}
```
### 四、选择排序
#### 直接选择排序
```
具体的排序过程为：
1.将整个记录序列划分为有序区和无序区，初始时有序区为空，无序区含有待排序的所有记录
2.在无序区选择关键码最小的记录，将其与无序区中的第一个元，使得有序区扩展一个记录，同时无序区减少了一个记录
3.不断重复步骤 2，直到无序区只剩下一个记录为止
```
![](images/Pasted%20image%2020230115162702.png)
```java
public static void main(String[] args) {  
    int[] nums = new int[]{6, 8, 9, 2, 3, 5, 4, 7, 1};  
    for (int i = 0; i < nums.length; i++) {  
        int minIndex = i;  
        for (int j = i; j < nums.length; j++) {  
            if(nums[j] < nums[minIndex]){  
                minIndex = j;  
            }  
        }  
        if(minIndex != i){  
            int temp = nums[i];  
            nums[i] = nums[minIndex];  
            nums[minIndex] = temp;  
        }  
    }  
    for (int num : nums) {  
        System.out.print(num + " ");  
    }  
}
```

#### 堆排序
1. 将待排序的序列构造成一个大顶堆（升序大顶堆降序小顶堆）
![](images/Pasted%20image%2020230122153325.png)
2. 将堆顶元素（根结点）和末尾元素进行互换。然后将剩余n-1个元素重新构造一个新的大顶堆
![](images/Pasted%20image%2020230122153343.png)
3. 重复进行第2步操作便能得到一个有序的序列

```java
public class HeapSort {  
    public static void main(String[] args) {  
        //int[] nums = {8, 9, 1, 7, 2, 3, 5, 4, 6};  
        int[] nums = {4, 6, 1, 2, 9, 8, 3, 5};  
        HeapSort heapSort = new HeapSort();  
        heapSort.heapSort(nums);  
        for (int num : nums) {  
            System.out.print(num + " ");  
        }  
    }  
  
    public void heapSort(int[] nums) {  
        // 调整为大顶堆  
        for (int i = nums.length / 2 - 1; i >= 0; i--) {  
            adjustSort(nums, i, nums.length);  
        }  
  
        // 取出第一个值，再调整大顶堆  
        for (int i = nums.length - 1; i > 0; i--) {  
            int temp = nums[0];  
            nums[0] = nums[i];  
            nums[i] = temp;  
  
            adjustSort(nums, 0, i);  
        }  
    }  
  
    /**  
     * 构建大顶堆  
     * 这个方法并不是将整个树调整成大顶堆  
     * 而是以i对应的非叶子结点的子树调整成大顶堆  
     *  
     * @param nums  
     * @param i      非叶子结点在数组中的索引(下标)  
     * @param length 进行调整的元素的个数，length是在逐渐减少的  
     */  
    public void adjustSort(int[] nums, int i, int length) {  
        int temp = nums[i];//当前的值  
        for (int j = 2 * i + 1; j < length; j = 2 * j + 1) {  
            if (j + 1 < length && nums[j] < nums[j + 1]) {//左节点小于右节点  
                j++;  
            }  
            if (nums[j] > temp) {  
                nums[i] = nums[j];  
                i = j;  
            } else {  
                break;  
            }  
        }  
        nums[i] = temp;  
    }  
}
```
### 五、归并排序
