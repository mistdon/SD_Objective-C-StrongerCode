# SD_Objective-C-
总结一些平常不一定知道的冷门知识，健壮代码

###1. 如何健壮的为数组添加数据
在OC代码中, 比较常见的一个Bug是数组越界, 为不可变数组添加或删除内容等, 这样的坑还是蛮多的，有什么办法可以防止这种类型错误呢？
```
//第一种情况,  数组越界, 但是即使数据越界了也不会让程序崩溃，而是继续执行，执行的是catch里面内容
    NSString *showname = nil; //假设这是显示在lable上的数据
    NSMutableArray *data = [NSMutableArray arrayWithObjects:@"shen",@"dong",@"wang",nil];
    @try {
         showname= [data objectAtIndex:3];
    } @catch (NSException *exception) {
        showname = @"shendong";
        DDLogError(@"exception = %@", exception);
    } @finally {
        DDLogDebug(@"This is some debug thing");
        DDLogInfo(@"showname = %@",showname);
    }
    //打印日志
      exception = *** -[__NSArrayM objectAtIndex:]: index 3 beyond bounds [0 .. 2]
     This is some debug thing
     showname = shendong

```
至于在Objective-C中使用try－catch的是与非就不再此处讨论了

###2. 如何在delloc中释放Observe对象
来看看CocoaLumberjack中DDFileLogger.m 中的实现
```
    // try-catch because the observer might be removed or never added. In this case, removeObserver throws and exception
    @try {
        [self removeObserver:self forKeyPath:NSStringFromSelector(@selector(maximumNumberOfLogFiles))];
        [self removeObserver:self forKeyPath:NSStringFromSelector(@selector(logFilesDiskQuota))];
    } @catch (NSException *exception) {
    }
```
其实在实际开发中，如果确定添加了observe，就没有这么做的必要了

