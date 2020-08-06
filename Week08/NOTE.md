学习笔记
-----------------

## 位运算

### 操作符
1. **按位与&: 均1为1，否则为0；**

按位与运算通常用来对某些位清0或保留某些位。例如把a 的高八位清 0 ， 保留低八位， 可作 a&255 运算 ( 255 的二进制数为0000000011111111)。

2. **按位或|: 有1为1，否则为0；**

3. **按位异或^: 异为1，同为0；**

4. 取反~：按位求反；

[负数的二进制表示方法（正数：原码、负数：补码） - andy_0212 - 博客园](https://www.cnblogs.com/andy-0212/p/10323502.html)
> **负数的二进制表示：以正数的补码来表示，也就是正数取反加一**。       -a = (~a + 1)

5. 左移<<：高位丢弃，低位补0；

6. 右移>>：对于有符号数，在右移时，符号位将随同移动。当为正数时， 最高位补0，而为负数时，符号位为1，最高位是补0或是补1 取决于编译系统的规定。

7. 无符号右移>>>:（java特有，高位总是补0）


### n & (n-1) 的巧妙运用

[位运算n & (n-1)的妙用_cfanr-CSDN博客_要检索整数的最右边的位](https://blog.csdn.net/navyifanr/article/details/19496459)


1. 判断一个数是否是2的方幂
a. 利用 n & (n-1) 对 n 最末位 1 置 0 的作用：(n != 0) && ((n & (n - 1)) == 0)
b. 利用 n & -n 只保留 n 最末位 1 的作用： (n != 0) && (n & (-n)) = n  

2. 求某一个数的二进制表示中1的个数
```
while (n >0 ) {
      count ++;
      n &= (n-1);
}
```

3.计算N!的质因数2的个数 = N!的质因数2的个数为 N-(N二进制表示中1的个数)



## 排序

### 初级排序 - O(n^2)

1. 选择排序(Selection Sort) 每次找最小值，然后放到待排序数组的起始位置。
```
public static void selectSort(int[] data) {
    int len = data.length;
    int min;
    int k = 0;
    for (int i = 0; i < len - 1; i++) {
        min = data[i];
        k = i;
        for (int j = i + 1; j < len; j++) {
            if (min > data[j]) {
                k = j;
                min = data[j];
            }
        }
        if (k != i) {         //减少频繁交换
            int temp = data[i];
            data[i] = min;
            data[k] = temp;
        }
    }
}
```


2. 插入排序(Insertion Sort) 从前到后逐步构建有序序列;对于未排序数据，在已排序序列中从后 向前扫描，找到相应位置并插入。
```
public void insertSort(int[] data) {
    int len = data.length;
    int temp = 0;
    for (int i = 0; i < len; i++) {
        temp = data[i];   //当前要排序的值
        int j = i - 1;    //要排的数的前一位
        while (j > 0 && temp < data[j]) {  //找到比 temp 大的数，将该数后移一位，并一直循环往前找到合适的位置
            data[j + 1] = data[j];
            j--;
        }
        //向后回溯一位，放置 temp
        data[j + 1] = temp;
    }
}
```

3. 冒泡排序(Bubble Sort) 嵌套循环，每次查看相邻的元素如果逆序，则交换。
```
public void bubbleSort(int[] data) {
    int len = data.length;
    for (int i = 0; i < len - 1; i++) {           //注意减一
        for (int j = 0; j < len - 1 - i; j++) {
            if (data[j] > data[j + 1]) {
                int tmp = data[j];
                data[j] = data[j + 1];
                data[j + 1] = tmp;
            }
        }
    }
}
```

### 高级排序 - O(N*LogN)

- 快速排序(Quick Sort)
数组取标杆 pivot，将小元素放 pivot左边，大元素放右侧，然后依次 对右边和右边的子数组继续快排;以达到整个序列有序。
```
void quickSort(int[] arr, int start, int end) {
	int key = arr[start];
	int i = start;
	int j = end;
	while (i < j) {	
		while (arr[j] > key && j > i) {
			j--;
		}
		arr[i] = arr[j];
		
		while (arr[i] < key && i < j) {
			i++;
		}
		arr[j] = arr[i];
	}
	arr[i] = key;
	if (i - 1 > start) {
		quickSort(arr, start, i - 1);
	}
	if (i + 1 < end) {
		quickSort(arr, i + 1, end);
	}
}
```

- 归并排序(Merge Sort)— 分治
1. 把长度为n的输入序列分成两个长度为n/2的子序列; 
2. 对这两个子序列分别采用归并排序;
3. 将两个排序好的子序列合并成一个最终的排序序列。

和快排区别：
> 归并:先排序左右子数组，然后合并两个有序子数组 
快排:先调配出左右子数组，然后对于左右子数组进行排序

```
public static void mergeSort(int[] data, int left, int right) {
    if (right <= left) {
        return;
    }
    int mid = (left + right) >> 1;

    mergeSort(data, left, mid);
    mergeSort(data, mid + 1, right);
    merge(data, left, mid, right);
}

private static void merge(int[] data, int left, int mid, int right) {
    int[] temp = new int[right - left + 1];
    int i = left, j = mid + 1, k = 0;
    while (i <= mid || j <= right) {
        if (j > right) {
            temp[k++] = data[i++];
            continue;    //注意是 continue, 不是 break
        }
        if (i > mid) {
            temp[k++] = data[j++];
            continue;
        }
        temp[k++] = data[i] <= data[j] ? data[i++] : data[j++];
    }

    for (int p = 0; p < temp.length; p++) {
        data[left + p] = temp[p];
    }
}
```

- 堆排序(Heap Sort) — 堆插入 O(logN)，取最大/小值 O(1)

1. 数组元素依次建立小顶堆 
2. 依次取堆顶元素，并删除

### 特殊排序 - O(n)

- 计数排序(Counting Sort) 
> 计数排序要求输入的数据必须是有确定范围的整数。将输入的数据值转化为键存 储在额外开辟的数组空间中;然后依次把计数大于 1 的填充回原数组

- 桶排序(Bucket Sort)
> 桶排序 (Bucket sort)的工作的原理:假设输入数据服从均匀分布，将数据分到有 限数量的桶里，每个桶再分别排序(有可能再使用别的排序算法或是以递归方式 继续使用桶排序进行排)。

- 基数排序(Radix Sort) 
> 基数排序是按照低位先排序，然后收集;再按照高位排序，然后再收集;依次类 推，直到最高位。有时候有些属性是有优先级顺序的，先按低优先级排序，再按 高优先级排序。