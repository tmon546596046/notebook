# pandas保存到csv报错（字符集问题）

## 错误信息

```
UnicodeEncodeError: 'gbk' codec can't encode character '\u2003' in position 18: illegal multibyte sequence
```

## 测试代码

```python
    df = pd.DataFrame([{'应用': '纠错助手', '用户名': '张恩铭', 'userno': '007992', '内容': '\u2003', '时间': datetime.datetime(2025, 1, 3, 5, 46, 52, 75000)}
])
    df.to_csv("gb18030.csv", encoding="GB18030")
    df.to_csv("utf8.csv", encoding="utf-8")
    df.to_csv("utf8-sig.csv", encoding="utf-8-sig")
    df.to_csv("gbk.csv", encoding="gbk")
```

## 原因

​	通过日志发现，只有到gbk的时候才会报错，其他编码都不会报错，但是gb18030显示的是"??"。仔细查看代码发现字符串中存在unicode代码点:\u2003，通过查看unicode代码点发现此2003代码点对应的就是一个不可见字符。原因应该是此代码点没有包含在gbk字符集中。解决方法可以使用别的字符集进行如utf-8进行保存。

​	实时替换由于首先与reload方法的依赖关系，最好的办法就是重新启动服务，不是简单的替换python文件内容。

## 参考资料

1. https://stackoverflow.com/questions/437589/how-do-i-unload-reload-a-python-module