# 判环算法

**问题**：给定一张有向图，每个点都有且只有一条出边。问是否存在环，若存在，还希望求出环的入口。如下图，B 就是环的入口，B、C、D 构成一个环。

```
A ---> B ---> C
       ^      |
       |      |
       |--D<--|
```

## Floyd 判环算法

在起点设置两个指针，每次迭代，快指针移动两条边，慢指针移动一条边。那么如果存在环，必然会在某一次迭代后，快指针和慢指针到达同一个点。

下面分析算法的正确性。我们设从起点到环入口长为 $n$，环长为 $m$，两指针从起点出发后迭代了 $S$ 轮。则两指针相遇的充要条件是 $S \ge n$ 且 $m|S$。而无论 $n$ 与 $m$ 的大小关系如何，从 $n$ 到 $n+m$ 的数中必然有一个数是 $m$ 的倍数。因此至多迭代 $n+m$ 次，两个指针就会相遇，从而也可推出算法的时间复杂度为 $O(n+m)$。

现在我们希望找到环的入口。一种方式是令其中一个指针回到起点，然后两个指针**以相同速度**移动多次后两者会在环入口相遇。正确性在于 $S + n \equiv n \pmod m$。

以 Leetcode 142 为例，如果有环返回环的入口，无环返回 `nullptr`。实现如下：
```cpp
ListNode *detectCycle(ListNode *head) {
    if (head == nullptr) return nullptr;
    ListNode *slow = head, *fast = head;
    for (; ; ){
        fast = fast->next;
        if (fast == nullptr) return nullptr;
        fast = fast->next;
        if (fast == nullptr) return nullptr;
        slow = slow->next;
        if (slow == fast) break;
    }
    fast = head;
    while (fast != slow)
        fast = fast->next,
        slow = slow->next;
    return slow;
}
```

## Brent 判环算法

Brent 判环算法和 Floyd 类似，也有两个指针，一个快一个慢，但这里快慢指针不是同步移动的。步骤如下：

1. 设定 $i=0$。
2. 快指针先走 $2^i$ 条边，如果中途碰到了慢指针就表明有环。
3. 慢指针瞬移到快指针位置，取 $i := i + 1$，回到 2。

比较容易看出这一算法的时间复杂度也是线性的。

<div id="refer-anchor"></div>

## 参考资料

1. [Cycle_detection - Wikipedia](https://en.wikipedia.org/wiki/Cycle_detection)