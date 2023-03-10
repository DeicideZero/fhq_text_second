# **图解HTTP**

## 1.Web及网络基础

### 1.1.http协议访问web

通过发送请求获取服务器资源的Web浏览器可称为客户端

Web使用HTTP(超文本传输协议)作为规范，完成从客户端到服务器端等一系列运作流程

### 1.2.网络基础TCP/IP

#### 1.2.1.TCP/IP协议族

不同的硬件、操作系统之间的通信都需要一种规则。我们把这种规则称为协议。

把与互联网相关联的协议集合起来总称为TCP/IP。也有说法认为，TCP/IP 是指 TCP 和 IP 这两种协议。还有一种说法认为，TCP/IP 是在 IP 协议的通信过程中，使用到的协议族的统称。

#### 1.2.2.TCP/IP的分层管理

TCP/IP 协议族按层次分别分为以下 4 层：应用层、传输层、网络层和数据链路层。

##### 应用层

应用层决定了向用户提供应用服务时通信的活动。

TCP/IP 协议族内预存了各类通用的应用服务。比如，FTP（File Transfer Protocol，文件传输协议）和 DNS（Domain Name System，域名系统）服务就是其中两类。

HTTP 协议也处于该层。

##### 传输层

传输层对上层应用层，提供处于网络连接中的两台计算机之间的数据传输。

在传输层有两个性质不同的协议：TCP（Transmission Control Protocol，传输控制协议）和 UDP（User Data Protocol，用户数据报协议）。

##### 网络层（网络互联层）

网络层用来处理在网络上流动的数据包。数据包是网络传输的最小数据单位。该层规定了通过怎样的路径（所谓的传输路线）到达对方计算机，并把数据包传送给对方。

与对方计算机之间通过多台计算机或网络设备进行传输时，网络层所起的作用就是在众多的选项内选择一条传输路线。

##### 链路层（数据链路层，网络接口层）

用来处理连接网络的硬件部分。包括控制操作系统、硬件的设备驱动、NIC（Network Interface Card，网络适配器，即网卡），及光纤等物理可见部分（还包括连接器等一切传输媒介）。硬件上的范畴均在链路层的作用范围之内。

#### 1.2.3.TCP/IP通信传输流

利用 TCP/IP 协议族进行网络通信时，会通过分层顺序与对方进行通信。发送端从应用层往下走，接收端则往应用层往上走。

首先作为发送端的客户端在应用层（HTTP 协议）发出一个想看某个 Web 页面的 HTTP 请求。接着，为了传输方便，在传输层（TCP 协议）把从应用层处收到的数据（HTTP 请求报文）进行分割，并在各个报文上打上标记序号及端口号后转发给网络层。在网络层（IP 协议），增加作为通信目的地的 MAC 地址后转发给链路层。这样一来，发往网络的通信请求就准备齐全了。接收端的服务器在链路层接收到数据，按序往上层发送，一直到应用层。当传输到应用层，才能算真正接收到由客户端发送过来的 HTTP请求。

发送端在层与层之间传输数据时，每经过一层时必定会被打上一个该层所属的首部信息。反之，接收端在层与层传输数据时，每经过一层时会把对应的首部消去。这种把数据信息包装起来的做法称为封装。

### 1.3.IP,TCP,DNS

##### IP

IP（Internet Protocol）网际协议位于网络层，IP 协议的作用是把各种数据包传送给对方。而要保证确实传送到对方那里，则需要满足各类条件。其中两个重要的条件是 IP 地址和 MAC地址（Media Access Control Address）。

IP 地址指明了节点被分配到的地址，MAC 地址是指网卡所属的固定地址。IP 地址可以和 MAC 地址进行配对。IP 地址可变换，但 MAC地址基本上不会更改。

IP 间的通信依赖 MAC 地址。在网络上，通信的双方通常是经过多台计算机和网络设备中转才能连接到对方。在进行中转时，会利用下一站中转设备的MAC地址来搜索下一个中转目标，此时会采用ARP 协议（Address Resolution Protocol）根据通信方的IP地址反查出对应的MAC地址。

在到达通信目标前的中转过程中，那些计算机和路由器等网络设备只能获悉很粗略的传输路线。无论哪台计算机、哪台网络设备，它们都无法全面掌握互联网中的细节。

#### TCP

TCP 位于传输层，提供可靠的字节流服务。

TCP 协议为了更容易传送大数据才把数据分割，而且 TCP 协议能够确认数据最终是否送达到对方。

为了准确无误地将数据送达目标处，TCP 协议采用了三次握手（three-way handshaking）策略。用TCP协议把数据包送出去后，它会向对方确认是否成功送达。握手过程中使用了TCP的标志——SYN（synchronize）和 ACK（acknowledgement）。

发送端首先发送一个带 SYN 标志的数据包给对方。接收端收到后，回传一个带有 SYN/ACK 标志的数据包以示传达确认信息。最后，发送端再回传一个带 ACK 标志的数据包，代表“握手”结束。

若在握手过程中某个阶段莫名中断，TCP 协议会再次以相同的顺序发送相同的数据包。

#### DNS

DNS位于应用层，提供域名到IP地址之间的解析服务。

DNS 协议提供通过域名查找 IP 地址，或逆向从 IP 地址反查域名的服务。

### 1.4.各种协议与HTTP协议的关系

HTTP协议:生成针对目标Web服务器的HTTP请求报文；对Web服务器请求的内容进行处理

TCP协议:为了方便通信，将HTTP请求报文按序号分割成报文段，把每个报文段可靠地传给对方；按序号以原来的顺序重组从对方那里接收到的报文段。

IP协议:搜索对方的地址一边中转一边传送

### 1.5.URI和URL

#### URI(统一资源标识符)

URI 就是由某个协议方案表示的资源的定位标识符。协议方案是指访问资源所使用的协议类型名称。

URI 用字符串标识某一互联网资源，而 URL 表示资源的地点（互联网上所处的位置）。 URL 是 URI 的子集。　表示指定的 URI，要使用涵盖全部必要信息的绝对 URI、绝对 URL以及相对 URL。相对 URL，是指从浏览器中基本 URI 处指定的 URL，形如 /image/logo.gif。

#### URI格式

绝对URI:http://user:pass@www.example.jp:80/dir/index.htm?uid=1#ch1

协议方案名:使用 http: 或 https: 等协议方案名获取访问资源时要指定协议类型。不区分字母大小写，最后附一个冒号(:)

登录信息(认证)(可选):指定用户名和密码作为从服务器端获取资源时必要的登录信息（身份认证）。

服务器地址:地址可以是类似hackr.jp 这种 DNS 可解析的名称，或是 192.168.1.1 这类 IPv4 地址名，还可以是 [0:0:0:0:0:0:0:1] 这样用方括号括起来的 IPv6 地址名

服务器端口号(可选):指定服务器连接的网络端口号。若用户省略则自动使用默认端口号。

带层次的文件路径:指定服务器上的文件路径来定位特指的资源。与 UNIX 系统的文件目录结构相似

查询字符串(可选):针对已指定的文件路径内的资源，可以使用查询字符串传入任意参数。

片段标识符(可选):使用片段标识符通常可标记出已获取资源中的子资源（文档内的某个位置）。但在 RFC 中并没有明确规定其使用方法。

## 2.HTTP协议

HTTP协议用于客户端和服务端之间的通信，用 HTTP 协议能够明确区分哪端是客户端，哪端是服务器端。

HTTP 协议规定，请求从客户端发出，最后服务器端响应该请求并返回。

请求报文是由请求方法、请求 URI、协议版本、可选的请求首部字段和内容实体构成的。

接收到请求的服务器，会将请求内容的处理结果以响应的形式返回。响应报文基本上由协议版本、状态码（表示请求成功或失败的数字代码）、用以解释状态码的原因短语、可选的响应首部字段以及实体主体构成。

HTTP 是一种不保存状态，即无状态（stateless）协议。HTTP 协议自身不具备保存之前发送过的请求或响应的功能。

使用 HTTP 协议，每当有新的请求发送时，就会有对应的新响应产生。

HTTP/1.1 虽然是无状态协议，但为了实现期望的保持状态功能，于是引入了 Cookie 技术。有了 Cookie 再用 HTTP 协议通信，就可以管理状态了。

HTTP 协议使用 URI 定位互联网上的资源。当客户端请求访问资源而发送请求时，URI 需要将作为请求报文中的请求 URI 包含在内。

### HTTP方法

向请求 URI 指定的资源发送请求报文时，采用称为方法的命令。方法的作用在于，可以指定请求的资源按期望产生某种行为。

#### GET:获取资源

GET 方法用来请求访问已被 URI 识别的资源。指定的资源经服务器端解析后返回响应内容。

#### POST:传输实体主体

POST 方法用来传输实体的主体。用 GET 方法也可以传输实体的主体，但一般不用 GET 方法进行传输，而是用 POST 方法。虽说 POST 的功能与 GET 很相似，但POST 的主要目的并不是获取响应的主体内容

#### PUT

PUT 方法用来传输文件。就像 FTP 协议的文件上传一样，要求在请求报文的主体中包含文件内容，然后保存到请求 URI 指定的位置。

鉴于 HTTP/1.1 的 PUT 方法自身不带验证机制，任何人都可以上传文件 , 存在安全性问题，因此一般的 Web 网站不使用该方法。

#### HEAD:获得报文首部

HEAD 方法和 GET 方法一样，只是不返回报文主体部分。用于确认URI 的有效性及资源更新的日期时间等。

#### DELETE

DELETE方法用来删除文件，是与PUT相反的方法。DELETE 方法按请求 URI 删除指定的资源。，HTTP/1.1 的 DELETE 方法本身和 PUT 方法一样不带验证机制，所以一般的 Web 网站也不使用 DELETE 方法。

#### OPTIONS

OPTIONS 方法用来查询针对请求 URI 指定的资源支持的方法。

#### TRACE:追踪路径

TRACE 方法是让 Web 服务器端将之前的请求通信环回给客户端的方法。

客户端通过 TRACE 方法可以查询发送出去的请求是怎样被加工修改 / 篡改的，用来确认连接过程中发生的一系列操作。

(容易引发XST攻击，通常不会用到)

#### CONNECT:要求用隧道协议连接代理

CONNECT 方法要求在与代理服务器通信时建立隧道，实现用隧道协议进行 TCP 通信。主要使用 SSL（Secure Sockets Layer，安全套接层）和 TLS（Transport Layer Security，传输层安全）协议把通信内容加密后经网络隧道传输。

### 持久连接

持久连接的特点是，只要任意一端没有明确提出断开连接，则保持 TCP 连接状态。

持久连接减少了 TCP 连接的重复建立和断开所造成的额外开销，减轻了服务器端的负载。另外，减少开销的那部分时间，使HTTP 请求和响应能够更早地结束，提高了Web页面的显示速度

在 HTTP/1.1 中，所有的连接默认都是持久连接

#### 管线化

管线化技术出现后，不用等待响应亦可直接发送下一个请求。能够做到同时并行发送多个请求，不需要一个接一个地等待响应。

### Cookie

Cookie 技术通过在请求和响应报文中写入 Cookie 信息来控制客户端的状态。

Cookie 会根据从服务器端发送的响应报文内的一个叫做 Set-Cookie的首部字段信息，通知客户端保存 Cookie。当下次客户端再往该服务器发送请求时，客户端会自动在请求报文中加入 Cookie 值后发送出去。服务器端发现客户端发送过来的 Cookie 后，会去检查究竟是从哪一个客户端发来的连接请求，然后对比服务器上的记录，最后得到之前的状态信息。

## 3.HTTP报文
