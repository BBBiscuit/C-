**C#中的网络通信编程**

网络相关的命名空间：using System.Net、using System.Net.Sockets

.NET定义了两个类来处理关于IP地址的问题

------

### 一、IPAddress  

**提供 Internet 协议 (IP) 地址**

1．public  byte[]  **GetAddressBytes** (); 

以字节数组形式提供 IPAddress 的副本

例子：`Byte[]  bytes = curAdd.GetAddressBytes();`

2．public static System.Net.IPAddress  **Parse** (string ipString);

**静态方法，返回一个IPAdrress实例**

3．public override string ToString (); 

将 Internet 地址转换为标准表示法。

### 二、IPEndPoint    

**IPEndPoint 对象来表示一个特定的IP地址和端口的组合，应用该对象的场景多是在讲socket绑定到本地地址或者将socket绑定到非本地地址。**

#### 1．构造函数

IPEndPoint(IPAddress, Int32) 

用指定的地址和端口号初始化 IPEndPoint 类的新实例。

#### 2．属性

public  System.Net.IPAddress  **Address** { get; set; } 

获取或设置终结点的 IP 地址。

public  override  System.Net.Sockets.AddressFamily  **AddressFamily** { get; } 

获取网际协议 (IP) 地址族。

public int **Port** { get; set; } 

获取或设置终结点的端口号。

#### 3 .  方法

public override System.Net.EndPoint  **Create** (System.Net.SocketAddress socketAddress);

从套接字地址创建终结点。

例子：

```c#
// Recreate the connection endpoint from the serialized information.

IPEndPoint endpoint = new IPEndPoint(0,0);

IPEndPoint clonedIPEndPoint = (IPEndPoint) endpoint.Create(socketAddress);

Console.WriteLine("clonedIPEndPoint: " + clonedIPEndPoint.ToString());
```

 public  override  System.Net.SocketAddress  **Serialize** ();

将终结点信息序列化为 SocketAddress 实例。

例子：

```c#
SocketAddress socketAddress = endpoint.Serialize();
```

public override string **ToString** ();

返回指定终结点的 IP 地址和端口号。

### 三、IPHostEntry

为 Internet 主机地址信息提供容器类。

属性

 public  System.Net.IPAddress[]   **AddressList** { get; set; } 

获取或设置与主机关联的 IP 地址列表。

public string **HostName** { get; set; }

获取或设置主机的 DNS 名称。

### 四、Dns 

提供简单的域名解析功能，静态类

#### 方法 

public  static  System.Net.IPHostEntry  **GetHostEntry** (System.Net.IPAddress address);

public  static  System.Net.IPHostEntry  **GetHostEntry** (string  hostNameOrAddress);

将主机名或 IP 地址解析为 IPHostEntry 实例。

### 五、Socket 

Socket类能够使用通信协议中列出的ProtocolType枚举（IP/TCP/UDP），使您能够同时同步和异步数据传输。

#### 1. 构造函数

Public Socket (Sockets.SocketType  socketType, ProtocolType  protocolType);

两个参数分别为socketType类型和协议类型，<u>两参数并不独立，前者是隐式的协议</u>。

Socket(AddressFamily, SocketType, ProtocolType)

AddressFamily参数为AddressFamily Enum值之一，指定 Socket 类的实例可以使用的<u>寻址方案</u>。（比如InterNetwork  2  IP版本4的地址） 

#### 2. 属性

public  System.Net.Sockets.SocketType  **SocketType** { get; } 

获取 Socket 的类型。返回值为SocketType枚举值之一，比如<u>Stream(1)</u>等

#### 3. 方法

**Connect**(String, Int32) 

**Connect**(IPAddress[], Int32)  

**Connect**(IPAddress, Int32) 

**Connect**(IPEndPoint)

与远程主机建立连接。

**Send** (Byte[], Int32, SocketFlags)

将数据发送到连接的 Socket。
