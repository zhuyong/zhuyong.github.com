title: java socket编程-数据的发送和接收
date: 2013-08-20 21:58:42
tags: java_socket
---
####数据的发送和接收
TCP/IP协议是以字节的方式传输用户数据的，并没有对其进行检查和修改。 
TCP/IP协议唯一约束的是，信息必须在块（chunks）中发送和接收，而块的长度必须是8的倍数，因此，我们可以认为在TCP/IP协议中传输的信息是字节序列。 
鉴于此，我们可以进一步把信息看作是数字序列或数组，每个数字表示的大小是0~255，即00000000~11111111。 
<!--more-->
#####简单数据类型是如何通过套接字进行发送和接收的？ 
传输信息时可以通过套接字将字节信息写入一个OutputStream实例中，或将其封装进一个DatagramPacket实例中，然后进行发送。 
但是这些操作所能处理的唯一数据类型是字节或字节数组。 

#####整型类型 
对于基本类型:整型，发送端和接收端需要约定的几个方面： 
1. 要传输的整型字节的大小(Size),即该类型所需要的字节长度； 
   如32位机器上,byte(1字节),short（2字节）,int(4字节)，long(8字节)； 
2. 对于超过一个字节的数据类型，我们必须需要知道字节序列发送的顺序:从左到右（big-endian:高位-->低位），从右到左（little-endian:低位-->高位）。 
3. 所传输的数值是有符号的还是无符号的；java中的四种基本整型都是有符号的，它们的值以二进制补码的方式存储。 

<strong>Code:</strong>怎样将消息的正确值存入字节数组？
```
    public static void main(String[] args) {
        int iValue = 8;
        short sValue = 1;
        ByteArrayOutputStream buf = new ByteArrayOutputStream();
        DataOutputStream out = new DataOutputStream(buf);
        try {
            // 输出流
            out.writeInt(iValue);
            out.writeShort(sValue);
            out.flush();
            // 使用字节数组中转
            byte[] msg = buf.toByteArray();
            // 输入流
            ByteArrayInputStream inbuf = new ByteArrayInputStream(msg);
            DataInputStream din = new DataInputStream(inbuf);
            // 按顺序读出
            int va = din.readInt();
            short vb = din.readShort();
            System.out.println("int =" + va + ",short=" + vb);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```
#####字符串和文本 
每个String实例都对应一个字符序列（数组,char[]类型）；
一个字符在Java内部表示为一个整数，在一组符号和一组整数之间的映射称为编码字符集。 

Java是使用的Unicode的国际标准编码字符集来表示char型和String型值。Unicode字符集将符号映射到整数0~65535之间，更适用于国际化程序。 

1. 发送者和接收者必须在符号与整数的映射方式上达成共识，才能使用文本信息进行通信。 
   对于每个整数值都比255小的一小组字符，不需要其他信息，因为其每个字符都能够作为一个单独的字节进行编码。 
   对于可能超过一个字节的大整数的编码方式，就有多种方式在线路上对其进行编码，因此发送者和接收者需要统一编码方案。 
   编码字符集和字符的编码方案结合起来称为字符集。 
Code:
```
String test = "Test!";
byte[] strBytes = test.getBytes();  // Default: UTF-8        
byte[] strBytes2 = test.getBytes("UTF-16BE");    
System.out.println("test utf-8:" + new String(strBytes) + 
                      ",test utf-16be:" + new String(strBytes2));
``` 
#####位操作，布尔值编码 
#####组合输入、输出流 

#####成帧和解析，数据的接收 
接收者如何能够准确的找到消息的结束位置？ 
1. 基于定界符（Delimiter-based）:消息的结束由一个唯一的标记（unique marker）指出，即发送者在传输完数据后显式添加一个特殊字节序列。这个特殊标记不能再传输的数据中出现。  
   基于定界符的方法通常用在以文本方式编码的消息中：定义一个特殊的字符或字符串来标识消息的结束。
   接收者只需要简单地扫描信息（以字节的方式）来查找定界符序列，并将定界符前面的字符串返回。  
   缺点：发送信息中不能含有定界符，否则接收者将提前认为消息已结束。
   解决办法：填充技术（stuffing）：对消息中出现的定界符进行修改，从而使接收者不将其识别为定界符。但是该办法需要发送者和接收者都扫描发送消息，以便对消息中的定界符进行修改和恢复。 
2. 显式长度（Explicit length）:在变长字段或消息前附加一个固定大小的字段，用来指示该字段或消息中包含了多少字节。 

TODO：
<strong>Code：</strong> 实现基于定界符的成帧方法 
Framer.java
```
public interface Framer {
    
    /*
     * Add frame message,Output info to specified OutputStream
     * 
     * */
    void frameMsg(byte[] message, OutputStream out ) throws IOException; 
    
    /*
     * Return current msg before   delimiter  and get next frame message
     * 
     * */
    byte[] nextMsg() throws IOException;
}
```
DelimFramer.java
```
public class DelimFramer implements Framer{
    private InputStream in; // data source
    
    private static final byte DELIMTER = '\n';  //  message delimter
    
    public DelimFramer(InputStream in) {
        this.in = in;
    }
    
    @Override
    public void frameMsg(byte[] message, OutputStream out) throws IOException {
        // Ensure that message doesn't contain DELIMTER
        for (byte b : message ){
            if (b == DELIMTER){
                throw new IOException("Message contains delimiter.");
            }
        }
        out.write(message);
        out.write(DELIMTER);
        out.flush();
    }
    @Override
    public byte[] nextMsg() throws IOException {
        ByteArrayOutputStream messageBuffer = new ByteArrayOutputStream();
        int nextByte ; 
        // fetch bytes until find DELIMTER
        while ((nextByte = in.read()) != DELIMTER ){
            if (nextByte == -1){
                System.out.println("size:" + messageBuffer.size());
                if(messageBuffer.size() == 0 ){
                    return null;
                }
                else{
                    throw new EOFException("Non-empty message without delimter.");
                }
            }
            messageBuffer.write(nextByte);
        }
        return messageBuffer.toByteArray();
    }
    
    public static void main(String[] args) {
        String s = "aaa";
        byte[] b = s.getBytes();
        
        ByteArrayInputStream in = new ByteArrayInputStream(b);
        DelimFramer dFramer = new DelimFramer(in);
        
        ByteArrayOutputStream out = new ByteArrayOutputStream();
        try {
            dFramer.frameMsg(b, out);
            dFramer.nextMsg();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
基于长度成帧
```
public class LengthFramer implements Framer {
    public static final int MAX_MESSAGE_LENGTH = 65535;
    public static final int BYTE_MASK = 0xff;
    public static final int SHORT_MASK = 0xffff;
    public static final int BYTE_SHIFT = 8;
    DataInputStream in;
    public LengthFramer(InputStream in) {
        this.in = new DataInputStream(in);
    }
    @Override
    public void frameMsg(byte[] message, OutputStream out) throws IOException {
        if (message.length > MAX_MESSAGE_LENGTH) {
            throw new IOException("message is too long.");
        }
        out.write((message.length >> BYTE_SHIFT) & BYTE_MASK);
        out.write(message.length & BYTE_MASK);
        out.write(message);
        out.flush();
    }
    @Override
    public byte[] nextMsg() throws IOException {
        int length;
        //
        length = in.readUnsignedShort();
        byte[] msg = new byte[length];
        in.readFully(msg);
        return msg;
    }
    public static void main(String[] args) {
        String str = "abc";
        byte[] buf = str.getBytes();
        ByteArrayInputStream in = new ByteArrayInputStream(str.getBytes());
        DataInputStream input = new DataInputStream(in);
        LengthFramer framer = new LengthFramer(input);
        ByteArrayOutputStream out = new ByteArrayOutputStream();
        try {
            // framer msg.
            framer.frameMsg(buf, out);
            framer.nextMsg();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```


#####构建和解析协议 
这部分内容主要是介绍了对在实现别人定义的协议时可能用到的技术。

#####Reference
1.Java TCP/IP Socket编程
