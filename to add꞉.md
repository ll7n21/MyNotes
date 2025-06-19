---
title: 'to add:'
created: '2024-12-14T09:29:37.553Z'
modified: '2025-06-19T08:21:14.842Z'
---

# to add:
unordered_map.insert({k,v})
# nullptr vs NULL
`NULL` is a macro expanded to `0` or `0L` whch can be serve as both an intergral type and a pointer.  
If `NULL` is `0L` on your compiler, When you have a function that can receive pointers to more than one type (e.g. `void f(int); void f(char *)`), `f(0L)` is ambiguous.  
`nullptr` can't be assigned to an integral type such as int but oinly a pointer type.  
Ref:
[Alex Allain: Better types in C++11: nullptr, enum classes and cstdint](https://www.cprogramming.com/c++11/c++11-nullptr-strongly-typed-enum-class.html)

# Inline Function
https://www.geeksforgeeks.org/inline-functions-cpp/

# const
https://softwareengineering.stackexchange.com/questions/332820/in-c-c-should-i-use-const-in-parameters-and-local-variables-when-possible


# Type Conversion
- [Explicit Type Conversion](https://en.cppreference.com/w/cpp/language/explicit_cast)


# [#ifndef v.s. #pragma once](https://stackoverflow.com/questions/1143936/pragma-once-vs-include-guards)
- [include guards](https://wiki.c2.com/?RedundantIncludeGuards): A standard preprocessor technique of preventing a header file from being included multiple times, this prevents redefinition errors.
- #pragma once: widely supoorted, not standard.
- [Why separate .h file and .cpp file?](https://stackoverflow.com/a/3247093)

# Reference
Reference is bound to an object, it can never be reassigned. When `=`is used on an reference, the right value is assigned to the object, not the reference.
```cpp
int a = 3, b = 4;
int& ar = a;
ar = b;
cout << a << endl; //4
cout << ar << endl; //4
cout << &a << endl; //0000000C062FF7E4
cout << &ar << endl; ////0000000C062FF7E4
```

## [pass by confernece or value to function?](https://stackoverflow.com/questions/2139224/how-should-i-pass-objects-to-functions)

# String
string.find()
string.substr()
stoi()
- [Converrt String to Integer](https://www.geeksforgeeks.org/convert-string-to-int-in-cpp/)
- `find()`; `find_first_of()`
- iterate string: `for (char& ch : s)`
- `void resize(size_t n, char c)`: can be used to shorten string to first n chars or pad string with specified character at the end.
## substr
`string substr(size_t pos=0, size_t len=npos) const` e.g. `"01234".substr(0,3) -> "012"

if len is not given then till the end of the string.
or can construct string with iterator: 
`std::string s7 (s0.begin(), s0.begin()+7);`

# STL
## vector
dynamic array, suits scenario that needs random access elements frequently, performance is down when inserting or deleting elements in the middle.
- constructor:
  - `vector(size_type count, const T& value)` e.g.
    - `vector<int> a(3,v)`-> a: [v,v,v]
    - `vector<vector<int>> b(2, vector<int>(3,v))`-> b: [[v,v,v],[v,v,v]]
  - `vector<int> a{1,2,3,4}` -> a: [1,2,3,4]
  - cannot construct uisng reverse iterator, will cause compilation error.
- append another vector to the end:
  - `a.append_range(b)` (C++ 23)
  - `a.insert_range(a.end(),b)` (C++23)
  - `a.insert(a.end(),b.cbegin(),c.cend())`
- resize
  - `a.rezie(5)`-> resize vector to size 5
  - `a.resize(15,0)`-> resize vector to size 15, initialize newly added elements to 0  
- comparison
  - it is ok to compare two vector elementwise using `vector1 == vector2`, time complexity is O(1). 
## list
bidirectional linked list, suits scenario that needs frequent insertion or deletion of elements in the middle, does not support random access.

## forward_list
simple unidirectional linked list. performance is good for insertion and deletion at front end, does not support reverse traversal.


## queue

### priority queue
Use priority to make min heap: ` priority_queue<int, vector<int>, greater<int>> pq;` (smallest on top)
the use of comparator sounds counter-intuitive -- using greater() for min-element-top-priority while usually less() comparator is used for ascending sort. [Reason](https://stackoverflow.com/questions/32748069/the-reason-of-using-stdgreater-for-creating-min-heap-via-priority-queue)

### deque
double-ended queue, suits scenario that needs fequently insertion or deletion of elements at both head and tail ends. Support random access.
- `a.push_front(e); a.pop_front(e)`
- `a.push_back(e); a.pop_back(e)`


## map vs unordered_map
map is sorted by key increasingly.
unordered_map suits situations when fast eccess to elements is desired and order of elements does not matter.
## unordered_map
- check if a key exists: `a.contains(k)` or `a.find(k)!=a.end()`.
- [at() vs find()](https://stackoverflow.com/questions/38734808/use-of-find-vs-at-in-map-unordered-map): use at() when you are certain this key exists in the map, find() when this key may or may not exists in the map.
- [using pair<int,int> as key will cause error](https://medium.com/@gulshansharma014/call-to-implicitly-deleted-default-constructor-of-unordered-map-pair-int-int-int-d3b2a6da0b41) because `unordered_map` requires the key type to be hashable and have well-defined equality operator.
- if need to modify content while looping, need to use for `auto&` instead of `auto`. [ref](https://stackoverflow.com/questions/45743126/how-come-iterating-through-unordered-map-but-cannot-change-its-mapped-value)
## set
* `erase(iterator first, iterator second)`: parameters are iterator to this set object. Passing iterators to another set would not work, this is not the way to get set difference, check out `set_difference()` instead.
* `set` vs `unordered_set`: `set` is implemented with binary search tree, elements are stored in increasing order by default; `unordered_set` is implmented with hash table.

## pair
- `#include <utility>`
- can assign both values in one statement via:
  - `pair<int, int> p = make_pair(v1, v2)`
  - `pair<int,int> p {v1, v2}`;
- operator `==` distiguishes first and second elements, hence `pair{1,2} != pair{2,1}`.

## iterator
- Why prefer `!=container.end()` over `<container.end()` in loop?  
Because <ins>operators`<`,`>`can only be used with RandomAccessIterator</ins>, while operator`!=`can also be used with InputIterator, ForwardIterator, and BidirectionalIterator.  
Ref:[Stackoverflow: Why is != used with iterator instead of <?](https://stackoverflow.com/a/6673775/2405914)
- Caution with using erase() when iterating
Because once call erase(it), the iterator will be unable to use.
Ref:[StackOverflow: What happens to an STL iterator after erasing it](https://stackoverflow.com/questions/433164/what-happens-to-an-stl-iterator-after-erasing-it-in-vs-unix-linux)
e.g.
```
vector<int>::iterator it = l.begin();
while (it != l.end())
    l.erase(it);//wrong, it will be unable to use and comparing it with l.end() cause error
    l=l.erase(it) //correct, since erase() return a new iterator in the updated container
```


# \<algorithm\>
## std::binary_search()
- time: O(log(n))
- only works on sorted data, returns bool.
- If want to search unsorted data or need the location of the item, use `std::find()` or the container's built-in `find()`instead.

## std::reverse()
- `void reverse(iterator first, iterator last)` reverse elements in `[first, last)`

## set_difference()
To get the difference between two sets (a-b):
```
std::set<int> result;
std::set_difference(a.begin(),a.end(),b.begin(),b.end(),inserter(result,result.end()));
```
# \<Utility\>
## std::exchange
`T exchange( T& obj, U&& new_value)` replaces obj with new value and returns the old value
```
doSomething(exchange(a,b))
//is equal to
doSomething(a)
a=b
``````


# IO
## printf, fprintf, snprintf
https://stackoverflow.com/questions/4627330/difference-between-fprintf-printf-and-sprintf
## read until eof
```
#include <fstream>
#include <sstream>
ifstream file("input.txt", ios::in);
string line;
int a, b;
while (getline(file, line)) {
	istringstream ss(line);
	ss >> a >> b;//any variables to read from the line
}
```

