# Python MetaClass笔记：以zipline Dataset为例

## 目的

我们希望创建一张表，里面定义的字段能够自动获取到字段名称与对应的表名称。类似于下面这样：

```python
class Column(object):
    def __init__(self, name, dataset):
        self.name = name
        self.dataset = dataset
class ChinaEquityPricing(DataSet):
    open = Column('open', ChinaChinaEquityPricing)
    high = Column('high', ChinaChinaEquityPricing)
    ...
```

但是这样有点不优雅，万一不小心敲错`high=Column('low')`就不好了。我们希望能够如下定义:

```python
class ChinaEquityPricing(DataSet):
    open = Column(float_type)
    high = Column(float_type)
    ...

```

当访问其属性的时候，属性能够自动获得其名称：

```python
ChinaEquityPricing.open.name
# open
ChinaEquityPricing.high.dataset
# ChinaEquityPricing
```

看出里面的魔法了吗？

我们在定义类`ChinaEquityPricing`的时候，`open = Column(float_type)`这里的`Column`实例是如何得知自己的`name`是`open`,`dataset`是`ChinaEquityPricing`的？

## zipline的解决方案

```python
import six


class Column(object):
    def __init__(self, column_type):
        self.column_type = column_type


class BoundColumn(object):
    def __init__(self, name, dataset):
        self.name = name
        self.dataset = dataset


class ColumnDescr(object):
    def __init__(self, column_name, dataset):
        self.column_name = column_name
        self.dataset = dataset

    def __get__(self, instance, owner):
        return BoundColumn(self.column_name, self.dataset)


class DataSetMeta(type):
    def __new__(cls, name, bases, attrs):
        new_type = super().__new__(cls, name, bases, attrs)
        for col_name, column in attrs.items():
            if isinstance(column, Column):
                column_descr = ColumnDescr(col_name, name)
                setattr(new_type, col_name, column_descr)
        return new_type


class DataSet(six.with_metaclass(DataSetMeta)):
    ndim = 2


class ChinaEquityPricing(DataSet):
    open = Column(float)
    close = Column(float)
```

测试

```python
if __name__ == '__main__':
    print(ChinaEquityPricing.close.name)
    print(ChinaEquityPricing.close.dataset)
```

```python
close
ChinaEquityPricing
```

解释：

`ChinaEquityPricing`类继承自`DataSetMeta`,其修改了类的生成方式。具体的说，其检测子类，如果遇到子类内定义了`Column`实例的属性，就将其变成`ColumnDescr`类的实例。而实例化`ColumnDescr`需要的是`col_name`和`dataset`. 这样的后果就是`ChinaEquityPricing`里面的`open`从`Column`的实例变成了`ColumnDescr`的实例。



当我们访问`ChinaEquityPricing`属性的时候，注意到此时其属性是`ColumnDescr`的实例,  触发了数据描述器(Data Descriptor)

```python
ChinaEquityPricing.close ----> ChinaEquityPricing.__dict__['close'].__get__(None, ChinaEquityPricing)
```

又返回一个`BoundColumn`实例。

也就是说，我们原来是在`ChinaEquityPricing`定义`Column`实例，但是通过一系列运作，最后得到了`BoundColumn`实例，这个实例里面包含了`name`和`dataset`.