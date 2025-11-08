# 快速排序

## 1 基本思路

快速排序的核心思想：<strong style="color: orange">Divide and Conquer</strong>，流程如下：

1. 从数组中选择一个 pivot；
2. 然后将所有比其小的的元素放到左边，将所有比其大的元素放到右边；
3. 再对左右两边递归进行相同的操作。



## 2 代码实现

`partition()`: 该函数用于实现最主要的分割功能，即将比 pivot 小的元素放在 pivot 左边，将比 pivot 大的元素放在 pivot 右边。这里使用了 "双指针" $i$ 和 $j$ 分别指向两个区域，$i$ 指向小的区域，$j$ 则用于扫描全局，节省了空间。

```cpp
int partition(int arr[], int left, int right)
{
  int pivot = arr[right];
  int i = left - 1;

  for (int j = left; j < right; j++)
  {
    if (arr[j] < pivot)
    {
      i++;
      swap(arr[i], arr[j]);
    }
  }

  swap(arr[i + 1], arr[right]);

  return i + 1; // return the correct position of pivot
}

void quickSort(int arr[], int left, int right)
{
  if (left < right)
  {
    int pivotIndex = partition(arr, left, right);
    quickSort(arr, left, pivotIndex - 1);
    quickSort(arr, pivotIndex + 1, right);
  }
}
```



# 相关习题

## 1 Kth Largest Element

> [!tip]
>
> 题目：[215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

题目解析：题目要求的很简单，即数组排序后，返回第 K 个最大的元素，要求时间复杂度控制在 $O(n)$

我们知道，快速排序的核心思想即，选择 pivot，然后将比其大的元素放在右边，将比其小的元素放在左边，因此，$arr[p + 1 ⋯ r]$ 中的元素都是大于 pivot 的，我们只需要确定当该序列中的长度为 $k - 1$ 时，pivot 刚好就是我们要找的元素，如果 $index\  of\  pivot < k$ ，那么就往右区间查找，否则往左区间查找

```cpp
class Solution
{
public:
  int findKthLargest(vector<int> &nums, int k)
  {
    int res = quickSelect(nums, k);

    return res;
  }

private:
  int quickSelect(vector<int> &nums, int k)
  {
    vector<int> big, small, equal;

    int n = nums.size();

    // Select an element randomly
    srand(time(0));
    int pivotIndex = rand() % n;

    int pivot = nums[pivotIndex];

    for (int i = 0; i < n; i++)
    {
      if (nums[i] < pivot)
        small.push_back(nums[i]);
      else if (nums[i] > pivot)
        big.push_back(nums[i]);
      else
        equal.push_back(nums[i]);
    }

    if (big.size() >= k)
      return quickSelect(big, k);
    else if (big.size() + equal.size() < k)
      return quickSelect(small, k - big.size() - equal.size());
    else
      return pivot;
  }
};
```

> [!note]
>
> 该方法所使用的额外空间较多

