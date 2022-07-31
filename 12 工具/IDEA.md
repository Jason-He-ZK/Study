安装与破解
设置字体与字符集
4.1、字体设置
file --> settings --> 输入font --> 设置字体样式以及字号大小。
创建Project，Module
创建package
怎么运行java程序
4.6、怎么运行：
代码上右键-->run 或者点击左侧的绿色箭头。 ctrl + shift + F10
快捷键
4.2、快速生成main方法
psvm，main
4.3、快速生成System.out.println()
sout
4.4、注意：IDEA是自动保存，不需要ctrl + s
4.5、删除一行
ctrl + y
4.7、左侧窗口中的列表怎么展开？怎么关闭？
左箭头关闭。 右箭头展开。 上下箭头移动。
4.8、idea中退出任何窗口，都可以使用esc键盘。（esc就是退出）
4.9、任何新增/新建/添加的快捷键是：
alt + insert
4.10、窗口变大，变小：
ctrl + shift + F12
4.11、切换java程序：从HelloWorld切换到User
alt + 左右箭头
4.12、切换窗口：
alt + 标号 alt + 1（打开，关闭） alt + 2
4.13、提示方法的参数：ctrl + p
4.14、注释：
单行注释：ctrl + / 多行注释：ctrl + shift + /
4.15、idea中怎么定位方法/属性/变量？
光标停到某个单词的下面，这个单词可能是：   方法名、变量名 停到单词下面之后，按ctrl键，出现下划线，点击跳转。
4.16、idea当中复制一行是ctrl + d
Ctrl+alt+o：清除无效的包
管理Tomcat
添加
file→settings→build→application servers→“+”→Tomcat
开关
run→edit configuration→“+”→Tomcat→local（remote远程，其他） 别忘了选择jdk debug模式可修改 发布网站   run—edit xxxx—deployment—“+”—artifact   英文别名，以“/”开头 修改后马上更新   两个“on”，选update classes and resource   debug启动 java: 程序包javax.servlet不存在   file—Project Structure—Libraries—“+”—Java，在弹出的窗口中选择tomcat所在的目录，在lib目录下找到servlet-api.jar这个jar包导入完成即可。
创建网站
new→module→java enterprise→web application4.0→起名（起名可以中文，后面起英文别名）
两个文件夹
src文件夹   动态资源文件 web文件夹   静态资源文件   WEB-INF文件夹     lib文件夹（自己建）（存放jar包）       jar包（project structure→modules→“+”）     web.xml（核心配置文件）
需要内容
数据库表 entity下创建实体类 util.JdbcUtil数据库工具类 WEB-INF下创建lib文件夹，存放mysql提供JDBC实现jar包   add as Library



### 快捷键

alt + F7  查看该方法在哪里被调用

### 插件

8 RestfulToolkit: 快速定位controller层接口、接口测试
10 Mybatis Log Plugin: 快速打印SQL语句
11 Free Mybatis Plugin: mybatis xml id与接口间跳转
4 Easy Code: 数据库表生成JavaBean
5 JRebel for IntelliJ: JavaWeb项目热部署
7 .ignore: 生成git ignore文件

天品
		1、String Manipulation 字符串处理插件
		3、JUnitGenerator V2.0  代码生成
		5、RestfulTool Restful服务开发工具集

2 CodeGlance: 右侧代码地图
3 Translation
4 Rainbow Brackets: 彩虹色括号
5 Grep Console: 日志着色控制台显示
6 Statistic: 代码统计
3、GsonFormatPlus json代码生成
6 Key Promoter X: 快捷键提示
4、Sequence Giagram 生成代码时序图
7、MyBatis X

快捷键
[https://www.jianshu.com/p/5de7cca0fefc](https://www.jianshu.com/p/5de7cca0fefc)

command+N 新建各种东西
command+退格 删除当前行
command+D 复制当前行
4. command+alt+M 将当前选中到代码块抽取为方法
6. alt+command+L 格式化代码
7. alt+enter 生成局部变量（introduce local variable）
9. shift+alt+⬇️ 将当前代码整体下移一行（上移同理）
13. command+alt+U 在当前类中，查看继承关系视图
14. command+alt+左键/右键 将光标返回到上次查看代码的地方
15. command+F12 查找当前类的方法

插件安装位置

cd  /Users/xxxx/Library/ApplicationSupport/JetBrains
