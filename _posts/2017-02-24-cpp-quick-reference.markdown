---
layout: post
title: C++ quick reference
published: true
categories:
date: 2017-02-24T14:17:00+10:00
---

## std data structures

### string

```
s.length()
```

```
string hello = "hello";
hello.substr(0, 1); // "h"
hello.substr(2, 2); // "ll"

hello.find("h"); // 0
hello.find("e"); // 1
```

```
for (int i = 0; i < s.length(); i++) {
    char c = s[j];
}
```

```
string s = "tes";
s+='t';
s.append(1, 't');
```

### vector

```
// Size of v
v.size()

// Sort v
std::sort(v.begin(), v.end())

// Remove the last item in v
v.pop_back();

// Reverse, e.g. [1, 2, 3] to [3, 2, 1]
reverse(v.begin(), v.end())
```


To sort custom types:

```
struct MyStruct
{
    int key;
    std::string stringValue;

    MyStruct(int k, const std::string& s) : key(k), stringValue(s) {}

    bool operator < (const MyStruct& str) const
    {
        return (key < str.key);
    }
};
```

### set

```
// Convert to vector
vector<string> set;
vector<string> v;
copy(set.begin(), set.end(), back_inserter(v));
```

### unordered_map

```
std::unordered_map<std::string, uint32_t> umap = {
    {"Alice", 23},
    {"Sam", 25},
    {"Thomas", 27}
};
```

```
// Check if an element exists in map
if (umap.find("Harry") == umap.end()) // does not exist

// Clear map
umap.clear()
```

```
unordered_map<int, ListNode *> map;
// map.find(1) == umap.end()

map[1] = node;
// map.find(1) != umap.end()

map[1] = NULL;
// map.find(1) != umap.end()

map.erase(1);
// map.find(1) == umap.end()
```

### stack

```
stack<int> s;

s.push(1);
s.push(2);

int t = s.top(); // ==2

s.pop();

s.top(); // ==1
```

## Constants

```
// Mutable pointer to a mutable character
char *p;

// Mutable pointer to a constant character
const char *p;

// Constant pointer to a mutable character
char * const p; 

// Constant pointer to a constant character
const char * const p;
```

```
const int n;
```

## Closures

```
string orig;
string str;

// `-> bool` can be omitted
auto pushToStr = [&] (int i) -> bool {
    if (i<orig.size()) {
        str.push_back(orig[i]);
        return true;
    }
    return false;
};
```
