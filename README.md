# 点击按钮, 打印按钮触发\(Action\) 所在的类

思路：

1. 创建UIButton 分类
2. 就是

3. \`\`\`  
   @implementation UIButton \(Category\)

4. \(void\)load{  
     SEL originSEL = @selector\(sendAction:to:forEvent:\);  
     SEL exchangeSEL = @selector\(\_\_sendAction:to:forEvent:\);

   Method originMethod = class\_getInstanceMethod\(self, originSEL\);  
     Method changeMethod = class\_getInstanceMethod\(self, exchangeSEL\);

   // 方法交换应该被保证，在程序中只会执行一次  
     static dispatch\_once\_t onceToken;  
     dispatch\_once\(&onceToken, ^{

   ```
     // 首先动态添加方法，实现是被交换的方法，返回值表示添加成功还是失败
     BOOL isAddSuccess = class_addMethod(self,
                                         originSEL,
                                         method_getImplementation(changeMethod),
                                         method_getTypeEncoding(changeMethod));

     if (isAddSuccess) {
         // 如果成功，说明类中不存在这个方法的实现
         // 将被交换方法的实现替换到这个并不存在的实现
         class_replaceMethod(self,
                             exchangeSEL,
                             method_getImplementation(originMethod),
                             method_getTypeEncoding(originMethod));
     }
     else{
         // 否则，交换两个方法的实现
         method_exchangeImplementations(originMethod, changeMethod);
     }
   ```

   }\);  
   }

// 替换方法

* \(void\)\_\_sendAction:\(SEL\)action to:\(id\)target forEvent:\(UIEvent \*\)event{  
    NSLog\(@"Class: %@ -&gt; SEL: %@",  
          NSStringFromClass\(\[target class\]\), NSStringFromSelector\(action\)\);

  // 此时调用替换方法就是调用原方法  
    \[self \_\_sendAction:action to:target forEvent:event\];  
  }

@end  
\`\`\`

