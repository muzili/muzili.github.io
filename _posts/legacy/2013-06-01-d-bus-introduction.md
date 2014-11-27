介绍
====

D-Bus 是一个用于低延时低开销(low-overhead)的进程间通讯的系统,
D-Bus是用于本机通信的目的，为系统的应用程序之间提供了一个快速轻量级的统一通信接口或者说中间件层。坦白将，D-Bus相对去其他进程间通信，在速度上并没有什么显著的优势,
Frank在"CORBA, DCOP and DBUS A performance comparison"一文中比较了DBUS,
CORBA和DCOP的性能, 这一点在"D-Bus
FAQ"中也有描述。但是由于其轻量和易于使用，仍然渐渐取代CORBA和DCOP,
成了Linux进程间通信的事实标准。在嵌入式平台上，D-Bus也在渐渐发力，在Maemo,
OpenEmbedded, Android和monlin等平台中都可以见到其身影。

-   低延时：因为它在设计时就避免了往返的交互并允许异步操作，更象X协议。
-   低开销：它使用了二进制协议，不需要象XML那样转换为文本格式/从文本格式转换。因为D-Bus是用于高敏感本机(high-resolution
    same-machine)的进程间通讯，基本上不用于互联网上的进程间通讯，这是令人感兴趣的优化部分。
-   易用性：它按照消息而不是字节流来工作，并且自动地处理了许多困难的IPC问题，并且D-Bus库以可以封装的方式来设计，这样开发者就可以使用框架里存在的对象/类型系统，而不用学习一种新的专用于IPC的对象/类型系统。

D-Bus支持一对一通信和发布/订阅模式.

D-Bus为两个专门的用例来设计：

-   系统总线：用于从系统向用户会话发送通知以及允许系统向用户会话请求输入。
-   会话总线：用于实现桌面环境(如GNOME和KDE)

{{{more}}}

消息协议(Message Protocol)
==========================

消息由头部和消息体组成，如果你把消息当作一个package，那头部就是地址，消息体就是包的内容。消息发送系统使用头部的信息来知道把消息送往何处，如何解释消息，接收者则解释消息体。消息体可以不带或带多个参数，这些参数是具有类型的值，如integer或byte数组。

消息头和消息体都使用相同类型的type系统及格式来序列化数据。每种值的类型都有报文(wire)格式，从某种别的表示把值转换为报文(wire)格式叫作列集(marshalling)，而把报文(wire)格式转回去则叫散集(unmarshalling).

类型签名(Type Signatures)
-------------------------

D-Bus协议并不包含列集数据中的类型标签，一组已列集的值必须具有知名类型签名。类型签名由类型代码构成，类型代码是用一个ASCII字符表示值的类型。因为使用了ASCII字符，类型签名一直会形成一个合法的ASCII码串，一个简单的串比较就能决定两个类型签名是否相同。
作为一个简单的例子，32位整数(INT32)的类型代码
是ASCII码字符'i'，所以含有单个INT32的一组值的签名将是：

``` {.c}
          "i"
```

一组包含两个INT32的值具有下面签名：

``` {.c}
          "ii"
```

所有基本类型以上面例子中的INT32一样。为了散/列集基本的类型，你只需要简单地从数据块中读出
相应于每种类型代码在签名中的值。除了基本类型，还有四种容器类型：结构，数据，可变体及字典。

结构也有类型代码，ASCII字符'r'，但是这种类型代码并不会在签名中出现。ASCII字符'('和')'用于标记结
构的开始和结束位置。例如，一个包含两个整数的结构的签名如下：

``` {.c}
             "(ii)"
```

结构可以嵌套，例如，一个包含一个整数和另一个结构的结构：

``` {.c}
            "(i(ii))"
```

存储该结构的值块将包含三个整数，类型签名能让你把"(i(ii))"与((ii)i)"或"(iii)"或"iii"区
分开来。

结构类型代码"r"当前没有在D-Bus协议中使用，但是它在实现协议的代码中是有用的，这种类型代码被指定为允许在非协议环境中操作。

数组的类型代码是'a'，数组类型代码必须跟随一个单一完整类型，此类型即为数组中元素的类型，例：

``` {.c}
"ai"
```

可变体类型代码是'v'，一个列集了的可变体类型的值将有单一完整类型的签名作为其值的一部分，
其签名将紧跟着列集的那种类型的值。
字典与结构基本一样，只是它使用花括号而不是括号，并且它有更多的限制：它仅仅作为数组元素
类型出现，它有准确的两个单一完整类型在花括号里，第一个(即key)必须是基本类型而不是容器类型
实现不能在数组外接受字典类型，不能接受0个，1个或多于两个域的字典项，不能接受以非基本类型为
键值的项，一个字典项总是一个键-值对。
字典项的第一个域总是键，在同一个字典数组中如果相同的键出现了两次那么该消息就被认为是坏的。
但是，因为性能的原因，并不要求实现拒绝带有重复键的字典。在大多数语言中，一个字典项数组将
被表示为map, hash表或字典对象。

下表总结了D-Bus的类型

\#+tablename D-Bus的类型签名

  ------------------- --------------------------------------------------- --------------------------------------------------------------------------------------------
  Conventional Name   Code                                                Descripttion
  INVALID             0 (ASCII NUL)                                       Not a valid type code, used to terminate signatures
  BYTE                121 (ASCII 'y')                                     8-bit unsigned integer
  BOOLEAN             98 (ASCII 'b')                                      Boolean value, 0 is FALSE and 1 is TRUE. Everything else is invalid.
  INT16               110 (ASCII 'n')                                     16-bit signed integer
  UINT16              113 (ASCII 'q')                                     16-bit unsigned integer
  INT32               105 (ASCII 'i')                                     32-bit signed integer
  UINT32              117 (ASCII 'u')                                     32-bit unsigned integer
  INT64               120 (ASCII 'x')                                     64-bit signed integer
  UINT64              116 (ASCII 't')                                     64-bit unsigned integer
  DOUBLE              100 (ASCII 'd')                                     IEEE 754 double
  STRING              115 (ASCII 's')                                     UTF-8 string (must be valid UTF-8). Must be nul terminated and contain no other nul bytes.
  OBJECT~PATH~        111 (ASCII 'o')                                     Name of an object instance
  SIGNATURE           103 (ASCII 'g')                                     A type signature
  ARRAY               97 (ASCII 'a')                                      Array
  STRUCT              114 (ASCII 'r'), 40 (ASCII '('), 41 (ASCII ')')     Struct
  VNRIANT             118 (ASCII 'v')                                     Variant type (the type of the value is part of the value itself)
  DICT~ENTRY~         101 (ASCII 'e'), 123 (ASCII '{'), 125 (ASCII '}')   Entry in a dict or map (array of key-value pairs)
  ------------------- --------------------------------------------------- --------------------------------------------------------------------------------------------

列集 Marshaling (Wire Format)
-----------------------------

对于给定类型的签名，字节块能够转换为具有类型的值，本节描述字节块的格式，对于所有的D-Bus
类型，字节序及对齐问题被统一处理。

一个字节块具有与之相关联的字节序，字节序以某种方式被发现，对于D-Bus消息，字节序是消息头的部分(“Message
Format” ). 目前，假定字节序是已知的little endian或big endian.

字节块中的每个值是“自然地”对齐的，例如，4字节值被与4字节边界对齐，8字节值与8字节边界对齐，为了正确地对齐值，可能有必要做对齐填充。对齐填充必须总是为了正确地对齐随后的值所需的最小的填充并且必须由nul字节组成。对齐填充不能是左未被始化(即不能包含垃圾)不能使用多于需要的填充。

据此，列集后类型如下：

\#+tablename D-Bus的列集对齐格式

  ------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- --------------------------------
  Conventional Name   Encoding                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              Alignment
  INVALID             Not applicable; cannot be marshaled.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  N/A
  BYTE                A single 8-bit byte.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1
  BOOLEAN             As for UINT32, but only 0 and 1 are valid values.                                                                                                                                                                                                                                                                                                                                                                                                                                                                     4
  INT16               16-bit signed integer in the message's byte order.                                                                                                                                                                                                                                                                                                                                                                                                                                                                    2
  UINT16              16-bit unsigned integer in the message's byte order.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  2
  INT32               32-bit signed integer in the message's byte order.                                                                                                                                                                                                                                                                                                                                                                                                                                                                    4
  UINT32              32-bit unsigned integer in the message's byte order.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  4
  INT64               64-bit signed integer in the message's byte order.                                                                                                                                                                                                                                                                                                                                                                                                                                                                    8
  UINT64              64-bit unsigned integer in the message's byte order.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  8
  DOUBLE              64-bit IEEE 754 double in the message's byte order.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   8
  STRING              A UINT32 indicating the string's length in bytes excluding its terminating nul, followed by non-nul string data of the given length, followed by a terminating nul byte.                                                                                                                                                                                                                                                                                                                                              4 (for the length)
  OBJECT~PATH~        Exactly the same as STRING except the content must be a valid object path (see below).                                                                                                                                                                                                                                                                                                                                                                                                                                4 (for the length)
  SIGNATURE           The same as STRING except the length is a single byte (thus signatures have a maximum length of 255) and the content must be a valid signature (see below).                                                                                                                                                                                                                                                                                                                                                           1
  NRRAY               A UINT32 giving the length of the array data in bytes, followed by alignment padding to the alignment boundary of the array element type, followed by each array element. The array length is from the end of the alignment padding to the end of the last element, i.e. it does not include the padding after the length, or any padding after the last element. Arrays have a maximum length defined to be 2 to the 26th power or 67108864. Implementations must not send or accept arrays exceeding this length.   4 (for the length)
  STRUCT              A struct must start on an 8-byte boundary regardless of the type of the struct fields. The struct value consists of each field marshaled in sequence starting from that 8-byte alignment boundary.                                                                                                                                                                                                                                                                                                                    8
  VNRIANT             A variant type has a marshaled SIGNATURE followed by a marshaled value with the type given in the signature. Unlike a message signature, the variant signature can contain only a single complete type. So "i" is OK, "ii" is not.                                                                                                                                                                                                                                                                                    1 (alignment of the signature)
  DICT~ENTRY~         Identical to STRUCT.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  8
  ------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- --------------------------------

有效对象路径(Valid Object Paths)
--------------------------------

一个对象路径是一个用于引用对象实例的名字，从概念上讲，D-Bus信息交换中每个参与者可能有任意数量的对象实例并且每个这样的实例都有一个路径，就象文件系统一样，一个应用中的对象实例形成一个层次树。

下面的规则定义了一个有效的对象路径，实现不能发送或接收带有非法对象路径的消息

-   路径可以有任意长度。
-   路径必须以ASCII字符'/'开始并且必须由/分隔的元素组成。
-   每个元素必须只包含ASCII字符"[A-Z][a-z][0-9]\_"
-   没有元素可能是空串
-   序列中不能出现多个"/"字符
-   尾部的'/'字符是不允许的除非路径是根路径(只有单一的'/'字符)

有效签名(Valid Signatures)
--------------------------

实现不能发送或接收无效的签名，有效的签名符合以下规则：

-   签名以nul字节结束。
-   签名是单一完整类型的列表，数组必须有元素类型，结构必须有"("和")"
-   签名中只能有类型代码与正反(花)括号，结构类型代码不允许在签名中，因为使用了括号。
-   容器类型最大嵌套深度是32个数组类型代码与32个左括号，这即是说最大总的递归深度是64，如一个数组的数组的数组的...结构的结构的结构的.."中有32个数组和32个结构
-   签名的最大长度为255
-   签名必须以nul来终结

消息格式(Message Format)
------------------------

消息由消息头和消息体组成，头部是有固定签名和意义的值块。消息体是另外的值块，带有在头部中指定的签名。头部的长度必须是8的倍数，这样当在一个缓冲区中存贮整个消息时可以允许消息体以8对齐开始。如果头部不是自然地终止于8字节边界上，则必须加上最多7字节的nul初始化的对齐填充。

消息体不需要终止于8字节边界上。
消息的最大长度，包括头，头对齐填充以及消息体是2的27次幂即134217728。实现不能发送或接收超过此大小的消息。

头部的签名是: "yyyyuua(yv)"，以更为可读的方式写出是：

BYTE, BYTE, BYTE, BYTE, UINT32, UINT32, ARRAY of STRUCT of
(BYTE,VNRIANT)

这些值具有下面的意思：

\#+tablename D-Bus 消息格式

  ----------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Value                               Descripttion
  1st BYTE                            Endianness flag; ASCII 'l' for little-endian or ASCII 'B' for big-endian. Both header and body are in this endianness.
  2nd BYTE                            Message type. Unknown types must be ignored. Currently-defined types are described below.
  3rd BYTE                            Bitwise OR of flags. Unknown flags must be ignored. Currently-defined flags are described below.
  4th BYTE                            Major protocol version of the sending application. If the major protocol version of the receiving application does not match, the applications will not be able to communicate and the D-Bus connection must be disconnected. The major protocol version for this version of the specification is 1.
  1st UINT32                          Length in bytes of the message body, starting from the end of the header. The header ends after its alignment padding to an 8-boundary.
  2nd UINT32                          The serial of this message, used as a cookie by the sender to identify the reply corresponding to this request.
  ARRAY of STRUCT of (BYTE,VNRIANT)   An array of zero or more header fields where the byte is the field code, and the variant is the field value. The message type determines which fields are required.
  ----------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 消息类型

会显示在头部的第二个字节中的消息类型

\#+tablename D-Bus的消息类型

  ------------------- --------------- ------------------------------------------------------------------------------------
  Conventional name   Decimal value   Descripttion
  INVALID             0               This is an invalid type.
  METHOD~CALL~        1               Method call.
  METHOD~RETURN~      2               Method reply with returned data.
  ERROR               3               Error reply. If the first argument exists and is a string, it is an error message.
  SIGNAL              4               Signal emission.
  ------------------- --------------- ------------------------------------------------------------------------------------

### 消息标志

消息头部中第三字节中的标志位

\#+tablename D-Bus的消息标志

  ------------------- ----------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Conventional name   Hex value   Descripttion
  NO~REPLYEXPECTED~   0x1         This message does not expect method return replies or error replies; the reply can be omitted as an optimization. However, it is compliant with this specification to return the reply despite this flag and the only harm from doing so is extra network traffic.
  NO~AUTOSTART~       0x2         The bus must not launch an owner for the destination name in response to this message.
  ------------------- ----------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 头部域(Header Fields)

在消息头尾部的数组包含了头部域，这里每个域都是一个字域代码紧接域值。头部必须包含该消息类型中需要的头部域，以及0个或更多的任意可选的头部域。本协议规范的未来版本可能加上新的域。实现必须忽略他们不能理解的域。实现不能开发他们自己的头部域，只有在改变此规范时才能引入新的头部域。
如果一个实现看到一个他所不期待的头部域代码，他必须忽略此域，因为他将成为本规范的新版本(但是与本版本兼容)的一部分，这也适用于已知的头部域出现在不期望的消息里。例如，如果Signal有一个答复序列，它必须被忽略即使它在规范的当前版本中没有意义。
但是，实现不能发送或接收已知头部域有着错误的类型存贮在域值里。如一个带有类型UINT32的INTERFACE域的消息被认为是坏的消息。

这里是当前定义的头部域

\#+tablename D-Bus消息头部域

  ------------------- -------------- -------------- ----------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Conventional Name   Decimal Code   Type           Required In             Descripttion
  INVALID             0              N/A            not allowed             Not a valid field name (error if it appears in a message)
  PATH                1              OBJECT~PATH~   METHOD~CALL~, SIGNAL    The object to send a call to, or the object a signal is emitted from. The special path /org/freedesktop/DBus/Local is reserved; implementations should not send messages with this path, and the reference implementation of the bus daemon will disconnect any application that attempts to do so.
  INTERFACE           2              STRING         SIGNAL                  The interface to invoke a method call on, or that a signal is emitted from. Optional for method calls, required for signals. The special interface org.freedesktop.DBus.Local is reserved; implementations should not send messages with this interface, and the reference implementation of the bus daemon will disconnect any application that attempts to do so.
  MEMBER              3              STRING         METHOD~CALL~, SIGNAL    The member, either the method name or signal name.
  ERROR~NAME~         4              STRING         ERROR                   The name of the error that occurred, for errors
  REPLY~SERIAL~}      5              UINT32         ERROR, METHOD~RETURN~   The serial number of the message this message is a reply to. (The serial number is the second UINT32 in the header.)
  DESTINATION         6              STRING         optional                The name of the connection this message is intended for. Only used in combination with the message bus, see the section called “Message Bus Specification”.
  SENDER              7              STRING         optional                Unique name of the sending connection. The message bus fills in this field so it is reliable; the field is only meaningful in combination with the message bus.
  SIGNATURE           8              SIGNATURE      optional                The signature of the message body. If omitted, it is assumed to be the empty signature "" (i.e. the body must be 0-length).
  ------------------- -------------- -------------- ----------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

有效名字(Valid Names)
---------------------

不同的名字在D-Bus消息中有一些约束，255的最长名字也适用于总线名字，接口以及成员。

### 接口名字(Interface names)

接口有类型为STRING的名字，意味着他们必须是有效的UTF-8字符串，但是也有一些另外的约束特别适用于接口名字：

-   接口名字由一个或多个由'.'字符分隔开的元素组成，所有元素必须包含至少一个字符
-   每个元素必须由ASCII字符 "[A-Z][a-z][0-9]\_"组成并且不能以数字开始
-   接口名字必须至少包含一个'.'字符(因而必须有两个元素)
-   接口名不能以"."字符开始
-   接口名不能超过最大长度限

### 总线名字(Bus names)

连接有一个或多个总线名字与之相连，一个连接只有一个总线名字并且是惟一无二的连接名字，在此连接的整个生命周期，这个名字归于此连接。总线名字是STRING类型的，意味着它必须是有效的UTF-8字符串，但是还有一些特别的约束用于总线名字：

-   总线名以':'开始，并且是惟一的连接名字
-   总线名字由一个或多个由'.'字符所分隔的元素组成，所有的元素必须包含至少一个字符
-   每个元素必须只包含ASCII字符"[A-Z][a-z][0-9]\_-".
    只有那些是惟一连接名字的一部分的元素可以以数字开头，在其它总线名字中的元素不能以数字开始
-   总线名字中至少包含一个'.'字符 (因此至少包含两个元素).
-   总线名字不能以"."字符开始
-   总线名字不能超过最大名字长度限制

### 成员(Member names)

成员(即方法或信号)名字：

-   必须只包含"[A-Z][a-z][0-9]\_"且不能以数字开始
-   不能包含"."字符
-   不能起始过最大名字长度限制。
-   最少有1字节的长度

### 错误名字(Error names)

错误名字与接口名字有相同的限制

消息类型(Message Types)
-----------------------

每个消息类型 (METHOD~CALL~, METHOD~RETURN~, ERROR, and
SIGNAL)都有自己期望的用法与头部域，本节描述了这些用法.

### 方法调用(Method Calls)

一些消息会调用远程对象的操作，这些消息被称作方法调用消息，具有类型标签METHOD~CALL~.在类型的语言中，这些消息很自然地被映射到对象的方法上。

方法调用消息需要有MEMBER头部域用以指明方法的名称，此消息还可能有一个INTERFACE域来给出接口，被调用的方法也是接口的一部分。在没有INTERFACE域时，如果同一对象上的两个接口有同名的方法名字，哪一个方法会被调用是没有定义的。在这种具有二义性的环境里，实现可以选择返回一个错误。但是，如果方法名是独一无二的，实现不能强制要求需要interface域。

方法调用消息也包括一个PATH域，该域用以指明在哪个对象上调用该方法。如果此调用正在消息总线中传播，消息也有一个DESTINATION域用以给出接收此消息的连接名称。

当应用程序处理方法调用消息时，它需要答复，答复由REPLY~SERIAL~}头部域确定
，此头部域也表明了被答复的方法调用的序列号。答复类型有两种，METHOD~RETURN和ERROR~。

如果答复类型为METHOD~RETURN~，答复消息的参数就是方法调用的返回值或"out
parameters"，如果答复类型为ERROR，那么会抛出例外，方法调用失败，此时没有返回值提供。对于同一个方法调用发送多个答复是没有意义的。

即使一个方法调用没有返回值，一个METHOD~RETURN的答复也是必须的~，这样调用者就能知道方法是否被成功地处理了。

METHOD~RETURN~}和ERROR答复消息必须有REPLY~SERIAL头部域~。

如果METHOD~CALL~}消息具有NO~REPLYEXPECTED~}标志，那么作为优化，接收到方法调用的用可以选择忽略答复消息(不论答复是METHOD~RETURN还是ERROR~），但是，忽略NO~REPLYEXPECTED标志具作出答复也是可以接受的~。

除非一个消息有NO~AUTOSTART标志~，如果目的名字不存在那么拥有此目的名字的程序会在消息被发送出前启动。消息会一直保留直到新的程序成功启动或者启动失败。如果启动失败，将会返回一个错误，此标志只与消息总线中的上下文有关，在没有中间总线的一对一的通讯中此标记被忽略。

### 信号发射(Signal Emission)

信号发射不象方法调用需要答复。信号发射只是简单单一信息类型SIGNAL。它必须有三个头域，PATH给出发送信号的对象，加上INTERFACE和
MEMBER给出信号的全称名字。INTERFACE头部域在信号中是必须的，尽管它在方法调用中是可选的。

### 错误(Errors)

ERROR类型信息大多数时通常是METHOD~CALL的答复~，但是也有可能是对任何类型信息的答复，例如，如果消息总线没有足够的内存来发送信号，它可能会返回ERROR来答复一个信号。

ERROR可以有任意的参数，但是如果第一个参数是STRING，它一定是一个错误消息，错误消息可以记入日志或以某种方式展示给用户。
本文档记法(Notation in this document)

Hello D-Bus with libdbus
========================

创建一个D-Bus Handle
--------------------

``` {.c}
   DBusError err;
   DBusConnection* conn;
   int ret;
   // initialise the errors
   dbus_error_init(&err);

   // connect to the bus
   conn = dbus_bus_get(DBUS_BUS_SESSION, &err);
   if (dbus_error_is_set(&err)) { 
      fprintf(stderr, "Connection Error (%s)\n", err.message); 
      dbus_error_free(&err); 
   }
   if (NULL == conn) { 
      exit(1); 
   }
```

dbus~busget会检查一个数组busconnection~}有没有初始化，如果没有初始化，则进行初始化。以后同一process调用dbus~busget都会从busconnection中返回一个connection~.
由此可见, 一个process的所以dbus 连接是共享的, 区别再用Interface和方法.

发送信号
--------

``` {.c}
   dbus_uint32_t serial = 0; // unique number to associate replies with requests
   DBusMessage* msg;
   DBusMessageIter args;

   // create a signal and check for errors 
   msg = dbus_message_new_signal("/test/signal/Object", // object name of the signal
         "test.signal.Type", // interface name of the signal
         "Test"); // name of the signal
   if (NULL == msg) 
   { 
      fprintf(stderr, "Message Null\n"); 
      exit(1); 
   }

   // append arguments onto signal
   dbus_message_iter_init_append(msg, &args);
   if (!dbus_message_iter_append_basic(&args, DBUS_TYPE_STRING, &sigvalue)) { 
      fprintf(stderr, "Out Of Memory!\n"); 
      exit(1);
   }

   // send the message and flush the connection
   if (!dbus_connection_send(conn, msg, &serial)) { 
      fprintf(stderr, "Out Of Memory!\n"); 
      exit(1);
   }
   dbus_connection_flush(conn);

   // free the message 
   dbus_message_unref(msg);

```

dbus~messagenewsignal~}会创建一个信号message,
目的地址是"/test/signal/Object", 接口名字是"test.signal.Type",
名字是"Test".
然后调用dbus~messageiterxxx~}填充参数,最后通过dbus~connectionsend发送出去~.实际是发送到dbus-daemon,
然后通过dbus-daemon转发出去.

接收信号
--------

``` {.c}
   // add a rule for which messages we want to see
   dbus_bus_add_match(conn, 
         "type='signal',interface='test.signal.Type'", 
         &err); // see signals from the given interface
   dbus_connection_flush(conn);
   if (dbus_error_is_set(&err)) { 
      fprintf(stderr, "Match Error (%s)\n", err.message);
      exit(1); 
   }

   // loop listening for signals being emmitted
   while (true) {

      // non blocking read of the next available message
      dbus_connection_read_write(conn, 0);
      msg = dbus_connection_pop_message(conn);

      // loop again if we haven't read a message
      if (NULL == msg) { 
         sleep(1);
         continue;
      }

      // check if the message is a signal from the correct interface
      // and with the correct name
      if (dbus_message_is_signal(msg, "test.signal.Type", "Test")) {
         // read the parameters
         if (!dbus_message_iter_init(msg, &args))
            fprintf(stderr, "Message has no arguments!\n"); 
         else if (DBUS_TYPE_STRING != dbus_message_iter_get_arg_type(&args)) 
            fprintf(stderr, "Argument is not string!\n"); 
         else {
            dbus_message_iter_get_basic(&args, &sigvalue);
            printf("Got Signal with value %s\n", sigvalue);
         }
      }

      // free the message
      dbus_message_unref(msg);
   }
```

如果一个process想接收某个信号,可以通过dbus~busaddmatch~}来选择想要接收的消息.本例比较简单,直接使用while
loop来读取message, 在现实应用中, 一般用select/poll IO复用.
当有消息到达时,可以通过dbus~connectionreadwrite~}和dbus~connectionpopmessage获取messgae~.
然后就是对于message的处理了.
也是用dbus~messageiterxxx来解析message参数~.

方法调用
--------

``` {.c}
   DBusMessage* msg;
   DBusMessageIter args;
   DBusPendingCall* pending;

   msg = dbus_message_new_method_call("test.method.server", // target for the method call
         "/test/method/Object", // object to call on
         "test.method.Type", // interface to call on
         "Method"); // method name
   if (NULL == msg) { 
      fprintf(stderr, "Message Null\n");
      exit(1);
   }

   // append arguments
   dbus_message_iter_init_append(msg, &args);
   if (!dbus_message_iter_append_basic(&args, DBUS_TYPE_STRING, &param)) { 
      fprintf(stderr, "Out Of Memory!\n"); 
      exit(1);
   }

   // send message and get a handle for a reply
   if (!dbus_connection_send_with_reply (conn, msg, &pending, -1)) { 
                                                           // -1 is default timeout
      fprintf(stderr, "Out Of Memory!\n"); 
      exit(1);
   }
   if (NULL == pending) { 
      fprintf(stderr, "Pending Call Null\n"); 
      exit(1); 
   }
   dbus_connection_flush(conn);

   // free message
   dbus_message_unref(msg);

   bool stat;
   dbus_uint32_t level;

   // block until we receive a reply
   dbus_pending_call_block(pending);

   // get the reply message
   msg = dbus_pending_call_steal_reply(pending);
   if (NULL == msg) {
      fprintf(stderr, "Reply Null\n"); 
      exit(1); 
   }
   // free the pending message handle
   dbus_pending_call_unref(pending);

   // read the parameters
   if (!dbus_message_iter_init(msg, &args))
      fprintf(stderr, "Message has no arguments!\n"); 
   else if (DBUS_TYPE_BOOLEAN != dbus_message_iter_get_arg_type(&args)) 
      fprintf(stderr, "Argument is not boolean!\n"); 
   else
      dbus_message_iter_get_basic(&args, &stat);

   if (!dbus_message_iter_next(&args))
      fprintf(stderr, "Message has too few arguments!\n"); 
   else if (DBUS_TYPE_UINT32 != dbus_message_iter_get_arg_type(&args)) 
      fprintf(stderr, "Argument is not int!\n"); 
   else
      dbus_message_iter_get_basic(&args, &level);

   printf("Got Reply: %d, %d\n", stat, level);

   // free reply and close connection
   dbus_message_unref(msg);   
```

方法调用可以有返回值,也可以没有返回值.使用dbus~messagenewmethodcall~},第一个参数是目的路径,第二个参数是对象路径,第三个参数是接口地址,第四个参数是方法.然后通过dbus~messageiterxxx~}传递参数.
如果有返回值,需要调用dbus~connectionsendwithreply~}.通过dbus~pendingcallblock等待返回~,如果有返回,
则调用dbus~pendingcallstealreply得到返回消息~.最后通过dbus~messageiterxxx对消息进行处理~.

方法实现
--------

``` {.c}
   // loop, testing for new messages
   while (true) {
      // non blocking read of the next available message
      dbus_connection_read_write(conn, 0);
      msg = dbus_connection_pop_message(conn);

      // loop again if we haven't got a message
      if (NULL == msg) { 
         sleep(1); 
         continue; 
      }

      // check this is a method call for the right interface and method
      if (dbus_message_is_method_call(msg, "test.method.Type", "Method"))
         reply_to_method_call(msg, conn);

      // free the message
      dbus_message_unref(msg);
   }

void reply_to_method_call(DBusMessage* msg, DBusConnection* conn)
{
   DBusMessage* reply;
   DBusMessageIter args;
   DBusConnection* conn;
   bool stat = true;
   dbus_uint32_t level = 21614;
   dbus_uint32_t serial = 0;
   char* param = "";

   // read the arguments
   if (!dbus_message_iter_init(msg, &args))
      fprintf(stderr, "Message has no arguments!\n"); 
   else if (DBUS_TYPE_STRING != dbus_message_iter_get_arg_type(&args)) 
      fprintf(stderr, "Argument is not string!\n"); 
   else 
      dbus_message_iter_get_basic(&args, &param);
   printf("Method called with %s\n", param);

   // create a reply from the message
   reply = dbus_message_new_method_return(msg);

   // add the arguments to the reply
   dbus_message_iter_init_append(reply, &args);
   if (!dbus_message_iter_append_basic(&args, DBUS_TYPE_BOOLEAN, &stat)) { 
      fprintf(stderr, "Out Of Memory!\n"); 
      exit(1);
   }
   if (!dbus_message_iter_append_basic(&args, DBUS_TYPE_UINT32, &level)) { 
      fprintf(stderr, "Out Of Memory!\n"); 
      exit(1);
   }

   // send the reply && flush the connection
   if (!dbus_connection_send(conn, reply, &serial)) { 
      fprintf(stderr, "Out Of Memory!\n"); 
      exit(1);
   }
   dbus_connection_flush(conn);

   // free the reply
   dbus_message_unref(reply);
}
```

方法的实现也比较简单, 本例中使用while loop, 在现实中一般使用select/poll
IO复用.当有消息到达时,可以通过dbus~connectionreadwrite~}和dbus~connectionpopmessage获取messgae~.
然后就是对于message的处理了.
也是用dbus~messageiterxxx来解析message参数~, 然后返回结果.

Hello D-Bus with vala/gdbus
===========================

D-Bus server
------------

``` {.c}
[DBus (name = "com.example.HelloWorld")]
public class HelloWorldService : Object
{
    private int counter;
    public int ping (string msg) {
        stdout.printf ("%s\n", msg);
        return counter++;
    }
}

void on_bus_aquired (DBusConnection conn)
{
    try {
        conn.register_object ("/com/example/hello", new HelloWorldService ());
    } catch (IOError e) {
        stderr.printf ("Could not register service\n");
    }
}

void main ()
{
    Bus.own_name (BusType.SESSION, "com.example.HelloWorld", BusNameOwnerFlags.NONE,
                  on_bus_aquired,
                  () => 
                  {
                  },
                  () => stderr.printf ("Could not aquire name\n"));


    new MainLoop ().run ();
}
#+end_#+BEGIN_SRC 

```

D-Bus client
------------

``` {.c}
[DBus (name = "com.example.HelloWorld")]
interface HelloWorld : Object
{
    public abstract int ping (string msg) throws IOError;
}

void main (string[] args)
{
    try {
        HelloWorld hello = Bus.get_proxy_sync (BusType.SESSION, "com.example.HelloWorld",
                                        "/com/example/hello");
        foreach (string arg in args) {
            int pong = hello.ping ("Hello from " + arg);
            stdout.printf ("%d\n", pong);
        }
    } catch (IOError e) {
        stderr.printf ("%s\n", e.message);
    }
}
```

Compile and execute
-------------------

### Compile

``` {.example}
valac --header=helloworld-client.h --save-temps --pkg gio-2.0 helloworld-client.vala -o helloworld-client
```

会生成中间文件helloworld-client.[ch]和可执行文件helloworld-client

``` {.example}
valac --header=helloworld-service.h --save-temps --pkg gio-2.0 helloworld-service.vala -o helloworld-service
```

会生成中间文件helloworld-service.[ch]和可执行文件helloworld-service

### Execute

``` {.example}
joshua@joshua-laptop:~/dbus-test$ ./helloworld-service 
```

会启动hello world service, 继续调用client

``` {.example}
joshua@joshua-laptop:~/dbus-test$ ./helloworld-client Josh Joe
0
1
2
```

这时候hello world service 会输出下面的的结果。

``` {.example}
joshua@joshua-laptop:~/dbus-test$ ./helloworld-service 
Hello from ./helloworld-client
Hello from Josh
Hello from Joe
```

Hello D-Bus with gdbus
======================

D-Bus server
------------

``` {.c}
#include <gio/gio.h>
#include <stdlib.h>

static GDBusNodeInfo *introspection_data = NULL;

/* Introspection data for the service we are exporting */
static const gchar introspection_xml[] =
  "<node>"
  "  <interface name='com.example.HelloWorld'>"
  "    <annotation name='com.example.Annotation' value='OnInterface'/>"
  "    <annotation name='com.example.Annotation' value='AlsoOnInterface'/>"
  "    <method name='Ping'>"
  "      <annotation name='com.example.Annotation' value='OnMethod'/>"
  "      <arg type='s' name='ping' direction='in'/>"
  "      <arg type='i' name='pong' direction='out'/>"
  "    </method>"
  "  </interface>"
  "</node>";

/* ---------------------------------------------------------------------------------------------------- */

static int count=0;

static void
handle_method_call (GDBusConnection       *connection,
                    const gchar           *sender,
                    const gchar           *object_path,
                    const gchar           *interface_name,
                    const gchar           *method_name,
                    GVariant              *parameters,
                    GDBusMethodInvocation *invocation,
                    gpointer               user_data)
{
  if (g_strcmp0 (method_name, "Ping") == 0)
    {
      const gchar *ping;

      g_variant_get (parameters, "(&s)", &ping);
      g_print("Recevied ping from %s\n", ping);
      g_dbus_method_invocation_return_value (invocation,
                                             g_variant_new ("(i)", count++));
    }
}

/* for now */
static const GDBusInterfaceVTable interface_vtable =
{
    handle_method_call,
};

static void
on_bus_acquired (GDBusConnection *connection,
                 const gchar     *name,
                 gpointer         user_data)
{
  guint registration_id;

  registration_id = g_dbus_connection_register_object (connection,
                                                       "/com/example/hello",
                                                       introspection_data->interfaces[0],
                                                       &interface_vtable,
                                                       NULL,  /* user_data */
                                                       NULL,  /* user_data_free_func */
                                                       NULL); /* GError** */
  g_assert (registration_id > 0);

}

static void
on_name_acquired (GDBusConnection *connection,
                  const gchar     *name,
                  gpointer         user_data)
{
}

static void
on_name_lost (GDBusConnection *connection,
              const gchar     *name,
              gpointer         user_data)
{
  exit (1);
}

int
main (int argc, char *argv[])
{
  guint owner_id;
  GMainLoop *loop;

  g_type_init ();

  /* We are lazy here - we don't want to manually provide
   * the introspection data structures - so we just build
   * them from XML.
   */
  introspection_data = g_dbus_node_info_new_for_xml (introspection_xml, NULL);
  g_assert (introspection_data != NULL);

  owner_id = g_bus_own_name (G_BUS_TYPE_SESSION,
                             "com.example.HelloWorld",
                             G_BUS_NAME_OWNER_FLAGS_NONE,
                             on_bus_acquired,
                             on_name_acquired,
                             on_name_lost,
                             NULL,
                             NULL);

  loop = g_main_loop_new (NULL, FALSE);
  g_main_loop_run (loop);

  g_bus_unown_name (owner_id);

  g_dbus_node_info_unref (introspection_data);

  return 0;
}

```

D-Bus client
------------

``` {.c}
#include <stdio.h>
#include <stdlib.h>
#include <gio/gio.h>

static void
ping (GDBusProxy *proxy)
{
    GVariant *result;
    GError *error;
    int pong;

    error = NULL;
    result = g_dbus_proxy_call_sync (proxy,
                                     "Ping",
                                     g_variant_new ("(s)", "Josh"),
                                     G_DBUS_CALL_FLAGS_NONE,
                                     -1,
                                     NULL,
                                     &error);
    g_assert_no_error (error);
    g_assert (result != NULL);
    g_variant_get (result, "(i)", &pong);
    g_print("%d\n", pong);
    g_variant_unref (result);
    exit(0);

}

static void
on_name_appeared (GDBusConnection *connection,
                  const gchar     *name,
                  const gchar     *name_owner,
                  gpointer         user_data)
{
    GDBusProxy *proxy;
    GError *error;

    error = NULL;
    proxy = g_dbus_proxy_new_sync (connection,
                                   G_DBUS_PROXY_FLAGS_NONE,
                                   NULL,                      /* GDBusInterfaceInfo */
                                   "com.example.HelloWorld", /* name */
                                   "/com/example/hello", /* object path */
                                   "com.example.HelloWorld",        /* interface */
                                   NULL, /* GCancellable */
                                   &error);
    g_assert_no_error (error);
    ping (proxy);
    g_object_unref (proxy);
}


static void
on_name_vanished (GDBusConnection *connection,
                  const gchar     *name,
                  gpointer         user_data)
{

    g_printerr ("Failed to get name owner for %s\n",
                name);
    exit (1);
}

int
main (int argc, char *argv[])
{
    guint watcher_id;
    GMainLoop *loop;
    g_type_init ();

    watcher_id = g_bus_watch_name (G_BUS_TYPE_SESSION,
                                   "com.example.HelloWorld",
                                   G_BUS_NAME_WATCHER_FLAGS_NONE,
                                   on_name_appeared,
                                   on_name_vanished,
                                   NULL,
                                   NULL);


    loop = g_main_loop_new (NULL, FALSE);
    g_main_loop_run (loop);
    g_bus_unwatch_name (watcher_id);
    return 0;
}

```

Compile and execute
-------------------

``` {.example}
gcc -Wall -g `pkg-config --cflags --libs gio-2.0` helloworld-client-gdbus.c -o helloworld-client-gdbus
```

build the gdbus client

``` {.example}
gcc -Wall -g `pkg-config --cflags --libs gio-2.0` helloworld-service-gdbus.c -o helloworld-service-gdbus
```

``` {.example}
joshua@joshua-laptop:~/dbus-test$ ./helloworld-service-gdbus 
```

会启动hello world service, 继续调用client

``` {.example}
joshua@joshua-laptop:~/dbus-test$ ./helloworld-client-gdbus
0
```

这时候hello world service 会输出下面的的结果。

``` {.example}
joshua@joshua-laptop:~/dbus-test$ ./helloworld-service-gdbus 
Received ping from Josh
```

{{{more}}}

