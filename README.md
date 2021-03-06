# 简化版Deployment实现

## 文章
[如何从零开始编写一个CRD](https://mp.weixin.qq.com/s/z_xM8QqpRUASDM1UyMzEeA "With a Title"). 

## 目的
这是一个简化版的Deployment的实现，实现了部分Kubernetes中Deployment的功能。

## 测试用例
- 启动后自动创建出一个MyDeployment的CRD
    - 【触发】启动应用
    - 【期望】可以看到创建出来的CRD
    - 【测试】kubectl get CustomResourceDefinition -o yaml 
- 创建一个MyDeployment: Nginx实例
    - 【触发】kubectl create -f my-deployment-instance.yaml
    - 【期望】可以看到级联创建出来的3个pod
    - 【测试】kubectl get pods 
- 手工删掉一个pod
    - 【触发】kubectl delete pods $pod_name 
    - 【期望】pod被重建
    - 【测试】kubectl get pods -w
- 暴露一个服务
    - 【触发】kubectl create -f my-deployment-service.yaml
    - 【期望】可以通过curl来访问nginx服务
    - 【测试】minikube service my-nginx-app --url 然后 curl
- 更新镜像
    - 【触发】kubectl replace -f my-deployment-instance-update-image-1.9.1.yaml
    - 【期望】pod的nginx版本被更新为1.9.1
    - 【测试】kubectl get pods -o yaml
- 扩容
    - 【触发】kubectl replace -f my-deployment-instance-update-scaleup-1.9.1.yaml
    - 【期望】pod被扩容到5个
    - 【测试】kubectl get pods
- 缩容
    - 【触发】kubectl replace -f my-deployment-instance-update-scaledown-1.9.1.yaml
    - 【期望】pod被缩容到2个
    - 【测试】kubectl get pods
- 扩容并更新镜像
    - 【触发】kubectl replace -f my-deployment-instance-update-image-and-scaleup-1.14.yaml
    - 【期望】pod被扩容5个，且nginx版本被更新为1.14
    - 【测试】kubectl get pods 然后 kubectl get pods -o yaml
- 删除一个MyDeployment
    - 【触发】kubectl delete mydeployments my-nginx-app
    - 【期望】MyDeployment被删掉，并且关联的pod也被级联删掉
    - 【测试】kubectl get mydeployments 然后 kubectl get pods
- 查看状态(TODO)
- 回滚(TODO)
- 状态更新【current，update-to-date，available】(TODO)
- describe EVENTS(TODO)