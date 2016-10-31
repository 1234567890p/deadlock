# deadlock
1.修改完example2的.dot截图
在example2.xml中将value值由3改为2即可。因为square的迭代次数变为了2，所以只生成了2个square。
2.example8的解释
进程定义
模块名字为“producer”,有两个<port type = “” name = “”/> 说明有2个端口output;
模块名字为“consumer”,有两个<port type = “” name = “”/> 说明有2个端口input
通道定义，有两根连线
数据通道上最多能容纳10个数据
链接规则:producer 和 consumer 之间有两个通道，分别为A,B。
producer A 输出口连接 C1 的输入口“in”；
producer B 输出口连接 C2 的输入口“in”；
C1 out 输出口连接 consumer 的输入口 “A”；
C2 out 输出口连接 consumer 的输入口 “B”；
1.当前位置小于len时：
   如果 PORT_OUTA端口能容纳1bit 的数据，将str 写到 producer 的端口“PORT_OUTA”上并输出 “p write to port A”
  否则，将 str 写到 producer 的端口“PORT_OUTB”上并输出 p write to port B(str)
2.当前位置若大于len时，进程终结。
如果 PORT_INB 端口存在1bit 的数据：
从 PORT_INA读取数据，并输出from port B (str)
从 PORT_INA读取数据，并输出：from port A (str)
由dot 图可以看出，producer 和 consumer 之间有2个通道 C1和C2，producer.c 和 consumer.c 按顺序运行，连接通道最多能容纳10个数据。
     在PORT_INB 端口（C2通道）不存在1bit的数据（consumer 不会取任何数据），即PORT_INA（C1通道）
     还能容纳数据（此时 producer 会往 A端口写 a 到 j 共10 个数据，然后向 B 端口写 k） 
     此时 PORT_INB 端口（C2通道）存在“k”这个数据，consumer 会从A端口读取2次数据。
     之后 PORT_INA（C1通道）的数据会小于10，producer 又会往 PORT_INA（C1通道）写数据
     consumer 会从PORT_INA（C1通道）读取2个数据，直到producer 往 A/B 写了20个数据为止
     
     
     4.产生死锁的4个必要条件
1.互斥（资源独占） 
一个资源每次只能给一个进程使用 
2.不可强占（不可剥夺） 
资源申请者不能强行的从资源占有者手中夺取资源，资源只能由占有者自愿释放 
3.请求和保持（部分分配，占有申请） 
一个进程在申请新的资源的同时保持对原有资源的占有（只有这样才是动态申请，动态分配） 
4.循环等待 
存在一个进程等待队列 {P1 , P2 , … , Pn}, 其中P1等待P2占有的资源，P2等待P3占有的资源，…，Pn等待P1占有的资源，形成一个进程等待环路。


count 值在这里实现一个delay.
 count的目的是methodA 与methodB 可以同时运行，此时A、 B 同时互相申请资源,将可能死锁。
之所以会死锁，就是在多次 count 之后，A，B 同时申请对方的资源，满足了循环等待条件，此时产生死锁的四个条件满足，发生死锁。
运行代码后线程被插入到调度队列，它会调用 a.methodB(a)方法；同时，运行 java Runable 代码，run()函数会直接调用运行，调用了b.methodB(a)方法。所以a，b 会互相调用资源产生死锁条件。

