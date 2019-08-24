### IO流对象
数据基本都是内存数据，程序结束了就断了
IO技术，数据持久保存，内存属于临时保存。
把内存中的数据持久化设备上，这个动作称为输出，Output操作
把设备上的数据读取到内存中，这个动作成为输入，Input操作
Input --> Output --> IO

### File类
主要表现: 文件夹，文件，路径
java.io包中
File继承自object：文件和目录路径名的抽象表示形式。
操作文件和文件夹路径
File具有平台无惯性，不挑系统
java.io.file: 将操作系统中的文件，目录，路径封装成File对象，提供方法，操作系统中的内容
文件: file, 目录: directory, 路径: path
静态成员变量: pathSeparator   File.pathSeparator;
pathSeparator: 与系统有关的路径，分隔符，为了方便，被标识为字符串
分隔符: windows: ";"   linux: ":"
separator: 与系统有关的默认名称分隔符，字符串
windows: "\"   linux: "/"

### 构造方法
File(String pathname); 通过将给定路径名字转换为抽象路径，来创建一个新的File实例。
路径名： 可以到文件夹，也可以到文件:   c:\\abc
```
File file = new File("d:\\aaa.txt");
```
windows下的文件夹和文件名，后缀名不区分大小写，Linux: 区分

路径: 
绝对路径： 在系统中具有唯一性
相对路径:  表示的是路径之间的相对关系

```
// 根据parent路径名称和child路径名称创建实例
// File file = new File(String parent, String child);
File file = new File("c:", "windows"); // 灵活性更强，可以单独操作控制父或子路径
```
根据parent抽象路径名和child路径名创建一个实例
```
File file = new File(File parent, String child); // 父路径是File类型，使用时可以调用方法
```
常用File file = new File(String pathname);

### File类的创建和删除
文件目录的创建和删除
File创建文件方法 createNewFile(); 返回布尔值
文件名和路径是之前的构造方法。如果存在返回false，如不存在，返回true
这个方法只能创建文件，不能生成文件夹
File 创建文件夹功能
mkdir() 返回布尔值，创建路径在File的构造方法中
mkdirs() 创建多层目录，推荐使用
```
File file new File("c:\\abc\a\b.txt");
file.mkdirs();
```
File类的删除功能
delete(); 返回布尔值，删除的路径在file构造方法中。
如文件不存在或打开状态，则删除失败，删除方法直接从硬盘抹掉

### 获取
getName() 返回此抽象路径名称标识的文件或目录的名称，返回末尾的文件名或文件夹名，只负责获取字符串功能
getPath(); 返回路径的字符串
length(): 返回路径中标识的文件的字节数，Long型
getAbsolutePath() 获取绝对路径，String类型
getAbsoluteFile() 获取绝对路径，File对象类型
getParent() 获取父路径 String
getParentFile() 获取父路径对象 File

### File 类的判断功能
exists(): 返回布尔值，判断此抽象路径是否存在
isDirectory(): 返回布尔值，判断构造方法中的路径是不是文件夹，先验证是否存在，再判断是不是文件夹或判断是不是文件
isFile(): 判断是不是文件

### File 获取功能
list：返回数组[]，获取路径中的文件夹名和文件名 String
listFile：返回File类型的数组
File.listRoots(): 静态方法，返回File数组，系统根目录

### 文件过滤
listFiles(FileFilter filter) 接口类型，传入接口实现类
FileFilter没有实现类，需要自己定义，然后重写抽象方法 accept(); 返回布尔值
```
public boolean accept(File pathname) {
    return true;
}
```
listFiles() 遍历目录的同时，获取到的是全路径，如果accept返回true，则加入listFiles，如果返回false，则舍弃

### 字节流
OutputStream：字节输出流，操控字节，是一个抽象类，作用，从java程序写出文件，不能写文件夹，方法都为写文件的方法
将输入写入文件，一个字节占8个2进制位，可以写任意文件，
write(int b): 写入1个字节
write(byte[] b) 写入字节数组
write(byte[] b, int, int) 写入字节数组，int开始写入索引，写几个
close() 关闭流对象，释放与流相关的资源，依赖系统使用之后要释放资源，否则被占用

已知子类
ByteArrayOutputStream
FileOutputStream
FilterOutputStream
ObjectOutputStream
OutputStream
PipedOutputStream

### FileOutputStream类
写入数据文件，学习父类方法，使用子类对象
子类中的构造方法，作用：绑定数据输出目的
参数：File，String
流对象使用步骤
1、创建流子类的对象，绑定数据目的， new FileOutputStream("c:\")
2、调用流对象的方法write
3、关闭释放资源 close
流对象的构造方法，可以根据构造方法创建文件，如文件已经存在，会覆盖原有文件
FileOutputStream 是字节流，write(100) 写入一个字节，会转为2进制，文本打开的时候会查表 100 --> d   97 --> a
文本中汉字占2个字节，阿拉伯数字占1个字节

字节数组:
byte[] bytes = { 65, 66, 67, 68} 负数字节代表汉字
getBytes(); 将字符串转为byte数组  "HAHA".getBytes();
```
File file = new File("c:\");
FileOutputStream  fos = new FileOutputStream(file, boolean); // 参数可以是字符串也可以是对象, 第二个参数代表写在开始处还是结尾处
// 在文件中写入换行： \r\n  \r: 回车   \n: 换行
```

### IO流的异常
1、保证流对象变量，作用域足够
2、catch里面没怎么处理异常
输出异常信息，目的看到哪里出现了问题
停下程序重新尝试
```
public class FileOutputStream {
    public static void main(Strng[] args) {
        FileOutputStream fos = null;
        try {
            fos = new FileOutputStream("c:\\a.txt");
            fos.write(100);
        } catch(IOException ex) {
            System.out.println(ex);
            throw new RuntimeException("文件写入失败")
        } finally {
            try {
                // 如果流对象创建失败了，不需要再关闭资源
                if (fos != null) {
                    fos.close();
                }
            } catch(IOException ex) {
                throw new RuntimeException("关闭资源失败")
            }
        }
    }
}
```

### 字节输入流
java.io.InputStream; 所有字节输入流的超类
作用：读取任意文件，每次读取1个字节
read() 从输入流中读取数据的下一个字节，返回int类型
read(byte[] b) 从输入流中读取一定数量的字节，并将其存储在缓冲区数组b中
子类： FileInputStream 读取文件
构造方法: 为这个流对象绑定数据源。
参数: File类对象，String对象
不要忘记关闭资源。
将整数强转成字符，(char)97 --> a
read 方法特点：
read() 执行一次，就会自动读取下一个字节，返回值返回的是读取到的字节，读取不到的时候返回 -1；

### FileInputStream读取(参数字节数组)
byte[] b = new byte[2];
一般byte写1024或1024的整数倍
fis.read(b); 传入一个字节数组，返回值是数组长度
String([]) 传入一个字节数组，会变成一个字符串
String str = new String(b);
如读取不到数组中会保留原有的值。
String(byte[], index, sum);
System.out.print(new String(b, 0, len));
while ((len = fis.read(bytes)) != -1);

### 字节流复制文件

采用素组缓冲提高效率
使用字节数组
FileInputStream 读取字节数组
FileOutputStream 写入字节数组

```
public class Copy {
    public static void main(String[] args) {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream("c:\\t.zip");
            fos = new FileOutputStream("d:\\t.zip");
            byte[] bytes = new byte[1024];
            int len = 0;
            while ((len = fis.read(bytes)) != -1) {
                fos.write(bytes, 0, len);
            }
        } catch (IOException ex) {
            System.out.println(ex);
            throw new RuntimeException("文件复制失败")
        } finally {
            try {
                if (fos != null ) {
                    fos.close();
                }
            } catch (IOException ex) {
                throw new RuntimeException("文件复制失败")Î
            }.finally {
                try {
                    if (fis != null ) {
                        fis.close();
                    }
                } catch (IOException ex) {
                    throw new RuntimeException("文件复制失败")Î
                }
            }
        }
    }
}
```

IO转换流