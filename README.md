# eureka-server单机部署

1、使用ip地址而不是主机名

```yaml
eureka:
  instance:
    prefer-ip-address: true
```

2、单机部署，不向服务注册中心注册自己

```yaml
eureka:
  client:
    fetch-registry: false
    register-with-eureka: false
```

3、使用默认配置，经常会将本机也当成Replicas，单机部署不存在Replicas的概念。因此设置实例的ip与客户端的ip一致，spring cloud能猜得到这个地址并不是Replicas。

```yaml
eureka:
  instance:
    ip-address: 172.16.29.116
  client:
      service-url:
        defaultZone: http://eureka:eureka@${eureka.instance.ip-address}:8761/eureka/
```

4、全部配置加在一起

```yaml
eureka:
  instance:
    prefer-ip-address: true
    ip-address: 172.16.28.129
  client:
    service-url:
      defaultZone: http://eureka:eureka@${eureka.instance.ip-address}:8761/eureka/
    fetch-registry: false
    register-with-eureka: false
```

## 打包生产环境

使用了maven的profie，默认激活了本地开发环境，如果打包部署到生产环境，需要指定profile并且提供-Dsecurity.user.password参数

```
# mvn clean package dockerfile:build -Pprod -Dsecurity.user.password=123456
```