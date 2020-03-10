# 排序算法

基础排序算法： 冒泡排序、选择排序、**插入排序**、**归并排序**、希尔排序、**快速排序**、**堆排序**

![](http://pelhans.com/img/in-post/alg/sort_summary.jpg)

### 1. 冒泡排序

代码：
```c++
/*
    1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
    2. 对第0个到第n-1个数据做同样的工作。这时，最大的数就“浮”到了数组最后的位置上。
    3. 针对所有的元素重复以上的步骤，除了最后已经选出的元素（有序）。
    4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较.
*/

// 稳定排序，平均 O(n**2)，最好 O(n), 最差 O(n**2),辅助空间 O(1)

void bubble_sort(vector<int> &nums)
{
    int n = nums.size();
    if (n==0) return;
    for (int i=0;i<n;i++)
    {
        for (int j=0;j<n-1-i;j++)
        {
            if (nums[j] > nums[j+1])
            {
                // swap(nums[j], nums[j+1]);
                int temp = nums[j];
                nums[j] = nums[j+1];
                nums[j+1] = temp;
            }
        }
}
```

### 2. 选择排序

![](https://www.runoob.com/wp-content/uploads/2019/03/selectionSort.gif)

代码：
```c++
/*
    1. 在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。
    2. 再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
    3. 以此类推，直到所有元素均排序完毕。
*/

// 不稳定排序，平均 O(n**2)，最好 O(n**2), 最差 O(n**2),辅助空间 O(1)

void select_sort(vector<int> &nums)
{
    int n = nums.size();
    if (n==0) return;
    for (int i=0;i<n-1;i++)
    {
        int idx = i; //每一趟循环比较时，idx用于存放较小元素的数组下标，这样当前批次比较完毕最终存放的就是此趟内最小的元素的下标，避免每次遇到较小元素都要进行交换。
        for (int j=i+1;j<n;j++)
        {
            if (nums[idx] > nums[j]);
            {
                idx = j;
            }
            if (idx !=i)
            {
                int temp = nums[idx];
                nums[idx] = nums[i];
                nums[i] = temp;
            }
        }
}
```

### 3. 插入排序（重要）

![](https://www.runoob.com/wp-content/uploads/2019/03/insertionSort.gif)

代码：
```c++
/*
   直接插入排序基本思想是每一步将一个待排序的记录，插入到前面已经排好序的有序序列中去，直到插完所有元素为止。
   1. 从第一个元素开始，该元素可以认为已经被排序
   2. 取出下一个元素，在已经排序的元素序列中从后向前扫描
   3. 如果被扫描的元素（已排序）大于新元素，将该元素后移一位
   4. 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置
   5. 将新元素插入到该位置后
   6. 重复步骤2~5
*/

// 稳定排序，平均 O(n**2)，最好 O(n), 最差 O(n**2),辅助空间 O(1)

void insert_sort(vector<int> &nums)
{
    int n = nums.size();
    if (n==0) return;
    // 从下标为1的元素开始选择合适的位置插入，因为下标为0的只有一个元素，默认是有序的
    for (int i=1;i<n;i++)
    {
          // 记录要插入的数据
          int temp  = nums[i];
          int j = i- 1;
          //与已排序的数逐一比较，大于temp时，该数移后
          while (j>=0) && (temp < nums[j])
          {
                nums[j+1] = nums[j];
                j--;
          }
          nums[j+1] = temp;
    }
}
```

### 4. 归并排序（重要）

![](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161218163120151-452283750.png)
![](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161218194508761-468169540.png)
代码：
```c++
/*
   将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为二路归并。归并排序是一种稳定的排序方法。
   
1、确定数组的大小，以及输入数组中的元素值；

2、将输入的数组进行分组归并；

3、将整个数组分成左右两个数组，左右两个数组再向下分，直至子数组的元素少于2个时，子数组将停止分割；

4、当左右子数组不能再分割，也是都是一个元素时，比较他们的大小，进行排序合并；

5、再排序合并上一级子数组为两个元素的数组，接着再排序合并上一级子数组为四个元素的数组；直至到排序合并刚开始的两个子数组，最后成为拍好序的数组；

*/

// 稳定排序，平均 O(nlogn)，最好 O(nlogn), 最差 O(nlogn),辅助空间 O(n)

void merge(int array[],int first,int last)
    {
        int temp[last];
        int mid=(first+last)/2;
        int i=0; //临时数组指针
        int l=first;//左序列指针
        int r=mid+1;//右序列指针
        while(l<=mid&&r<=last)//sort the ele of the left array and the right array
        {
            if(array[l]<array[r])
                temp[i++]=array[l++];
            else
                temp[i++]=array[r++];
        }
        while(l<=mid) //将左边剩余元素填充进temp中
        {
            temp[i++]=array[l++];
        }
        while(r<=last) //将右序列剩余元素填充进temp中
        {
            temp[i++]=array[r++];
        }
        i=0;
        //将temp中的元素全部拷贝到原数组中
        while(first<=last)
        {
            array[first++]=temp[i++];
        }    
    }

void mergesort(int data[],int first,int last)
    {
        if(first<last)
        {
            int mid=(first+last)/2;
            mergesort(data,first,mid);  //左边归并排序，使得左子序列有序
            mergesort(data,mid+1,last); //右边归并排序，使得右子序列有序
            merge(data,first,last); //将两个有序子数组合并操作
        }
    }   
```

### 5. 希尔排序

![](https://images2015.cnblogs.com/blog/1024555/201611/1024555-20161128110416068-1421707828.png)
代码：
```c++
/*
   数组列在一个表中并对列分别进行插入排序，重复这过程，不过每次用更长的列（步长更长了，列数更少了）来进行。
   
1、确定数组的大小，以及输入数组中的元素值；

2、将输入的数组进行分组归并；

3、将整个数组分成左右两个数组，左右两个数组再向下分，直至子数组的元素少于2个时，子数组将停止分割；

4、当左右子数组不能再分割，也是都是一个元素时，比较他们的大小，进行排序合并；

5、再排序合并上一级子数组为两个元素的数组，接着再排序合并上一级子数组为四个元素的数组；直至到排序合并刚开始的两个子数组，最后成为拍好序的数组；

*/

// 不稳定排序，平均 O(nlogn)-O(n^2)，最好 O(nlogn), 最差 O(n**2),辅助空间 O(1)


void Shell_sort(int a[],size_t n)
{
	int i,j,k,group;
	for (group = n/2; group > 0; group /= 2)//增量序列为n/2,n/4....直到1
	{
		for (i = 0; i < group; ++i)
		{
			for (j = i+group; j < n; j += group)
			{
				//对每个分组进行插入排序
				if (a[j - group] > a[j])
				{
					int temp = a[j];
					k = j - group;
					while (k>=0 && a[k]>temp)
					{
						a[k+group] = a[k];
						k -= group;
					}
					a[k] = temp;
				}
			}
		}
	}
}
```