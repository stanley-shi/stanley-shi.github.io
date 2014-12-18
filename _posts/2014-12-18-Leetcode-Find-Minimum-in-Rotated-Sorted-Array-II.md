## Description

Follow up for "Find Minimum in Rotated Sorted Array":

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

The array may contain duplicates.

## Analysis

The array should be like this: first goes up, and then at some point it suddenly meet the smallest number and then goes up again.

The thought is still using binary search:

    get the medium value m;
    if m < start and m < end data;
      the smallest value should at left half;
    else if m > start and m > end data;
      the smallest value should at right half;
      
Some edge conditions:

+ if there're duplicated items in the array, we might have to search both left side and right side and then compare the result;

## Code

Please find the code here: [Stanley@Github](https://github.com/stanley-shi/learning/blob/master/leetcode/Find-Minimum-in-Rotated-Sorted-Array-II/Solution.java)
