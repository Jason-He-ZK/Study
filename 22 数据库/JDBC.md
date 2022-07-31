## 是什么

Java DataBase Connectivity（Java 语言连接数据库）

## JDBC 的本质

JDBC 是 SUN 公司制定的一套接口（interface）

- java.sql.*; (这个软件包下有很多接口。)

面向接口调用、面向接口写实现类，这都属于面向接口编程。

为什么要面向接口编程

- 解耦合：降低程序的耦合度，提高程序的扩展力。
- 多态机制就是非常典型的：面向抽象编程。（不要面向具体编程）

为什么 SUN 制定一套 JDBC 接口呢？

- 因为每一个数据库的底层实现原理都不一样，每一个数据库产品都有自己独特的实现原理。

## JDBC 开发前的准备工作

先从官网下载对应的驱动 jar 包，然后将其配置到环境变量 classpath 当中。

classpath=.;D:\course\06-JDBC\resources\MySql Connector Java 5.1.23\mysql-connector-java-5.1.23-bin.jar

IDEA 有自己的配置方式

## JDBC 编程六步（需要背会）

1. 注册驱动

作用：告诉 Java 程序，即将要连接的是哪个品牌的数据库
2. 获取连接

表示 JVM 的进程和数据库进程之间的通道打开了，这属于进程之间的通信，重量级的，使用完之后一定要关闭通道。
3. 获取数据库操作对象

专门执行 sql 语句的对象
4. 执行 SQL 语句

DQL DML....
5. 处理查询结果集

只有当第四步执行的是 select 语句的时候，才有这第五步处理查询结果集。
6. 释放资源

使用完资源之后一定要关闭资源。Java 和数据库属于进程间的通信，开启之后一定要关闭。

```java
public static void main(String[] args) {
    // 可以从配置文件中获取信息
    ResourceBundle resourceBundle = ResourceBundle.getBundle("peizhiwenjian");
    String driver = resourceBundle.getString("driver");
    String url = resourceBundle.getString("url");
    String user = resourceBundle.getString("user");
    String password = resourceBundle.getString("password");
    Connection conn = null;
    PreparedStatement ps = null;
    ResultSet rs = null;
    try {
        // 1 注册驱动
        Class.forName(driver);
        // 2 获取连接
        conn = DriverManager.getConnection(url,user,password);
        // 3 获取预编译的操作对象
        String sql = "select * from t_user where asdf = ? and fdsa = ?"; // ?是占位符
        ps = conn.prepareStatement(sql);
        /*
        prepareStatement优点
            1 避免了SQL注入
            2 执行效率更高，相同语句编译一次，可能运行多次
            3 编译阶段有安全检查
        极少数情况才会用Statement来支持SQL注入（拼接）
         */
        // 4 执行SQL语句
        ps.setString(1,"aaaa"); // 给占位符传值,标号从1开始
        ps.setInt(2,123);
        // int count = ps.executeUpdate(); // DML 增删改
        rs = ps.executeQuery(); // DQL 查
        // 5 处理查询结果集
        while (rs.next()) {
            // String name = rs.getString(1); // 标号从1开始
            String name = rs.getString("name"); // 查询后如有重命名列，用新的列名
            int sex = rs.getInt("sex");
        }
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException throwables) {
        throwables.printStackTrace();
    } finally {
        // 6 释放资源
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if (ps != null) {
            try {
                ps.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
}
```

## JDBC 的事务

```java
// 2 获取连接
conn = DriverManager.getConnection(url,user,password);
conn.setAutoCommit(false); // 关闭自动提交
// 3 获取预编译的操作对象
// 4 执行SQL语句
// 5 处理查询结果集
conn.commit(); // 事务完成后手动提交


// 在SQLException异常处理中设置回滚 
if (conn != null) {
    try {
        conn.rollback();
    } catch (SQLException e) {
        e.printStackTrace();
    }
}
```

## JDBC 工具类

```java
import java.sql.*;

public class DBUril {
    // 类加载，只执行一次就行
    static {
        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
    // 工具类的构造方法一般是私有的
    private DBUril() {
    }

    /**
     * 获取数据库连接对象
     * @return 连接对象
     * @throws SQLException
     */
    public static Connection getConnection() throws SQLException {
        String url = "jdbc:mysql://localhost:3306/mytest？serverTimezone=UTC";
        String user = "root";
        String password = "333";
        return DriverManager.getConnection(url, user, password);
    }

    /**
     * 释放资源
     * @param conn 数据库连接对象
     * @param ps 数据库操作对象
     * @param rs 查询结果集
     */
    public static void close(Connection conn, Statement ps, ResultSet rs)  {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if (ps != null) {
            try {
                ps.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
}
```
