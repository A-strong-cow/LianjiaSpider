# LianjiaSpider

### 存在问题:

def parse(self,response):
  ...
  if a<=100:
    self.A_parse(respose)
  ....

def A_parse(self,response):
  ...
  yield scrapy.Resquest(...，callback=self...)

#### 以上代码中，不管A_parse中yield的是不是parse,都会与parse中的self.A_parse(respose)这种调用方式形成阻塞，即：爬虫爬了一圈就结束了，该问题可通过注释A_parse的yield来复现，不知道什么原因

### 暂解决方式：

在parse中使用yield scrapy.Request(response.url, ..., callback=self.A_parse)的方式，再发一次请求来实现的，但是这是一个很糟糕的方式，重复消费掉了很多请求资源，对性能也会造成很大影响

github:https://github.com/A-strong-cow/LianjiaSpider.git