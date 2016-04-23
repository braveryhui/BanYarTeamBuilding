# PSR-0规范

####什么是psr0？

> 强调自动加载的方式
下文描述了若要使用一个通用的自动加载器(autoloader)

####psr0规范:

1. 一个完全标准的命名空间(namespace)和类(class)的结构是这样的：\()
2. 每个命名空间(namespace)都必须有一个顶级的空间名(namespace)("组织名(Vendor Name)")。
3. 每个命名空间(namespace)中可以根据需要使用任意数量的子命名空间(sub-namespace)。
4. 从文件系统中加载源文件时，空间名(namespace)中的分隔符将被转换为 DIRECTORY_SEPARATOR。
5. 类名(class name)中的每个下划线_都将被转换为一个DIRECTORY_SEPARATOR。下划线_在空间名(namespace)中没有什么特殊的意义。
6. 完全标准的命名空间(namespace)和类(class)从文件系统加载源文件时将会加上.php后缀。
7. 组织名(vendor name)，空间名(namespace)，类名(class name)都由大小写字母组合而成。

例01
```
DoctrineCommonIsolatedClassLoader => /path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php
SymfonyCoreRequest => /path/to/project/lib/vendor/Symfony/Core/Request.php
ZendAcl => /path/to/project/lib/vendor/Zend/Acl.php
ZendMailMessage => /path/to/project/lib/vendor/Zend/Mail/Message.php
```