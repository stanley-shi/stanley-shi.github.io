---
layout: post
title: Jump Game II
author: Stanley Shi
comments: True
tags: coding leetcode

---

## Description
You can find the problem here: [https://oj.leetcode.com/problems/jump-game-ii/](https://oj.leetcode.com/problems/jump-game-ii/)

	Given an array of non-negative integers, you are initially positioned at the first index of the array.
	
	Each element in the array represents your maximum jump length at that position.
	
	Your goal is to reach the last index in the minimum number of jumps.
	
	For example:
	Given array A = [2,3,1,1,4]
	
	The minimum number of jumps to reach the last index is 2. (Jump 1 step from index 0 to 1, then 3 steps to the last index.)


## Thoughts
The most strait forward method is to use a recursive method to calculate the problm. Assume arrar A with length N, and the value jump[i] is the number of jumps the i'th number nedded to reach teh last index. The problem turns to be find jump[0]. It recursived like this: 

    if A[0] > N-1:
        jump[0] = 1; 
    else:
    	jump[0] = find minimum {
    	    for m = 1 to A[0]; do
    	    	output 1+ jump[0+m]
    	    done
    	}

Some edge cases:

+ if A[i] <= 0, then the value of jump[i] will be Integer.MAX_VALUE
+ A[N-1] = 0

So the most simple solution would be to traverse from A[N-2] to A[0], calculate each jump[i]; and at last return A[0];

The overall complexity would be O(N^2);

## But

When I implemented this code, the judge says it exceeds the runtime limit. So I have to find others ways.

Rethink it over again, I cannot figure out any other method to have less than O(N^2) complexity. So I start to look for some way to do some prune to the required calculation.

By looking at the problem, I found the prune point. Take the example array as an example, [2,3,1,1,4], for the second number (3), it can reach the next 3 numbers (1,1,4); but the third number (1), it can only reach the next 1 number (1). This means all numbers that are reachable by the third number can be reachable by the second number; so the step for the second number must be less than or equals to teh step of the third number. In this case, we can ignore the third number.

More formally, the prune point would be:

	for any numbers i,j that 0<=i< j <=len-1
	if A[i] > A[j] and i+A[j] >= j+A[j];
		skip the step calculation of A[j]
		
In the implementation, I marked step[j]=-2 to skip it

Please find the code below:

## Code

	public class Solution {
	  public int jump(int[] A) {
	    if (A == null || A.length == 1)
	      return 0;
	    int len = A.length;
	    int[] steps = new int[len];
	    Arrays.fill(steps, -1);
	    steps[len - 1] = 0;
	    fill(A, steps);
	    return steps[0];
	  }
	
	  /**
	   * This function will perform bad in case the array is large;	   
	   */
	  public void fill(int[] A, int[] steps) {
	    int len = A.length;
	    for (int i = 0; i < len - 1; i++) {
	      if (i + A[i] >= len - 1) {
	        steps[i] = 1;
	      } else {
	          if(steps[i]==-2)continue;
	        for (int j = i + 1; j < len - 1 && j < i + A[i]; j++) {
	          if (A[j] == 0)
	            steps[j] = Integer.MAX_VALUE;
	          else if (i + A[i] >= j + A[j]) {
	            steps[j] = -2;
	          }
	        }
	      }
	    }
	    for (int ind = len - 1; ind >= 0; ind--) {
	      if (steps[ind] >= 0 || steps[ind] == -2)
	        continue;
	      int min = Integer.MAX_VALUE;
	      for (int j = ind + 1; j <= len - 1 && j - ind <= A[ind]; j++) {
	        if (steps[j] >= 0 && steps[j] < min)
	          min = steps[j];
	        if (min == 0)
	          break;
	      }
	      if (min == Integer.MAX_VALUE)
	        steps[ind] = Integer.MAX_VALUE;
	      else
	        steps[ind] = min + 1;
	    }
	  }
	
	}
