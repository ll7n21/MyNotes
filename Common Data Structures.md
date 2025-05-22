---
title: Common Data Structures
created: '2025-01-01T16:27:38.589Z'
modified: '2025-05-22T17:44:25.406Z'
---

# Common Data Structures
## trees
- [Red Black Tree Common Interview Questions](https://sg.indeed.com/career-advice/interviewing/red-black-questions)
### Binary Search Tree
[One Pass Traverse Binary search Tree](https://leetcode.com/problems/all-elements-in-two-binary-search-trees/solutions/464073/c-one-pass-traversal)
```
void iterateLeftMostBranch(TreeNode* pNode, stack<TreeNode*>& pNodesAsc) {
//given a node, push all nodes on the branch from itself to its leftmost leaf onto the stack
    while (pNode != nullptr) {
        pNodesAsc.push(pNode);
        pNode = pNode->left;
    }
}
vector<int> getAllElementsOfOneTree(TreeNode* root) {
    stack<TreeNode*> pNodesAsc;//stores pointer to nodes of the tree, will pop pNodes in value ascending order
    vector<int> nodeValues;//stores values of nodes in the tree in ascending order
    iterateLeftMostBranch(root, pNodesAsc);
    while (!pNodesAsc.empty()) {
        TreeNode* current = pNodesAsc.top();
        pNodesAsc.pop();
        nodeValues.push_back(current->val);
        iterateLeftMostBranch(current->right,pNodesAsc);
    }
    return nodeValues;
}
```
## heap
### Min Heap
Min heap essentially is binary tree.
it allows quick retrieval and removal of minimum elements.
in C++, Priority Queue can be used to make min or max heap.
e.g. min map of integer:
` priority_queue<int, vector<int>, greater<int>> pq;`

# Common Algorithms
## Sort
### [Merge Sort](https://medium.com/karuna-sehgal/a-simplified-explanation-of-merge-sort-77089fe03bb2)
- Time: O(nlog(n))
- Space: O(n)
### [Insertion Sort](https://www.geeksforgeeks.org/insertion-sort-algorithm/)
- Time: O(n<sup>2</sup>) average/worst case; O(n) if the list is already sorted
- Space: O(n)
### Other Sorts
[Gap Method](https://www.geeksforgeeks.org/efficiently-merging-two-sorted-arrays-with-o1-extra-space/): a variation of Shell/Bubble, used to efficiently merge 2 sorted arrays).
- Time: O(nlog(n))
- Space: O(1)
## Search
### Binary Search
- can be implemented in [iterative approach](https://www.freecodecamp.org/news/binary-search-in-c-algorithm-example/) or recursive approach.
- Time: O(log(n))
- Space: O(1)
# Other Topics
## Big O Notation
- [StackOverFlow: Big O, how do you calculate/approximate it?](https://stackoverflow.com/questions/3255/big-o-how-do-you-calculate-approximate-it/)
- [StackOverFlow: What is a plain english explanation of big-o-notation](https://stackoverflow.com/questions/487258/what-is-a-plain-english-explanation-of-big-o-notation)

# Named Algorithms
## Floyd's Cycle Finding Algorithm
Uses Fast pointer with 2* speed of slow pointer to identify loop.

# Resources
- [NST: Dictionary of Algorithms and Data Structures](https://xlinux.nist.gov/dads/)
- [xoax.net/ComputerScience](https://xoax.net/sub_comp_sci/)
- [Leetcode: Blind 75 question list](https://leetcode.com/discuss/general-discussion/460599/blind-75-leetcode-questions)
