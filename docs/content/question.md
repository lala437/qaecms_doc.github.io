> 如何修改后台和用户中心路径?
````
后台路径可在config/qaecms.php 中修改 admin_path 值
用户中心 可在config/qaecms.php 中修改 user_path 值

例如:默认路径为: qaecmsadmin/index
修改之后路径就为: 修改后路径/index

!!!注意  修改路径之后  请进入程序目录 执行 php artisan route:clear
````
> 如何重置后台账号密码
````
以root用户权限 进入程序目录
执行: php artisan init 新用户名 新密码 
例如: php artisan init newuser newpass
````