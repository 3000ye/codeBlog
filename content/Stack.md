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

**注意**：如果对空文本输入退格字符，文本继续为空。

### 题解

**Method One**: 直接法

- 遍历字符串，将其文本记录在栈中
- 判断两个栈是否相等

## 1021. Remove Outermost Parentheses

```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        stack<char> stk;
        string res;

        for (auto c : s) {
            if (stk.empty()) stk.push(c);
            else {
                if (c == '(') {  // stk 必定不为空
                    res += c;
                    stk.push(c);
                }
                else {
                    stk.pop();
                    if (!stk.empty()) res += c;
                }
            }
        }

        return res;
    }
};
```

### 题面

有效括号字符串为空 `""`、`"(" + A + ")"` 或 `A + B` ，其中 `A` 和 `B` 都是有效的括号字符串，`+` 代表字符串的连接。

-   例如，`""`，`"()"`，`"(())()"` 和 `"(()(()))"` 都是有效的括号字符串。

如果有效字符串 `s` 非空，且不存在将其拆分为 `s = A + B` 的方法，我们称其为**原语（primitive）**，其中 `A` 和 `B` 都是非空有效括号字符串。

给出一个非空有效字符串 `s`，考虑将其进行原语化分解，使得：`s = P_1 + P_2 + ... + P_k`，其中 `P_i` 是有效括号字符串原语。

对 `s` 进行原语化分解，删除分解中每个原语字符串的最外层括号，返回 `s` 。

### 题解

**Method One**: 直接法

- 使用栈来维护括号的匹配关系，遍历字符串
- 如果栈为空则直接入栈
- 如果 `c = (` ，此时栈不可能为空，入栈，`res += c`
- 如果 `c = )` ，栈弹出栈头，此时需判断栈是否为空
    - 如果为空，则说明已经走到括号的最外面
    - 否则，`res += c`

## 1047. Remove All Adjacent Duplicates In String

```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        string res;

        for (auto c : s) {
            if (res.empty()) res.push_back(c);
            else {
                if (res.back() == c) {
                    while (!res.empty() and res.back() == c) res.pop_back();
                }
                else res.push_back(c);
            }
        }

        return res;
    }
};
```

### 题面

给出由小写字母组成的字符串 `S`，**重复项删除操作**会选择两个相邻且相同的字母，并删除它们。

在 `S` 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

### 题解

**Method One**: 直接法

- 用 `string` 模拟栈，按题意操作即可

## 1475. Final Prices With a Special Discount in a Shop

```cpp
typedef pair<int, int> pii;

class Solution {
public:
    vector<int> finalPrices(vector<int>& prices) {
        stack<pii> stk;  // 递增单调栈

        for (int i = 0, l = prices.size(); i < l; i ++) {
            if (!stk.empty() and prices[i] <= stk.top().first) {
                while (!stk.empty() and prices[i] <= stk.top().first) {
                    prices[stk.top().second] -= prices[i];
                    stk.pop();
                }
            }

            stk.push({prices[i], i});
        }

        return prices;
    }
};
```

### 题面

给你一个数组 `prices` ，其中 `prices[i]` 是商店里第 `i` 件商品的价格。

商店里正在进行促销活动，如果你要买第 `i` 件商品，那么你可以得到与 `prices[j]` 相等的折扣，其中 `j` 是满足 `j > i` 且 `prices[j] <= prices[i]` 的 **最小下标** ，如果没有满足条件的 `j` ，你将没有任何折扣。

请你返回一个数组，数组中第 `i` 个元素是折扣后你购买商品 `i` 最终需要支付的价格。

### 题解

**Method One**: 单调栈

- 维护一个递增的单调栈，并使用 `pair<value, idx>` 记录价格和下标
- 当有元素弹出时，说明遇到了更小的价格，则修改其价格 `prices[idx]`

## 1544. Make The String Great

```cpp
class Solution {
public:
    string makeGood(string s) {
        string stk;

        for (auto c : s) {
            if (stk.empty() or abs(stk.back() - c) != 32) stk.push_back(c);
            else stk.pop_back();
        }

        return stk;
    }
};
```

### 题面

给你一个由大小写英文字母组成的字符串 `s` 。

一个整理好的字符串中，两个相邻字符 `s[i]` 和 `s[i+1]`，其中 `0<= i <= s.length-2` ，要满足如下条件:

-   若 `s[i]` 是小写字符，则 `s[i+1]` 不可以是相同的大写字符。
-   若 `s[i]` 是大写字符，则 `s[i+1]` 不可以是相同的小写字符。

请你将字符串整理好，每次你都可以从字符串中选出满足上述条件的 **两个相邻** 字符并删除，直到字符串整理好为止。

请返回整理好的 **字符串** 。题目保证在给出的约束条件下，测试样例对应的答案是唯一的。

**注意：** 空字符串也属于整理好的字符串，尽管其中没有任何字符。

### 题解

**Method One**: 直接法

- 使用 `string` 来维护答案
- 遍历 `s` 时按照题意判断即可

## 1598. Crawler Log Folder

```cpp
class Solution {
public:
    int minOperations(vector<string>& logs) {
        int cnt = 0;
        for (auto i : logs) {
            if (i == "../") cnt = max(0, -- cnt);
            else if (i == "./") continue;
            else cnt ++;
        }

        return cnt;
    }
};
```

### 题面

每当用户执行变更文件夹操作时，LeetCode 文件系统都会保存一条日志记录。

下面给出对变更操作的说明：

-   `"../"` ：移动到当前文件夹的父文件夹。如果已经在主文件夹下，则 **继续停留在当前文件夹** 。
-   `"./"` ：继续停留在当前文件夹**。**
-   `"x/"` ：移动到名为 `x` 的子文件夹中。题目数据 **保证总是存在文件夹 `x`** 。

给你一个字符串列表 `logs` ，其中 `logs[i]` 是用户在 `i<sup>th</sup>` 步执行的操作。

文件系统启动时位于主文件夹，然后执行 `logs` 中的操作。

执行完所有变更文件夹操作后，请你找出 **返回主文件夹所需的最小步数** 。

### 题解

**Method One**: 直接法

- 直接按照题面模拟即可