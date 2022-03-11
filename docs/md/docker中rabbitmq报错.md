# 解决docker中rabbitmq报错Management API returned status code 500
> rabbitmq可视化管理页面不显示图表，而且会报告代码为500的错误
```bash
# 进入rabbitmq容器的bash
sudo docker exec -it rabbit /bin/bash

# 进入bash后依次执行下面两行
cd /etc/rabbitmq/conf.d/
echo management_agent.disable_metrics_collector = false > management_agent.disable_metrics_collector.conf

# 退出并重启docker容器
sudo docker restart rabbit
```
