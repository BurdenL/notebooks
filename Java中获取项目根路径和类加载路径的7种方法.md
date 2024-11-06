**引言**

  在**web**项目开发过程中，可能会经常遇到要获取项目根路径的情况,那接下来我就总结一下，java中获取项目根路径的7种方法,主要是通过thisClass和System，线程和request等方法。

   (1):this.getClass().getResource("/")；

   (2):file.getCanonicalPath()；

   (3):this.getClass().getClassLoader()；

   (4):System.getProperty("user.dir")；

   (5):System.getProperty("java.class.path")；

   (6):Thread.currentThread().getContentClassLoader()；

   (7):request.getSession().getServletContext()；

代码如下:

```java
import java.io.File;
import java.io.IOException;
import java.net.URL;
/**获取项目根路径的方法*/
public class MyUrlDemo {

     
     public static void main(String[] args) {
         MyUrlDemo muDemo = new MyUrlDemo();
         try {
             muDemo.showURL();
         } catch (IOException e) {
             // TODO Auto-generated catch block
             e.printStackTrace();
         }
     }

     public void showURL() throws IOException {

         // 第一种：获取类加载的根路径   D:\git\daotie\daotie\target\classes
         File f = new File(this.getClass().getResource("/").getPath());
         System.out.println("path1: "+f);

         // 获取当前类的所在工程路径; 如果不加“/”  获取当前类的加载目录  D:\git\daotie\daotie\target\classes\my
         File f2 = new File(this.getClass().getResource("").getPath());
         System.out.println("path1: "+f2);

         // 第二种：获取项目路径    D:\git\daotie\daotie
         File directory = new File("");// 参数为空
         String courseFile = directory.getCanonicalPath();
         System.out.println("path2: "+courseFile);

 
         // 第三种：  file:/D:/git/daotie/daotie/target/classes/
         URL xmlpath = this.getClass().getClassLoader().getResource("");
         System.out.println("path3: "+xmlpath);
 
         // 第四种： D:\git\daotie\daotie
         System.out.println("path4:" +System.getProperty("user.dir"));
         /*
          * 结果： C:\Documents and Settings\Administrator\workspace\projectName
          * 获取当前工程路径
          */

         // 第五种：  获取所有的类路径 包括jar包的路径
         System.out.println("path5: "+System.getProperty("java.class.path").split(";")[0]);         
　　　　　　// 第六种：  获取项目路径  D:/git/daotie/daotie.target/classes/
         System.out.println("path6: "+Thread.currentThread().getContentClassLoader().getResource("").getPath());         //第七种  表示到项目的根目录下, 要是想到目录下的子文件夹,修改"/"即可         String path7 = request.getSession().getServletContext().getRealPath("/"));         System.out.pringln("path7: "+path7);
} }
```

[![复制代码](https://assets.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 **结果:**

![img](https://img2018.cnblogs.com/blog/1508950/201905/1508950-20190524091331587-174258113.png)

 