# javascript & node 前端测试框架

## Sinon    

   支持Spy（函数监控&间谍）、Stubs（存根）、mock、Fake Timers等    
   	
   - [Sinon官网](http://sinonjs.org/)
   - [Unit Test like a Secret Agent with Sinon.js](http://www.elijahmanor.com/unit-test-like-a-secret-agent-with-sinon-js/)

## Jasmine

  
[Jasmine官网](http://jasmine.github.io/2.2/introduction.html)
      
```
expect(a).toBe()；              // 相等
expect(false).not.toBe(true);  // 不相等
expect(a).toEqual(12);         // 值相等（如对象）
expect("foo bar baz").toMatch("bar"); // 正则匹配
expect("foo bar baz").not.toMatch(/quux/); // 正则不匹配
expect(a.foo).toBeDefined();    // 已定义变量
expect(a.bar).not.toBeDefined();  //未定义变量
expect(a.foo).not.toBeUndefined();  //值是未定义
expect(a.bar).toBeUndefined();      //值不是未定义
expect(a).toBeNull();              // 是空值
expect(foo).not.toBeNull();        // 不是空值
expect(foo).toBeTruthy();          // 是布尔值true
expect(a).not.toBeTruthy();        // 不是布尔值
expect(a).toBeFalsy();             // 是布尔值假值
expect(foo).not.toBeFalsy();       // 非布尔值假值
expect(a).toContain("bar");        // 数组包含 bar
expect(a).not.toContain("quux");   // 数组不包含 bar

```

## Mocha should

[Mocha官网](http://mochajs.org/)

```

```

[Should](https://github.com/tj/should.js)


## Qunit



## Karma

   自动化测试工具
   
   [官网](http://karma-runner.github.io/)
