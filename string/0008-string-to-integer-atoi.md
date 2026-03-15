# String to Integer (atoi)

**LeetCode 8 · String to Integer (atoi)**  
**难度：Medium | 方法：字符串模拟**

---

## 思路

这题考的是细节处理，按顺序处理4个模块：
1. 去空格
2. 判符号
3. 读数字
4. 检查范围

**口诀：去空格 → 判符号 → 一位一位读数字 → 乘符号 → 检查范围**

---

## 代码

```python
class Solution:
    def myAtoi(self, s: str) -> int:
        # 模块1：预处理
        s = s.lstrip()       # 去掉字符串左边的空格
        if not s:            # 如果字符串是空的，返回 0
            return 0

        # 模块2：判符号
        i = 0                # 指针从 0 开始
        sign = 1             # 符号默认为正数 1
        if s[i] == '+':      # 遇到 + ，跳过它
            i += 1
        elif s[i] == '-':    # 遇到 - ，跳过它，sign 改成 -1
            i += 1
            sign = -1

        # 模块3：读数字
        parsed = 0                        # 存解析出来的数字，初始为 0
        while i < len(s):                 # 没到字符串结尾就继续
            cur = s[i]                    # 取当前位置的字符
            if not cur.isdigit():         # 不是数字就停
                break
            parsed = parsed * 10 + int(cur)  # 旧的 × 10 + 新的
            i += 1                        # 指针右移
        parsed *= sign                    # 乘上符号

        # 模块4：检查范围
        if parsed > 2**31 - 1:           # 超过最大值
            return 2**31 - 1             # 返回 2147483647
        elif parsed < -2**31:            # 小于最小值
            return -2**31                # 返回 -2147483648
        else:
            return parsed
```

---

## 中文框架

```
1.  去掉字符串左边的空格
2.  如果字符串是空的，返回 0
3.  指针 i 从 0 开始
4.  符号 sign 默认为 1
5.  如果第一个字符是 +，指针右移
6.  如果第一个字符是 -，指针右移，sign 改成 -1
7.  parsed 初始为 0
8.  没到字符串结尾就继续循环
9.      取当前位置的字符存进 cur
10.     如果 cur 不是数字，停止
11.     parsed = 旧的 × 10 + 新的
12.     指针右移
13. 乘上符号
14. 如果超过最大值，返回最大值
15. 如果小于最小值，返回最小值
16. 否则返回 parsed
```

---

## 例子演示

`s = "   -42"`

```
步骤1：lstrip() → s = "-42"
步骤2：not s → False，继续
步骤3：i=0, sign=1
步骤4：s[0]='-' → i=1, sign=-1
步骤5：parsed=0
       i=1, cur='4', isdigit ✅ → parsed = 0*10+4 = 4,   i=2
       i=2, cur='2', isdigit ✅ → parsed = 4*10+2 = 42,  i=3
       i=3, 到结尾，退出循环
步骤6：parsed = 42 * -1 = -42
步骤7：-42 在范围内 → return -42
```

---

## 关键点

- `s.lstrip()` 只去左边空格，不去右边
- `not s` 检查字符串是否为空
- `isdigit()` 检查字符是不是数字
- `parsed * 10 + int(cur)` 是核心公式，每读一位数字往左腾一格
- `parsed *= sign` 在循环**外面**，不是里面
- 整数范围：`-2**31` 到 `2**31 - 1`，即 `-2147483648` 到 `2147483647`

---

## 常见错误

| 错误 | 正确 |
|------|------|
| `i -= 1` | `i += 1` 指针要右移 |
| `parsed *= sign` 在循环里 | 在循环**外面** |
| `if not s` 写成 `if s is empty` | Python 用 `not s` |
| `elif` 写成 `else if` | Python 用 `elif` |

---

## 时间复杂度

- Time: O(n)
- Space: O(1)
