# TCP的三次握手和四次挥手

## 1、三次握手——建立连接

- [ ] C 向 S发送报文 SYN seq = x
- [ ] S 向C 发送报文SYN seq = y  ACK = x + 1
- [ ] C 向 S发送报文ACK = y + 1

三次握手的目的建立安全的连接，C发送一个随机的 标志位值， S将该标志位值+1 并且生成另一个随机的标志位值一起发给C，C拿到后将收到的随机标志位值+1返给S，这样双方都能确定对方是安全的。

------

## 2、四次挥手——断开连接

- [ ] C 向 S发送报文 FIN seq = x + 2 ACK = x + 1
- [ ]  S 向C 发送报文 ACK = x + 3
- [ ]  S 向 C 发送报文 FIN seq = y  + 1
- [ ] C 向 S发送报文ACK = y + 1

步骤1 C 告诉 S 我没有数据要发送了，可以关闭连接了，但是C可以接受S的数据

步骤2 S告诉C 我知道你的没有数据了

步骤3 S告诉C 我也没有数据发送给你了 我也可以关闭连接了

步骤4 C告诉S 好的