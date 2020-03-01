# static analysis


> 明日的程序员是未来的魔法师


## program analysis


- analysis

    - 可靠性 reliability
    
        - 空值的引用 null pointer  但是你也可以去判断它
        ```JavaScript
        let arr
        if (arr == null) {
            arr = []
        }
         ```
         
        - memory leak  内存快满了
        
        - and so on
        
    - 安全 security
    
        - private information leak 用户密码泄漏
        
        - injection attack 黑客攻击
        
            - 解决办法 运行程序之前 在编译时刻 完成所有的安全可靠性鉴定
            
            - 静态时刻分析你的程序
            
        - 名词 : 编译优化 compiler optimization
           
            - 可以优化程序 code elimination
            
            - 大部分使用 静态分析  debug
            
        - program understanding


- market 废话太多了

    - academia
    
        - systems
        
        - security
        
    - industries
    
    
- 啥是 语法 语义

    - 可靠
    
    - 安全
    
    - 高效
    

- 三观

    - 不要浮躁
    
    - 不要随波逐流
    
    - 要快乐
    
    - 审视自己
    
    - 比较
    
    - 仔细思考
    
    - 花时间 能走多远
    
    - 自己判断
    
    
- 废话真多

    - 写程序
    
        - 测试 assert 断言
        
        - private information
        
        - null  pointers
        
    - perfect static analysis 是不存在的
    
        - sound     爷爷
        
        - truth     老爸
        
        - complete  儿子
        
    - useful static analysis
    
        - 本来不是bug 你说是 bug 
        
        - necessity of soundness
        
            - 编译优化
            
            - 程序验证
            
            ```markdown
            b = B.new()
            c = C.new()
            a.fld = b
            a.fld = c
            
            b' = a.fld
            // unsound  true
            // sound false
            ```
            - unsound  
            
            - sound 更加专业 分析两个分支（上面的例子）
            
- bird's eye view

```markdown
if (input) { // 显然 input 这样写代码肯定是有问题的 存在隐式转换
    x = 1
} else {
    x = 0
}
x = ?
 ```   
 都是正确的， 静态分析简单来说就是 分析结果有那些可能的情况
 
```markdown
1, when input true x = 1
   when input false x = 0
   
   sound precise expensive
   
2, x = 1 or x = 0
    
    sound imprecise cheap
    
这是在玩文字游戏吗
```  
 
要学会 取舍 精度和速度平衡   类似 时间和空间 取舍
          
 
- analysis

    - abstraction 抽象
    
        - judge + - * / all the variables
        
        - concrete domain ints  &&  abstract domain
        ```markdown
        // variable   concrete domain ints                //signs abstract domain
        v = 10                                            +
        v = 1                                             -
        v = -1                                            0
        v = 0                                             T unknown
        v = e ? 1 : -1                                    L  undefined
        v = w / 0
        ```
    
    - over_approximation  过近似
        
        - transfer function
        
        ```markdown 
        + + + = +
        - + - = -
        0 + 0 = 0
        + + - = T unknown
        + / + = +
        - / - = -
        T / 0 = L undefined
        + / - = -
        ```
        
        转换下面的表达式
        ```markdown
        x = 10     x = +
        y = -1     y = -
        z = 0      z = 0
        a = x + y  a = T unknown
        b = z / y  b = 0
        c = a / b  c = L undefined // 1
        p = arr[y] p = L undefined // 2
        q = arr[a] q = L undefined // 3 只要里面 可能一个有毛病 他就 gg
        
        1, 2 static analysis is useful
        
        3 over_approximation  过近似 static analysis products false positives
        ```
        
    - 名词： 控制流  control flows  我还爆破流 明明是控制语句 循环语句
    ```markdown
    let x = 1
    let y 
    if (input.length > 0) {
      y = 10
    } else {
      y = -1
    }
    let z = x + y
    ```   