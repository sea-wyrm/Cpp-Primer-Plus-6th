# 第四章 复合类型

本章很重要（特别是指针部分），需要掌握的内容为:

- 创建和使用数组；
- 创建和使用C-风格字符串；
- 创建和使用 string 类字符串；
- 使用方法 getline() 和 get() 读取字符串；
- 混合输入字符串和数字；
- 创建和使用结构体；
- 创建和使用共用体；
- 创建和使用枚举类型；
- 创建和使用指针；
- 使用 new 和 delete 管理动态内存；
- 创建动态数组；
- 创建动态结构；
- 自动存储、静态存储和动态存储；
- vector 和 array 类简介。

## 4.1 数组

数组（array) 是一种数据格式，能够存储多个**同类型**的值。每个值都存储在一个独立的数组元素中，计算机在内存中依次存储数组的各个元素。

> 即一个数组所在的内存区域是连续的。

要创建数组，可使用声明语句。数组声明应指出以下三点： 

- 存储在每个元素中的值的类型； 
- 数组名； 
- 数组中的元素数。

在C++中，通过在简单变量后面添加中括号（其中包含 元素数目）来完成数组声明。而数组中的每一个元素都看作是一个简单变量。声明数组的通用格式为 : 

`typeName arrayName[arraySize]`

表达式arraySize指定元素数目，它必须是整型常数（如10）或const 值，也可以是常量表达式（如8 * sizeof（int）），即其中所有的值在编 译时都是已知的。具体说，arraySize不能是变量，变量的值是在程序 运行时设置的。如：

```Cpp
int months[12];   // 声明一个包含 12 个整数的数组
```

C++ 通过使用下标来访问数组中的每一个元素，C++ 数据从0开始编号，数组总长度-1 是最后一个元素，如上：months[0] 为第一个元素，months[11] 是最后一个元素。

![image-20210721153711828](https://static.fungenomics.com/images/2021/07/image-20210721153711828.png)

### 4.1.1 

C++可以在声明语句中初始化数组，只需提供一个用逗号分隔的值列表（初始化列表），并将它们用花 括号括起即可。如 ：

```Cpp
int yamcosts[3] = {20, 30, 5};
```
如果没有初始化数组的值，则其元素值将是不确定的，这意味着元素的值为以前驻留在该内 存单元中的值。

> 将sizeof运算符用于数组名，得到的将是整个数组 中的字节数。

### 4.1.2 数组的初始化规则

只有在定义数组时才能使用初始化，此后就不能使用了，**也不能将一个数组赋给另一个数组**：

![image-20210721154858602](https://static.fungenomics.com/images/2021/07/image-20210721154858602.png)

> 这个方式和 Python 完全不同，原因可能是 Python 以引用为基础赋值，C++此处则是值赋值，只能一个一个来（声明时除外）。

但此后，仍然可以通过下标分别给数组中的元素赋值。注意，**这里是赋值不再是初始化**。

初始化数组时，提供的值可以少于数组的元素数目，编译器将把其他元素设置为 0。因此，将数组中所有的元素都初始化为0非常简单—只要显式地将第 一个元素初始化为0，然后编译器会自动将其他元素都初始化为0。

*还有一种方法，如果初始化数组时方括号内（[ ]）为空，C++编译器将计算元素个 数。但这是很糟糕的做法，不提倡，不举例。*

### 4.1.3 C++11数组初始化方法

数组以前就可使用列表初始化， 但C++11中的列表初始化新增了一些功能。

- 首先，初始化数组时，可省略等号（=）；
- 其次，可不在大括号内包含任何东西，这将把所有元素都设置为零；
- 第三，列表初始化禁止缩窄转换。

![image-20210721160721085](https://static.fungenomics.com/images/2021/07/image-20210721160721085.png)

在上述代码中，第一条语句不能通过编译，因为将浮点数转换为整 型是缩窄操作，即使浮点数的小数点后面为零。第二条语句也不能通过 编译，因为1122011超出了char变量的取值范围（这里假设char变量的长 度为8位）。第三条语句可通过编译，因为虽然112是一个int值，但它在 char变量的取值范围内。

## 4.2 字符串

字符串是存储在内存的一片连续字节中的一系列字符，这里的**连续**是重点。
存储在连续字节中的一系列字符意味着可以将字符串存储在char数 组中，其中每个字符都位于自己的数组元素中。字符串提供了一种存储 文本信息的便捷方式。本章介绍两种处理字符串的方法：（1）C-风格字符串；（2）基于 string 类的方法。

C-风格字符串具有 一种特殊的性质：以空字符（null character）结尾，空字符被写作 \0， 其ASCII码为0，用来标记字符串的结尾。

![image-20210721161457763](https://static.fungenomics.com/images/2021/07/image-20210721161457763.png)

这两个数组都是char数组，但只有第二个数组是字符串，空字符对 C-风格字符串而言至关重要。C++有很多处理字符串的函数，其 中包括cout使用的那些函数。它们都逐个地处理字符串中的字符，直到 到达空字符为止。如果使用cout显示上面的cat这样的字符串，则将显示 前7个字符，发现空字符后停止。使用cout显示上面的dog数 组（它不是字符串），cout将打印出数组中的8个字母，并接着将内存 中随后的各个字节解释为要打印的字符，直到遇到空字符为止（因此不加空字符是C-风格字符串中极其危险的行为）。

有一种更好的、 将字符数组初始化为字符串的方法—只需使用一个用引号括起的字符串 即可，这种字符串被称为字符串常量（string constant）或字符串字面值 （string literal），如下所示：

![image-20210721162122919](https://static.fungenomics.com/images/2021/07/image-20210721162122919.png)

用引号括起来的字符串**隐式**地包括结尾的空字符，因此不用显式地包括它。

最需要注意的是，**使用C-风格字符串，在确定存储字符串所需的最短数组时，别忘了+1，要将结尾的空字符计算在内**。

![image-20210721162704816](https://static.fungenomics.com/images/2021/07/image-20210721162704816.png)

![image-20210721162840015](https://static.fungenomics.com/images/2021/07/image-20210721162840015.png)

> 这里的赋值改为：`char shirt_size[2] = "S";`  才是正确的。

### 4.2.1 拼接字符串常量

事实上，任何两个由空 白（空格、制表符和换行符）分隔的字符串常量都将自动拼接成一个。 因此，下面所有的输出语句都是等效的：

![image-20210721163221414](https://static.fungenomics.com/images/2021/07/image-20210721163221414.png)

注意，拼接时不会在被连接的字符串之间添加空格，第二个字符串 的第一个字符将紧跟在第一个字符串的最后一个字符（不考虑\0）后 面。第一个字符串中的\0字符将被第二个字符串的第一个字符取代。

### 4.2.2 在数组中使用字符串

从程序清单4.2中可以学到什么呢？
首先，sizeof运算符指出整个数组的长度：15字节，但 strlen( )函数返回的是存储在数组中的字符串的长度，而不是数组本身的长度。
另外，strlen( ) 只计算可见的字符，而不把空字符计算在内。因此，对于Basicman，返回的值为8，而不是 9。如果cosmic是字符串，则要存储该字符串，数组的长度不能短于strlen（cosmic）+1。
由于name1和name2是数组，所以可以用索引来访问数组中各个字符。例如，该程序使用 name1[0]找到数组的第一个字符。另外，该程序将name2[3]设置为空字符。这使得字符串在第 3个字符后即结束，虽然数组中还有其他的字符（参见图4.3）。

![image-20210721164300455](https://static.fungenomics.com/images/2021/07/image-20210721164300455.png)

程序清单 4.2 使用 cin 暗含两个问题:

- 遇到空格结束；
- 输入字符串长于目标数组。

### 4.2.3 字符串输入

cin是如何确定已完成字符串输入呢？由于不能通过键盘输入空字 符，因此cin需要用别的方法来确定字符串的结尾位置。cin使用空白 （空格、制表符和换行符）来确定字符串的结束位置，这意味着cin在 获取字符数组输入时只读取一个单词。读取该单词后，cin将该字符串 放到数组中，并自动在结尾添加空字符。

这个例子的实际结果是，cin把Alistair作为第一个字符串，并将它 放到name数组中。这把Dreeb留在输入队列中。当cin在输入队列中搜索 用户喜欢的甜点时，它发现了Dreeb，因此cin读取Dreeb，并将它放到 dessert数组中。

![image-20210721171126781](https://static.fungenomics.com/images/2021/07/image-20210721171126781.png)

另一个问题是，输入字符串可能比目标数组长（运行中没有揭示出 来）。像这个例子一样使用cin，确实不能防止将包含30个字符的字符 串放到20个字符的数组中的情况发生。

文章将这个问题的系统处理留到第 17 章，但下文也能解决。

### 4.2.4 每次读取一行字符串输入

每次读取一个单词通常不是最好的选择。具体地说，需要采用面向行而不是面向单词的方法。istream中的类（如 cin）提供了一些面向行的类成员函数：`getline()` 和 `get()`。这两个函数都读取一 行输入，直到到达换行符（回车符）。区别是，`getline()` 将读入并丢弃换行符，而 `get()` 则将换行符保留在输入队列中。**注意是还保留在输入流里，没有被读走！**

1．面向行的输入：getline( )

getline( )函数读取整行，它使用通过回车键输入的换行符来确定输入结尾。通过 `cin.getline()` 调用。该函数有两个参数，第一个参数是存储输入行的数组名称，第二个参数是要读取的字符数。如果这个参数为20，则函数最多读取19个字符或者碰到换行符为止，并自动在结尾处添加空字符。如：

```Cpp
cin.getline(array_name, 20);
```

例子程序清单 4.4:

![image-20210722004651618](https://static.fungenomics.com/images/2021/07/image-20210722004651618.png)

![image-20210722004809212](https://static.fungenomics.com/images/2021/07/image-20210722004809212.png)

2．面向行的输入：get( )

`iostream` 中的 `get()` 虽然和 `getline()` 类似，它们接受的参数相同，解释参数的方式也相同，并且都读取到行尾。但 get 将换行符保留在输入队列中，假设我们连续两次 调用 `get( )` 那么要注意了。

第一次调用后，换行符将被留在输入队列中，因此第二次调用时看到的第一个字符便是换行符，此时 `get()` 以为已经到达行尾了，而没有发现任何可读取的内容。**如果不借助于帮助，`get()` 将不能跨过该换行符**。

这时要用 `get()` 的另一种变体——不带任何参数，可读取下一个字符（即使是换行符），因此可以用它来处理换行符，为读取下一行输入做好准备。例子：

![image-20210722005755295](https://static.fungenomics.com/images/2021/07/image-20210722005755295.png)

由于 `cin.get(name, ArSize)` 返回的还是 cin 对象，因此还可以合并起来调用：

```Cpp
cin.get(name, ArSize).get();  // 这样也能同时将末尾的换行符读走
```
但风险就是，**假如 ArSize 小于一行的字符数，那么调用 `get()` 后就会发生字符丢失**。

`getline()` 成员函数也同样可以合并调用连续读取字符串：

```Cpp
cin.getline(name1, ArSize).getline(name2, ArSize);
```

这语句将把输入中 连续的两行分别读入到数组name1和name2 中，其效果与两次调用 `cin.getline()` 相同。

为什么要使用 `get()`，而不是 `getline()` 呢？
首先，老式实现没有 `getline()`。其次，`get()` 使输入更仔细。例如，假设用`get( )` 将一行读入数组中。**如何知道停止读取的原因是由于已经读取了整行，而不是由于数组已填满呢？** （getline，判断不了）， 查看下一个输入字符，如果是换行符，说明已读取了整行；否则，说明该行中还有其他输入。

总之， `getline()` 使用起来简单一些，但 `get( )` 使得检查错误更简单些。

3．空行和其他问题

当 `getline()` 或 `get()` 读取空行时，将发生什么情况？最初的做法是， 下一条输入语句将在前一条 `getline()` 或 `get()` 结束读取的位置开始读取； 但当前的做法是，当 `get()`（不是getline）读取空行后将设置失效位 （failbit）。这意味着接下来的输入将被阻断，要用 `cin.clear()` 的命令来恢复输入。

另一个潜在的问题是，输入字符串可能比分配的空间长。如果输入行包含的字符数比指定的多，则 `getline()` 和 `get()` 将把余下的字符留在输入队列中，`getline()` 还会设置失效位，并关闭后面的输入。

### 4.2.5 混合输入字符串和数字

![image-20210722012337405](https://static.fungenomics.com/images/2021/07/image-20210722012337405.png)

清单程序4.6 的一个问题是：
当cin读取年份，将回 车键生成的换行符留在了输入队列中。后面的cin.getline( )看到换行符 后，将认为是一个空行，并将一个空字符串赋给address数组。解决之道 是，在读取地址之前先读取并丢弃换行符。这可以通过几种方法来完 成，其中包括使用没有参数的get( )和使用接受一个char参数的get( )。

也可以利用表达式cin>>year返回cin对象，将调用拼接起来：

```Cpp
(cin >> year).get();  // or (cin >> year).get(ch);
```

## 4.3 string 类简介

`string` 类包含于头文件 `string` 并位于名称空间 `std` 中，使用起来比字符数组简单，它提供了将字符串作为一种数据类型的表示方法，符合 C++ 的风格。
string 类定义隐藏了字符串的数组性质，让我们能够像处理普通变量那样处理字符串。

在很多方面，使用 string 对象的方式和使用字符数组相同：

- 可以使用 C-风格字符串来初始化 string 对象；
- 可以使用 cin 来将键盘输入存储到 string 对象中；
- 可以使用 cout 来显示 string 对象；
- 可以使用数组表示法来访问存储在 string 对象中的字符。

string 对象和字符数组的主要区别是，可以将 string 对象声明为简单变量，而不是数组。

类设计让程序能够自动处理string的大小。例如，str1的声明创建一 个长度为0的 string 对象，但程序将输入读取到str1中时，将自动调整 str1 的长度。

这使得与使用数组相比，使用string对象更方便，也更安全。从理论上说，可以将char 数组视为一组用于存储一个字符串的 char 存储单元，而 string 类变量是一个表示字符串的实体。

###  4.3.1 C++11字符串初始化

![image-20210722141823630](https://static.fungenomics.com/images/2021/07/image-20210722141823630.png)

### 4.3.2 string 赋值、拼接和附加

string 可以如简单变量那般操作。比如，虽然不能将一个数组赋给另一个数组，但却可以将一个 string 对象赋给另一个 string 对象。

![image-20210722142110617](https://static.fungenomics.com/images/2021/07/image-20210722142110617.png)

可以使用运算符 + 将两个string 对象合并起来，还可以使用运算符 += 将字符串附加到string对象的末尾。

```Cpp
string str1 = "aa";
string str2 = "bb";
string str3;

str3 = str1 + str2;  // 正确
str3 += str1;        // 正确
str3 += " cc";       // 一样正确
```

### 4.3.3 string类的其他操作

处理string对象的语法通常比使用C字符串函数简单，尤其是执行较为复杂的操作时。例如，对于下述操作：

`str3 = str1 + str2;`

使用C-风格字符串时，需要使用的函数如下：

```Cpp
strcpy(charr3, charr1);
strcat(charr3, charr2);
```

`strcpy()` 和  `strcat()` 函数包含在头文件 cstring 中。函数 `strcpy()` 将字符串复制到字符数组中，使用函数 `strcat()` 将字符串附加到字符数组末尾。
另外，使用字符数组时，**总是存在目标数组过小，无法存储指定信息的危险**。

![image-20210722143658095](https://static.fungenomics.com/images/2021/07/image-20210722143658095.png)

函数strcat( )试图将全部12个字符复制到数组site中，这将覆盖相邻的内存。这可能导致程序终止，或者程序继续运行，但数据被损坏。**string类具有自动调整大小的功能，从而能够避免这种问题发生**。


### 4.3.4 string类I/O

![image-20210722144341278](https://static.fungenomics.com/images/2021/07/image-20210722144341278.png)

在用户输入之前，该程序指出数组charr中的字符串长度为27，这比 该数组的长度要大。这里要两点需要说明。首先，为初始化的数组的内 容是未定义的；其次，函数strlen( )从数组的第一个元素开始计算字节 数，直到遇到空字符。在这个例子中，在数组末尾的几个字节后才遇到 空字符。对于未被初始化的数据，第一个空字符的出现位置是随机的， 因此您在运行该程序时，得到的数组长度很可能与此不同。

用户输入之前，str中的字符串长度为0。这是因为未被初始化的string对象的长度被自动设置为0。 **这是和字符串数组不同的地方**。

源代码中是将一行输入读取到string对象中的代码是：`getline(cin, str);`。这个 `getline()` 不是类方法，它将 cin 作为参数，指出去哪里查找输入，另外，也没有指出字符串长度 的参数，因为 `string` 对象将根据字符串的长度自动调整自己的大小。

### 4.3.5 其他形式的字符串字面值

除char类型外，C++还有类型wchar_t；而C++11新 增了类型char16_t和char32_t。可创建这些类型的数组和这些类型的字符 串字面值。对于这些类型的字符串字面值，C++分别使用前缀L、u和U 表示，下面是一个如何使用这些前缀的例子：

![image-20210722162656588](https://static.fungenomics.com/images/2021/07/image-20210722162656588.png)

C++11还支持Unicode字符编码方案UTF-8。
C++11新增的另一种类型是原始（raw）字符串，并使用前缀 R 来标识，原始字符表示的就是自己，例如，序列 `\n` 不表示换行符，而表示两个常 规字符—斜杠和n，因此在屏幕上显示时，将显示这两个字符。

## 4.4 结构体简介

**结构体（Struct）**是 一种比数组更灵活的数据格式，因为同一个结构体可以存储多种不同类型的数据，从而将数据的表示合并到一起。

> 原文称 struct 为 “结构”，我认为这不符合一贯以来的称呼，因此在这个笔记中我一律改为通常在 `C/C++` 中所称呼的 “结构体”。

结构体是用户定义的类型，而结构体声明定义了这种类型的数据属性。结构体的定义需要用 关键字 **struct** 做标识。 定义了类型后，便可以创建这种类型的变量。以下例子是一个结构体的定义，它使用了一个适合用于存储字符串的 char数组、一个float和一个double。列表中的每一项都被称为结构体成员，因此 infatable 结构体有3个成员（参见图4.6）

![image-20210722163912443](https://static.fungenomics.com/images/2021/07/image-20210722163912443.png)

定义之后就可以将这个结构体当做常规的 C++ 数据类型（如同 int、double、string等）来使用。

在结构体类型中，可以通过使用成员运算符 `.` 来访问各个成员。

### 4.4.1 在程序中使用结构体

结构声明的位置很重要。

![image-20210722165020076](https://static.fungenomics.com/images/2021/07/image-20210722165020076.png)

结构体的初始化方式和数组一样，使用逗号分隔值列表，并将这些值用花括号括起。如:

![image-20210722165157045](https://static.fungenomics.com/images/2021/07/image-20210722165157045.png)

花括号中每个值可以独占一行，也可以将他们都放在同一行中，注意变量之间有逗号隔开就行，其他的随意。

### 4.4.2 C++11 结构体初始化

![image-20210722165537179](https://static.fungenomics.com/images/2021/07/image-20210722165537179.png)

### 4.4.3 结构体可以将string类作为成员吗?

可以，只要编译器支持 string 类型就没问题，唯一要注意的问题注意添加名字空间 std 。 如:

![image-20210722165802188](https://static.fungenomics.com/images/2021/07/image-20210722165802188.png)

### 4.4.4 其他结构属性

结构体和 C++ 内置类型的用法类似，它可以作为参数传递给函数，也可以让函数返回一个结构体。另外，**还可以使用赋值运算符（=）将结构体赋给另一个同类型的结构体**，这样结构体中每个成员都将被设置为另一个结构中相应成员的值，即使成员是数组。这种赋值被称为成员赋值（memberwise assignment）。

结构体类型还可以同时完成定义结构体和创建结构体变量的工作，只需将变量名放在结束括号的后面即可：

![image-20210722170446818](https://static.fungenomics.com/images/2021/07/image-20210722170446818.png)

但不推荐这样做，将结构体定义和变量声明分开，可以使程序更易于阅读和理解。

### 4.4.5 结构体数组

创建结构体数组的方法和创建 C++ 基本类型的数组完全相同。

要初始化结构体数组，可以使用初始化数组的规则（用逗号分隔每个元素的值，并将这些值用花括号括起）和初始化结构体的规则（用逗号分隔每个成员的值，并将这些值用花括号括起）。由于数组中的每个 元素都是结构，因此可以使用结构初始化的方式来提供它的值。

![image-20210722170912740](https://static.fungenomics.com/images/2021/07/image-20210722170912740.png)

## 4.5 共用体

共用体（union），也叫联合体是一种数据格式，它能够存储不同的数据类型， 但只能同时存储其中的一种类型。也就是说，结构体可以同时存储 int、 long和double，共用体只能存储int、long或double。共用体的句法与结构相似，但含义不同。如：

![image-20210722171406230](https://static.fungenomics.com/images/2021/07/image-20210722171406230.png)

**共用体每次只能存储一个值**，因此它必须有足够的空间来存储最大的成员，所以，共用体的长度为其最大成员的。

共用体的用途之一是，当数据项使用两种或更多种格式（但不会同时使用）时，可节省内存空间，其实对当前内存充裕的计算机来说，并非很有必要使用共用体。

## 4.6 枚举

这也是一个很少用的类型，略。
![image-20210722171825483](https://static.fungenomics.com/images/2021/07/image-20210722171825483.png)

## 4.7 指针和自由存储空间

计算机在存储数据时必须跟踪的3种基本属性：

- 信息存储在何处；
- 存储的值为多少；
- 存储的信息是什么类型。

C++ 提供了一种策略，可以在程序内部跟踪内存单元，这个策略以指针为基础。C++ 中 **指针是一个变量，其存储的是值的地址，而不是值本身**。

首先，我们如何找到常规变量的地址？在C/C++中只需对变量应用地址运算符 `&`，就可以获得它的位置；例如，如果 `home` 是一个变量，则 `&home` 是它在内存中的地址。

**使用常规变量时，值是指定的量，而地址为派生量**。接下来看看指针策略，它是C++内存管理编程理念的核心。

![image-20210722232030057](https://static.fungenomics.com/images/2021/07/image-20210722232030057.png)

指针处理存储数据的新策略刚好相反，**指针将地址视为指定的量，而将值视为派生量**，因此，指针名表示的是地址。`*` 运算符被称为**间接值（indirect velue）或解除引用 （dereferencing）运算符**，应用于指针得到该地址处所存储的值。例如，假设 `manly` 是一个指针，则 `manly` 表示的是一个地址，`*manly` 表示存储在该地址处的值，即 `*manly` 和常规变量等效。**变量的指针和变量的值本身本质只不过是一个硬币的两个面**。

> 变量是编译时分配出来的一个有名称的内存，这是变量的实质。而指针是一个可以通过名称直接访问内存的别名，内存里存储的东西就是变量的值。

![image-20210722233255845](https://static.fungenomics.com/images/2021/07/image-20210722233255845.png)

### 4.7.1 声明和初始化指针

不同数据类型存储值时的内存格式不同，所以指针声明时必须 **指定指针指向的数据类型**，声明一个指针的通用方式:

```
typeName *p_name;
```

`*` 符用于指明该变量是指针，每个指针变量声明时都需要带 `*`。

![image-20210722233705755](https://static.fungenomics.com/images/2021/07/image-20210722233705755.png)

使用指针的时候要注意区分地址是地址，值是值。指针本身也是一种数据类型，它的值是地址，这个地址也有它自己的字节长度（由计算机系统内定），它的长度和它这个地址里存储的数值没有任何关系。指向 int 类型和指向 double 类型或者 string 类型的指针类型的字节长度都是一样的，它们都是地址。

### 4.7.2 指针的危险

**创建指针时，计算机将分配用来存储指针这个变量本身的内存，但不会分配指针所指向的数据所需的内存**。为数据提供空间是一个独立的步骤，如果忽略这一步麻烦就大了。如：

```Cpp
long *fellow;       // 创建一个指向 long 类型的指针
*fellow = 223323;   // 指针还不知道指向哪个地址，就强制赋值，大错特错
```
声明时，只是告诉程序 `fellow` 是一个 `long` 指针，程序只给 `fellow` 这个指针变量分配了一个属于 `fellow` 自己的存储空间，至于它里面要存什么地址值还完全不知道。必须先申请一个存 `long` 类型的地址空间，然后赋值给 `fellow`，最后才可以将 223323 赋值过去。 

> 一定要在对指针应用解除引用运算符 `*` 之前，将指针初始化为一个确定的、适当的地址，这是关于使用指针的铁律。

### 4.7.3 指针和数字

指针不是整型，虽然计算机通常把地址当作整数来处理。在有些平台 中，int 类型是个2字节值，而地址是个4字节值。

### 4.7.4 使用new来分配内存

指针真正的用武之地在于，运行阶段分配未命名的内存用于存储值。C++ 通过 new 运算符为变量分配内存。

程序员要告诉new，需要为哪种数据类型分配内存；new将找到一个长 度正确的内存块，并返回该内存块的地址。程序员的责任是将该地址赋 给一个指针。下面是一个这样的示例：

```Cpp
int *pn = new int;
```
`new int` 告诉程序，需要适合存储 `int` 的内存。`new` 运算符根据类型来确定需要多少字节的内存。然后，它找到这样的内存，并返回其地址。 接下来，将地址赋给 `pn`，`pn` 是被声明为指向 `int` 的指针。现在，`pn` 是地址，而 `*pn` 是存储在那里的值。

**注意：`pn` 指向的内存不是变量（那是一个地址，是不变的），内存里存储的东西才是可变的。**

> 这引出了一个问题：pn指向的内存没 有名称，如何称呼它呢？我们说pn指向一个数据对象，这里的“对象”不 是“面向对象编程”中的对象，而是一样“东西”。术语“数据对象”比“变 量”更通用，它指的是为数据项分配的内存块。

为一个数据对象（可以是结构，也可以是基本类型）获得并指定分 配内存的通用格式如下：

```Cpp
typeName *pointer_name = new typeName;
```

需要指出的另一点是，`new` 分配的内存块通常与常规变量声明分配的内存块不同。常规变量存储在被称为栈 （stack）的内存区域中，而 `new` 从被称为堆（heap）或自由存储区（free store）的内存区域分配内存。

如果内存分配失败，`new` 运算符将返回空指针（null pointer），其值为 0。

### 4.7.5 使用 delete 释放内存

**`delete` 运算符，用于将不再使用的内存归还给内存池**，归还或释放（free）的内存可供程序的其他部分使用。使用 `delete` 时，后面要加上指向内存块的指针（这些内存块最初是用new分配的）。

```Cpp
int *ps = new int;   // 声明指针并分配一个可以存 int 类型的内存给指针
...
delete ps;  // 归还内存
```

归还 ps 指向的内存，并不会删除指针 ps 本身，它可以继续用来指向新分配的内存。
**`new` 和 `delete` 一定要配对使用，否则会导致程序发生内存泄露（memory leak）**，即分配出去的内存拿不回来，无法再使用。

![image-20210723144325225](https://static.fungenomics.com/images/2021/07/image-20210723144325225.png)



另外，不要尝试释放已经释放的内存块，C++标准指出，这样做的结果将是不确定的，这意味着什么情况都可能发生。

> 一般来说，不要创建两个指向同一个内存块的指针，因为这将增加 错误地删除同一个内存块两次的可能性。

另外，不能使用 `delete` 释放声明变量所获得的内存，**只能用它释放 `new` 分配的内存**，这是关键点。

![image-20210723105004298](https://static.fungenomics.com/images/2021/07/image-20210723105004298.png)

### 4.7.6 使用new来创建动态数组

对于大型数据（如数组、字符串和结 构），应使用new，这正是new的用武之地。

在编译时给数组分配内存被称为静态联编（static binding），这就是为什么只能分配固定大小的数组。但使用 `new` 时，在运行阶段需要数组，则创建它；如果不需 要，则不创建，还可以选择数组的长度，这被称为动态联 编（dynamic binding），意味着数组是在程序运行时创建的。

1．使用 new 创建动态数组

创建动态数组很容易；只要将数组的元素类型和元素数 目告诉new即可。必须在类型名后加上方括号，其中包含元素数目。为数组分配内存的通用格式如下：

```
typeName *pointer_name = new typeName [num_elements];
```

给个例子如：

```Cpp
int *psome = new int [10];  // 分配可以存储 10 个 int 的内存块
```

`new` 返回第一个元素的地址，并将该地址被赋给指针 `psome`。

释放这一块内存时要这样做：
```Cpp
delete [] psome;
```
方括号告诉程序，应释放整个数组，而不仅仅是指针指向的元素。 **注意delete和指针之间有方括号**。对于 ANSI/ISO标准来说，`new` 与 `delete` 的格式要匹配，否则导致的后果是不确定。

总之，使用new和delete时，应遵守以下规则：

- 不要使用 `delete` 来释放不是 `new` 分配的内存；
- 不要使用 `delete` 释放同一个内存块两次；
- 如果使用 `new []` 为数组分配内存，则应使用 `delete []` 来释放；
- 如果使用 `new []` 为一个实体分配内存，则应使用 `delete`（没有方括 号）来释放(这一点要找例子来理解)；
- 对空指针应用 `delete` 是安全的。

2．使用动态数组

创建动态数组后，如何使用它呢？**只需要将指针名当做数组名，然后按照数组的访问方式即可**，C/C++ 中数组和指针是基本等价的（但也有实质区别，后续再说）。例子：

```Cpp
int *psome = new int [10];
```

对于第1个元素，可以使用 `psome[0]`，而不是 `*psome`；对于第2个元 素，可以使用 `psome[1]`，依此类推。

程序清单4.18演示了如何使用new来创建动 态数组以及使用数组表示法来访问元素；它还指出了指针和真正的数组名之间的根本差别（注意红框）。

![image-20210723111823885](https://static.fungenomics.com/images/2021/07/image-20210723111823885.png)

将`p3` 加1导致它指向第2个元素而不是第1个。将它减1后，指针将指向原来的值，这样程序便可以给 `delete[]` 提供正确的地址。(这叫指针移动)

## 4.8 指针、数组和指针算术

指针算术是有特殊之处的。

**指针和数组基本等价的原因在于指针算术（pointer arithmetic）和 C++ 内部处理数组的方式**。

> 将整数变量加1 后，其值将增加1；但将指针变量加1后，增加的量等于它指向的类型的 字节数。将指向double的指针加1后，如果系统对double使用8个字节存 储，则数值将增加8；将指向short的指针加1后，如果系统对short使用2 个字节存储，则指针值将增加2。

### 4.8.1 程序说明

![image-20210723112411334](https://static.fungenomics.com/images/2021/07/image-20210723112411334.png)

**将指针变量加1后，其增加的值等于指向的类型占用的字节数**。 也就是跳到下一个存储值的地址上。

从该程序的输出可知，`*（stacks + 1）`和 `stacks[1]` 是等价的。同样， `*（stacks + 2）`和 `stacks[2]` 也是等价的。通常，使用数组表示法时，`C++` 都执行下面的转换：

```
arrayname[i] becomes *(arrayname + i)
```

如果使用的是指针，而不是数组名，则C++也将执行同样的转换：

```
pointername[i] becomes *(pointername + i)
```
因此，在很多情况下，可以相同的方式使用指针名和数组名。也就是说，使用 `new` 来创建数组以及使用指针来访问不同的元素时，只要把指针当作数组名对待即可。

**【注意】数组名被解释为其第一个元素的地址，而对数组名应用地址运算符 时，得到的是整个数组的地址，区别很大。**



![image-20210723113548998](https://static.fungenomics.com/images/2021/07/image-20210723113548998.png)

### 4.8.2 指针小结

1．声明指针
要声明指向特定类型的指针，请使用下面的格式：

```Cpp
typeName *pointer_name;
```

2．给指针赋值

应将内存地址赋给指针。可以对变量名应用 `&` 地址运算符，来获得被命名的内存的地址，或 `new` 运算符返回未命名的内存的地址。

3．对指针解引用

对指针解引用意味着获得指针指向的值。`*` 是指针应用解引用或间接值运算符。

4．区分指针和指针所指向的值

如果 `pt` 是指向int的指针，则 `*pt` 不是指向 `int` 的指针，而是完全等同于 一个 `int` 类型的变量，`pt` 才是指针。

5．数组名

大多数情况下，C++将数组名视为数组的第一个元素的地址。一种例外情况是，将sizeof运算符用于数组名用时，此时将返回整个数组的长度（单位为字节）。

6．指针算术

C++允许将指针和整数相加。加1的结果等于原来的地址值加上指向的对象占用的总字节数，也就是移动到下一个存储值的地址。还可以将一个指针减去另一个指针，获得两个指针的差。后一种运算将得到一个整数，仅当两个指针指向同一个数 组（也可以指向超出结尾的一个位置）时，这种运算才有意义，这将得到两个元素的间隔。

7．数组的动态联编和静态联编

使用数组声明来创建数组时，将采用静态联编，即数组的长度在编 译时设置。
使用 `new[]` 运算符创建数组时，将采用动态联编（动态数组），即将在运行时为数组分配空间，其长度也将在运行时设置。使用完这种数组后，应使用 `delete []` 释放其占用的内存。

8．数组表示法和指针表示法

使用方括号数组表示法等同于对指针解引用。数组名和指针变量都是如此，因此对于指针和数组名，既可以使用 指针表示法，也可以使用数组表示法。

![image-20210723115130804](https://static.fungenomics.com/images/2021/07/image-20210723115130804.png)

### 4.8.3 指针和字符串

数组和指针的特殊关系可以扩展到C-风格字符串。

```Cpp
char flower[10] = "rose";
cout << flower << "s are red.\n";
```

以上代码中，数组名是第一个元素的地址，因此cout语句中的flower是包含字符r 的 char 元素的地址。cout 对象认为 char 的地址是字符串的地址，因此它打 印该地址处的字符，然后继续打印后面的字符，直到遇到空字符 (\0)  为止。总之，如果给cout提供一个字符的地址，则它将从该字符开始打 印，直到遇到空字符为止。

**这意味着可以将指向char的指针变量作为cout的参数，因为它也是 char的地址。**
在C++中，用引号括起的字符串像数组名一样，也是第一个元素的地 址。这意味着对于数组中的字符串、用引号括起的字符串常量以及指针 所描述的字符串，处理的方式是一样的，都将传递它们的地址。与逐个 传递字符串中的所有字符相比，这样做的工作量确实要少。

> 在 cout 和多数 C++ 表达式中，`char` 数组名、`char` 指针以及用引号括起的字符串常量都被解释为字符串第一个字符的地址。

请不要使用字符串常量或未被初始化的指针来接收输入。为避免这 些问题，也可以使用std::string对象，而不是数组。

在将字符串读入程序时，应使用已分配的内存地址。该地址可以是数组名，也可以是使用new 初始化过的指针。

注意这段代码及其输出：

![image-20210723122618520](https://static.fungenomics.com/images/2021/07/image-20210723122618520.png)

一般来说，如果给cout提供一个指针，它将打印地址。但如果指针 的类型为 `char *`，则 cout将显示指向的字符串。如果要显示的是字符串的 地址，则必须将这种指针强制转换为另一种指针类型，如 `int *`（上面的 代码就是这样做的）。

经常需要将字符串放到数组中。初始化数组时，请使用=运算符； 否则应使用strcpy( )或strncpy( )。应使用strcpy( )或strncpy( )，而不是赋值运算符来将字符串赋给数组。

> 这是 C-风格 字符串的操作方式，C++ string 类型则并不需要如此，会简单得多，不用担心字符串会导致数 组越界，并可以使用赋值运算符而不是函数strcpy( )和strncpy( )。

### 4.8.4 使用 new 创建动态结构体

在运行时创建数组优于在编译时创建数组，对于结构也是如此。通过使用new，可以创建动态结构。

将 new 用于结构体由两步组成：创建结构体和访问其成员。要创建结构体，需要同时使用结构体类型和 new。

```Cpp
inflatable *ps = new inflatable;
```
这将把足以存储inflatable结构的一块可用内存的地址赋给ps。这种 句法和C++的内置类型完全相同。

此时要访问结构体成员，需通过箭头成员运算符 `−>`，它用于指向结构体的指针，就像点运算符可用于结构体名一样，如 `ps->price` 指向了结构体中的 price 成员。

![image-20210723134422484](https://static.fungenomics.com/images/2021/07/image-20210723134422484.png)

> 如果结构标识符是结构名，则使用句点运算符；如果标识符是指向结构 的指针，则使用箭头运算符。

另一种访问结构成员的方法是先对指针解引用。如果 `ps` 是指向结构的指针，则 `*ps` 就是被指向的值—结构本身。由于 `*ps` 是一个结构，因此 `(*ps).price` 是 该结构的 `price` 成员。

![image-20210723134903499](https://static.fungenomics.com/images/2021/07/image-20210723134903499.png)

### 4.8.5 自动存储、静态存储和动态存储

根据用于分配内存的方法，C++有3种管理数据内存的方式：自动存储、静态存储和动态存储（有时也叫作自由存储空间或堆）。在存在时间的长短方面，以这3种方式分配的数据对象各不相同。

1. 自动存储

在函数内部定义的常规变量使用自动存储空间，被称为自动变量 （automatic variable），只在包含它的代码中有效，这意味着它们在所属的函数被调用时自动产生，在该函数结束时消亡。

自动变量通常存储在栈中。

2. 静态存储

静态存储是整个程序执行期间都存在的存储方式。使变量成为静态 的方式有两种：一种是在函数外面定义它；另一种是在声明变量时使用 关键字 `static`。

3. 动态存储

`new` 和 `delete` 运算符提供了一种比自动变量和静态变量更灵活的方法。它们管理了一个内存池，这在C++中被称为自由存储空间（free store）或堆（heap）。该内存池同用于静态变量和自动变量的内存是分开的。

## 4.9 类型组合

指针数组，指向指针的指针，也叫二维指针可以用来创建动态二维数组，类型较为复杂。

<img src="https://static.fungenomics.com/images/2021/07/image-20210723150508488.png" alt="image-20210723150508488" style="zoom:70%;" />

## 4.10 数组的替代品

STL 标准库中的模板类 `vector` 和 `array` 是数组的替代品。

### 4.10.1 模板类 vector

模板类 `vector` 类似于 `string` 类，也是一种动态数组，创建时要包含头文件 vector 。可以在运行阶段设置 `vector` 对象的长度，也可在末尾附加新数据，还可以在中间插入新数据，`vector` 的长度不要求必须设定，因为它会依据插入的数据量自动增长，容量增长的速度是 2 的 `n` 次方。

基本上，它是使用 `new` 创建动态数组的替代品。实际上，`vector` 类确实使用 `new` 和`delete` 来管理内存，但这种工作是自动完成的。

`vector` 的声明方式：

```Cpp
#include<vector>
vector<typeName> v_name(number);
```

这样 v_name 就是一个 vector 对象，但 number 不是必须的。

`vector` 的最完整文档和例子，应参照网站 <http://cplusplus.com/reference/vector/vector> 。 

### 4.10.2 模板类array（C++11）

`vector` 类的功能比数组强大，但付出的代价是效率稍低（其实没那么低效）。如果您需要的是长度固定的数组，使用数组是更佳的选择，但代价是不那么方便和安全。有鉴于此，C++11 新增了模板类 `array`，它也位于名称空间 `std` 中。与数组一样，`array` 对象的长度也是固定的，也使用栈（静态内存分配），而不是自由存储区，因此其效率与数组相同，但更方便，更安全。

要创建array对象，需要包含头文件array。array对象的创建语法与 vector稍有不同：

```Cpp
#include<array>

array<typeName, num_element> arr;
```

与创建 `vector` 对象不同的是，n_element 不能是变量，必须是固定的常量和普通数组要求一样。

> 通过 `array` 模板类创建数组有什么优势？

<https://cplusplus.com/reference/array/array>

### 4.10.3 比较数组、vector 对象和 array 对象

这三者的异同是什么？

首先，无论是数组、`vector` 对象还是 `array` 对象，都可使用标准数组表示法来访问各个元素；

其次，从地址可知，`array` 对象和数组存储在相同的内存区域（即栈）中，而`vector` 对象存储在另一个区域（自由存储区或堆）中；

第三，**可以将一个 `array` 对象直接赋给另一个 `array` 对象，而标准数组，必须逐元素复制数据** (使用 array 模板类的两个好处之一，另一个好处是控制数组超界风险)。

例子：

![image-20210723153517389](https://static.fungenomics.com/images/2021/07/image-20210723153517389.png)

![image-20210723160219245](https://static.fungenomics.com/images/2021/07/image-20210723160219245.png)

在上面代码中注意一个语句：

```Cpp
a1[-2] = 20.2;
```

这是什么意思？这个语句会被转换为：
```Cpp
*(a1 - 2) = 20.2;
```

含义是：找到a1指向的地方，向前移两个double元素，并将 20.2存储到目的地。也就是说，将信息存储到数组的外面。**与C语言一 样，C++也不检查这种超界错误**，也就是说数组的这种行为是不安全的，C++ 却不禁止，要小心。

`vector` 和 `array` 对象能够禁止这种行为吗？如果您让它们禁止，它们就能禁止。另外，这些类还让您能够降低意外超界错误的概率。例如，它们包含成员函数 `begin()` 和 `end()`，让您能够确定边界，以免无意间超界。


## 4.11 总结

数组、结构体和指针是C++的3种复合类型。

数组可以在一个数据对 象中存储多个同种类型的值。通过使用索引或下标，可以访问数组中各个元素。

结构体可以将多个不同类型的值存储在同一个数据对象中，可以使用成员关系运算符`.` 来访问其中的成员。使用结构体的第一步是创建结构体模板，它定义结构存储了哪些成员。**模板的名称将成为新类型的标识符，然后就可以声明这种类型的结构变量**。

指针是被设计用来存储地址的变量，指针指向它存储的地址。指针声明指出了指针指向的对象的类型。对指针应用解引用运算符 `*`，将得到指针指向的位置中的值。

字符串是以空字符为结尾的一系列字符。字符串可用引号括起的字符串常量表示，其中隐式包含了结尾的空字符。可以将字符串存储在 char数组中，可以用被初始化为指向字符串的char指针表示字符串。

`string` 对象将根据要存储的字符串自动调整其大小，用户可以使用赋值运算符来复制字符串。

`new` 运算符允许在程序运行时为数据对象请求内存。该运算符返回获得内存的地址，可以将这个地址赋给一个指针，程序将只能使用该指针来访问这块内存。使用解引用运算符 `*` 获得其值；如果数据对象是数组，则可以像使用数组名那样使用指针来访问元素；如果数据对象是结构体，则可以用指针解引用运算符 `->` 访问其成员。

指针和数组紧密相关。如果 `ar` 是数组名，则表达式 `ar[i]` 被解释为  `*(ar + i)`（ `i` 可以是负值，表示指针左移），其中数组名被解释为数组第一个元素的地址。这样，数组名的作用和指针相同。反过来，可以使用数组表示法，通过指针名来访 问new分配的数组中的元素。

运算符 `new` 和 `delete` 允许显式控制何时给数据对象分配内存，何时将内存归还给内存池。
