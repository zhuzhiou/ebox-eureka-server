# eureka-server单机部署

## 构建Docker镜像

使用了maven的profie，默认激活了本地开发环境，如果打包部署到生产环境，需要指定profile。

```
mvn clean package dockerfile:build -Pprod
```

## 使用阿里云容器镜像服务

```
docker login --username=zhuzhiou@qq.com registry.cn-shenzhen.aliyuncs.com
```

## 将镜像推送到Registry

```
docker push registry.cn-shenzhen.aliyuncs.com/ebox/eureka-server:latest
```

## 拉取镜像

```
docker pull registry.cn-shenzhen.aliyuncs.com/ebox/eureka-server:latest
```

## 运行应用程序

```
docker run -it --rm --name eureka-server --hostname eureka1.cn-guangzhou-1.ebox --dns 192.168.0.86  -p 8761:8761 registry.cn-shenzhen.aliyuncs.com/ebox/eureka-server:latest
```

## 知识点

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

3、defaultZone默认为http://localhost:8761/eureka，如果主机名与该域名不一致，会设置defaultZone指向的主机为Replicas。因此需要设置defaultZone，确定serviceUrl的域名与主机名一致。

```yaml
eureka:
  instance:
    ip-address: 172.16.29.116
  client:
      service-url:
        defaultZone: http://${eureka.instance.ip-address}:8761/eureka/
```

4、全部配置加在一起

```yaml
eureka:
  instance:
    prefer-ip-address: true
    ip-address: 172.16.28.129
  client:
    service-url:
      defaultZone: http://${eureka.instance.ip-address}:8761/eureka/
    fetch-registry: false
    register-with-eureka: false
```