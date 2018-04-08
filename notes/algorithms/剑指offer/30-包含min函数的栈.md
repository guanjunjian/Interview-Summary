# 30-包含min函数的栈

**题目描述**

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的min函数。在该栈中，调用min、push及pop的时间复杂度都是O(1)。

**思路分析**

使用一个辅助栈来保存目前栈内的最小值。

栈每新添加一个元素，与辅助栈中的最小值进行比较

- 如果新值比辅助栈栈顶要小，则在辅助栈中压入新值
- 否则，再次在辅助栈中压入辅助栈的栈顶

**我的实现**

```c++
#include <assert.h>
class Solution {
public:
	void push(int value) {
        st.push(value);
        if (minSt.size() == 0 || value <= minSt.top())
			minSt.push(value);
        else
		    minSt.push(minSt.top());
	}
	void pop() {
		assert(st.size() && minSt.size());
		st.pop();
		minSt.pop();
	}
	int top() {
		assert(st.size());
		return st.top();
	}
	int min() {
		assert(st.size() && minSt.size());
		return minSt.top();
	}
	stack<int> st;
	stack<int> minSt;
};
```

