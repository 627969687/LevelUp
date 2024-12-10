# 1.基本使用
** KVO：Key Value Observing，键值监听 **
1. 添加监听
2. observer回调方法里do something
3. 移除监听
	1. 添加多少个需要移除多少个


|     |     |     |
| --- | --- | --- |
|     |     |     |

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