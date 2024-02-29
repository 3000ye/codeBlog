---
weight: 2
title: Stack
math: true
---

# Stack

## 20. Valid Parentheses

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;

        for (auto c : s) {
            if (stk.size() == 0) stk.push(c);
            else if (stk.top() == '(' and c == ')') stk.pop();
            else if (stk.top() == '[' and c == ']') stk.pop();
            else if (stk.top() == '{' and c == '}') stk.pop();
            else stk.push(c);
        }

        return stk.size() == 0 ? true : false;
    }
};
```

### 题面

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1.  左括号必须用相同类型的右括号闭合。
2.  左括号必须以正确的顺序闭合。
3.  每个右括号都有一个对应的相同类型的左括号。

### 题解

**Method One**: 直接法

- 遍历字符串，如果栈为空则入栈
- 如果栈不为空，则判断栈头和当前字符是否匹配
    - 如果匹配，则栈弹出
    - 否则入栈
- 最终判断栈是否为空，如果为空则合法

## 225. Implement Stack using Queues

```cpp
class MyStack {
private:
    int stk[200];
    int head;
public:
    MyStack() : head(-1) {}

    void push(int x) {
        stk[++ head] = x;
    }
    
    int pop() {
        return stk[head --];
    }
    
    int top() {
        return stk[head];
    }
    
    bool empty() {
        return head == -1;
    }
};
```

### 题面

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1.  左括号必须用相同类型的右括号闭合。
2.  左括号必须以正确的顺序闭合。
3.  每个右括号都有一个对应的相同类型的左括号。

### 题解

**Method One**: 数组模拟

- 数据范围 `100`，则直接开一个长度足够的数组
- 通过下标 `head` 来模拟入栈和弹出等

## 496. Next Greater Element I

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        stack<int> stk;
        int flag[10010];
        memset(flag, -1, sizeof(flag));

        for (auto i : nums2) {
            while (!stk.empty() and stk.top() < i) {
                int x = stk.top();
                flag[x] = i;
                stk.pop();
            }

            stk.push(i);
        }

        vector<int> res(nums1.size(), -1);
        for (int i = 0, l = nums1.size(); i < l; i ++) {
            res[i] = flag[nums1[i]];
        }

        return res;
    }
};
```

### 题面

`nums1` 中数字 `x` 的 **下一个更大元素** 是指 `x` 在 `nums2` 中对应位置 **右侧** 的 **第一个** 比 `x` 大的元素。

给你两个 **没有重复元素** 的数组 `nums1` 和 `nums2` ，下标从 **0** 开始计数，其中`nums1` 是 `nums2` 的子集。

对于每个 `0 <= i < nums1.length` ，找出满足 `nums1[i] == nums2[j]` 的下标 `j` ，并且在 `nums2` 确定 `nums2[j]` 的 **下一个更大元素** 。如果不存在下一个更大元素，那么本次查询的答案是 `-1` 。

返回一个长度为 `nums1.length` 的数组 `ans` 作为答案，满足 `ans[i]` 是如上所述的 **下一个更大元素** 。

### 题解

**Method One**: 单调栈

- 我们需要在 `nums2` 中找到 `x` 右侧比 `x` 大的数字 `y`
- 构造一个单调栈，遍历 `nums2`，记录被弹出的元素的值 `x` 和新插入的 `y`
- 这些元素会被弹出，说明 `x` 的右侧存在比 `x` 大的 `y`
- 最后遍历 `nums1` 找到每个 `x` 对应的 `y`

注意：需注意数据范围 `[0, 1e4]` 开足够长的数组和下标的记录。

## 682. Baseball Game

```cpp
class Solution {
public:
    int calPoints(vector<string>& operations) {
        stack<int> stk;

        for (auto s : operations) {
            if (s == "+") {
                int x = stk.top(); stk.pop();
                int y = stk.top(); stk.pop();

                stk.push(y); stk.push(x); stk.push(x + y);
            }
            else if (s == "D") stk.push(stk.top() * 2);
            else if (s == "C") stk.pop();
            else stk.push(std::stoi(s));
        }

        int sum = 0;
        while (!stk.empty()) {
            sum += stk.top();
            stk.pop();
        }

        return sum;
    }
};
```

### 题面

你现在是一场采用特殊赛制棒球比赛的记录员。这场比赛由若干回合组成，过去几回合的得分可能会影响以后几回合的得分。

比赛开始时，记录是空白的。你会得到一个记录操作的字符串列表 `ops`，其中 `ops[i]` 是你需要记录的第 `i` 项操作，`ops` 遵循下述规则：

1.  整数 `x` - 表示本回合新获得分数 `x`
2.  `"+"` - 表示本回合新获得的得分是前两次得分的总和。题目数据保证记录此操作时前面总是存在两个有效的分数。
3.  `"D"` - 表示本回合新获得的得分是前一次得分的两倍。题目数据保证记录此操作时前面总是存在一个有效的分数。
4.  `"C"` - 表示前一次得分无效，将其从记录中移除。题目数据保证记录此操作时前面总是存在一个有效的分数。

请你返回记录中所有得分的总和。

### 题解

**Method One**: 直接法

- 用栈模拟题面的操作

## 844. Backspace String Compare

```cpp
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        stack<char> x, y;
        for (auto c : s) {
            if (c == '#') {
                if (!x.empty()) x.pop();
            }
            else x.push(c);
        }

        for (auto c : t) {
            if (c == '#') {
                if (!y.empty()) y.pop();
            }
            else y.push(c);
        }

        return x == y;
    }
};
```

### 题面

给定 `s` 和 `t` 两个字符串，当它们分别被输入到空白的文本编辑器后，如果两者相等，返回 `true` 。`#` 代表退格字符。

**注意：**如果对空文本输入退格字符，文本继续为空。

### 题解

**Method One**: 直接法

- 遍历字符串，将其文本记录在栈中
- 判断两个栈是否相等