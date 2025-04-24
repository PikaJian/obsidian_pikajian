# leetcode 153

**Idea**:

Divide and conquer.

Evenly Split the array into two sub-arrays, and find the minimums of them, return the smaller one.

findMin(a\[0..n\]) = min(findMin(a\[0..n/2\], a\[n/2..n\])

**Key property**:

One of the sub-array will be a sorted array, it takes O(1) to find the minimal element, just the first element.

![153-ep38-1.png](http://zxi.mytechroad.com/blog/wp-content/uploads/2017/09/153-ep38-1.png)![153-ep38-2.png](http://zxi.mytechroad.com/blog/wp-content/uploads/2017/09/153-ep38-2.png)

**Time complexity**:
分兩半必有一半是遞增, 遞增那半找最小是(1)
所以不是T(n) = T(n/2) + T(n/2)  + 1 = 2T(n/2) + 1 = O(n)
而是 T(n) = O(1) + T(n/2) = O(logn)
