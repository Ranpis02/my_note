[toc]

# 1 指针变量的运算

1. 指针变量不能直接相加，也不能直接相乘或相除
2. 如果两个指针变量指向的是同一块连续空间中的不同存储单元，在这两个指针变量才可以相减

**示例**

```c
#include<stdio.h>
int main() {
    int i = 5, j = 10;
    int a[10] = {1, 2, 3, 4, 5};
    // int* p = &i;
    // int* q = &j;
    // p - q => 没有实际意义
    int* p = &a[1];
    int* q = &a[4];
    printf("%d", q - p);    // 数组中的空间为连续的内存，指向数组中的元素的两指针相减得到的是其对应的索引号之差
    // printf("%d", q + p); //  同样地，只支持相减操作，不能进行相加、相乘或者相除操作
    
    return 0;
}
```



# 2 指针变量所占用的存储空间

首先，我们先说结论：<strong style="color:red">在同一个环境下，无论指针变量指向的是何种数据类型，所占用的字节数相同。</strong>

> 在 32 位机上占用 4 个字节，在 64 位机上占 8 字节

**示例**

```c
int _int_type = 0;
double _double_type = 0.0;
char _char_type = 'a';
float _float_type = 0.0;

int* _int_ptr = &_int_type;
double* _double_ptr = &_double_type;
float* _float_ptr = &_float_type;
char* _char_ptr = &_char_type;

printf("整型指针所占字节数为：%d\n", sizeof(_int_ptr));
printf("单精度浮点型指针所占字节数为：%d\n", sizeof(_float_ptr));
printf("双精度浮点型指针所占字节数为：%d\n", sizeof(_double_ptr));
printf("指针所占字节数为：%d\n", sizeof(_char_ptr));
```



# 3 动态内存分配

动态内存分配是伴随着传统数组的缺点一起产生的。

传统数组的缺点：

1. 传统数组长度固定，必须提前指定

   ```c
   int a[5];   // 合法
   int len = 5;
   int b[len]; // 非法
   ```

2. 传统数组的内存无法手动释放内存，只能等待程序结束或者栈退出后系统帮我们释放

3. 传统数组的长度不能在函数运行过程中动态扩充或缩小

4. 传统方式定义的数字组不能跨函数使用，例如：<u>在 A 函数中定义的数组在 A 运行期间可以被其他函数使用，在 A 函数运行结束后不能被其他函数使用</u>

所以我们就引入动态内存分配 malloc 函数，malloc 是 memory（内存）和 allocate（分配）两个单词的缩写

malloc 函数的声明如下：

```c
void *malloc(size_t size)
```

> 说明：
>
> - malloc 函数中的参数类型为 size_t(相当于 unsigned long long)，即传入的参数为一个无符号整数
> - 返回的数据类型为 void*（空指针） ，我们在接收的时候需要将其进行强制转换（转换为其他类型的指针）,该指针指向分配的内存空间中的第一个字节的地址
> - 使用 malloc 时，需要引入对应的库 `<malloc.h>`

动态内存分配的典型应用场景：==为数组和结构体分配内存==

```c
#include<stdio.h>
#include <malloc.h>
int main() {
    int* arr = (int*)malloc(sizeof(int) * 4); // 创建一个 int 类型的数组，数组的大小为 4

    for(int i = 0; i < 4; ++i) {
        arr[i] = i;
    }
    
    for(int i = 0; i < 4; ++i) {
        printf("%d ", arr[i]);
    }
    puts("");
    printf("%d", sizeof(arr) / sizeof(int));    // 对于这里，有一点一直没有弄清楚，为什么 arr 数组的大小还是 2

    free(arr);

    return 0;
}
```

除了 malloc 之外，`<malloc.h>` 中还存在如下的函数供我们使用：

- `free`：`void free (void * memblock)`，用于释放指针所指向的内存空间
- `colloc`：`void* calloc(size_t num_elements,size_t element_size);`,和 malloc 一样，用于分配内存空间，不同的是，在分配的同时会进行初始化（如果是整型就是 0）
- `recolloc`：`void* realloc(void* ptr, size_t size)`，用于调整已分配的 ptr 指向的内存块的大小

> 特别注意：
>
> 1. 使用 malloc 或 colloc 分配好内存空间后要进行释放操作，因为内存是从**堆**中申请的，如果在使用完后不及时释放，显然会造成内存泄漏的问题（同时，正是由于在堆中，所以即使栈退出后也可以继续使用）
> 2. 释放空间后的指针，并不会因为空间的释放而释放（<strong style="color:red">释放的是指针指向的空间，而不是指针</strong>）,所以建议在释放完成之后将指针置为 NULL，避免其成为野指针
> 3. 我们在分配内存时是有可能分配失败的，分配失败返回 NULL(0)，windows 平台由于采用了虚拟存储的技术，所以基本不会出现这种问题，但是在做嵌入式开发时就出现这种情况是很常见的
> 4. 上面的四种函数在 `<stdlib.h>` 中也被定义了，所以也可以使用该库

**示例代码**

```c
#include <stdio.h>
#include <stdlib.h>

void _PRINT_POINTER(
    int **_ptr,
    int _len)
{ // 这个地方切记一定要使用二级指针或是引用的方式，否则我们仅仅是通过拷贝的方式进行赋初值，换言之，对原来的
  // arr 没有任何影响，并且还在堆中浪费一个无效的内存块
    for (int i = 0; i < _len; ++i) {
        printf("%d ", (*_ptr)[i]);
    }
    printf("\n");
}

// 手写一个初始化函数
void _INIT_POINTER(int **_ptr, int _len, int _default_value)
{
    for (int i = 0; i < _len; ++i) {
        (*_ptr)[i] = _default_value;
    }
}

int main()
{
    int *arr = (int *)malloc(sizeof(int) * 5);
    _PRINT_POINTER(&arr,
                   5); // 可以看到打印出来的数据都是随机的，说明并没有将其初始化

    if (arr != NULL) {
        free(arr); // 注意：分配是有可能分配失败的，所以我们需要确认其非空后进行释放,同时将其置为NULL
        arr = NULL;
    }

    printf("\n--------------------------\n");

    // 使用 colloc 函数进行分配
    arr = (int *)calloc(5, sizeof(int));
    _PRINT_POINTER(&arr, 5); // 已全部被初始化，数值为 0

    // 使用
    _INIT_POINTER(&arr, 5, 2);

    _PRINT_POINTER(&arr, 5);

    // 是同 recalloc 重新分配内存空间
    realloc(arr, 10 * sizeof(int));

    if (arr != NULL) {
        free(arr);
        arr = NULL;
    }

    return 0;
}
```



# 4 枚举

枚举（enumeration）是一种派生数据类型，**它是由若干用户定义的若干枚举常量的集合。**

枚举的定义格式与结构体定义格式类似，如下：

```c
enum <类型名> {<枚举常量表>};
```

**注意：**

1. 枚举常量表中只能由标识符组成，不能由整型常量、字符常量等组成

   ```c
   typedef enum Color {
       orange, red, green, blue    // 正确格式
       // 1, 2, 3     // 错误格式
       // '1', '2', '3'    // 错误格式
       
   } Color;  
   ```

2. 枚举常量代表该枚举的变量可能取到的值，我们可以手动赋值，也可以让编译器为我们默认赋值，默认情况下从 0 开始递增，==枚举常量值 = 前一个枚举常量值 + 1==

   ```c
   typedef enum Weekday { 
       Sun = 0,
       Mon = 1,
       Tue,
       Wed,
       Thu,
       Fri,
       Sat 
   } Weekday;
   
   int main() {
       printf("%d %d %d %d %d %d %d", Sun, Mon, Tue, Wed,Thu, Fri, Sat);
       // 0 1 2 3 4 5 6
       return 0;
   }
   ```

3. 区分枚举常量和枚举变量：枚举常量通过 enum 关键字定义后可以直接拿来使用，而枚举变量则需要声明后使用，枚举常量的值只能取对应枚举常量

   ```c
   Weekday day_01, day_02, day_03;
   day_01 = Tue;
   // day_02 = 12; // 错误定义格式
   // day_01 = 2;  // 错误定义格式
   ```

