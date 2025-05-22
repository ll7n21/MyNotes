---
attachments: [Clipboard_2025-05-15-18-45-48.png, Clipboard_2025-05-15-18-46-13.png, Clipboard_2025-05-15-18-55-12.png]
deleted: true
title: Question 996 solution
created: '2025-05-14T17:41:33.007Z'
modified: '2025-05-22T17:34:01.220Z'
---

# Question 996 solution
# Intuition
The task essentially can be broken down to:
1. Find out all permutations of the list
2. If a permutation has any adjacent element (e1,e2) that have e1+e2 not a perfect square, don't count it in.

## How to construct permutation?
Let's say we have a list of `[a1,a2,...,an]`, for now suppose all elements' value are unique, we can define a recursive function `f(list)`.
`f([a1,a2,...,an])`will find out all list that have `ai` in first postion (i=1, 2, ..., n), followed by all permutations of the remaining elements except `ai`.
And we can define a base case if `n=1`,the result is only 1 list with the 1 element.
e.g.
`f([1,2,3])` will find out:
* `[1,f([2,3])]`
* `[2,f([1,3])]`
* `[3,f([1,2])]`
`perm([2,3])` will find out:
* `[2,f(3)]` -> `[2,3]`
* `[3,f(2)]` -> `[3,1]`
etc.
This can be done with a simple for loop, just take each iterated element as the first element of the permutation and continue.

Imagine the input list a basket, each time we pull an element from the basket and confirm that this element will be the next element in our result. All other elements remained in the basket will be positioned behind.

The reality is, it is possible that there are duplicate values in the given list and will render duplicate permutations. and we should avoid repeatedly pulling elements of identical values to save efforts.
e.g. Given a list (1,17,1,8).
![](@attachment/Clipboard_2025-05-15-18-46-13.png)
In step 1, we could have pick any of the 4 elements from the basket (1,17,1,8). but because the first and third elements have identical values, we don't need to proceed with the 3rd options.
Similarly, at step 3, let's say if and 8 and 17 has been pulled out from the basket consequtively, there are two elements of value 1 remained in the basket. We only need to pull a value 1 out once and no need to go with the other path.

## check if adjacent elements make perfect square while constructing permutation
We have saved some efforts by purging unnecessary branches originated from duplicate values above.

We could further purging some other branches if we found the permutation have adjacent elements does not expectation (sum=perfect square).

So each time when we pull an element from the basket, if the element does not makes a perfect square sum with the existing element in front of it, there is no need to go further.
e.g.
![](@attachment/Clipboard_2025-05-15-18-55-12.png)

# Approach
Because there could exist multiple elements of duplicate values, and we are counting the unique permutation, I use a map to stores the values available to put into each "current" first position, i.e. this map essentially is "the basket".
The key of the map is the unique numerical value, and the value is the count (freequency) -- how many times this unique numerical values appears in the input list.

-- So each time when we pull a value from the basket and decide it will be the value for the current next position, we only pick element of unique numerical value once.

I define a recursive DFS method:
`void search(unordered_map<int, int>& frequencies, int elementAhead, int& counter,int positionLeft)`
I use `positionLeft` to track how many positions are left to be filled, the map `frequencies` tracks the elements remained available to be used.
I use `elementAhead` to cache the numerical value of the element we picked last time, so we only pick element that can form perfect square with this 'last element value' to continue the search. The special case very first element which has no element ahead of it, in this case no branch are to be skip.
```cpp []
void search(unordered_map<int, int>& frequencies, int elementAhead, int& counter,int positionLeft) {
        if (positionLeft == 0) {
          /* base case: successfully filled all positions to form a permutation. */
            counter++;
            return;
        }
        for (auto& freq : frequencies) {
            /*pull an available numerical value k to fill into current position, and continue search.
              - if (k,v) has v=0, element of that specific unique value k has been used up and not available for use.
              - if v does not form a perfect square with the picked element ahead of it, skip this branch.
            */
            if (freq.second > 0 && (elementAhead==-1 || isPerfectSquare(elementAhead, freq.first))) {
                frequencies[freq.first]--;
                search(frequencies, freq.first, counter, positionLeft - 1);
                frequencies[freq.first]++;
            }   
        }
    }      
```


# Complexity
- Time complexity: $O(n! * n)$ in the worst case where `n` is the number of unique integers in the input list.

- Space complexity: $O(n)$

# Code
```cpp []
class Solution {
public:
    int numSquarefulPerms(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1)
            return 0;
        if (n == 2)
            return isPerfectSquare(nums[0],nums[1]);
        int counter = 0;
        unordered_map<int, int> frequencies;
        for (int& num : nums)
            frequencies[num]++;
        int elementAhead = -1;
        search(frequencies, elementAhead,counter,n);
        return counter;
        
    }
    bool isPerfectSquare(int num1,int num2) {
        double root = sqrt(num1+num2);
        return ceil(root) == floor(root);
    }
    void search(unordered_map<int, int>& frequencies, int elementAhead, int& counter,int positionLeft) {
        if (positionLeft == 0) {
            counter++;
            return;
        }
        for (auto& freq : frequencies) {
            if (freq.second > 0 && (elementAhead==-1 || isPerfectSquare(elementAhead, freq.first))) {
                frequencies[freq.first]--;
                search(frequencies, freq.first, counter, positionLeft - 1);
                frequencies[freq.first]++;
            }   
        }
    }      
};
```
