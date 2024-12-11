# 1.基本使用
**KVO：Key Value Observing，键值监听**
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
# 2.1 被修改的指向
![KVO1.jpg](https://raw.githubusercontent.com/627969687/LevelUp/main/resource/202412110157016.jpg)
通过拦截并生成新的类对象方式完成监听：
1. 在原本实例对象的isa指向类对象的过程中
2. runtime构建了一个KVO的中间类，修改实例对象isa的指向为指向自己
# 2.2 NSKVONotifying_xxx的内部

| **_NSSetObjectValueAndNotify** | 内部实现                                                               |
| ------------------------------ | ------------------------------------------------------------------ |
| 调用`willChangeValueForKey:`     | 记录了旧的值                                                             |
| 调用原来的set方法                     |                                                                    |
| 调用`didChangeValueForKey:`      | 调用`observeValueForKeyPath:ofObject:change:context:`方法告诉监听者属性值发生改变。 |

1. 通过重写`setAge:`添加`Foundation`框架的`_NSSetObjectValueAndNotify`方法完成监听
	1. 通过通知的方式完成监听
2. 通过重写`class`防止`NSKVONotifying_xxx`这个真正的类被暴露
	1. 该类是通过`runtime`动态插入并且没有被暴露的意义
3. 通过重写`dealloc`处理KVO自己的内存释放
4. 增加`_isKVOA`

# 2.3 手动触发KVO
手动触发KVO的方法：
1. 调用`willChangeValueForKey`记录旧的值
2. 调用`didChangeValueForKey`触发`observeValueForKeyPath:ofObject:change:context:`并把旧的值和新的值（当前值）返回给监听者
注：所以手动触发`observeValueForKeyPath:ofObject:change:context:`该方法前提就是获取旧的值和新的值（当前值）。由于不需要调用setter方法，所以这里旧的值等于新的值（当前值）。