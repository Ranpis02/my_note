# 1 property

`property` 用于将方法包装成属性，让**方法可以以属性的形式被访问和调用**

语法格式：

```python
property(fget=None, fset=None, fdel=None, doc=None) -> property attribute
```

- `fget` 是**获取属性值的方法**。
- `fset` 是**设置属性值的方法**。
- `fdel` 是**删除属性值的方法**。
- `doc` 是**属性描述信息**。如果省略，会把 `fget` 方法的 `docstring` 拿来用

**示例代码**

```python
class Student:
    def __init__(self):
        self._age = None
    def get_age(self):
        return self._age
    def set_age(self, age):
        self._age = age
    def del_age(self):
        del self._age
    
    age = property(get_age, set_age, del_age, '学生年龄')
    
# 创建 Student 对象
s = Student()

# 输出属性的文档字符串
Student.age.__doc__

# 设置属性
s.age = 18

# 获得属性
s.age

# 删除属性
del s.age
```



:bulb:使用 `@property` 装饰器进替换上述写法

- 被 `@property` 装饰的方法是**获取属性值的方法**，被装饰方法的名字会被用做 `属性名`
- 被 `@属性名.setter` 装饰的方法是**设置属性值的方法**。
- 被 `@属性名.deleter` 装饰的方法是**删除属性值的方法**。

```python
class Student:
    def __init__(self):
        self._age = None
    
    @property
    def age(self):
        return self._age
    
    @age.setter
    def age(self, age):
        self._age = age
        
    @age.deleter
    def age(self):
        del self.age
```



