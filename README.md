* [Introduction](#introduction)
* [Install](#install)
* [Usage](#usage)
  * [正常响应](#正常响应)
  * [报错，即server error](#报错即server-error)
  * [其他响应](#其他响应)
  * [自定义status对象](#自定义status对象)

## Introduction
一个人让你使用起来非常舒服的web项目响应请求框架

## Install
1. 克隆此项目
```
git clone https://github.com/ilanky/web-response.git
```
2. 上传到你自己的maven仓库
```
mvn install
```

## Usage
1. 引入maven
```
        <dependency>
            <groupId>cn.ilanky.web</groupId>
            <artifactId>web-response</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>
```
2. 扫描本包
```java
@ComponentScan(basePackages = "cn.ilanky.web.*")
```
### 正常响应
1. 不带result
```java
    @GetMapping("/hello")
    public void hello(){
        ResultContext.ok();
    }
```
响应为
```json
{"msg":"OK","result":null,"code":200}
```
2. 带result
```java
    @GetMapping("/hello")
    public void hello(){
        ResultContext.ok("hello world");
    }
```
可传入任意对象，采用Jackson进行json化

响应为
```json
{"msg":"OK","result":"hello world","code":200}
```

### 报错，即server error
```java
    @GetMapping("/hello")
    public void hello(){
        ResultContext.serverError();
    }
```
响应为
```json
{"msg":"Internal Server Error","result":null,"code":500}
```
带结果的同上

### 其他响应
1. 不带result
```java
    @GetMapping("/hello")
    public void hello(){
        ResultContext.set(CommonResultStatus.OK);
    }
```
2. 带result
```java
    @GetMapping("/hello")
    public void hello(){
        ResultContext.set(CommonResultStatus.OK).setResult("hello world");
    }
```
具体可看CommonResultStatus这个类

### 自定义status对象
demo如下
```java
/**
 * @author ilanky
 * @date 2021年 05月04日 19:51:37
 */
public enum SupportStatus implements StatusCarrier {

    NOT_TIMES(2000,"test");


    private int status;
    private String desc;

    SupportStatus(int status, String desc) {
        this.status = status;
        this.desc = desc;
    }

    @Override
    public Map<String, Object> toMap() {
        Map<String,Object> map = new HashMap<>();
        map.put("status", status);
        map.put("desc", desc);
        return map;
    }
}
```
最终会调用toMap方法然后进行json化。需要定制返回参数自定义实现即可

其他使用同上

