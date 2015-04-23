# HyFrame
简洁、流畅、安全、自适应移动设备的后台管理框架（基于ThinkPHP）

# Overview
框架基于ThinkPHP和Metronic二次封装，集成了众多现代特性（移动适配、AJAX为主），适用于多人协作开发中大型项目的后台管理框架。

## 框架特色
框架基于Thin Controller; Fat Model; Robust Service! 的设计原则，封装了多级角色的路由及权限验证，主要业务采用统一的控制器入口（一般无需写控制器），集成众多安全机制、文件上传及转储、系统日志等，更为强大的是框架封装的Model可以通过简单的配置数组来实现CURD的页面效果（列表显示、检索、新增、删除、修改、表单验证等等）！

框架经过宏奕工作室数个项目的测试、完善，除了功能强大，开发敏捷之外，也具备较强的可扩展性。通过对多人协作开发的优化，对大项目高并发的处理，相信会为您的项目开发提供帮助！

# Get Statred
## 规范：
编码规范：标识符命名规范参考ThinkPHP手册。关键几点：类名（MyName），方法、变量（myName）,常量和配置项（MY_NAME），HTML中id、class（my-name）

数据库规范：表名用复数，字段用小写，单词间用下划线。一般都有id（主键，无符号int，自增）、status（无符号int、默认1、大于9表示逻辑删除）、create_time（无符号int、时间戳）、update_time（无符号init、时间戳）这四个字段。外键、经常需要检索的字段需要建索引（如users表中的name字段）。善于利用视图。

统一格式：赋值（=）、比较（>、!= ...）、? :、运算符（+、-、.=、+= ...），前后加一个空格。“,”之后加一个空格

## 框架部分实现说明
SESSION：框架默认将SESSION存在数据库中，在高并发、大容量下会有更好的表现（但是框架本身使用的SESSION很小，只有几kb），支持分布式数据库。

能CACHE不SESSION：为了减少数据库请求，我们会采取缓存的策略。如框架会自动将frame_setting表中的系统配置项缓存等，采用Cache方式缓存。再如，对于用户的权限规则，我们不同于众多框架那样进行Session缓存，而是根据角色分类进行Cache，这样可以大大减轻服务器的磁盘开销。

密码：先求密码连同一个复杂字符串（可配置）的sha1哈希值（保证密文不可破解），再通过不固定的IV（初始化向量）的AES方式加密，以保证相同的密码的加密结果不同，避免根据常用密码频率高来穷举破解。

登录：系统支持单点登录（可配置是否开启）。实现：页面输出一个随机秘钥用于用户名的AES加密；密码连同复杂字符串做SHA1哈希运算，并从中截取出用于密码AES加密的秘钥进行加密，提交加密后的用户名密码表单后，服务器以逆向思路进行比对。

