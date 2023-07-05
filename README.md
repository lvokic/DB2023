## Storage

> `inline`是一个关键字，用于向编译器提供一个提示，建议将函数的定义内联展开。
>
> 在给定的上下文中，`inline`关键字在函数定义之前使用，指示编译器将函数体内联展开。函数定义内联展开的意思是，在调用函数的地方，编译器会直接将函数体的代码插入到调用的位置，而不是生成一个函数的调用指令。
>
> 使用`inline`关键字可以提高函数调用的效率，特别是对于一些简单、频繁调用的函数。通过内联展开，可以减少函数调用的开销，避免函数调用的额外开销，例如压栈、跳转等操作。
>
> 在给定的代码中，如果在函数定义之前使用了`inline`关键字，那么编译器有可能会将`Get()`函数的定义内联展开，以提高函数调用的效率。但是需要注意的是，`inline`只是一个建议，具体是否内联展开还取决于编译器的决策和优化策略。编译器可能会根据一些因素，例如函数的复杂性、调用频率、编译器设置等，来决定是否内联展开函数。
>
> 需要指出的是，使用`inline`关键字并不是强制要求编译器进行内联展开，而是提供了一个建议。编译器最终是否内联展开函数，还是根据自身的规则和优化策略来决定。

> `template<>` 是用于对模板进行特化的关键字。
>
> 在给定的上下文中，`template<>` 表示对特定模板进行特化，指示编译器为指定的模板参数提供特殊化的实现。
>
> 在代码示例中，`template<>` 的作用是对 `std::hash` 模板进行特化，特化的类型是 `PageId`。通过在 `template<>` 后面提供空的尖括号 `<>`，表示对 `std::hash` 模板的默认参数进行特化。
>
> 在特化的结构体 `std::hash<PageId>` 中，可以为 `PageId` 类型定义一个特殊化的哈希函数，以适应特定的需求或数据结构。
>
> 需要注意的是，`template<>` 关键字通常与特化的结构体或函数定义一起使用，以明确指示编译器对特定的模板进行特化。在特化中，可以针对特定的模板参数提供不同的实现，以满足特定的需求或处理方式。

> `constexpr` 关键字用于声明一个编译时常量，这意味着变量的值在编译时就已经确定，并且可以在编译期间进行计算和使用。它使得编译器能够在编译时进行优化，并在需要的地方直接使用常量的值，而不需要在运行时进行计算。

> `explicit` 是一个关键字，用于修饰类的构造函数，指示编译器不要进行隐式的类型转换。
>
> 在 C++ 中，构造函数可以被用于执行隐式的类型转换。例如，如果一个类定义了一个带有单个参数的构造函数，并且该参数的类型与另一个类型存在隐式的转换关系，那么在需要该类类型的地方，编译器会自动调用该构造函数进行隐式转换。
>
> 使用 `explicit` 关键字可以显式地指示编译器只能进行显式的类型转换，禁止隐式的类型转换。当构造函数被声明为 `explicit` 时，只能通过显式地调用构造函数来创建对象，而不能通过隐式转换来进行。
>
> 使用 `explicit` 关键字可以防止意外的类型转换，增加代码的可读性和明确性。它可以在构造函数的声明前加上 `explicit` 关键字，或者在构造函数的定义中使用。
>
> 需要注意的是，`explicit` 关键字只能应用于类的构造函数，而不能应用于其他类型的函数。它只对单参数构造函数有效，对于多参数的构造函数，不能使用 `explicit` 关键字。



#### Page.h

> ```c++
> inline int64_t Get() const {
>         return (static_cast<int64_t>(fd << 16) | page_no);
>   		}
> ```
>
> 给定的代码是一个名为`Get()`的成员函数，返回一个`int64_t`类型的值。
>
> `Get()`函数的返回值通过位运算的方式计算得到，包括按位或（`|`）操作和按位左移（`<<`）操作。
>
> 让我们逐步解释代码：
>
> 1. `fd << 16`：这个语句对变量`fd`进行了位左移操作，左移了16位。`<<`运算符将`fd`的二进制表示向左移动16位。这相当于将`fd`乘以2^16。
>
> 2. `static_cast<int64_t>(fd << 16)`：位左移操作的结果被转换为`int64_t`类型。这是为了确保结果值可以存储在64位有符号整数中。
>
> 3. `page_no`：这是另一个类型未知的变量，表示某个页面编号。
>
> 4. `fd << 16 | page_no`：按位或（`|`）操作将步骤2中的位左移值与`page_no`的值结合起来。按位或操作将结果的每个位设置为1，如果在`fd << 16`或`page_no`中对应的位中有任一位为1。
>
> 5. 最后，`Get()`函数返回位或操作的结果值。
>
> 总结起来，`Get()`函数通过按位运算的方式将位左移后的`fd`值与`page_no`的值组合起来，返回一个64位有符号整数。

> ```c++
> struct PageIdHash {
>     size_t operator()(const PageId &x) const { return (x.fd << 16) | x.page_no; }
> };
> ```
>
> 这段代码定义了一个结构体 `PageIdHash`，它实现了一个函数调用运算符（`operator()`），用于计算 `PageId` 对象的哈希值。
>
> 在这个哈希函数中，它使用了位运算的方式计算哈希值。具体来说，它将 `PageId` 对象 `x` 的 `fd` 成员变量进行位左移操作（`x.fd << 16`），然后将结果与 `x.page_no` 成员变量进行按位或操作（`|`）。最后，将计算得到的结果作为哈希值返回。
>
> 这个哈希函数的目的是为了将 `PageId` 对象映射到一个哈希表中的桶（bucket），以便在哈希表中进行高效的查找和插入操作。通过使用位左移和按位或操作，可以将 `fd` 和 `page_no` 组合成一个唯一的整数值，用作哈希表中的键。
>
> 需要注意的是，这个哈希函数假设 `fd` 和 `page_no` 是适合用于哈希计算的类型，并且不会导致哈希冲突（即不同的 `PageId` 对象不会产生相同的哈希值）。如果这两个成员变量存在哈希冲突的风险，可能需要考虑其他哈希算法或冲突解决策略来改进哈希函数的实现。

> ```c++
> template <>
> struct std::hash<PageId> {
>     size_t operator()(const PageId &obj) const { return std::hash<int64_t>()(obj.Get()); }
> };
> ```
>
> 这段代码是对 `PageId` 类型的哈希函数进行特化，使其能够与标准库的 `std::hash` 模板进行配合使用。
>
> 在这个特化的 `std::hash` 结构体中，它实现了一个函数调用运算符（`operator()`），接受一个 `PageId` 对象 `obj` 作为参数，并返回一个 `size_t` 类型的哈希值。
>
> 在函数体内部，它调用了 `std::hash<int64_t>()` 来计算 `obj.Get()` 的哈希值。`obj.Get()` 是 `PageId` 类中的成员函数，根据之前的代码可知，它返回一个 `int64_t` 类型的值，该值是通过位运算计算得到的。
>
> 通过调用 `std::hash<int64_t>()` 来计算 `obj.Get()` 的哈希值，可以利用标准库提供的哈希函数实现来对 `PageId` 类型进行哈希计算。这样就能够在使用标准库的哈希容器（例如 `std::unordered_map`）时，直接使用 `PageId` 对象作为键，并正确地进行哈希和查找操作。
>
> 需要注意的是，为了使这个特化的哈希函数生效，通常需要包含 `<functional>` 头文件，因为 `std::hash` 是在该头文件中定义的。另外，为了正确使用这个特化的哈希函数，需要确保 `PageId` 类型已经定义了 `Get()` 成员函数，并且 `int64_t` 类型的哈希函数可用。

#### Defs.h

> ```c++
> template<typename T, typename = typename std::enable_if<std::is_enum<T>::value, T>::type>
> std::ostream &operator<<(std::ostream &os, const T &enum_val) {
>     os << static_cast<int>(enum_val);
>     return os;
> }
> ```
>
> 这段代码定义了一个模板函数 `operator<<`，用于重载输出流（`std::ostream`）中插入操作符（`<<`），以便输出枚举类型的值。
>
> 该模板函数具有两个模板参数：`T` 和一个默认类型参数。使用 `std::enable_if` 结合 `std::is_enum` 可以实现对枚举类型的特化。
>
> 在函数的实现中，它首先将枚举值 `enum_val` 转换为整数类型，然后通过输出流 `os` 将整数值输出到流中。最后，它返回输出流对象的引用。
>
> 具体步骤如下：
>
> 1. 使用 `std::enable_if` 结合 `std::is_enum` 检查类型 `T` 是否为枚举类型。`std::is_enum<T>::value` 用于判断 `T` 是否为枚举类型，如果是，则 `std::enable_if` 返回 `T` 类型本身。
>
> 2. 在模板函数的参数列表中，第二个参数 `typename = typename std::enable_if<std::is_enum<T>::value, T>::type` 用于启用类型 `T` 为枚举类型时的特化。
>
> 3. 在函数体中，通过 `static_cast<int>(enum_val)` 将枚举值 `enum_val` 转换为整数类型，并通过 `os` 输出流将整数值写入流中。
>
> 4. 最后，返回输出流对象 `os` 的引用，以支持链式操作。
>
> 该模板函数的作用是使得可以直接使用输出流插入操作符 `<<` 来输出枚举类型的值。通过将枚举值转换为整数类型，可以将其输出到流中，以便进行显示或存储。

latest：make -j4运行，代码编写完成前无法正常运行



## RECORD

#### Bitmap(位图)

> 

#### Rm_file_handle

> ```c++
> struct RmPageHandle {
>     const RmFileHdr *file_hdr;  // 当前页面所在文件的文件头指针
>     Page *page;                 // 页面的实际数据，包括页面存储的数据、元信息等
>     RmPageHdr *page_hdr;        // page->data的第一部分，存储页面元信息，指针指向首地址，长度为sizeof(RmPageHdr)
>     char *bitmap;               // page->data的第二部分，存储页面的bitmap，指针指向首地址，长度为file_hdr->bitmap_size
>     char *slots;                // page->data的第三部分，存储表的记录，指针指向首地址，每个slot的长度为file_hdr->record_size
> RmPageHandle(const RmFileHdr *fhdr_, Page *page_) : file_hdr(fhdr_), page(page_) {
>     page_hdr = reinterpret_cast<RmPageHdr *>(page->get_data() + page->OFFSET_PAGE_HDR);
>     bitmap = page->get_data() + sizeof(RmPageHdr) + page->OFFSET_PAGE_HDR;
>     slots = bitmap + file_hdr->bitmap_size;
> }
> 
> 	// 返回指定slot_no的slot存储收地址
> char* get_slot(int slot_no) const {
>     return slots + slot_no * file_hdr->record_size;  // slots的首地址 + slot个数 * 每个slot的大小(每个record的大小)
> 	}
> };
> ```
