·
## 安装

## 激活
https://www.yuque.com/docs/share/29d5dc6a-d47b-4841-a496-5068389cfe1f

## 设置
字体设置
file --> settings --> 输入font --> 设置字体样式以及字号大小。

## 快捷键
[IDEA常用快捷键【win-mac对比】](https://blog.csdn.net/Redemption___/article/details/122147175)

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


## 插件

[精品 IDEA 插件大汇总！值得收藏（鱼皮）](https://mp.weixin.qq.com/s/dVNleEGBr9h5RsZIgB-JVQ)

**核心**

Alibaba_Java_Coding_Guidelines-2.1.1   代码规范
CodeGlance_Pro-1.4.8   右侧代码地图

EasyCode-1.2.6-java.RELEASE   数据库表生成JavaBean
gittoolbox-212.9.7   git管理
GrepConsole-12.14.211.6693.0   日志控制台着色显示
GsonFormatPlus   Json类生成
ignore-4.4.2   生成git的ignore文件
intellij-rainbow-brackets-6.25   彩虹括号
Json2Pojo   JSON转换
JumpToLine   调试跳转
Key-Promoter-X-2022.2   快捷键提示
MavenRunHelper   maven辅助
mybatis-log-plugin-free-1.3.0   快速打印SQL语句日志
MybatisX-1.5.4   Mybatis辅助
RestfulTool-1.4.5   接口开发工具
SequenceDiagram-2.2.0   生成代码时序图
StringManipulation   字符串处理插件
TabNine-0.7.20   智能代码补全
TranslationPlugin-3.3.4   翻译
zh.222.168   中文







## 待整理


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


