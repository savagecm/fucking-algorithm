# 如何判定括号合法性

对括号的合法性判断是一个很常见且实用的问题，比如说我们写的代码，编辑器和编译器都会检查括号是否正确闭合。而且我们的代码可能会包含三种括号 `[](){}`，判断起来有一点难度。

本文就来聊一道关于括号合法性判断的算法题，相信能加深你对**栈**这种数据结构的理解。

题目很简单，输入一个字符串，其中包含 `[](){}` 六种括号，请你判断这个字符串组成的括号是否合法。

```text
Input: "()[]{}"
Output: true

Input: "([)]"
Output: false

Input: "{[]}"
Output: true
```

解决这个问题之前，我们先降低难度，思考一下，**如果只有一种括号 `()`**，应该如何判断字符串组成的括号是否合法呢？

## 一、处理一种括号

字符串中只有圆括号，如果想让括号字符串合法，那么必须做到：

**每个右括号 `)` 的左边必须有一个左括号 `(` 和它匹配**。

比如说字符串 `()))((` 中，中间的两个右括号**左边**就没有左括号匹配，所以这个括号组合是不合法的。

那么根据这个思路，我们可以写出算法：

```cpp
bool isValid(string str) {
    // 待匹配的左括号数量
    int left = 0;
    for (char c : str) {
        if (c == '(')
            left++;
        else // 遇到右括号
            left--;

        if (left < 0)
            return false;
    }
    return left == 0;
}
```

如果只有圆括号，这样就能正确判断合法性。对于三种括号的情况，我一开始想模仿这个思路，定义三个变量 `left1`，`left2`，`left3` 分别处理每种括号，虽然要多写不少 if else 分支，但是似乎可以解决问题。

但实际上直接照搬这种思路是不行的，比如说只有一个括号的情况下 `(())` 是合法的，但是多种括号的情况下， `[(])` 显然是不合法的。

仅仅记录每种左括号出现的次数已经不能做出正确判断了，我们要加大存储的信息量，可以利用栈来模仿类似的思路。

## 二、处理多种括号

栈是一种先进后出的数据结构，处理括号问题的时候尤其有用。

我们这道题就用一个名为 `left` 的栈代替之前思路中的 `left` 变量，**遇到左括号就入栈，遇到右括号就去栈中寻找最近的左括号，看是否匹配**。

```cpp
bool isValid(string str) {
    stack<char> left;
    for (char c : str) {
        if (c == '(' || c == '{' || c == '[')
            left.push(c);
        else // 字符 c 是右括号
            if (!left.empty() && leftOf(c) == left.top())
                left.pop();
            else
                // 和最近的左括号不匹配
                return false;
    }
    // 是否所有的左括号都被匹配了
    return left.empty();
}

char leftOf(char c) {
    if (c == '}') return '{';
    if (c == ')') return '(';
    return '[';
}
```

坚持原创高质量文章，致力于把算法问题讲清楚，欢迎关注我的公众号 labuladong 获取最新文章：

![labuladong](../.gitbook/assets/labuladong.jpg)

[张三](https://github.com/savagecm/fucking-algorithm/tree/74a3d0667e10a50b9b4b0f58ebf1f33c326ee0bc/高频面试系列/any_link_you_want/README.md) 提供 Java 代码：

```java
public boolean isValid(String str) {
    // ...
}

private char leftOf(char c) {
    // ...
}
```

[kalok87](https://github.com/kalok87) 提供 Python3 代码：

```python
def isValid(self, s: str):
        left = [] # 定义一个左栈，记录所有的左括号
        match = {'}':'{', ']':'[', ')':'('} # 定义一个字典，检查当前str是否是右括号
        right = {'}', ']', ')'} # 定义一个右括号集合，方便快速检查

        # 进行循环，如果当前str是左括号，则入栈；如果是右括号，则检查左栈的最后一个元素是不是
        # 与其对应。
        for x in s:
            if x in right:
                if len(left) == 0 or match[x] != left[-1]: 
                    return(False) # 如果对应的左栈元素不符（括号种类不同或左栈为空），返回False
                else:
                    left.pop() # 移除左栈最顶端的元素
            else:
                left.append(x) # 当前str是左括号，入左栈

        if len(left) == 0:
            return(True) # 如果左栈为空（左右括号数相等），返回True
        else:
            return(False)
```

[上一篇：如何k个一组反转链表](k-ge-yi-zu-fan-zhuan-lian-biao.md)

[下一篇：如何寻找消失的元素](xiao-shi-de-yuan-su.md)

[目录](../#目录)
