# Linux 分割大文件

## 场景

今天需要拆分一个`18G`的数据库`sql`文件, 程序肯定不能直接读取加载到内存内. 现在需要考虑如果对大文件进行拆分. 其实后期加入遇到大的日志文件也会遇到拆分.

## 步骤

使用**split**命令对大文件进行拆分.

```
split -l 100000 xxx.file -d -a 4
```