##《精通Objective-C》读书笔记

### 第二章  使用类

1. 下划线访问属性与（点语法和getter/setter）访问属性的区别

   一般情况下，外部的类访问对象a的属性时，应当使用a.name或者[a name]方法，但是若在类内部，即对象a可能还没有创建好，此时应当使用 _name的下划线方式进行访问。

---

###第三章 对象和消息传递

1. 工厂方法

   工厂方法是指用于创建和初始化类的便捷方法，它是类方法，如下所示：

   ```objective-c
   //Class name : Person
   -(id) initWithName:(NSString*)name;
   +(id) personWithName:(NSString*)name;//工厂方法
   
   //implemention
   -(id) initWithName:(NSString*)name {
       if(self = [super init]) {
           _name = name;
       }
       return self;
   }
   +(id) personWithName:(NSString*)name {
       return [[[self class] alloc] initWithName:name];
   }
   ```

   在工厂方法中，使用[self class]获取当前实例对象对应的类，与直接使用类名相比，这种方式可以在被子类调用时，能够返回与子类相同的对象。

---

###第四章 内存管理

1. Objective-C可执行程序的组成

   可执行代码：二进制代码，存储于代码区域，以静态方式分配内存，与程序的生命周期相同。

   常量字符串：存储在常量区域，程序结束后由系统释放。

   

   链接信息：存储于静态（全局）区域，以静态方式分配内存，与程序的生命周期相同。

   重定位信息：存储于静态（全局）区域，以静态方式分配内存，与程序的生命周期相同。

   **初始化和未初始化的程序数据**：包括静态变量和程序常量，在编译期间进行初始化（static，const）。存储于静态 

   ​                                                （全局）区域，以静态方式分配内存，与程序的生命周期相同。

   

   **局部数据**：存储在栈中，包括局部变量、方法的输入参数、返回值、方法调用结束后继续执行程序的代码地址等。

   ​                  静态分配栈内存是编译器完成的，比如自动变量(auto)的分配； 动态分配栈内存由alloca函数完成。

   **动态数据**：运行时OC通过alloc方法动态创建的对象，存储在堆中，需要对其进行内存管理。

2. 栈与堆的区别

  栈：

  * 栈区中的变量由**编译器**负责分配和释放，由系统自动完成。只要栈的剩余空间大于所申请空间，系统将为程序提供内存，否则将报异常提示栈溢出。

  * 栈是**向低地址**扩展的数据结构，是一块**连续**的内存的区域。栈顶的地址和栈的最大容量是系统预先规定好的。

  堆：

  * 操作系统有一个**记录空闲内存地址的链表**。当系统收到程序的申请时，会遍历该链表，寻找第一个空间大于所申请空间的堆结点，然后将该结点从空闲结点链表中删除，并将该结点的空间分配给程序。由于找到的堆结点的大小不一定正好等于申请的大小，系统会自动的将多余的那部分重新放入空闲链表中，所以容易产生内存碎片。

  * 堆是**向高地址**扩展的数据结构，是**不连续**的内存区域。


3. static、const、extern、和宏定义的区别与联系

   static：static修饰**局部变量**时延长其 **生命周期** 到程序结束，并且在内存中只有一份，不会改变局部变量的作用域；

   ​            static修饰**全局变量**，将其 **作用域** 限制在所在文件才能访问，生命周期不改变，仍旧是随程序一起结束。

   const：限制其**右边**的量为只读而不被改变。

   * 与static一起使用，定义一个静态常量：static  NSString * const key = @"name"; 【场景：在某一个文件中经常使用的常量】 
   * 与extern一起使用，定义一个全局常量：extern NSString* const key = @"name"; 【场景：在多个文件之间共享的常量，因为如果使用static const组合则需要在每个文件都定义一次；一般会新建一个GlobalConst的文件，统一管理extern const联合修饰的全局常量】

   extern：满足访问全局变量的需求，先在当前文件查找有没有全局变量，若没有找到，则会去其他文件查找 ；

   ​             extern int a;这里的a并不会分配内存，只是声明其为全局变量。 

   宏定义：#define NAME @“JackPanda”;    宏定义产生的常量与const定义的常量（只读变量）的区别：

   1. 宏的执行发生在预编译时期，const发生在编译时期。

   2. 宏只是进行替换，没有类型检查，不会报错；const会有类型检查，可以报错。

   3. 宏可以定义函数，const不可以。

   4. 大量使用宏会导致编译时间变久，因为每次都要重新替换。

4. 在ARC下，使用生命周期限定符（__ weak、__ strong、__ unsafe__ unretained、__ autoreleasing）的正确姿势：NSString * __ strong name = @"JackPanda";

5. __ strong修饰的变量的含义是：被修饰的引用对应的变量会在作用范围内（通常是变量声明时所在的花括号对的内部，如方法、循环、条件语句块等）一直被保留，常规变量的默认修饰符为__ strong。

---

### 第五章 预处理器 

1. Objective-C编译过程

   * 词法分析：将源代码拆分成多个记号（token），每个记号是一个独立地语言元素，如关键字、操作符、名称标识符等。预处理操作即是在此期间。

   * 语法分析：通过检查语法的正确与否创建出抽象语法树（AST）。

   * 生成中间代码：将AST用于生成中间代码或者机器语言代码，并进行优化。

   * 汇编：接收上一阶段生成的代码，将其转换成目标平台上可执行的机器代码。

   * 链接：将汇编得到的一段或者多段代码合并成一个独立的可执行程序。

2. 预处理器的工作内容

   1. 文本翻译：将输入的源代码拆分成代码行、使用单个字符替换三字符组合、使用单个空格替换注释、将断开的多个连续行合并为较长的代码行。

   2. 记号转换：将上一步处理过的代码转换成token序列。

   3. 基于预处理语言的转换：如果token序列中含有预处理语言元素，则根据这些token进行转换。

      注：前两个操作是自动进行的，第三个操作是根据源代码中的“预处理器语言”函数执行的。

3. 预处理器语言

   * 预处理器指令（由预处理器而不是编译器处理）

     1. 头文件包含

        * 预处理器获取被包含的文件的文本，并插入到当前文件中。
        * 引号包含（""）方式首先从源文件所在的当前目录查找被包含的头文件，找不到再去默认目录中搜索；尖括号包含（<>）方式会去默认目录搜索相关头文件。在使用时应当使用尖括号引用标准头文件，用引号引用自己编写的头文件。

        * "#inlcude"与"#import"的区别：#import可以防止递归包含，确保头文件只在当前文件中被包含一次，所以一般使用#import包含OC头文件；若要在使用#include时防止递归包含，应当在被包含的头文件中添加**包含警卫**，如下代码：

          ```objective-c
          #ifndef ATOM_H
          #define ATOM_H
          @interface Atom:NSObject
          //Atom的接口声明
          @end
          #endif
          ```

          代码中的#ifndef指令（条件编译）和#define指令共同组成了包含警卫。

     2. 条件编译（根据条件是否成立，确定是否包含部分或者全部源文本）

        * \#if

          用于测试条件表达式，与#endif指令配对使用。

        * \#elif

          用于条件判定为否时，#elif位于#if和#endif之间，当有多个#elif时，第一个为true的条件表达式对应的条件文本会被包含。

        * \#else

          在#if和#elif都为false的时候，#else对应的文本会被包含。

        * \#endif

          与#if、#ifdef配对使用

        * \#ifdef

          当#ifdef指令中的宏被定义过时，相关文本会被包含到源文件中。

        * \#ifndef

          当其指令中的宏未被定义时，包含相关文本代码。

     3. 诊断

        \#warning "警告信息"

        \#error "错误信息"

        \#line 行号 "文件名"  //生成一条错误信息，带有错误发生的文件名和行号

     4. "#pragma"指令

        设置超出Objective-C语言范畴的、额外的编译器选项，如#pragma mark、#pragma clang等

   * 宏

     * \#undef 可以解除宏定义

     * 宏定义函数时要主要添加括号，因为宏只是文本的替换，不加括号容易产生错误

     * 使用\符号可以换行，#可以将参数字符串化：#define f(x) #x; 则 f(foo)等价于"foo"。

---

### 第六章 专家级技巧：使用ARC

1. 获取和释放对象的所有权

   获取：alloc、new、copy、mutableCopy

   释放：

   * 重新分配变量（改变指针的指向，编译器会自动插入release代码）

   * 给变量赋值nil（在设置nil的语句后面，编译器会自动插入release代码）

   * 释放对象的所有者（若一个对象的属性也是在堆内存中创建的，则该对象被调用dealloc释放时，它拥有的属性对象也会由编译器发送release消息，此外编译器还会像该类的父类发送的dealloc消息以完成对象图的生命周期的管理；Foundation框架包含各种集合类[对象集]，集合类拥有存储在其中的对象的所有权，当集合类的实例被释放时，ARC会自动像集合类中的每个对象发送release消息）

2. 通过类中属性的getter方法获取属性对象，在ARC下的内存管理方式：ARC会自动插入retain和autorelease。

3. 桥接转换

   * 场景：Apple提供的基于C语言的Core Foundation框架和基于Objective-C的Foundation框架需要互用转换。

   * 方法：直接桥接和ARC桥接。

     1. 直接桥接：只能**手动管理Core Foundation**框架数据类型对象的内存，**ARC下会报错**。分为隐式转换和显示转换，如下所示：

        ```objective-c
        CFStringRef cstr = CFStringCreateWithCString(NULL, "JackPanda", kCFStringEncodingASCII);
        //隐式转换
        NSString* str = cstr;
        //显示转换
        NSString* str = (NSString*)cstr;
        ```

        

     2. ARC桥接转换（ARC下）

        * \__bridge：不改变被转换变量的内存管理方式，即将CF（Core Foundation）类型的对象转换成F（Foundation）类型的对象，变量的内存管理方式仍然是手动管理的，将F类型的对象转换成CF类型的对象，变量的内存管理方式仍然是ARC。所以需要注意内存泄漏和悬挂指针问题。
        * \__bridge__retained：将F类型的变量转换成CF类型的变量，按照手动方式管理内存。
        * \__bridge__transfer：将CF类型的变量转换成F类型的变量，按照ARC方式管理内存。

        使用方法：(桥接转换标记  目的数据类型) 变量名，如( \__bridge__transfer NSString*) cstr。

---

### 第七章 运行时系统 

1. OC运行时的动态特性包括：

   * 动态类型（id）
   * 消息机制（objc_msgSend）
   * 动态绑定（多态性，给id类型的对象发送消息）
   * 动态决议（通过重写NSObject的resolveInstanceMethod:方法或者resolveClassMethod:方法动态地为类添加方法的实现，通过调用class_addMethod()方法）
   * 动态加载（根据需要在运行时动态加载NSBundle包，包括与应用程序相关的**代码**与**数据资源**、软件框架或库、自定义代码的**插件**）
   * 内省（NSObject类中的用于内省的API，方便在运行时查询与方法、类与对象等的信息，如isKindOfClass:、responseToSelector:、是否遵守某协议的conformsToProtocol:、为选择器提取方法签名的methodSignatureForSelector:）

2. 创建选择器(SEL)：@selector()方式在编译期创建，NSSelectorFromString()方式在运行时创建。

   ```objective-c
   //编译器创建SEL选择器
   SEL sel1 = @selector(sumAdd:plus:);
   //运行时创建SEL选择器(Foundation框架)
   SEL sel2 = NSSelectorFromString(@"sumAdd:plus:");
   ```

---

### 第八章 运行时系统的结构 

1. 