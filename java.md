##一、实现两个页面的参数传递：
1. application.setAttribute()方法：
    假设页面一是jsp，页面二是servlet，从jsp传参给servlet：
    页面一加入参数：假设加入一个用户名，值为 郭珂鹏
        
        String value = "郭珂鹏";
        application.setAttribute("username", value);//value的类型是Object，可以替换为任意类型

    页面二用name变量接收传入的参数：
    ServletContext application = getServletContext();//如果是servlet则需加上这行
    String name = (String)application.getAttribute("username");//页面一的参数是什么类型，括号里就写什么类型

2. 用jsp:forward方法：
    同样假设页面一是jsp，页面二是servlet，从jsp传参给servlet：
    页面一的代码：

        <html>
            <head>
            <title>test</title>
        </head>
        <body>
            <jsp:forward page="servlet的路径">
                <jsp:param name="username" value="郭珂鹏"/>
            </jsp:forward>
        </body>
    </html>

    页面二的doGet方法中：
    String name = request.getParameter("username");

##二、JDBC访问数据库：
    
    import java.sql.*;

    public class JDBCDemo {
    static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";  
    static final String DB_URL = "jdbc:mysql://localhost/test";//这里填数据库地址
    static final String USER = "root";//数据库用户名
    static final String PASS = "";//数据库密码
    static String sql = "SELECT * FROM user";//执行查询的语句

    public static void main(String[] args){
	   try{
		  Class.forName("com.mysql.jdbc.Driver");//注册驱动
		  Connection conn = DriverManager.getConnection(DB_URL,USER,PASS);//建立连接
		  Statement stmt = conn.createStatement();
		  ResultSet rs = stmt.executeQuery(sql);//执行语句
		  while(rs.next()){//这里写什么参考数据库的内容，也就是数据库存的类型和内容往里套，连获得结果带打印
			 String host  = rs.getString("id");
			 String user = rs.getString("name");
			 String pwd = rs.getString("pwd");
			 String email = rs.getString("email");
			 String grade = rs.getString("grade");
			 System.out.print("host: " + host);
			 System.out.print(" user: " + user);
			 System.out.print(" pwd: " + pwd);
			 System.out.print(" email: " + email);
			 System.out.println(" grade: " + grade);
		  } 
	   }catch(Exception e){
		  System.out.println("发生错误");
       }finally{
        //释放资源
        rs.close();
        stmt.close();
        conn.close();
        }
    }

##三、重定向与请求转发：
两个都是切换页面的方法，假设从begin.jsp切换到end.jsp，方法如下：

begin.jsp中用重定向：<br>
response.sendRedirect("end.jsp");<br>
begin.jsp中用请求转发：<br>
request.getRequestDispatcher("end.jsp");<br>

区别：这两个都能切换页面，但原理完全不同，重定向是指服务器将这个end.jsp的
位置通过response传回给浏览器，浏览器再建立新的请求访问这个页面。<br>
而请求转发是服务器自身行为，服务器直接将页面切换到end.jsp并将内容直接发回
给浏览器，在浏览器看来它还在访问begin.jsp，但页面已经是end.jsp的内容了。<br>
可以使用：

    request.getRequestDispatcher("end.jsp").forward(request, response);
将页面附带的参数通过请求转发传给下一个页面。

##四、重载与覆盖的区别：
重载是指类中方法同名不同参数的两个方法，<br>例如：<br>
    
    public int sum(int a, int b)
和

    public int sum(int a, int b, int c)
它们同名不同参，使用时调用参数是什么样子的就调用什么样子参数的方法。

注意：不同返回值不能构成重载，<br>例如<br>

    public void fuck()
和
    
    public double fuck();
就不构成重载。
而覆盖是指子类重写父类的同名同参方法，<br>例如：<br>

    public class Father{
        public void say(){
            System.out.println("我是父类");
        }
    }

    public class Son extends Father{
        public void say(){
            System.out.println("我是子类");
        }
    }

    public class Main{
        public static void main(String[] args){
            Son son = new Son();
            son.say();//结果是 我是子类 因为子类覆盖了父类的方法
            //如果子类中没有写say，那么上面的代码结果就是 我是父类
        }
    }

##五、JDBC的逻辑顺序：

__参考上面JDBC代码。__

##六、throws throw try catch finally后通常紧跟什么样的代码逻辑：

__将错误传出/释放资源__

##七、BufferedReader FileReader InputStreamReader的特点：（Reader、Writer都只能操作文本）
BufferedReader可以从字符流中读取文本，并缓冲各个字符，效率高。<br>
FileReader可以从文件中读取文本文件的内容。<br>
InputStreamReader可以转换字节流和文本，可以读取字节并解码成字符例如：<br>

    BufferedReader input = new BufferedReader(new InputStreamReader(System.in));
这样就可以读取标准输入流输入的文本信息。（功能类似Scanner）

#八、JDBC访问数据库获取书籍：
__参考上面JDBC代码。__

##九、学生类的继承：
    public abstract class Person{
        public String name;
        abstract void info();
    }

    public class Student extends Person{
        public double score;
        public void info(){
            System.out.println("我是学生，我叫"+ name +"我的成绩是" + score);
        }
    }

##十、如果已经存在就删除文件（提示单词File类，调用实例方法delete），若果没有存在就首先创建该文件（提示单词createNewFile方法），接下来使用FileOutStream类创建对象，将“我有一只小毛驴，从来也不骑。”这句话写入到文件中：
    import java.io.*;

    public class Demo {
        public static void main(String[] args) {
            File file = new File("test.txt");
            if (file.exists()) {
                file.delete();
            }
            FileOutputStream fileOut = null;
            try {
                file.createNewFile();
                fileOut = new FileOutputStream(file);
                fileOut.write("我有一只小毛驴，我从来也不骑".getBytes());
            } catch (IOException e) {
                System.out.println("发生错误");
            } finally {
                try {
                    fileOut.close();
                } catch (IOException e) {
                    System.out.println("发生错误");
                }
            }
        }
    }