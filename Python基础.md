# 1， 运行

## 1, 直接运行源程序

- 指定使用python执行 `python test.py`

- 在源代码中指定Python的程序 `#!/usr/bin/python` 并赋予可执行权限， 然后执行 `test.py`

## 2， 字节代码（源代码编译后生成的后缀为pyc文件）

```
# 编译方法
import py_compile
py_compile.compile("test.py")
```

## 3， 优化代码（经过优化的源文件，后缀名为'.pyo'）

- 方法 `python -O -m py_compile test.py`
