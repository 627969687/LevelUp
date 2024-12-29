# 1. 什么是block
block实际上是一个把代码快封装起来用于在某些时刻调用的对象，格式长得有点像方法
```objc
^{
    NSLog(@"this is a block!");
    NSLog(@"this is a block!");
    NSLog(@"this is a block!");
    NSLog(@"this is a block!");
};
```
```objc
返回值类型 (^block变量名)(参数1，参数2) = ^(参数1，参数2) {

};

```

# 2. 基本使用
1. 无返回值、无参数
```objc
void (^testBlock) (int);
    roseBlock = ^(int num){
        for (int i = 0; i < num; ++i) {
            printf("  {@} \n");
            printf("   |  \n");
            printf("  \\|/ \n");
            printf("   | \n");
        }
    }; 
testBlock(2);
```
2. 无返回值、有参数
```objc

```
3. 有返回值、无参数
```objc

```
4. 有返回值、有参数
```objc

```

# 3. 本质