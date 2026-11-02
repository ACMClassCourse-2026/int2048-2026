# int2048

高精度计算是一种程序设计的算法。由于中央处理器的字长限制，如 32 位 CPU 中一个整数最大只能取值 4,294,967,295。因此在进行更大范围的数值计算中，往往要采取模拟手段。通常通过分离字符的方法通过数字数组进行输入、通过数组倒序输出、通过模拟竖式计算进行计算。一般而言，主要模拟的是按位运算，可以用不同的进位制达成不同的目的。

本次作业要求用 C++ 实现整数的高精度加减乘除。

### 实现要求

要求实现一个 C++ 的大整数类，命名为 `int2048`。

项目的文件结构如下
```
├── CMakeLists.txt
├── README.md
├── data
│   ├── Integer1
│   └── Integer2
└── src
    ├── int2048.cpp
    └── include/int2048.h
```

其中，本次大作业所有的源代码都在 `src` 文件夹下。

允许使用的标准库头文件为 `<complex>`、`<cstdio>`、`<cstring>`、`<iostream>`、`<string>`、`<vector>`。

`src/include/int2048.h` 文件给出了本次作业中 `int2048` 大整数类所需要实现的所有函数接口。

你需要在 `src/include/int2048.h` 文件中为 `int2048` 大整数类设计成员变量。

考虑到在 C++ 的程序设计中，将类的成员函数接口与实现分离，是一个良好的设计习惯，所以，我们**建议**你在 `src/int2048.cpp` 文件中实现在 `src/include/int2048.h` 中声明的所有成员函数。

当然，你也可以在**兼容当前所有接口**的基础之上，自己添加新的接口或修改原有接口（例如实现 `swap` 函数、右值构造函数）。

#### 一些特殊要求

**本题的输出不接受前导0，请你在输出时确保你已经去除所有的前导0。**

**如果一个大整数的值为0，请输出0。本题不接受-0作为输出。**

**你不需要考虑除数或模数为 0 的情况。在该情况下，程序的行为属于未定义行为。我们保证测试点中不会出现这样的数据。**

在我们的大整数类的实现中，对于除法操作 `x / y` ，其结果总是向下取整（即类似 python 中整除操作的表现）:

```python
# 这是一段 python 代码
print(10 // 3, -10 // 3, 10 // -3, -10 // -3)
# 输出 3 -4 -4 3
# 因为 -10 / -3 = 3.333... 向下取整即为 3
# 而 10 / -3 = -3.333... 向下取整即为 -4
```

需要注意的是，C++ 中，[有符号整数的除法](https://en.cppreference.com/w/cpp/language/operator_arithmetic)是向 0 取整。

```C++
// 这是一段 C++ 代码
std::cout
    << 10 / 3   << ' '
    << 10 / -3  << ' '
    << -10 / 3  << ' '
    << -10 / -3 << '\n';
// 输出 3 -3 -3 3
// 因为 -10 / -3 = 3.333 向 0 取整即为 3
// 而 10 / -3 = -3.333... 向 0 取整即为 -3
```

而对于取模操作 `x % y` ，其被定义为 `x % y = x - (x / y) * y` 。

### 关于提交

由于功能的限制，OJ 系统每次只能接受一个头文件。因此，在提交程序前你需要做如下的操作：

假设你的 `src/include/int2048.h` 有如下结构
```c++
#pragma once
#ifndef SJTU_BIGINTEGER
#define SJTU_BIGINTEGER

// your hpp source code

#endif
```

以及你的 `src/int2048.cpp` 有如下结构
```c++
#include "int2048.h"

// your cpp source code

```

那么在提交时，请新创建一个 `submit.h` 文件，结构如下：
```c++
#pragma once
#ifndef SJTU_BIGINTEGER
#define SJTU_BIGINTEGER

// your hpp source code

// your cpp source code

#endif
```

并将其复制粘贴到 OJ 上提交即可。

### 本地测试

仓库已经准备好了批量测试。完成代码后，按下面的步骤操作即可。

第一步，生成构建文件（只需要执行一次）。其中 `Release` 表示开启编译优化，测试会跑得更快：

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
```

第二步，编译所有测试程序：

```bash
cmake --build build
```

第三步，运行全部测试：

```bash
ctest --test-dir build
```

终端只会显示每个测试是 `Passed` 还是 `Failed`，不会打印测试数据。全部通过时，最后会显示 `100% tests passed`。

如果有测试失败，CTest 会在最后列出失败的用例，并告诉你日志位置。例如：

```text
The following tests FAILED:
    2 - Integer1_2 (Failed)
Output from these tests are in: .../build/Testing/Temporary/LastTest.log
```

完整的测试日志在 `build/Testing/Temporary/LastTest.log`，失败用例列表在 `build/Testing/Temporary/LastTestsFailed.log`。日志可能很长，建议用 `less` 查看，不要直接用 `cat`：

```bash
less build/Testing/Temporary/LastTest.log
```

进入 `less` 后，输入 `/Integer1_2` 可以搜索对应的测试，按 `n` 跳到下一个匹配位置。日志里的 `diff` 结果中，`<` 开头的是期望输出，`>` 开头的是实际输出。

如果需要查看失败用例的详细输出，可以只重新运行失败的测试，并把输出保存到文件：

```bash
ctest --test-dir build --rerun-failed --output-on-failure > /tmp/ctest.log 2>&1
less /tmp/ctest.log
```

目前共有 23 个公开测试：`data/Integer1` 中 5 个，`data/Integer2` 中 18 个。测试名由“文件夹名_文件名”组成，例如 `data/Integer1/2.cpp` 对应的测试名是 `Integer1_2`。

只测试 Integer1 或 Integer2：

```bash
ctest --test-dir build -R Integer1
ctest --test-dir build -R Integer2
```

只测试某一个用例，例如 `data/Integer1/2.cpp`：

```bash
ctest --test-dir build -R Integer1_2
```

这里的 `-R` 表示“只运行名字中包含指定文字的测试”。修改代码后，只需要重新执行第二步和第三步；如果新增了测试文件，则需要从第一步开始重新执行。

如果某个用例失败且日志里的 `diff` 输出很长，可以单独运行它，并用 `cmp` 只看第一个不同的位置，避免把整份数据打印到终端：

```bash
./build/Integer1_2 < data/Integer1/2.in > /tmp/actual.txt
cmp data/Integer1/2.out /tmp/actual.txt
```

`cmp` 会直接指出第一个不同处的字节和行号。

需要注意：

- 公开测试只有 23 个，全部通过不代表 OJ 一定能拿满分，隐藏测试点会覆盖更多边界情况；
- 请不要修改 `data/` 下的测试文件；
- 每个测试的 10 秒超时只是为了防止程序死循环，不代表 OJ 的实际时限。

编译产生的 `build/` 目录已经被 `.gitignore` 忽略，不会影响提交。

### 评分标准

总分共占比 6\%
其中

- Integer 占比 100\%
- code review 若码风较差将酌情扣分

#### B 班

- 构造函数以及关系运算符 10\%
- 无符号高精度加减法 10\%
- 有符号高精度加减法 10\%
- 高精度乘法 10\%
- 高精度除法 15\%
- 压位高精度加、减、乘法 30\%
- 压位高精度除法 15\%
- BONUS：压位快速高精度乘法（分治乘法） 1\%
- BONUS：压位快速高精度乘法（快速傅立叶变换） 3\%
- BONUS：压位快速高精度除法（二分加速试商） 1\%
- BONUS：压位快速高精度除法（分治除法/牛顿迭代法） 3\%

#### A 班

- 构造函数以及关系运算符 10\%
- 无符号高精度加减法 5\%
- 有符号高精度加减法 5\%
- 高精度乘法 5\%
- 高精度除法 5\%
- 压位高精度加、减、乘法 30\%
- 压位高精度除法（二分加速试商） 20\%
- 压位快速高精度乘法（快速傅立叶变换） 20\%
- BONUS：压位快速高精度除法（分治除法/牛顿迭代法） 5\%

PS：BONUS 部分对代码性能也有要求，需要通过对应的测试数据点才能得到分数。且 BONUS 上限为 5\%，超过不溢出。

如果你写了 BONUS 的除法并且通过了对应的测试点，那么你将不需要实现二分加速试商除法。

对于 A 班同学，需要特别注意的是，你的 int2048 将会在之后的大作业 python 解释器中被使用。请务必保证你的代码有足够的健壮性，能够应对各种极端情况。



### 截止时间

#### B 班

第 10 周周四（11 月 19 日）19:00

#### A 班

第 9 周周四（11 月 12 日）19:00

### 数据范围

参考具体题目。

**所有测试点时限均设置为 std 耗时的 2.5 倍以上。**

### 参考资料

[OI Wiki-高精度计算](https://oi-wiki.org/math/bignum/)

[OI Wiki-快速傅里叶变换](https://oi-wiki.org/math/poly/fft/)

[OI Wiki-快速数论变换](https://oi-wiki.org/math/poly/ntt/)

[高精度基础算法](https://baobaobear.github.io/post/20210228-bigint1/)

[分治除法以及相关算法](https://baobaobear.github.io/post/20210320-bigint3/)

[分治除法](https://ridiculousfish.com/blog/posts/labor-of-division-episode-iv.html
)

[牛顿迭代法](https://www.luogu.com.cn/blog/88403/solution-p5432)
