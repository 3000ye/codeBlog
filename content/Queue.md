---
weight: 3
title: Queue
math: true
---

# Queue

## 387. First Unique Character in a String

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        int ls[200];

        for (auto c : s) { ls[c] ++; }

        for (int i = 0, l = s.size(); i < l; i ++) {
            if (ls[s[i]] == 1) return i;
        }

        return -1;
    }
};
```

### 题面

给定一个字符串 `s` ，找到 _它的第一个不重复的字符，并返回它的索引_ 。如果不存在，则返回 `-1` 。

### 题解

**Method One**: 直接法

- 用一个数组记录每个字符出现的次数
- 遍历字符串，找到第一个只出现一次的字符

# 933. Number of Recent Calls

```cpp
class RecentCounter {
private:
    queue<int> que;

public:
    RecentCounter() {}
    
    int ping(int t) {
        que.push(t);
        while (que.back() - que.front() > 3000) que.pop();
        
        return que.size();
    }
};
```

```c
class RecentCounter {
private:
    vector<int> ls;
    int idx;

public:
    RecentCounter() : ls(), idx(0) {}
    
    int ping(int t) {
        ls.push_back(t);

        while (t - ls[idx] > 3000) idx ++;
        return ls.size() - idx;
    }
};
```

### 题面

写一个 `RecentCounter` 类来计算特定时间范围内最近的请求。

请你实现 `RecentCounter` 类：

-   `RecentCounter()` 初始化计数器，请求数为 0 。
-   `int ping(int t)` 在时间 `t` 添加一个新请求，其中 `t` 表示以毫秒为单位的某个时间，并返回过去 `3000` 毫秒内发生的所有请求数（包括新请求）。确切地说，返回在 `[t-3000, t]` 内发生的请求数。

**保证** 每次对 `ping` 的调用都使用比之前更大的 `t` 值。

### 题解

**Method One**: 直接法

- 用队列维护 `ping` 的时间
- 每次 `ping` 操作都先让 `t` 入队，然后将不满足条件的弹出
- 返回队列长度即可

## 2073. Time Needed to Buy Tickets



### 题面

有 `n` 个人前来排队买票，其中第 `0` 人站在队伍 **最前方** ，第 `(n - 1)` 人站在队伍 **最后方** 。

给你一个下标从 **0** 开始的整数数组 `tickets` ，数组长度为 `n` ，其中第 `i` 人想要购买的票数为 `tickets[i]` 。

每个人买票都需要用掉 **恰好 1 秒** 。一个人 **一次只能买一张票** ，如果需要购买更多票，他必须走到  **队尾** 重新排队（**瞬间** 发生，不计时间）。如果一个人没有剩下需要买的票，那他将会 **离开** 队伍。

返回位于位置 `k`（下标从 **0** 开始）的人完成买票需要的时间（以秒为单位）。

### 题解

