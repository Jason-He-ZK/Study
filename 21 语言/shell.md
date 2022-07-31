定义变量：不加美元符号

使用变量：在变量名前面加美元符号

echo $your_name

echo ${your_name}

只读

myUrl="https://www.google.com"

readonly myUrl

删除（无法删除只读变量）

unset variable_name

单引号字符串

变量是无效的

单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

双引号的优点：

可以有变量

可以出现转义字符

your_name="runoob"

str="Hello, I know you are \"$your_name\"! \n"

拼接

greeting_2='hello, '$your_name' !'
