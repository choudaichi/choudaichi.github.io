# docker中oracle导入dmp文件
```bash
# 将dmp文件拷贝到docker中，冒号前为容器id
sudo docker cp /mnt/d/develop/miam_prod20220304.dmp 55d8dc2f1b89:/tmp

# 打开docker bash
sudo docker exec -it oracle11g bash

# 导出参数
export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
export PATH=$ORACLE_HOME/bin:$PATH
export ORACLE_SID=helowin
export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK #选择和数据库一致的字符集

# 导入语句
imp miam_prod/miam_prod@helowin fromuser=miam_prod touser=miam_prod file=/tmp/miam_prod20220304.dmp log=/tmp/imp.log feedback=1000

```
