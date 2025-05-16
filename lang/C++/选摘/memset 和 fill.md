**fill函数的作用是**：按照单元赋值**，**将一个区间的元素都赋予val值。函数参数：fill(vec.begin(), vec.end(), val); val为将要替换的值。

```cpp
#include <algorithm>
fill(vec.begin(), vec.end(), val); //原来容器中每个元素被重置为val
```

**memset函数的作用是：**按照字节填充某字符

```cpp
#include <cstring>
const int INF = 0x3f3f3f3f;
memset(a,INF,sizeof(a));
```

**赋值的元素**

- 因为memset函数按照字节填充，所以一般memset只能用来填充char型数组，（因为只有char型占一个字节）。如果填充int型数组，只能填充**0、-1 和 inf（正负都行）**。因为00000000 = 0，-1同理，如果我们把每一位都填充“1”，会导致变成填充入“11111111”。如果我们将inf设为0x3f3f3f3f，0x3f3f3f3f的每个字节都是0x3f！所以要把**一段内存全部置为无穷大，我们只需要 **memset(a,0x3f,sizeof(a))。无穷小可以将 `-INF `设为0x8f。
- 而fill函数可以赋值任何值

**运行效率**

memset比fill处理速度快一些，所以在能满足需要时，推荐用memset。