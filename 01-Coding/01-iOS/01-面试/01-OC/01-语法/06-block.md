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
# 2.1 定义block
1. 无返回值、无参数
```objc
void (^testBlock) = ^ {
	NSLog(@"test");
}
testBlock();
```
2. 无返回值、有参数
```objc
void(^testBlock)(int, NSString) =^(int a, NSString *b) {
	NSLog(@"%d--%@",a,b);
}
```
3. 有返回值、无参数
```objc
NSString*(^testBlock) =^ {
	return "返回值";
}
```
4. 有返回值、有参数
```objc
int(^testBlock)(int, int) =^(int a, int b) {
	return a+b;
}
```
# 2.2 block作为方法的参数
```objc
- (void)testFunc:(int(^)(int))testBlock {
	 testBlock(1)
}

- (void)testFunc2:(void(^)(NSData *data, NSError *error))completion {
	NSData *data = [NSData new];
	NSError *data = [NSError new];
	completion(data, error)
}
```
# 2.4 block作为方法的返回值
```objc

```
# 2.5 block和typedef
```objc
typedef int (^TestBlock)(NSData *data, NSError *error);

- (void)testFunc2:(TestBlock)completion {
	NSData *data = [NSData new];
	NSError *data = [NSError new];
	completion(data, error)
}
```

# 3. 本质