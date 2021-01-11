#  第一天学习心得

> 先说说今天的题目 LeetCode 编号 2题 4题 5题
>
> 本来是不难，奈何我太弱了。
>
> 我真是硬着头皮在写，最后还是抄了作业（真香！）
>
> 都是java写的  以后就刚java

![image-20210111232501368](C:\Users\MECHREVO\AppData\Roaming\Typora\typora-user-images\image-20210111232501368.png)

写完的话已经11点25分嗯 今天感觉自己太弱了  还在玩耍  。

##  1.无重复字符最长子串（LeetCode2）

###  题目

![image-20210111232739357](C:\Users\MECHREVO\AppData\Roaming\Typora\typora-user-images\image-20210111232739357.png)

###  说说这个题的思路  我觉得比较好的想法

这个题还有个问题就是不能先算数字再求。这样浪费时间而且效果不好。

首先题目要求求出两个数相加  然后要考虑进位。那就会有像个数不等长的问题，可以将不等长的部分看成空。

其次就是进位问题，可以通过取余的方法的到进位后个位的数，同时可以把进位值通过/10方法直接得到。

解决这些思路就可以写啦。

###  代码如下：

```java
public static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    int grouth = 0;
    ListNode suml = new ListNode(0);
    ListNode sum = suml;
    while(l1!=null||l2!=null||grouth>0){
        int v1 = l1==null?0:l1.val;
        int v2 = l2==null?0:l2.val;
        int sumval = v1+v2+grouth;
        grouth = sumval/10;
        sumval = sumval%10;

        sum.val+=sumval;

        if (l1!=null) l1=l1.next;
        if (l2!=null) l2=l2.next;

        if (l1!=null||l2!=null||grouth>0){
            sum.next = new ListNode();
            sum = sum.next;
        }

    }
    return suml;
}
```

###  总的来说不难，但是我自己手速和经验都不足，写了很久很久……

##  2.寻找两个正序数组的中位数（leetcode 88 4）

这个题首先去写的话 我个人觉得最直观的方法就是两个合起来出一个新的数组然后直觉取下标。

![image-20210111233308948](C:\Users\MECHREVO\AppData\Roaming\Typora\typora-user-images\image-20210111233308948.png)

我自己其实也尝试直觉算到/2的位置，奈何我不行。，

看了看官方的教程我去写了88题。

首先88题也贴过来

![image-20210111233341354](C:\Users\MECHREVO\AppData\Roaming\Typora\typora-user-images\image-20210111233341354.png)

可以说是母子题了然后解题如下

就是也看了大佬的思路 我自己写的太油腻就参考了一下。

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int[] temp = new int[nums1.length];
        int i = 0,j=0;
        int k = 0;
        while (i<m&&j<n){
            temp[k++] =nums1[i]<=nums2[j]?nums1[i++]:nums2[j++] ;
        }
        while (i<m) temp[k++]=nums1[i++];
        while (j<n) temp[k++]=nums2[j++];
        for (int l = 0; l < temp.length; l++) {
            nums1[l]=temp[l];
        }
    }
}
```

知道怎么拼那么4题迎刃而解！

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int[] temp = new int[m+n];
        int i=0,j=0;
        int k=0;
        while (i<m&&j<n){
            temp[k++]=nums1[i]<=nums2[j]?nums1[i++]:nums2[j++];
        }
        while (i<m) temp[k++]=nums1[i++];
        while (j<n) temp[k++]=nums2[j++];


        return (temp[(m+n+1)/2-1]+temp[(n+m+2)/2-1])/2.0;
    }
}
```

##  3.最长回文数（LeetCode 5）

![image-20210111233657366](C:\Users\MECHREVO\AppData\Roaming\Typora\typora-user-images\image-20210111233657366.png)

这个题不用多说，大手子门都知道。我等菜菜先膜拜。

我其实想用中心法写emmmm或者暴力，我都没写出来。最后看了官方的例子，也是学到了怎么去思考和理解。

首先判断特殊情况，如果长度小于2，就直接出结果。

然后构建dpmap，主对角线都为true。

从列开始进行计算，因为这个计算一半嘛，就是对角线以上的从00到10 11这样 （略过对角线了）

接下来如果头尾不一致肯定是false

然后如果长度小于3（这个是个推论  手画画就明白了子串为啥要小于3）就true

否则就回到上面写的i+1 ，j-1位置

最后判断最长子串是否要更新。

```java
class Solution {
    public String longestPalindrome(String s) {
        // 动态规划解法
        int len = s.length();
        if(len<2){
            return s;
        }
        int maxlen = 1;
        int begin = 0;
        //dp map 构建
        boolean [][] dp = new boolean[len][len];
        for (int i = 0; i < len; i++) {
            dp[i][i]=true;
        }
        char [] chararray = s.toCharArray();
        for (int j = 1; j < len; j++) {
            for (int i = 0; i < j; i++) {
                if (chararray[i]!=chararray[j]){
                    dp[i][j]=false;
                }else {
                    if (j-i<3){
                        dp[i][j]=true;
                    }else {
                        dp[i][j]=dp[i+1][j-1];
                    }
                }
                //判断值
                if(dp[i][j]&&j-i+1>maxlen){
                    maxlen= j-i+1;
                    begin= i;
                }
            }
        }
        return s.substring(begin,begin+maxlen);
    }
}
```

##  4.总结一下 

其实题目对于我来说却是很吃不消，而且第一天就……（抄作业）觉得不太好。但是感觉却是在这样有压力的环境可以提升一下学习的渴望。坚持下去，我想把这20天努力走完，小伙伴们加油！