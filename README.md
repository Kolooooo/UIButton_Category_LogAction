# 点击按钮, 打印按钮触发\(Action\) 所在的类

分类UIButton 类

```
@implementation UIButton (Category)

+ (void)load{
    [self __exchangeMethodWithOriginSEL:@selector(sendAction:to:forEvent:)
                            exchangeSEL:@selector(__sendAction:to:forEvent:)];
}

- (void)__sendAction:(SEL)action to:(id)target forEvent:(UIEvent *)event{
    NSLog(@"Class: %@ -> SEL: %@",
          NSStringFromClass([target class]), NSStringFromSelector(action));
    
    [self __sendAction:action to:target forEvent:event];
}

@end

```



