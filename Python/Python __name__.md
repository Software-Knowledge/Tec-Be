__name__在python中的使用
---

# 1. __name__的使用
1. __name__是一个变量，之所以有前后下划线，是因为是系统定义的名字。
2. __name__就是标识模块的名字的一个系统变量，当前的模块是主模块，那么这个模块是主模块，那么这个模块的名字就是__main__，否则如果是被import的模块，则这个模块名字为文件名字(不含.py)。
3. 不同情况:
    1. 如果是被导入:__name__的值为模块名字
    2. 如果是被直接执行:__name__的值是`__main__`