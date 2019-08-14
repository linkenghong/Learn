## 日常问题总结
* 程序中有中文出现如下错误：
> SyntaxError: Non-UTF-8 code starting with '\xcf' in file pets.py on line 4, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details

解决方法：在程序开头加（注意=符号前后不能有空格！！）`# coding=gbk`
* 列表很神奇！

元组内元素不可变，但若元组内元素是列表，则列表是可以改变

函数传递时，传递的实参不能改变，但若传递的是列表，列表可以改变

* 文件开头插入一行：

[StackOverflow问题1](https://stackoverflow.com/questions/4454298/prepend-a-line-to-an-existing-file-in-python)、[问题2](https://stackoverflow.com/questions/5914627/prepend-line-to-beginning-of-a-file/5917395)的这两个问题已经说的很详细了，也就是文件操作在modes是'a'或'a+'时，都是在文件尾写入的，无论当前指针停留在哪，这其实是无论哪种语言都是这样的。
正确方法如下：
```
def line_prepender(filename, line):
    with open(filename, 'r+') as f:
        content = f.read()
        f.seek(0, 0)
        f.write(line.rstrip('\r\n') + '\n' + content)
```

* 一个例子

```
def draw_line(x_data, y_data, title, y_legend):
    xy_map = []
    for x, y in groupby(sorted(zip(x_data, y_data)), key=lambda _: _[0]):
        y_list = [v for _, v in y]
        xy_map.append([x, sum(y_list) / len(y_list)])
    x_unique, y_mean = [*zip(*xy_map)]
    x_unique = [str(x) for x in x_unique]
    line_chart = pygal.Line()
    line_chart.title = title
    line_chart.x_labels = x_unique
    line_chart.add(y_legend, y_mean)
    line_chart.render_to_file(title+'.svg')
```
第3行其实是groupby的固定使用（示例如下），其中k是分类的关键值，data就是按这个分类的。g是分组后的一个个组。keyfunc是计算关键值函数的，在上例中`key = lambda _: _[0]` 中 lambda 是一个匿名函数，：前是输入，后是输出，_其实就是sorted(...)的元素，所以这个就是说按sorted(...)中的第一个元素分类，不清楚可以把sorted(...)打印出来。
```
for k, g in groupby(data, keyfunc):
```

* if __name__ == '__main__':

这在大多数程序中都会有，[这个问题](https://stackoverflow.com/questions/419163/what-does-if-name-main-do)说的很清楚。当这个py是当作主程序运行时，该语句下代码会直接运行。若该py是被其他调用时，则不会运行该语句下代码，如此便于调试。

* 装饰器

当要增强某个函数的功能（通常是应用于有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景），但又不希望修改该函数，这种在代码运行期间动态增加功能的方式，称之为“装饰器”（Decorator）。

[这篇文章](https://foofish.net/python-decorator.html)解释得挺清楚。因为函数也是一个对象，所以可以写一个函数A，执行一些操作后，再返还函数B，然后用函数A套函数B。

* DataFrame 行遍历
```
for index, row in df.iterrows():
```

* python import时返回上级目录
```
import sys
sys.path.append("..")
```

* 相对路径：

假设test1文件夹下有test2文件夹，test2文件夹有test.py文件，其中形如'./test/'的路径，这就是相对路径，这个路径相对的并不是这个py文件所在位置，而是相对于执行该文件时的位置。

例如在test1文件夹下执行`py test2\test.py`那么就是test1文件夹下的test，同理test2文件夹下执行`py test.py`，那么就test2文件夹下的test。