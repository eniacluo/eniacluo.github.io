---
layout: post
title:  "数据结构-讲义与复习提纲1"
date:   2019-11-04 19:29:00 -0500
categories: cs
tag: [教程]
---

## 数据结构

首先要弄明白的一个问题是，为什么我们需要数据结构和算法？

其实在计算机的内存当中，数据的存放是线性的。也就是说，地址从小到大依次增大，数据就是简单地沿着一个方向依次排列，这样说，所谓的**数据结构**其实只有一种，就是「线性」存储。我们把这种内存中真正存放的方式叫作**物理数据结构**。

问题就是，我们对数据的四种基本操作：增加、删除、查找、更改，也即增、删、改、查，如果全部都是按照线性的方式去存效率低下，在一些有着特定的应用中，算法复杂度会很高。所以人们根据相邻存放的两个数据之间对应关系的不同，又引申出了**逻辑数据结构**的概念。那么，相邻存放的两个数据之间究竟有哪几种对应关系呢？答案是三种，1-1，1-m，m-n。就是我们平时说的「1对1」，「1对多」，和「多对多」。大体上来说，总共就是这三种数据结构。具体来说，他们分别指的是「线性表」、「树」和「图」。所谓的「1对多」指的是每一个数据的后面有多个数据和它相邻。其实在我们的现实世界中，所有人与人或者人与物之间的关系都可以化成这三种。像「一夫一妻」、「一夫多妻」说的就是这样的关系。

我们学习数据结构，一方面要学习的是它的逻辑数据结构，另一方面是要学习同这套结构相适应的算法，类似于我们上面说的增删改查。比如说，在一个二叉树中，怎么查找一个节点？我们需要设计和这个二叉树配套的算法，不然这个数据结构也就没什么意义了。

从这个角度讲，数据结构和算法之间的关系是相互依赖的关系。

这里还有一个问题要注意的就是，说到底，其实所有的关系全部都是「多对多」的关系，只不过「1对1」，「1对多」是「多对多」关系的特例。所以当「图」中每个节点都是一对多的，那么这个「图」就化归成了「树」，比如最小生成树就是这么来的；而当二叉「树」所有的节点都只有左孩子节点的时候，这个「树」就化归成了「链表」，也就是「线性表」的一种。

### 线性表

线性表主要是两种，一个是数组，一个是链表。这里暂且不谈广义线性表。

数组是按照下标来索引数据的，而链表必须得从头开始查找数据。这就导致数组在查找和修改数据是比链表快的。

另一方面，我们如果想向他们中间插入数据或是删除数据，数组很麻烦，他需要把后面的数据依次顺移或是前移，而链表几乎只需要2-3步就可以完成，所以它增加和删除数据是比数组要快的。

头插法：
{% highlight java %}
Node p = new Node();
p.next = head.next;
head.next = p;
{% endhighlight %}

尾插法：
{% highlight java %}
Node p = new Node();
tail.next = p;
tail = p;
{% endhighlight %}

值得注意的是，如果我们把数组里的节点依次用头插法形成一个链表，那么最后这个链表的顺序就是和原来的数组的顺序相反。所以，一道题说怎样把链表反向？就可以利用头插法的这一特性。

{% highlight java %}
Node p = head;
Node q = p.next;
head.next = null;
while (p != null) {
	p.next = head.next;
	head.next = p;
	p = q;
	q = q.next;
}
{% endhighlight %}

当然这个题还有另外两种思路。此处略。参见LC206。

讲到数组必然需要提到排序。

排序问题是非常典型的接触算法的入门基础问题，掌握好了对很多算法问题就有很深入的认识。

首先有一类排序是在排序的过程中总有一个排序区 Sorted Area 和非排序区 Unsorted Area。
- 插入排序
- 冒泡排序
- 选择排序

#### 插入排序

{% highlight java %}
public void insertSort(int[] nums) {
	for (int i = 1; i < nums.length; i++) {
		int temp = nums[i];
		int j = i - 1;
		while (j >= 0 && temp < nums[j]) {
			nums[j+1] = nums[j];
			j--;
		}
		nums[j+1] = temp;
	}
}
{% endhighlight %}


#### 冒泡排序

{% highlight java %}
public void bubbleSort(int[] nums) {
	for (int i = 0; i < nums.length - 1; i++) {
		for (int j = 0; j < nums.length - i - 1; j++) {
			if (nums[j] > nums[j+1]) {
				int temp = nums[j];
				nums[j] = nums[j+1];
				nums[j+1] = temp;
			}
		}
	}
}
{% endhighlight %}

#### 选择排序

{% highlight java %}
public void selectSort(int[] nums) {
	for (int i = nums.length - 1; i > 0; i--) {
		int maxIndex = 0;
		for (int j = 0; j < i; j++) {
			if (nums[j] > nums[maxIndex]) 
				maxIndex = j;
		}
		int temp = nums[maxIndex];
		nums[maxIndex] = nums[i];
		nums[i] = temp;
	}
}
{% endhighlight %}


还有一类是利用分治法 Divide-and-conquer，将大问题化归为小问题递归求解。
- 归并排序 
  + 自顶向下 Top-down 
  + 自底向上 Bottom-up
- 快速排序

#### 归并排序-自顶向下

{% highlight java %}
public void mergeSort(int[] nums) {
	mergeSort(nums, 0, nums.length - 1);
}

public void mergeSort(int[] nums, int begin, int end) {
	if (begin < end) {
		int middle = (begin + end) / 2;
		mergeSort(nums, begin, middle);
		mergeSort(nums, middle+1, end);
		merge(nums, begin, middle, end);
		Sort.print(nums);
	}
}

public void merge(int[] nums, int begin, int middle, int end) {
	int[] array = new int[end - begin + 1];
	int i = begin, j = middle+1, k = 0;
	while (i <= middle && j <= end) {
		if (nums[i] < nums[j]) 
			array[k++] = nums[i++];
		else
			array[k++] = nums[j++];
	}
	while (i <= middle)
		array[k++] = nums[i++];
	while (j <= end)
		array[k++] = nums[j++];

	for (int r = 0; r < end - begin + 1; r++) 
		nums[r+begin] = array[r];
}
{% endhighlight %}

#### 归并排序-自底向上

{% highlight java %}
public void mergeSort(int[] nums) {
	mergeSort(nums, 0, nums.length - 1);
}

public void mergeSort(int[] nums, int begin, int end) {
	int gap = 1;
	while (gap*2 < nums.length) {
		for (int i = 0; i + 2*gap < nums.length; i += 2*gap) {
			merge(nums, i, i+gap-1, i+2*gap-1);
			Sort.print(nums);
		}
		gap *= 2;
	}
	merge(nums, 0, gap-1, nums.length-1);
}

public void merge(int[] nums, int begin, int middle, int end) {
	int[] array = new int[end - begin + 1];
	int i = begin, j = middle+1, k = 0;
	while (i <= middle && j <= end) {
		if (nums[i] < nums[j]) 
			array[k++] = nums[i++];
		else
			array[k++] = nums[j++];
	}
	while (i <= middle)
		array[k++] = nums[i++];
	while (j <= end)
		array[k++] = nums[j++];

	for (int r = 0; r < end - begin + 1; r++) 
		nums[r+begin] = array[r];
}
{% endhighlight %}

#### 快速排序

{% highlight java %}
public void quickSort(int[] nums, int begin, int end) {
	if (begin < end) {
		int left = begin, right = end;
		int temp = nums[left];
		while (left < right) {
			while (left < right && nums[right] > temp)
				right--;
			nums[left] = nums[right];
			while (left < right && nums[left] < temp)
				left++;
			nums[right] = nums[left];
		}
		nums[left] = temp;
		print(nums);
		quickSort(nums, begin, left-1);
		quickSort(nums, left+1, end);
	}
}
{% endhighlight %}

其他的奇技淫巧。
- 桶排序
