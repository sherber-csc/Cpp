## C++基础

https://www.w3schools.com/cpp/default.asp（学习网址，基础部分不在记录，直接上网站查找使用）

### 1.常量

`const` 和 `#define` 都用于定义常量的区别

| 特性         | `const`        | `#define`        |
| ------------ | -------------- | ---------------- |
| **类型检查** | 是             | 否               |
| **作用域**   | 遵循作用域规则 | 全局有效         |
| **调试支持** | 可见符号和值   | 不可见           |
| **使用建议** | 定义常量       | 定义宏或编译选项 |

### 2.原码、反码和补码

**原码、反码、补码**是计算机中表示有符号数的三种不同编码方式，它们主要用于整数的存储和运算，尤其是在二进制运算中。

------

#### **2.1. 原码（Sign-Magnitude）**

- **定义**：直接使用**最高位作为符号位**（0 表示正数，1 表示负数），其他位表示绝对值。

- 特点

  ：

  - 最高位是符号位，剩余部分表示数值的大小。
  - **0 有两种表示方式**（+0 和 -0）。
  - **加减运算较复杂**，需要区分正负号并进行处理。

🔹**示例（8位二进制）**：

- +5+5 → `0000 0101`
- −5-5 → `1000 0101`

------

#### **2.2. 反码（Ones' Complement）**

- **定义**：**正数的反码与原码相同**，**负数的反码是对其原码取反（符号位不变，其他位取反）**。

- 特点

  ：

  - **0 仍然有两种表示方式**（`0000 0000` 和 `1111 1111`）。
  - **加法运算复杂**，需要额外处理进位。

🔹**示例（8位二进制）**：

- +5+5 → `0000 0101`
- −5-5（原码 `1000 0101` 取反）→ `1111 1010`

------

#### **2.3. 补码（Two's Complement）**

- 定义

  ：

  - **正数的补码与原码相同**。
  - **负数的补码等于其反码加 1**（符号位不变，其他位取反后加 1）。

- 特点

  ：

  - **唯一的零**（`0000 0000`）。
  - **运算简单**（加法、减法都可以直接使用二进制加法处理）。
  - **现代计算机普遍采用补码表示整数**。

🔹**示例（8位二进制）**：

- +5+5 → `0000 0101`

- −5-5

  ：

  1. **原码**：`1000 0101`
  2. **反码**：`1111 1010`
  3. **补码**（反码 +1）：`1111 1011`

------

#### **2.4. 计算机为何使用补码？**

1. **解决了 0 的双重表示问题**（补码只有一个 `0000 0000`）。

2. **加减法运算统一**（加法和减法都可以直接用二进制加法处理）。

3. 补码的表示范围更合理

   ：

   - 8 位补码范围：`-128`（`1000 0000`）~ `+127`（`0111 1111`），比原码和反码多了一个负数。

------

#### **2.5. 总结**

| 方式     | 正数表示                 | 负数表示                     |
| -------- | ------------------------ | ---------------------------- |
| **原码** | 与绝对值相同，最高位为 0 | 最高位 1，其他位与绝对值相同 |
| **反码** | 与原码相同               | 符号位不变，其他位取反       |
| **补码** | 与原码相同               | 反码 +1                      |

**补码是计算机实际使用的二进制表示方式**，因为它能使加减法运算更简单、高效。

### 3.运算符

在 C++ 中，**位运算**（Bitwise Operations）是针对二进制位进行操作的运算符。常见的有 **按位与（&）、按位或（|）、按位异或（^）**，它们的作用如下：

#### 3.1.位与运算符 `&`

**定义**：
 按位与运算符 `&` 会对两个整数的二进制位进行 **逐位与**（AND）运算。

- 只有当两个对应位 **都为 1** 时，结果才为 1，否则为 0。

**示例**：

```cpp
#include <iostream>
using namespace std;

/*
     &     有0必0
	 &&    有假必假
*/

int main() {
	// 1. 位与运算符的定义
	int a = 0b1010;   // 10
	int b = 0b0110;   // 6
	//      0b0010;   // 2
	cout << (a & b) << endl;
	cout << "--" << endl;

	// 2. 奇偶性
	cout << 5 % 2 << endl;      // % 优先级高、效率高
	cout << (5 & 1) << endl;    // & 优先级低、效率高
	//   0b101
	//   0b001
	cout << "--" << endl;

	// 3. 获取一个数二进制的末5位
	int c = 0b1010010101001;  // 后五位 01001
	cout << (c & 0b11111) << endl;
	cout << "--" << endl;

	// 4. 将末五位归零
	int d = 0b11111111111111111111111111100000;
	cout << (c & d) << endl; // 0b1010010100000
	cout << "--" << endl;
 
	// 5. 消除末尾连续的1
	int e = 0b101010111111;
	// e+1= 0b101011000000;
	//  & = 0b101010000000;
	cout << (e & (e + 1)) << endl;

	// 6. 2的幂判定
	int f = 0b100000000;
	// f-1= 0b011111111;
	//   &= 0b000000000 = 0;
	(f > 0) && ((f & (f - 1)) == 0);
    
	return 0;
}
```

**解释**：

```
  0101   (5)
& 0011   (3)
--------
  0001   (1)
```

✅ **用途**：

- 用于 **清零某些位**（`x & 0 = 0`）。
- 检测二进制位是否为 1（如 `if (x & (1 << k))` 检查第 k 位是否为 1）。

#### 3.2. 按位或运算符 `|`

**定义**：
 按位或运算符 `|` 进行 **逐位或**（OR）运算。

- 只要 **任意一个对应位是 1**，结果就是 1，否则为 0。

**示例**：

```cpp
#include <iostream>
using namespace std;

/*
      |    位或  ：有1即1
	  ||   逻辑或：有真即真
*/

int main() {
	// 1. 位或的定义
	int a = 0b1010;     // 10
	int b = 0b0110;     // 6
	//  | = 0b1110      // 14
	cout << (a | b) << endl;
	cout << "---" << endl;

	// 2. 设置标记位
	int c = 0b100111;
	//      0b101111;
	cout << (c | (0b1000)) << endl;
	cout << "---" << endl;

	// 3. 置空标记位
	// 0b100111
	int d = 0b000001;
	cout << ((c | d) - d) << endl;
	cout << "---" << endl;

	// 4. 低位连续0变成1
	int e = 0b1010010000;
	// e-1= 0b1010001111;
	//   -> 0b1010011111;
	int f = 0b1010011111;
	cout << f << endl;
	cout << (e | (e - 1)) << endl;


	return 0;
}
```

**解释**：

```
  0101   (5)
| 0011   (3)
--------
  0111   (7)
```

✅ **用途**：

- 用于 **设置某些位为 1**（`x | 1 = 1`）。
- 用于组合标志位，如权限控制。

#### 3.3.按位异或运算符 `^`

**定义**：
 按位异或运算符 `^` 进行 **逐位异或**（XOR）运算。

- 只有 **两个对应位不同** 时，结果才为 1，相同则为 0。

**示例**：

```cpp
#include <iostream>
using namespace std;

/*
       ^  异或
*/
int main() {
	// 1. 异或的定义
	int a = 0b1010;
	int b = 0b0110;
	//  ^ = 0b1100;
	cout << (a ^ b) << endl;
	cout << "---" << endl;

	// 2. 标记位取反
	int c = 0b1000101;
	//      0b0001000
	cout << c << endl;
	cout << (c ^ 0b1000) << endl;
	cout << ((c ^ 0b1000) ^ 0b1000) << endl;
	cout << "---" << endl;

	// 3. 变量交换
	int d = 17;
	int e = 19;
	d = d ^ e;
	e = d ^ e;  // = d' ^ e = d ^ e ^ e = d ^ 0 = d
	d = d ^ e;  // = d' ^ d = d ^ e ^ d = d ^ d ^ e = 0 ^ e = e
	cout << d << ' ' << e << endl;
	cout << "---" << endl;
	// 3.1 任何数和0异或，还是它本身
	// 3.2 两个相同的数异或，结果为0
	// 3.3 异或满足交换律和结合律
	// 异或：不带进位的二进制加法


	// 4. 出现奇数次的数
	
	// 5. 加密
	int x = 1314;
	cout << "520" << x << endl;
	int y = (x ^ 3135);
	cout << "520" << y << endl;
	cout << "520" << (y^3135) << endl;

	return 0;
}
```

**解释**：

```
  0101   (5)
^ 0011   (3)
--------
  0110   (6)
```

✅ **用途**：

- 用于 **翻转某些位**（`x ^ 1` 翻转 x 的最后一位）。

- 交换两个数而不使用临时变量

  ：

  ```cpp
  int a = 5, b = 3;
  a = a ^ b;
  b = a ^ b;
  a = a ^ b;
  ```

- **检测两个数是否相等**（`a ^ b == 0` 表示 a 和 b 相等）。

------

**总结**

| 运算符 | 作用     | 规则                                                         |
| ------ | -------- | ------------------------------------------------------------ |
| `&`    | 按位与   | 1 & 1 = 1，其他情况 = 0                                      |
| `      | `        | 按位或                                                       |
| `^`    | 按位异或 | **相同为 `0`，不同为 `1`**（`0 ^ 0 = 0`，`1 ^ 1 = 0`，`0 ^ 1 = 1`，`1 ^ 0 = 1`） |

位运算在**硬件编程、加密算法、优化算法**等领域非常有用，掌握这些可以提高你的代码效率！🚀



