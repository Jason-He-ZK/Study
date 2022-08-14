# 安装
查看版本：brew -v
安装软件：brew install 软件名
搜索软件：brew search 软件名
卸载软件：brew uninstall 软件名
更新所有软件：brew update
更新具体软件：brew upgrade 软件名
显示已安装软件：brew list
查看哪些已安装的程序需要更新： brew outdated
显示包依赖：brew reps
显示帮助：brew help

/opt/homebrew/opt

# 开发软件
## MySQL
### 安装
brew install mysql
brew services start mysql
```
$ mysql_secure_installation
Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?
// 是否启用验证密码插件。如果想设置简单的密码选N，我这里选的启用
Press y|Y for Yes, any other key for No: Y

There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 0
Please set the password for root here.

New password:

Re-enter new password:

Estimated strength of the password: 50
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

// 是否移除匿名用户
Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

// 是否禁止root远程登录
Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.

// 是否删除test数据库
Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

// 是否重新加载权限表。选是上述修改才能生效
Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done!
```
### 修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '12345678'

## Redis
brew install redis
brew services start redis
修改密码
redis-cli
config set requirepass 12345678
