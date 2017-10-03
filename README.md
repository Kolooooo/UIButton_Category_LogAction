# 点击按钮, 打印按钮触发所在的类

分类UIButton 类

```
+ (void)load{
    [self __exchangeMethodWithOriginSEL:@selector(sendAction:to:forEvent:)
                            exchangeSEL:@selector(__sendAction:to:forEvent:)];
}
```



