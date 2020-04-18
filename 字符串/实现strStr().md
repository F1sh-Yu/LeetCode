# 实现strStr()
> 实现  strStr()函数。  
> 给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  **-1**。  

这道题可以直接用双指针解，其实也就是暴力解法，将字符串中的每一个字符作为起始，判断其后的字符是否与needle字符串相一致，在python中使用列表的**切片**就可以很轻易的实现。
但我又想起了考研时学到的KMP算法，于是就动手写写看，KMP算法的原理就不讲了，实现KMP主要就是两部分，第一部分建立一个`next`数组，第二部分就是用`next`数组去进行匹配。思路很简单，但是写起来的时候，对于边界的处理的确把我给难倒了。
首先是建立`next`数组的代码：

```
        kmp = [-1]
        j = -1
        i = 0
        while(i < len(needle)):
            if(j == -1 or needle[i] == needle[j]):
                i += 1
                j += 1
                kmp.append(j)
            else:
                j = kmp[j]
        cur = 0
```
初始值为`-1`是很关键的地方，如果没有这个`-1`，`j = kmp[j]`倒退的时候，因为`kmp[0]`必定是为0的，那么就会无限循环。
匹配的代码：

```
        def check(haystack,needle,kmp):
            i = 0
            j = 0
            while i < len(haystack) and j <len(needle):
                if j == -1 or haystack[i] == needle[j]:
                    i += 1
                    j += 1
                else:
                    j = kmp[j]
            if j == len(needle):
                return i - j
            else:
                return -1
```
为什么这里`j`的初始值为0？这时因为在建立`next`数组时，第一次比较是发生在`needle[0]`和`needle[1]`之间的，而后续实际匹配的时候，是发生在`haystack[0]`和`needle[0]`之间。

