# Pow(x,n)

> 实现 [pow(*x*, *n*)](https://www.cplusplus.com/reference/valarray/pow/) ，即计算 x 的 n 次幂函数。

##解题思路

计算x的n次幂，最简单的办法就是用x做n次的乘法，但这种方法当n过大的时候，复杂度就会过高。

为了降低复杂度，可以**利用已求出的n次幂**，来计算后续的答案，如x的平方相乘即可得到x的四次方，而不需要连乘四次x。

通过这种方法所求出的n次幂，n的值都是2的幂，但题目所要求的n可能是任意数字，因为**幂做乘法就是指数做加法**，这就意味着**只需要将2的各个幂值相加等于题给的n**即可，也就是n的二进制表示。

具体算法就是将n的值不断右移（除以2），判断每一位是否为1，同时x的值不断倍增，若当前位为1，则将当前的x加入结果中，即乘以当前`ans`的值。

## 代码

```java
class Solution {
    public double myPow(double x, int n) {
        double ans = 1;
        long N = n;
        N = Math.abs(N);
        boolean flag = n>0?true:false;
        while(N>0){
            if(N%2==1)ans *= x;
            x *= x;
            N >>= 1;
        }
        return flag?ans:1/ans;
    }
}
```

