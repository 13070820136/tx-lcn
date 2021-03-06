# LCN分布式事务框架

## 框架特点

1. 支持各种基于spring的db框架
2. 兼容springcloud、dubbo
3. 使用简单，代码完全开源
4. 基于切面的强一致性事务框架
5. 高可用，模块可以依赖dubbo或springcloud的集群方式做集群化，TxManager也可以做集群化
6. 支持本地事务和分布式事务共存


## 特别说明

1. 同一个模块单元下的事务是嵌套的。
2. 不同事务模块下的事务是独立的。

备注：框架在多个业务模块下操作更新同一个库下的同一张表下的同一条时，将会出现锁表的情况，会导致分布式事务异常，数据会丧失一致性。

方案：
  希望开发者在设计模块时避免出现多模块更新操作（insert update delete）同一条数据的情况。

## 使用示例

分布式事务发起方：
```java

    @Override
    @TxTransaction
    public boolean hello() {
        //本地调用
        testDao.save();
        //远程调用方
        boolean res =  test2Service.test();
        //模拟异常
        int v = 100/0;
        return true;
    }
    
```

分布式事务被调用方(test2Service的业务实现类)
```java

    @Override
    public boolean test() {
        //本地调用
        testDao.save();
        return true;
    }

```

如上代码执行完成以后两个模块都将回滚事务。

说明：只需要在分布式事务的**开启方**添加`@TxTransaction`注解即可。详细见demo教程


## 目录说明

lorne-tx-core 是LCN分布式事务框架的切面核心类库

dubbo-transaction 是LCN dubbo分布式事务框架

springcloud-transaction 是LCN springcloud分布式事务框架

tx-manager 是LCN 分布式事务协调器


## 关于框架的设计原理

见 [TxManager](https://github.com/1991wangliang/tx-lcn/blob/master/tx-manager/README.md)


## demo 说明

demo里包含jdbc\hibernate\mybatis版本的demo

dubbo版本的demo [dubbo-demo](https://github.com/1991wangliang/dubbo-lcn-demo)

springcloud版本的demo [springcloud-demo](https://github.com/1991wangliang/springcloud-lcn-demo)


技术交流群：554855843