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
expect(e).toBeLessThan(pi);        // e小于pi
expect(pi).not.toBeLessThan(e);    // pi 不小于 e
expect(pi).toBeGreaterThan(e);		// pi 大于 e
expect(e).not.toBeGreaterThan(pi);   // e 不大于 pi
expect(foo).not.toThrow();           // 函数 不抛出异常
expect(bar).toThrow();					// 函数抛出异常
expect(foo).toThrowError("foo bar baz");   // new TypeError('foo bar baz') 
expect(foo).toThrowError(/bar/);  		   // 函数是否包含特定的异常
expect(foo).toThrowError(TypeError);
expect(foo).toThrowError(TypeError, "foo bar baz");
expect(foo).toEqual(1);   // 函数是否包含值
expect(true).toEqual(true);
beforeEach(function() {   // 函数调用之前（安装前）初始
    foo += 1;
});

afterEach(function() { // 函数调用之后（卸载）
    foo = 0;
});
beforeAll(function() {// 函数全局调用之前（安装前）
    foo = 1;
});
afterAll(function() {// 函数全局调用之后（卸载）

    foo = 0;
});
```

## Mocha should

[Mocha官网](http://mochajs.org/)

```
同步CODE：
describe('Array', function() {
  describe('#indexOf()', function() {
    it('should return -1 when the value is not present', function() {
      [1,2,3].indexOf(5).should.equal(-1);
      [1,2,3].indexOf(0).should.equal(-1);
    });
  });
});

异步CODE：
describe('User', function() {
  describe('#save()', function() {
    it('should save without error', function(done) {
      var user = new User('Luna');
      user.save(function(err) {
        if (err) throw err;
        done();
      });
    });
  });
});

也可以用done实现接收一个error
describe('User', function() {
  describe('#save()', function() {
    it('should save without error', function(done) {
      var user = new User('Luna');
      user.save(done);
    });
  });
});

should.js: http://shouldjs.github.io/#assertion-iterator
expect.js: https://github.com/mjackson/expect


```

[Should](https://github.com/tj/should.js)

## supertest
支持HTTP测试 

[supertest官网](https://github.com/visionmedia/supertest)

```
不使用任何测试框架：
var request = require('supertest')
  , express = require('express');

var app = express();

app.get('/user', function(req, res){
  res.send(200, { name: 'tobi' });
});

request(app)
  .get('/user')
  .expect('Content-Type', /json/)
  .expect('Content-Length', '20')
  .expect(200)
  .end(function(err, res){
    if (err) throw err;
  });
  
使用mocha框架：
describe('GET /user', function(){
  it('respond with json', function(done){
    request(app)
      .get('/user')
      .set('Accept', 'application/json')
      .expect('Content-Type', /json/)
      .expect(200, done);
  })
})

失败断言：
describe('GET /users', function(){
  it('respond with json', function(done){
    request(app)
      .get('/user')
      .set('Accept', 'application/json')
      .expect(200)
      .end(function(err, res){
        if (err) return done(err);
        done();
      });
  });
});

预期断言：
describe('GET /user', function(){
  it('user.name should be an case-insensitive match for "tobi"', function(done){
    request(app)
      .get('/user')
      .set('Accept', 'application/json')
      .expect(function(res) {
        res.body.id = 'some fixed id';
        res.body.name = res.body.name.toUpperCase();
      })
      .expect(200, {
        id: 'some fixed id',
        name: 'TOBI'
      }, done);
  });
});

多文件上传断言：
request(app)
.post('/')
.field('name', 'my awesome avatar')
.attach('avatar', 'test/fixtures/homeboy.jpg')
...


```

## Karma

   自动化测试工具
   
   [官网](http://karma-runner.github.io/)

## mock

[官网](https://github.com/nuysoft/Mock)
