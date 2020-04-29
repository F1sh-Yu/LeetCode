# Z字型变换

> 将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。
>
> 比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
>
> L    C    I    R
> E T O E S I  I  G
> E    D    H   N
> 之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。
>
> 请你实现这个将字符串进行指定行数变换的函数：
>
> string convert(string s, int numRows);
>

处理图形题有两种办法：

- 还原题目所给的图形变换，得到答案
- 寻找图形变换从初始到结束的规律，一步到位

## 方法一

仔细观察所谓的Z字形排列，其实就是依次对每一行推入一个元素，到了第`0`行或第`numRows-1`行，则向反方向移动。知道了这一本质之后，只要写代码来模拟，输出最后的结果即可。

使用一个`curRow`值记录当前行，`godown`记录当前行进的方向，每推入一个值后，`curRow`根据`godown`的方向进行加`1`或减`1`，到达第`0`行和第`numRows-1`行的时候，`godown`变换方向取相反值。

代码如下：

```java
class Solution {
    public String convert(String s, int numRows) {
        if(numRows==1)return s;
        List<StringBuilder> ret = new ArrayList<>();
        int curRow = 0;
        boolean godown = false;
        for(int i=0;i<Math.min(numRows,s.length());i++)ret.add(new StringBuilder());
        for(char c:s.toCharArray()){
            ret.get(curRow).append(c);
            if(curRow==0||curRow==numRows-1)godown = !godown;
            curRow += godown?1:-1;
        }
        StringBuilder ans = new StringBuilder();
        for(StringBuilder sb:ret)ans.append(sb);
        return ans.toString();
    }
}
```

## 方法二

因为最后是按行输出，可以观察z排列之后每行的值在原字符串中的位置，然后直接将这些值从字符串中抽出，组成新的字符串输出。

对于所有整数k：

- 第`0`行中的值，对应的是原数组中`k*(numRows*2-2)`的值
- 第`numRows-1`行的值，对应的是`k*(numRows*2-2)+numRows-1`的值
- 第`i`行的值，对应的是索引`k*(2⋅numRows−2)+i `以及` (k+1)(2*numRows-2)-i `

可以看出第`i`行中的`k*(2⋅numRows−2)+i `是适用于第`0`行和第`numRows-1`行的，所以只需要在中间行再加上索引` (k+1)(2*numRows-2)-i `的值就可以了。

代码如下：

```java
class Solution {
    public String convert(String s, int numRows) {

        if (numRows == 1) return s;

        StringBuilder ret = new StringBuilder();
        int n = s.length();
        int cycleLen = 2 * numRows - 2;

        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j + i < n; j += cycleLen) {
                ret.append(s.charAt(j + i));
                if (i != 0 && i != numRows - 1 && j + cycleLen - i < n)
                    ret.append(s.charAt(j + cycleLen - i));
            }
        }
        return ret.toString();
    }
}
```

