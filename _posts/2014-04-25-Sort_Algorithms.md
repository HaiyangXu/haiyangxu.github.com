---
layout: post
title: "排序算法总结"
description: ""
categories: ["算法"]
tags: ["排序算法","Sort Algorithm"]
published: true
---
{% include JB/setup %} 

今天抽了半天时间整理了一下集中排序算法，总结一下。
#一、基于比较的排序
基于比较排序的时间复杂度下限是$O(n\log{n})$ ，稳定排序有冒排序、插入排序、归并排序，非稳定排序：选择排序、快速排序【待补全】
##1. 冒泡排序(Bubble Sort)
**[思想]**
把数组中较大的数看成较轻的“气泡”，对数组美进行一次冒泡操作就把最轻的“气泡”（最大的数）移到了序列组后面（如果不好理解把最小的数认为是最轻的气泡是一样的，相应的每次把最小的数移到最后面去），对序列进行多次冒泡操作，就得使序列排序完成了。
**[实现]**
具体来说冒泡操作是这样的：

> 给定一个序列，从坐开始遍历，每次将其与相邻后续进行比较，如果大于相邻后续则将两个数交换，这样对一个序列进行“冒泡”后，最大的数排到了最后面。

定义bubble函数为冒泡操作：

    void bubble(int A[],int start,int end) //数组区间为[start,end)
        {
            for(int i=start;i<end-1;++i)
                if(A[i]>A[i+1]) swap(A[i],A[i+1]);
        }
        
冒泡排序就是对序列，进行多次冒泡操作，最后就得到了有序排列：

    void bubble_sort(int A[],int start,int end)
        {
            for(int i=end;i>start;--i)
                bubble(A[],start,i);
        }
        
**[算法分析]**

对长度为n的序列，进行冒泡操作的序列长度依次为：$n , (n-1) , (n-2) , ··· , 3 , 2 , 1$ ，冒泡操作的时间复杂度为$O(n) $则，总的时间为：
$$n + n-1 + n-2 + ··· + 3 + 2 + 1=\frac{n(n-1)}{2}$$
渐进时间复杂度为$O(n^2)$

##2. 插入排序(Insertion Sort)
**[思想]**
插入排序的思想主要是将后续元素插入到已排序子序列的正确位置，具体来说是将待排序数组分为左右两部分，左边部分为已经排好序，依次将右边序列的头一个元素插入到左边正确的位置，并增长了左边序列的长度；初始时将左边部分为长度为1的子序列（即已排序）开始，到左边序列长度增长到序列总长，排序完成。
**[实现]**
插入排序中的插入操作设为子函数`void insert(A[],int start,int last)`，左边已排序序列为`[start,last)`，要将`A[last]`插入到正确位置，则:

    void insert(int A[],int first,int last)
    {
        int x=A[last];
        int i=last;
        while(A[i-1]>x&&i>first){A[i]=A[i-1]; --i;}
        A[i]=x;
    }

对左边子序列依次进行插入操作，得到排序插入结果：

    void insertion_sort(int A[] ,int start,int end)
    {
        for(int i=start+1; i<end;++i)
            insert(A[],start,i);
    }
    
**[算法分析]**
对长度为$n$的序列进行插入排序时，一次进行插入的子序列长度为：$1,2,3,···,(n-2) , (n-1) ,n $，插入操作很明显可在$O(n)$时间完成，则总时间为：
$$1+2+3+···+(n-2) +(n-1) +n=\frac{n(n-1)}{2}$$
渐进时间复杂度为$O(n^2)$，与冒泡排序一致。
##3. 选择排序(Slection Sort)
**[思想]**
选择排序算法思想和前面冒泡和插入排序也大同小异，主要思想是查找序列`A[i,end)`的最小值，与序列的最左边元素`A[i]`交换，这样每次选择最小元素交换后最小元素就移到了最左边，左边子序列即为有序序列，依次减小进行选择交换操作的序列长度到1时排序完成。
**[实现]**
选择排序中查找最小元素函数返回最小值的位置：

    int select_min(int A[],int start,int end)
    {
        int index=start;
        while(++start<end)
        {
            if(A[index]>A[start])
                index=start;
        }
        return index;
    }

选择排序依次选择最右边序列的最小值进行交换操作，完成选择排序：

    void selection_sort(int A[],int start,int end)
    {
        for(int i=start;i<end;++i)
        {
            int index=select_min(A[],i,end);
            swap(A[start],A[index]);
        }
    }
    
**[算法分析]**
选择排序的算法复杂度和冒泡排序算法一致。
##4. 快速排序(Quick Sort)
**[思想]**
快速排序是基于分治思想的排序方法，主要步骤：

 - **分解：**选择一个基元P，将序列分成两部分，左边子序列元素全部 < P，右边子序列元素全部 > P
 - **递归：**对左右子序列递归调用快速排序
 - **归并：**因为子序列都是原址排序，所以完成后不需要合并操作，已为有序数组

**[实现]**
分解序列的子函数,随机从序列中选择pivot将数组分割为两半，方法可使用两个索引分别指向首位位置，向中移动的时候和pivot比较，不符合的地方交换位置，最后两个指针相遇时数组分为两部分左边小于pivot，右边大于pivot：

      i                                       j
      ↓                                       ↓
    |   |   |   |   |   |   |   |   |   |   |   |

代码：    

    int partition(int A[],int first,int last)
    {
        int pivot=(last-first)*rand()/RAND_MAX+first;
        while(true)
        {
            while(A[first]<A[pivot])++first;
            while(A[last]>=A[pivot])--last;
            if(first>=last)return first;
            swap(A[first],A[last]);
            --last;
            ++first;
        }
    }
    
快速排序算法调用分割子函数的到分割位置，并对左右两部分数组分别迭代快速排序：

    quick_sort(int A[],int start,int end)
    {
        int mid=partiton(A,start,end);
        quick_sort(A,start,mid);
        quick_sort(A,mid,end);
    }

**[算法分析]**
快速排序的时间复杂度计算分为两部分：分割部分和迭代部分。
很显然，分割部分的时间复杂度为$O(n)$ ；迭代部分的分析就有些复杂了，迭代部分的时间复杂度迭代公式和分割区间的大小有关：

 - **最坏情况** 每次分割都分割成1 和 n-1子序列，则迭代公式为：$$T(n)=T(n-1)+O(n)\quad 时间复杂度为：  O(n^2)$$
 - **平均情况** 因为随机选择分割点，可认为平均情况每次分割都均分成两部分，则迭代公式为：$$T(n)=2T(n/2)+O(n)\quad 时间复杂度为：  O(n\log{n})$$

快排的时间复杂度与分割区间有关，若每次分割为均分可到达最优时间复杂度。因为随机选择分割点所以可以认为总体下是渐进均分的，在实际中快排都可以得到较好的运行时间。可以通过限制划分区间的最小值，当划分的区间小于阈值时直接对区间使用简单的排序算法（如插入排序）完成排序，而不进行递归调用快排。
##5. 归并排序(Merge Sort)
**[思想]**
归并排序是分治法经典的例子，其主要思想是把待排序区间分成均等的左右部分，对左右两部分递归调用归并排序，最后对已分别排序的左右两部分进行归并，时间复杂度为$O(n\log{n})$，空间复杂度为$O(n)$。

**[实现]**
归并排序：

    void merge_sort(int A[],int start, int end)
    {
        int mid=(start+end)/2
        merge_sort(A,start,mid);
        merge_sort(A,mid,end);
        merge(A,start,end);
    }
    
合并子函数：
    
    void merge(int A[], int start, int end)
    {
        int mid=(start+end)/2;
        vector<int> left(mid-start,0);
        vector<int> right(end-mid,0);
        for(int i=start;i<mid;++i)left[i]=A[i];
        for(int i=mid;i<end;++i)right[i]=A[i];
        int l=0,r=0;
        while(l<left.size()&&r<right.size())
        {
            if(left[l]<right[r]){A[start+l+r]=left[l];++l;}
            else {A[start+l+r]=right[r];++r;}
        }
        while(l<left.size()){A[start+l+r]=left[l];++l;}
        while(r<right.size()){A[start+l+r]=right[r];++r;}
    }

**[算法分析]**
合并子流程的时间复杂度和空间复杂度都为$O(n)$ ,因为是对均分成两部分进行迭代，迭代公式为：$$T(n)=2T(n/2)+O(n)\quad 时间复杂度为：  O(n\log{n})$$
##6. 堆排序(Heap Sort)
**[思想]**
堆是一种近似完全二叉树的数据结构，并且具有性质：最大（小）堆除了根节点，所有节点都小（大）于父节点，而完全二叉树可以使用连续的数组进行存储。下面仅已最大堆进行描述，最小堆是相同的原理。

所谓堆排序，不过是把待排序数组`[0,n]`抽象成堆；对这颗树从最底层节点依次向上，调用最大堆保持子流程，使整棵树成为一个最大堆，从堆中循环移除最大元素到数组末端，并保持子堆完整性，最后完成排序。

具体的细节是：最大堆中最大元素存储在`A[0]`了，交换`A[0],A[n]`则最大元素存储到`A[n]`了，把`A[n]`移出堆，并对`[0,n-1]`调用最大堆保持子流程。循环到堆大小为1时数据排序完成了。

堆排序也是隐含的用到了分治的思想，具体就是在构建最大堆的时候分别对把子树变成最大堆，然后往上构建。堆排序和归并排序一样是原址排序。

使用顺序数组存储的大小为n的堆（近似完全二叉树）具有以下性质：

 - 定义叶子节点高度 为0 ，树高度为$\lfloor \log{n}\rfloor$
 - 高度为$h$的节点至多有$\lceil \frac{n}{2^{n+1}}\rceil$ 个
 - 叶子节点为$\lfloor n/2 \rfloor +1 ,\cdots,n-2,n-1,n $
 
**[实现]**

最大堆保持子函数,输入为数组A下标i ，i的左右子树已经是最大堆：

    void max_heapify(int A[],int i,int size)
    {
        int l=2i;//left child tree
        int r=2i+1;// right child tree
        int largest=i;
        if(l<size&&A[l]>A[largest])largest=l;
        if(r<size&&A[r]>A[largest])largest=r;
        if(largest!=i)
        {
            swap(A[i],A[largest]);
            max_heapify(A,largest,size);
        }
    }

建堆过程是对非叶子节点从下向上调用最大堆保持：
    
    void build_max_heap(int A[],int size)
    {
        for(int i=size/2;i>0;--i)
            max_heapify(A,i,size);
    }
    
使用对进行排序的过程是先对数组建堆，然后交换最大元素到末尾，并减小堆大小，调用最大堆保持函数使得新堆恢复成最大堆，循环交换出最大元素，最后得到排序数组：
    
    void heap_sort(int A[],int size)
    {
        build_max_heap(A,size);
        for(int i=size;i>1;--i)
        {
            swap(A[0],A[i];
            size=size-1;
            max_heapify(A,0,size);
        }
    }
    
    
**[算法分析]**
堆排序时进行了一次建堆操作，n次最大堆保持操作,根据堆的性质可计算出来为$O(nlogn)$

#二、线性时间排序
##7. 计数排序(Count Sort)
**[思想]**
计数排序使用额外存储空间，对每个数进行计数，计算数据前面有多少个数，然后直接把数组中的数放到相应位置。
**[实现]**

**[算法分析]**

##8. 桶排序(Bucket Sort)
**[思想]**

**[实现]**

**[算法分析]**

##9. 基数排序(Radix Sort)
**[思想]**

**[实现]**

**[算法分析]**