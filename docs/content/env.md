# 安装环境
> 快简易CMS基于laravel5.8开发,因此对php版本和扩展有一定的要求,所以强烈推荐使用宝塔面板做环境部署,如果需要自己安装环境 以下也会给出具体配置要求

**宝塔面板环境选择**
- 1、PHP 7.4(安装fileinfo扩展,删除禁用函数putenv,超时时间设置为3600)
- 2、nginx 1.18
- 3、mysql 5.6+

**自定义环境配置**
- PHP >= 7.1.3
- PHP OpenSSL 扩展
- PHP PDO 扩展
- PHP Mbstring 扩展
- PHP Tokenizer 扩展
- PHP XML 扩展
- PHP Ctype 扩展
- PHP JSON 扩展
- PHP BCMath 扩展

> 不推荐使用不可自定义环境配置的虚拟主机等,出现未知问题暂不提供解答,当然头铁的还是可以自己去尝试

