---
title: javaIO
date: 2023-03-22 22:31:31
tags: java
categories: java
---

# javaI

参考了网络上的一些示例和java文档中的说明，归纳了比较常见的一些用法

## 简单概念

- 字节流
- 字符流

- 输入流
- 输出流

参考网址:[主要看这个网址的第一张图](https://blog.csdn.net/qq_52519008/article/details/127135476)

## FileInputStream

### 构造方法

FileInputStream(File file) 
          通过打开一个到实际文件的连接来创建一个 FileInputStream，该文件通过文件系统中的 File 对象 file 指定。 
FileInputStream(FileDescriptor fdObj) 
          通过使用文件描述符 fdObj 创建一个 FileInputStream，该文件描述符表示到文件系统中某个实际文件的现有连接。 
FileInputStream(String name) 
          通过打开一个到实际文件的连接来创建一个 FileInputStream，该文件通过文件系统中的路径名 name 指定。 

### 读取方法

 int read() 
          从此输入流中读取一个数据字节。 
 int read(byte[] b) 
          从此输入流中将最多 b.length 个字节的数据读入一个 byte 数组中。 
 int read(byte[] b, int off, int len) 
          从此输入流中将最多 len 个字节的数据读入一个 byte 数组中。 

### 说明

java.io.InputStream:字节输入流,此抽象类是表示字节输入流的所有类的超类。
定义了所有子类共性的方法:

- public abstract int read()从输入流中读取数据的下一个字节。

- public int read(byte[] b) 从输入流中读取一定数量的字节，并将其存储在缓冲区数组 b 中。

- public void close() 关闭此输入流并释放与该流关联的所有系统资源。

  

InputStream的子类之一FileInputStream（文件字节输入流）,作用是把硬盘文件中的数据,读取到内存中使用。
   定义了构造方法:

- public FileInputStream(String name)通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名 name命名。

- public FileInputStream(File file) 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的 File对象 file命名。
  使用数组读取，每次读取多个字节，减少了系统间的IO操作次数，从而提高了读写的效率，建议开发中使用

下面代码中的show1和show2展示了两种不同的初始化和文件读取方法

```java
import java.io.FileInputStream;
import java.io.IOException;
    
public class InputStreamTest {
    public static void main(String[] args) throws IOException {
        show1();
   }
  /**show1():从文件中读取单个字节
   * 1.创建FileInputStream对象,构造方法中绑定要读取的数据源
   * 2.使用FileInputStream对象中的方法read,读取文件
   * 3.释放资源
   */
   public void show1() throws IOException {  
        //1.
        /* 构造方法的作用:
            1.会创建一个FileInputStream对象
            2.会把FileInputStream对象指向构造方法中要读取的文件的第一个字节
         */
        FileInputStream fis = new FileInputStream("09_IOAndProperties\\c.txt"); //文件内容为abc
        //FileInputStream fis = new FileInputStream(new File("09_IOAndProperties\\c.txt"));
        
        //2.int read()通过JVM，再通过OS，读取文件中的指针指向的字节并提升到int返回,连续读取时指针依次向后移，读取到文件的末尾返回-1
        len = fis.read();
        System.out.println(len);//97 a
        len = fis.read();
        System.out.println(len);// 98 b
        len = fis.read();
        System.out.println(len);//99 c
        len = fis.read();
        System.out.println(len);//-1
       /*
        int len = 0; //记录读取到的字节
        while((len = fis.read())!=-1){
            System.out.print((char)len);//abc
        }
        */
 
        //3.
        fis.close();
    }
 
   /**show2():从文件中读取字节数组
    *1.创建FileInputStream对象,构造方法中绑定要读取的数据源
    *2.使用FileInputStream对象中的方法read读取文件
    *3.关闭资源
    */
    public void show2() throws IOException {
        //1.
        FileInputStream fis = new FileInputStream("09_IOAndProperties\\b.txt");//文件内容为ABCDE
        
        //2.
        //int read(byte[] b) 从输入流中读取一定数量的字节，并将其存储在缓冲区数组 b 中。
        byte[] bytes = new byte[2]; // byte[]起到缓冲作用,存储每次读取到的多个字节,数组的长度一把定义为1024(1kb)或者1024的整数倍
        //
        //不同于读取单个字节int len记录每次读取的有效字节个数
        int len = fis.read(bytes); 
        System.out.println(len);//2
        //System.out.println(Arrays.toString(bytes));//[65, 66]
        //
        //String(byte[] bytes) :把字节数组转换为字符串
        System.out.println(new String(bytes));//AB
        len = fis.read(bytes);
        System.out.println(len);//2
        System.out.println(new String(bytes));//CD
        len = fis.read(bytes);
        System.out.println(len);//1
        //
        //String(byte[] bytes, int offset, int length) 把字节数组的一部分转换为字符串 offset:数组的开始索引 length:转换的字节个数
        System.out.println(new String(bytes,0,len));
        //System.out.println(new String(bytes)); ED 错于当数组容量大于剩余字节时，上次读取的数据没有被完全替换而重复读取，所以要通过 len ，获取有效的字节.
        len = fis.read(bytes);
        System.out.println(len);//-1       
        /*
        byte[] bytes = new byte[1024];
        int len = 0;
        while((len = fis.read(bytes))!=-1){
            //
            //当数组容量大于读取内容时，直接转换成字符串会生成大量空格，所以只转换有效字节
            System.out.println(new String(bytes,0,len));
        }
        */
 
        //3.
        fis.close();
    }
}
```

## BufferedInputStream（缓冲字节流）

### 构造方法

BufferedInputStream(InputStream in) 
          创建一个 BufferedInputStream 并保存其参数，即输入流 in，以便将来使用。

### 读取方法

 int read() 
          参见 InputStream 的 read 方法的常规协定。 
 int read(byte[] b, int off, int len) 
          从此字节输入流中给定偏移量处开始将各字节读取到指定的 byte 数组中。 

### 说明

缓冲字节流是为高效率而设计的，真正的读写操作还是靠`FileOutputStream`和`FileInputStream`，所以其构造方法入参是这两个类的对象也就不奇怪了。

```java
public class IOTest {

	public static void write(File file) throws IOException {
		// 缓冲字节流，提高了效率
		BufferedOutputStream bis = new BufferedOutputStream(new FileOutputStream(file, true));

		// 要写入的字符串
		String string = "松下问童子，言师采药去。只在此山中，云深不知处。";
		// 写入文件
		bis.write(string.getBytes());
		// 关闭流
		bis.close();
	}

	public static String read(File file) throws IOException {
		BufferedInputStream fis = new BufferedInputStream(new FileInputStream(file));

		// 一次性取多少个字节
		byte[] bytes = new byte[1024];
		// 用来接收读取的字节数组
		StringBuilder sb = new StringBuilder();
		// 读取到的字节数组长度，为-1时表示没有数据
		int length = 0;
		// 循环取数据
		while ((length = fis.read(bytes)) != -1) {
			// 将读取的内容转换成字符串
			sb.append(new String(bytes, 0, length));
		}
		// 关闭流
		fis.close();

		return sb.toString();
	}
}

```

## InputStreamReader

InputStreamReader 是字节流通向字符流的桥梁

### 构造方法

InputStreamReader(InputStream in) 
          创建一个使用默认字符集的 InputStreamReader。 
InputStreamReader(InputStream in, Charset cs) 
          创建使用给定字符集的 InputStreamReader。 
InputStreamReader(InputStream in, CharsetDecoder dec) 
          创建使用给定字符集解码器的 InputStreamReader。 
InputStreamReader(InputStream in, String charsetName) 
          创建使用指定字符集的 InputStreamReader。 

### 读取方法

int read() 
          读取单个字符。 
 int read(char[] cbuf, int offset, int length) 
          将字符读入数组中的某一部分。 

### 说明

**字符流适用于文本文件的读写**，`OutputStreamWriter`类其实也是借助`FileOutputStream`类实现的，故其构造方法是`FileOutputStream`的对象

```java
public class IOTest {
	
	public static void write(File file) throws IOException {
		// OutputStreamWriter可以显示指定字符集，否则使用默认字符集
		OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(file, true), "UTF-8");

		// 要写入的字符串
		String string = "松下问童子，言师采药去。只在此山中，云深不知处。";
		osw.write(string);
		osw.close();
	}

	public static String read(File file) throws IOException {
		InputStreamReader isr = new InputStreamReader(new FileInputStream(file), "UTF-8");
		// 字符数组：一次读取多少个字符
		char[] chars = new char[1024];
		// 每次读取的字符数组先append到StringBuilder中
		StringBuilder sb = new StringBuilder();
		// 读取到的字符数组长度，为-1时表示没有数据
		int length;
		// 循环取数据
		while ((length = isr.read(chars)) != -1) {
			// 将读取的内容转换成字符串
			sb.append(chars, 0, length);
		}
		// 关闭流
		isr.close();

		return sb.toString()
	}
}

```

## FileReader

Java提供了`FileWriter`和`FileReader`简化字符流的读写，`new FileReader(file)`等同于`new InputStreamReader(new FileInputStream(file, true))`

### 构造方法

FileReader(File file) 
          在给定从中读取数据的 File 的情况下创建一个新 FileReader。 
FileReader(String fileName) 
          在给定从中读取数据的文件名的情况下创建一个新 FileReader。 

### 读取方法

和InputStreamReader的方法一样，都是从InputStreamReader那里继承的

### 说明

```java
public class IOTest {
	
	public static void write(File file) throws IOException {
		FileWriter fw = new FileWriter(file, true);

		// 要写入的字符串
		String string = "松下问童子，言师采药去。只在此山中，云深不知处。";
		fw.write(string);
		fw.close();
	}

	public static String read(File file) throws IOException {
		FileReader fr = new FileReader(file);
		// 一次性取多少个字节
		char[] chars = new char[1024];
		// 用来接收读取的字节数组
		StringBuilder sb = new StringBuilder();
		// 读取到的字节数组长度，为-1时表示没有数据
		int length;
		// 循环取数据
		while ((length = fr.read(chars)) != -1) {
			// 将读取的内容转换成字符串
			sb.append(chars, 0, length);
		}
		// 关闭流
		fr.close();

		return sb.toString();
	}
}

```

## BufferedReader**（字符缓冲流）**

### 构造方法

BufferedReader(Reader in) 
          创建一个使用默认大小输入缓冲区的缓冲字符输入流。 
BufferedReader(Reader in, int sz) 
          创建一个使用指定大小输入缓冲区的缓冲字符输入流。 

### 读取方法

 int read() 
          读取单个字符。 
 int read(char[] cbuf, int off, int len) 
          将字符读入数组的某一部分。 
 String readLine() 
          读取一个文本行。 

### 说明

整个文档里面只有它是支持readline的

```java
public class IOTest {
	
	public static void write(File file) throws IOException {
		// BufferedWriter fw = new BufferedWriter(new OutputStreamWriter(new
		// FileOutputStream(file, true), "UTF-8"));
		// FileWriter可以大幅度简化代码
		BufferedWriter bw = new BufferedWriter(new FileWriter(file, true));

		// 要写入的字符串
		String string = "松下问童子，言师采药去。只在此山中，云深不知处。";
		bw.write(string);
		bw.close();
	}

	public static String read(File file) throws IOException {
		BufferedReader br = new BufferedReader(new FileReader(file));
		// 用来接收读取的字节数组
		StringBuilder sb = new StringBuilder();

		// 按行读数据
		String line;
		// 循环取数据
		while ((line = br.readLine()) != null) {
			// 将读取的内容转换成字符串
			sb.append(line);
		}
		// 关闭流
		br.close();

		return sb.toString();
	}
}

```

## InputStreamReader类与FileReader类关系

1、FileReader类仅仅是InputStreamReader的简单衍生并未扩展任何功能

2、FileReader类读取数据实质是InputStreamReader类在读取，而InputStreamReader读取数据实际是StreamDecoder类读取

3、因此在使用字符输入流的时候实际是StreamDecoder类在发挥作用

```java
public class FileReader extends InputStreamReader
```

## System.in

static InputStream in 
          “标准”输入流。 

## Scanner

实现的接口： `Iterator<String> `

### 构造方法

Scanner(File source) 
          构造一个新的 Scanner，它生成的值是从指定文件扫描的。 
Scanner(InputStream source) 
          构造一个新的 Scanner，它生成的值是从指定的输入流扫描的。 
Scanner(Readable source) 
          构造一个新的 Scanner，它生成的值是从指定源扫描的。 
Scanner(String source) 
          构造一个新的 Scanner，它生成的值是从指定字符串扫描的。 

此外还有的构造方法在以上的基础上，可以允许程序员指定字符集

### 常用方法

boolean hasNextInt() 
          如果通过使用 nextInt() 方法，此扫描器输入信息中的下一个标记可以解释为默认基数中的一个 int 值，则返回 true。 
boolean hasNextLine() 
          如果在此扫描器的输入中存在另一行，则返回 true。 
int nextInt() 
          将输入信息的下一个标记扫描为一个 int。 
String nextLine() 
          此扫描器执行当前行，并返回跳过的输入信息。 

此外，还有nextByte nextFloat nextDouble hasNextFloat这种方法

### 说明

一个可以使用正则表达式来解析基本类型和字符串的简单文本扫描器。

以下代码使用户能够从 `System.in` 中读取一个数： 

```java
     Scanner sc = new Scanner(System.in);
     int i = sc.nextInt();
```

以下代码使 `long` 类型可以通过 `myNumbers` 文件中的项分配：

```java
Scanner sc = new Scanner(new File("myNumbers"));
      while (sc.hasNextLong()) {
          long aLong = sc.nextLong();
      }
```

可以看出一般next和hasNext是一起用的

它可以通过`File` `String` `inputStream`进行构造

# JavaO

## FileOutputStream

### 构造方法

FileOutputStream(File file) 
          创建一个向指定 File 对象表示的文件中写入数据的文件输出流。 
FileOutputStream(File file, boolean append) 
          创建一个向指定 File 对象表示的文件中写入数据的文件输出流。  
FileOutputStream(String name) 
          创建一个向具有指定名称的文件中写入数据的输出文件流。 
FileOutputStream(String name, boolean append) 
          创建一个向具有指定 name 的文件中写入数据的输出文件流。 

### 写入方法

void write(byte[] b) 
          将 b.length 个字节从指定 byte 数组写入此文件输出流中。 
 void write(byte[] b, int off, int len) 
          将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此文件输出流。 
 void write(int b) 
          将指定字节写入此文件输出流。 

## BufferedOutputStream

### 构造方法

BufferedOutputStream(OutputStream out) 
          创建一个新的缓冲输出流，以将数据写入指定的底层输出流。 
BufferedOutputStream(OutputStream out, int size) 
          创建一个新的缓冲输出流，以将具有指定缓冲区大小的数据写入指定的底层输出流。 

### 写入方法

void flush() 
          刷新此缓冲的输出流。 
void write(byte[] b, int off, int len) 
          将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此缓冲的输出流。 
void write(int b) 
          将指定的字节写入此缓冲的输出流。 

## OutputStreamWriter

OutputStreamWriter 是字符流通向字节流的桥梁

### 构造方法

OutputStreamWriter(OutputStream out) 
          创建使用默认字符编码的 OutputStreamWriter。 
OutputStreamWriter(OutputStream out, Charset cs) 
          创建使用给定字符集的 OutputStreamWriter。 
OutputStreamWriter(OutputStream out, CharsetEncoder enc) 
          创建使用给定字符集编码器的 OutputStreamWriter。 
OutputStreamWriter(OutputStream out, String charsetName) 
          创建使用指定字符集的 OutputStreamWriter。 

### 写入方法

 void flush() 
          刷新该流的缓冲。 
 void write(char[] cbuf, int off, int len) 
          写入字符数组的某一部分。 
 void write(int c) 
          写入单个字符。 
 void write(String str, int off, int len) 
          写入字符串的某一部分。 

## FileWriter

### 构造方法

FileWriter(File file) 
          根据给定的 File 对象构造一个 FileWriter 对象。 
FileWriter(File file, boolean append) 
          根据给定的 File 对象构造一个 FileWriter 对象。 
FileWriter(String fileName) 
          根据给定的文件名构造一个 FileWriter 对象。 
FileWriter(String fileName, boolean append) 
          根据给定的文件名以及指示是否附加写入数据的 boolean 值来构造 FileWriter 对象。 

### 写入方法

和OutputStreamWriter一样，继承于OutputStreamWriter

## BufferedWriter

### 构造方法

BufferedWriter(Writer out) 
          创建一个使用默认大小输出缓冲区的缓冲字符输出流。 
BufferedWriter(Writer out, int sz) 
          创建一个使用给定大小输出缓冲区的新缓冲字符输出流。 

### 写入方法

 void flush() 
          刷新该流的缓冲。 
 void newLine() 
          写入一个行分隔符。 
 void write(char[] cbuf, int off, int len) 
          写入字符数组的某一部分。 
 void write(int c) 
          写入单个字符。 
 void write(String s, int off, int len) 
          写入字符串的某一部分。 

## System.out

static PrintStream out 
          “标准”输出流。 
