# 简单介绍

printf能够输出一段字符串，而变量以"% + ..."占位符的形式嵌入于字符串中。
以下是一个简单的例子：
```c
int age = 10;
printf("Your age is %d", age);
```
The output is: Your age is 10
以下是更复杂的用法:

# 占位符的格式

%\[_parameter_]\[_flags_]\[_width_]\[._precision_]\[_length_]_type_

## Type 字段

我们最为熟悉的%d 代表着以十进制输出, %c 代表以字符输出. 更多的type 详见下表:

|字符|描述|
|---|---|
|%|打印字面上的 % 字符（此类型不接受任何标志、宽度、精度、长度字段）。|
|d, i|作为带符号整数的输出。对于输入， %d 和 %i 是同义词，但在与 [scanf](https://en.wikipedia.org/wiki/Scanf() "Scanf()") 一起使用时有所不同（使用 %i 将会将以 0x 为前缀的数字解释为十六进制，以 0 为前缀的数字解释为八进制）。|
|u|打印十进制无符号整数。|
|f, F|以正常（[定点 (fixed-point)](https://en.wikipedia.org/wiki/Fixed-point_arithmetic "Fixed-point arithmetic")）表示法表示的双精度数。 f 和 F 在输出无穷大或 NaN 时略有不同（对于 f，输出为 inf、infinity 和 nan；对于 F，输出为 INF、INFINITY 和 NAN）。|
|e, E|以标准形式（_d_._ddd_e±_dd_）表示的双精度值。使用 E 会引入指数E (而不是e)。指数始终包含至少两位数字；如果值为零，则指数为 00。在 Windows 中，默认情况下，指数包含三位数字，例如 1.5e002，但这可以通过 Microsoft 特定的 `_set_output_format` 函数进行更改。|
|g, G|以正常或指数表示法中更适当的方式表示的双精度数。 g 使用小写字母， G 使用大写字母。此类型与定点表示法略有不同. 小数点右边没有作用的零不显示。此外，整数没有小数点。|
|x, X|作为十六进制数的无符号整数。 x 使用小写字母， X 使用大写字母。|
|o|作为八进制数的无符号整数。|
|s| [以 null 结尾的字符串](https://en.wikipedia.org/wiki/Null-terminated_string "Null-terminated string")。|
|c|字符。|
|p|打印一个void* 型指针的地址。|
|a, A|以十六进制表示法表示的双精度数，以 0x 或 0X 开头。 a 使用小写字母， A 使用大写字母。[5](https://en.wikipedia.org/wiki/Printf#cite_note-5)[6](https://en.wikipedia.org/wiki/Printf#cite_note-6)（C++11 的 iostreams 有一个与此相同的 hexfloat）。|
|n|什么都不打印，但将到目前为止写入的字符数写入整数指针参数中。<br>在 Java 中，此操作打印换行符。[7](https://en.wikipedia.org/wiki/Printf#cite_note-7)|

## Length 字段

| 字符 | 描述 |
|------|------|
| hh   | 对于整数类型，使 printf 期望一个从 char 升级而来的 int 大小的整数参数。|
| h    | 对于整数类型，使 printf 期望一个从 short 升级而来的 int 大小的整数参数。|
| l    | 对于整数类型，使 printf 期望一个 long 大小的整数参数。对于浮点类型，此标志被忽略。在 varargs 调用中使用 float 参数时，它总是被提升为 double。[4] |
| ll   | 对于整数类型，使 printf 期望一个 long long 大小的整数参数。|
| L    | 对于浮点类型，使 printf 期望一个 long double 参数。|
| z    | 对于整数类型，使 printf 期望一个 size_t 大小的整数参数。|
| j    | 对于整数类型，使 printf 期望一个 intmax_t 大小的整数参数。|
| t    | 对于整数类型，使 printf 期望一个 ptrdiff_t 大小的整数参数。|

例子
```c
int x = -1;
printf("0x%lx\n", (unsigned long)x);
```
输出：`ffffffffffff0001`

## Presicion 字段

- 对于浮点数类型，它指定输出的是, 四舍五入的小数点右边的位数。
- 对于字符串类型，它限制了输出的字符数,超过的部分会被截断。

精度可以这么设置:
- 直接在语句中写出
- 传参, 用  ==\*==  号占位

下面是一个例子:
```c
#include <stdio.h>

int main() {
    // 示例 1: 对于浮点数类型，precision 指定小数点右边的位数
    double pi = 3.141592653589793;
    printf("Pi with precision 2: %.2f\n", pi);
    // 输出: Pi with precision 2: 3.14

    // 示例 2: 对于字符串类型，precision 指定输出字符的数量，超过则截断字符串
    const char *message = "Hello, World!";
    printf("Limited characters: %.8s\n", message);
    // 输出: Limited characters: Hello, Wo

    // 示例 3: precision 字段为动态值，通过另一个参数指定
    int dynamicPrecision = 3;
    printf("Dynamic precision: %.*s\n", dynamicPrecision, "abcdef");
    // 输出: Dynamic precision: abc

    return 0;
}

```

## Width 字段

width设置了输出的**最小**宽度,通常用于填充定长的输出
如果数字超出宽度.不会被截断
和precision类似, 有数字与传参两种形式:
例子: `printf("%*d", 5, 10)` will result in  `   10`, 共5个字符

## Flag 字段

| 字符 | 描述 |
|------|------|
| -    | 左对齐输出此占位符的内容。（默认是右对齐输出。）|
| +    | 对于正有符号数，添加正号（+），对于负有符号数，添加负号（-）。默认情况下，对于正数不添加任何东西。|
| 空格  | 对于正有符号数，添加空格；对于负有符号数，添加负号（-）。如果存在 + 标志，则此标志被忽略。（默认情况下，对于正数不添加任何东西。）|
| 0    | 当指定 'width' 选项时，对于数字类型，在数字前面添加零。默认情况下，在数字前添加空格。|
| '    | 对于十进制的整数或指数，应用千位分隔符。|
| #    | 替代形式：<br>对于 g 和 G 类型，不移除尾随零。<br>对于 f、F、e、E、g、G 类型，输出始终包含小数点。<br>对于 o、x、X 类型，对非零数字前面分别添加 0、0x、0X。|

对于`-`符号的解释: 与前面所说的width结合更好理解,就是相当于在数字后面添加空格而不是前面
例子:
```c
#include <stdio.h>

int main() {
    printf("%-8s|\n", "Hello");
    printf("%-8s|\n", "World");
    printf("%-8s|\n", "123");

    return 0;
}

```
输出:
```text
Hello   |
World   |
123     |
```

## Parameter 字段

在 POSIX 标准中引入了一个扩展，即“Parameter field”（参数字段），该字段允许在格式说明符中指定参数的顺序。
具体来说，`n$` 表示第 n 个参数，允许在格式说明符中以不同的顺序或使用不同的格式说明符多次输出参数。
例子:
```c
#include <stdio.h>
int main() {
    // 使用参数字段 %2$ 表示第二个参数，%1$ 表示第一个参数
    printf("%2$d %2$#x; %1$d %1$#x", 16, 17);
    // 输出：17 0x11; 16 0x10
    return 0;
}
```
Attention: 如果任何一个占位符指定了一个参数，那么所有其他的占位符也**必须**指定一个参数。

==注意：== 在非 POSIX 的 Microsoft Windows 中，对此特性的支持被放置在一个名为 `_printf_p` 的单独函数中。

# 总结

本文详细介绍了printf的基础以及进阶用法，并给了相应的代码示例。
以下是一个综合了上面所有字段的例子：
```c
#include <stdio.h>
int main()
{
        long double x = 3.1415926535;
        long double y = -3.1415926535;
        printf("The first argument is %1$-16.3Lf, The second argument is %2$-16.3Lf\n", x, y);
        return 0;
}
```
输出: `The first argument is 3.142           , The second argument is -3.142`

#c 