[面试题 example](https://www.1point3acres.com/bbs/thread-596229-1-1.html)

问了class和instance的区别
    MyAns: 
        Class: a "template"/"blue print" used to create the instances.
            The class is created by "def" 
        Instance: each instance is an individual object.
            Have different atrributes.
        Ref: https://alfredjava.wordpress.com/2008/07/08/class-vs-object-vs-instance/
问了field attribute和instance attribute的区别
    MyAns:
        Ref: https://dzone.com/articles/python-class-attributes-vs-instance-attributes
        
Class attributes: a variable belong to the class.
    Different instances usually have a same value. 
    Its value is usually assigned while we define the class.
    

Instance attribute: a variable belong to one specific object(instance). 
    Different instance usually have different value
    Its value is usally assigned while create/initialize a instance.

E.g.
```
class ExampleClass(object):
    class_attr = 0

    def __init__(self, instance_attr):
        self.instance_attr = instance_attr
```

Note:
    https://www.python-course.eu/python3_class_and_instance_attributes.php
    Reassign value of a class_attr on the class, all the class_attr will change. Reassign value of a class_attr on the instance, it will create a new instance_attr with the same name as class_attr, so, looks like only this instance's class_attr have a value change, the class_attr for other instances remain same with class.
    
I.e. 

```
    Def A()
        ca = 0
    X=A(), Y=A()
```
    
如果set A.ca = 1 
    Then X.ca 和 Y.ca 都会变成1
如果 set X.ca = 1
    只有 X.ca 是 1
    A.ca, Y.ca 都是 0
    
这是因为 class_attr 和 instance_attr  被 store在不同地方

找instance_attr 只需要 x.__dict__
找 Class_attr 需要 x.__class__.__dict___
    
