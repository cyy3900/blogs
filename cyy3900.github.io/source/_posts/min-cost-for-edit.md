---
title: 最小的编辑代价
date: 2018-05-14 21:58:30
tags: [算法,LeetCode]
categories: 算法
---
题目来源：牛客网、LeeCode
#### [最小的编辑代价](https://www.nowcoder.com/questionTerminal/04f1731f32e246b4a19688972d5e2600?source=relative)

#### [Edit Distance](https://leetcode.com/problems/edit-distance/description/)

两题类似，最小的编辑代价这题的更具普遍意义，下面将讲解此题。

#### 题目：[最小的编辑代价](https://www.nowcoder.com/questionTerminal/04f1731f32e246b4a19688972d5e2600?source=relative)

对于两个字符串A和B，我们需要进行插入、删除和修改操作将A串变为B串，定义c0，c1，c2分别为三种操作的代价，请设计一个高效算法，求出将A串变为B串所需要的最少代价。

给定两个字符串A和B，及它们的长度和三种操作代价，请返回将A串变为B串所需要的最小代价。保证两串长度均小于等于300，且三种代价值均小于等于100。

测试样例：
"abc",3,"adc",3,5,3,100
返回：8

### 解题思路：
此题可以用动态规划的思想来解决。用矩阵dp[i][j]表示将A[0,……，i-1]编辑为B[0,………,j-1]花费的最小代价。

 1. dp[0][0]表示A空字符串编辑为B空字符串的代价为0。
 2. 矩阵的第一行dp[0][j]，表示A空字符串编辑为B[0,……，j-1]花费的最小代价，则可以通过将空字符串A中插入B[0,……，j-1]即可，那么dp[0][j]=c0*j。
 3. 矩阵的第一列dp[i][0]，表示A[0，……，i-1]编辑为B空字符串的最小代价，则可以通过将字符串A删除就可以得到空字符串B，那么dp[i][0]=c1*i。
 4. 其他位置可以按照从上到下，从左到右的顺序依次计算，dp[i][j]的值只可能通过dp[i-1][j]、dp[i][j-1]、dp[i-1][j-1]等得到，也就是一下的几种情况：
+ dp[i][j]可以通过dp[i-1][j]计算得来，也就是A[0,……，i-1]线编辑成A[0,……，i-2]，也就是删除一个字符A[i-1],然后通过A[0,……，i-2]编辑成B[0,……，j-1]，dp[i][j]=dp[i-1][j]+c1。
+ dp[i][j]可以通过dp[i][j-1]计算得到，也就是A[0,……，i-1]先编辑成B[0,……，j-2]，再插入一个字符B[j-1]编辑成B[0,……，j-1],那么dp[i][j]=dp[i][j-1]+c0。
+ dp[i][j]可以通过dp[i-1][j-1]计算得到，分为两种情况：1，A[i-1] == B[j-1]，那么dp[i][j] = dp[i-1][j-1]。2，A[i-1] != B[j-1],则可以先把A[0,……，i-2]编辑为B[0,……，j-2]，再将A[i-1]修改为B[j-1],那么dp[i][j] = dp[i-1][j-1] + c2。

5. 最后选出以上四种情况下的最小值作为dp[i][j]的最小编辑代价。dp矩阵右下角的那个值就是最小的编辑代价。

### 题目代码

``` stylus
package tech.qfmz;

import org.junit.Test;
/**
 * class_name: MinCost
 * package: tech.qfmz
 * describe: TODO
 * creat_user: cyy
 * creat_date: 2018/5/14
 * creat_time: 22:41
 **/
public class MinCost {
    @Test
    public void test(){
        int minCost = findMinCost("abc", 3, "adc", 3, 5, 3, 100);
        System.out.println(minCost);
    }
    public int findMinCost(String A, int n, String B, int m, int c0, int c1, int c2) {
        /**
         * method_name: findMinCost
         * param: [A, n, B, m, c0, c1, c2]
         * describe: TODO
         * creat_user: cyy
         * creat_date: 2018/5/14
         * creat_time: 22:41
         **/
        // write code here
        if (A == null || B == null || n < 1 || m < 1){
            return 0;
        }
        int [][]dp = new int[n+1][m+1];
        for (int i = 1; i <= n; i++) {
            dp[i][0] = c1 * i;
        }
        for (int j = 1; j <= m; j++) {
            dp[0][j] = c0 * j;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (A.charAt(i-1) == B.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1];
                }else {
                    dp[i][j] = dp[i-1][j-1] + c2;
                }
                dp[i][j] = Math.min(dp[i][j],dp[i-1][j] + c1);
                dp[i][j] = Math.min(dp[i][j],dp[i][j-1] + c0);
            }
        }
        return dp[n][m];
    }
}

```

### 最后附上LeetCode上[Edit Distance](https://leetcode.com/problems/edit-distance/description/)的代码

``` stylus
package tech.qfmz;

import org.junit.Test;

/**
 * class_name: MinCost
 * package: tech.qfmz
 * describe: TODO
 * creat_user: cyy
 * creat_date: 2018/5/14
 * creat_time: 22:41
 **/
public class MinCost {
    @Test
    public void test() {
        System.out.println(minDistance("horse","ros"));
    }

    public int minDistance(String word1, String word2) {
        /**
         * method_name: minDistance
         * param: [word1, word2]
         * describe: 动态规划求解最小的编辑距离
         * creat_user: cyy
         * creat_date: 2018/5/14
         * creat_time: 22:59
         **/
        if (word1 == null || word2 == null) {
            return 0;
        }
        int row = word1.length() + 1;
        int col = word2.length() + 1;
        int [][] dp = new int[row][col];
        for (int i = 1; i < row; i++) {
            dp[i][0] = i;
        }
        for (int j = 1; j < col; j++) {
            dp[0][j] = j;
        }
        for (int i =1 ; i < row; i++) {
            for (int j = 1; j < col; j++) {
                if (word1.charAt(i-1) == word2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1];
                }else {
                    dp[i][j] = dp[i-1][j-1] + 1;
                }
                dp[i][j] = Math.min(dp[i][j],dp[i-1][j]+1);
                dp[i][j] = Math.min(dp[i][j],dp[i][j-1]+1);
            }
        }
        return dp[row-1][col-1];
    }
}

```


