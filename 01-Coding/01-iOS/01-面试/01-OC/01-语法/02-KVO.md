# 1.基本使用
** KVO：Key Value Observing，键值监听 **
1. 添加监听（监听的键值一致、参数一致都不会被替换，**添加多少次就要移除多少次**）
2. observer回调方法里do something
3. 移除监听
	1. 添加多少个需要移除多少个

<br>

`addObserver:forKeyPath:options:context:`的一些参数
1. observer：监听者
2. keyPath：键值
3. context：上下文指针，空类型指针（无法修改，作为一些只读的各种类型数据使用）
4. options：监听选项

| **NSKeyValueObservingOptions**   | **监听选项**                                                  |
| -------------------------------- | --------------------------------------------------------- |
| NSKeyValueObservingOptionNew     | 新值                                                        |
| NSKeyValueObservingOptionOld     | 旧值                                                        |
| NSKeyValueObservingOptionInitial | 添加监听之后立即发送一次消息                                            |
| NSKeyValueObservingOptionPrior   | 监听的键值发生修改前预先发送一次消息，加上`willChangeValueForKey:`就是每次修改发送两次消息 |

```objc
**@interface** Person : NSObject
**@property** (**assign**, **nonatomic**) **int** age;
**@property** (**assign**, **nonatomic**) **int** height;
**@end**

- (**void**)viewDidLoad {
    [**super** viewDidLoad];
	**self**.person1 = [[Person alloc] init];
	**self**.person1.age = 1;
	**self**.person1.height = 11;

    // 给对应的属性（键值）添加监听
    NSKeyValueObservingOptions options = NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld;
    [**self**.person1 addObserver:**self** forKeyPath:@"age" options:options context:@"123"];
    [**self**.person1 addObserver:**self** forKeyPath:@"height" options:options context:@"456"];
}

// 修改被监听的属性
- (**void**)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    **self**.person1.age = 20;
    **self**.person1.height = 30;
}

// 当监听对象的属性值发生改变时，就会触发
- (**void**)observeValueForKeyPath:(NSString *)keyPath ofObject:(**id**)object change:(NSDictionary<NSKeyValueChangeKey,**id**> *)change context:(**void** *)context {
    NSLog(@"监听到%@的%@属性值改变了 - %@ - %@", object, keyPath, change, context);
}

- (**void**)dealloc {
    // 移除监听
    [**self**.person1 removeObserver:**self** forKeyPath:@"age"];
    [**self**.person1 removeObserver:**self** forKeyPath:@"height"];
}
```

# 2.本质


# 3.调用流程