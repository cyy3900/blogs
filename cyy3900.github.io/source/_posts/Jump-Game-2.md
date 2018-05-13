---
title: Jump Game  2
date: 2018-05-13 22:45:38
tags: 算法
categories: 算法
---
### [跳跃游戏 Ⅱ](https://leetcode.com/problems/jump-game-ii/description/)

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

Example:

Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.
Note:

You can assume that you can always reach the last index.

### 一、动态规划的方法
用一个数组dist[i]表示跳到i用的最少的步数，dist[i]如果可以从[0,……，i-1]跳一步过来，那么dist[i]的值就是从[0,……，i-1]跳一步过来最少的步数。

``` stylus
package tech.qfmz;

import org.junit.Test;
/**
 * class_name: JumpGame2
 * package: tech.qfmz
 * describe: TODO
 * creat_user: cyy
 * creat_date: 2018/5/13
 * creat_time: 23:23
 **/
public class JumpGame2 {

    @Test
    public void test(){
        int[]nums = {2,3,1,1,4};
        System.out.println(jump(nums));
    }

    /**
     * method_name: jump
     * param: [nums]
     * describe: TODO
     * creat_user: cyy
     * creat_date: 2018/5/13
     * creat_time: 23:28
     **/
    public int jump(int[] nums) {
        if (nums == null || nums.length <= 0){
            return 0;
        }
        int []dist = new int[nums.length];
        int min  = Integer.MAX_VALUE;
        for (int i = 1; i < nums.length; i++) {
            min = Integer.MAX_VALUE;
            for (int j=i-1;j>=0;j--){
                if (nums[j] >= i-j && min > dist[j] + 1){
                    min = dist[j]+1;
                }
            }
            dist[i] = min;
        }
        return dist[nums.length-1];
    }
}

```
这种方法的复杂度是O(n<sup>2</sup>)，经测试超时。那么只能使用Plan B

### 二、贪心算法

用一个变量minstep记录跳的最少次数，edge表示在跳了minstep次后的最远位置，maxReach表示多跳一次、跳的最远的位置。跳跃的次数不断增加，记录当前跳跃次数下跳的最远的位置，一旦能够跳到终点，则找到了最少的步数。

``` stylus
public int jump(int[] nums) {
        if (nums == null || nums.length <= 0){
            return 0;
        }
        int edge = 0;
        int minStep = 0;
        int maxReach = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > edge){
                minStep++;
                edge = maxReach;
                if (edge >= nums.length-1){
                    return minStep;
                }
            }
            maxReach = Math.max(maxReach,i+nums[i]);
        }
        return minStep;
    }
```
该方法的空间复杂度和时间复杂度都是O(n)。

