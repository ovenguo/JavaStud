##һ��ʵ������ҳ��Ĳ������ݣ�
1. application.setAttribute()������
    ����ҳ��һ��jsp��ҳ�����servlet����jsp���θ�servlet��
    ҳ��һ����������������һ���û�����ֵΪ ������
        
        String value = "������";
        application.setAttribute("username", value);//value��������Object�������滻Ϊ��������

    ҳ�����name�������մ���Ĳ�����
    ServletContext application = getServletContext();//�����servlet�����������
    String name = (String)application.getAttribute("username");//ҳ��һ�Ĳ�����ʲô���ͣ��������дʲô����

2. ��jsp:forward������
    ͬ������ҳ��һ��jsp��ҳ�����servlet����jsp���θ�servlet��
    ҳ��һ�Ĵ��룺

        <html>
            <head>
            <title>test</title>
        </head>
        <body>
            <jsp:forward page="servlet��·��">
                <jsp:param name="username" value="������"/>
            </jsp:forward>
        </body>
    </html>

    ҳ�����doGet�����У�
    String name = request.getParameter("username");

##����JDBC�������ݿ⣺
    
    import java.sql.*;

    public class JDBCDemo {
    static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";  
    static final String DB_URL = "jdbc:mysql://localhost/test";//���������ݿ��ַ
    static final String USER = "root";//���ݿ��û���
    static final String PASS = "";//���ݿ�����
    static String sql = "SELECT * FROM user";//ִ�в�ѯ�����

    public static void main(String[] args){
	   try{
		  Class.forName("com.mysql.jdbc.Driver");//ע������
		  Connection conn = DriverManager.getConnection(DB_URL,USER,PASS);//��������
		  Statement stmt = conn.createStatement();
		  ResultSet rs = stmt.executeQuery(sql);//ִ�����
		  while(rs.next()){//����дʲô�ο����ݿ�����ݣ�Ҳ�������ݿ������ͺ����������ף�����ý������ӡ
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
		  System.out.println("��������");
       }finally{
        //�ͷ���Դ
        rs.close();
        stmt.close();
        conn.close();
        }
    }

##�����ض���������ת����
���������л�ҳ��ķ����������begin.jsp�л���end.jsp���������£�

begin.jsp�����ض���<br>
response.sendRedirect("end.jsp");<br>
begin.jsp��������ת����<br>
request.getRequestDispatcher("end.jsp");<br>

���������������л�ҳ�棬��ԭ����ȫ��ͬ���ض�����ָ�����������end.jsp��
λ��ͨ��response���ظ��������������ٽ����µ�����������ҳ�档<br>
������ת���Ƿ�����������Ϊ��������ֱ�ӽ�ҳ���л���end.jsp��������ֱ�ӷ���
�������������������������ڷ���begin.jsp����ҳ���Ѿ���end.jsp�������ˡ�<br>
����ʹ�ã�

    request.getRequestDispatcher("end.jsp").forward(request, response);
��ҳ�渽���Ĳ���ͨ������ת��������һ��ҳ�档

##�ġ������븲�ǵ�����
������ָ���з���ͬ����ͬ����������������<br>���磺<br>
    
    public int sum(int a, int b)
��

    public int sum(int a, int b, int c)
����ͬ����ͬ�Σ�ʹ��ʱ���ò�����ʲô���ӵľ͵���ʲô���Ӳ����ķ�����

ע�⣺��ͬ����ֵ���ܹ������أ�<br>����<br>

    public void fuck()
��
    
    public double fuck();
�Ͳ��������ء�
��������ָ������д�����ͬ��ͬ�η�����<br>���磺<br>

    public class Father{
        public void say(){
            System.out.println("���Ǹ���");
        }
    }

    public class Son extends Father{
        public void say(){
            System.out.println("��������");
        }
    }

    public class Main{
        public static void main(String[] args){
            Son son = new Son();
            son.say();//����� �������� ��Ϊ���า���˸���ķ���
            //���������û��дsay����ô����Ĵ��������� ���Ǹ���
        }
    }

##�塢JDBC���߼�˳��

__�ο�����JDBC���롣__

##����throws throw try catch finally��ͨ������ʲô���Ĵ����߼���

__�����󴫳�/�ͷ���Դ__

##�ߡ�BufferedReader FileReader InputStreamReader���ص㣺��Reader��Writer��ֻ�ܲ����ı���
BufferedReader���Դ��ַ����ж�ȡ�ı�������������ַ���Ч�ʸߡ�<br>
FileReader���Դ��ļ��ж�ȡ�ı��ļ������ݡ�<br>
InputStreamReader����ת���ֽ������ı������Զ�ȡ�ֽڲ�������ַ����磺<br>

    BufferedReader input = new BufferedReader(new InputStreamReader(System.in));
�����Ϳ��Զ�ȡ��׼������������ı���Ϣ������������Scanner��

#�ˡ�JDBC�������ݿ��ȡ�鼮��
__�ο�����JDBC���롣__

##�š�ѧ����ļ̳У�
    public abstract class Person{
        public String name;
        abstract void info();
    }

    public class Student extends Person{
        public double score;
        public void info(){
            System.out.println("����ѧ�����ҽ�"+ name +"�ҵĳɼ���" + score);
        }
    }

##ʮ������Ѿ����ھ�ɾ���ļ�����ʾ����File�࣬����ʵ������delete��������û�д��ھ����ȴ������ļ�����ʾ����createNewFile��������������ʹ��FileOutStream�ഴ�����󣬽�������һֻСë¿������Ҳ�������仰д�뵽�ļ��У�
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
                fileOut.write("����һֻСë¿���Ҵ���Ҳ����".getBytes());
            } catch (IOException e) {
                System.out.println("��������");
            } finally {
                try {
                    fileOut.close();
                } catch (IOException e) {
                    System.out.println("��������");
                }
            }
        }
    }