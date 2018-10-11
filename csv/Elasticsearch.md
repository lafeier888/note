Elasticsearch

# Get ting start

查看java版本和jdk安装路径

``` 
java -version
echo $JAVA_HOME
```

下载地址 https://www.elastic.co/downloads/elasticsearch

解压即可

运行：

```
cd elasticsearch-6.0.1/bin
./elasticsearch
```

启动报错：can not run elasticsearch as root

解决：创建新用户

```shell
groupadd elkgroup

useradd elkuser -g elkgroup -p elkuser

chown -R elkuser:elkgroup ./elasticsearch-6.4.1/

su elkuser:elkgroup 
```
基本使用
```shell
curl -XGET  'localhost:9200/_cat/health?v&pretty'  查看集群状态
curl -XGET 'localhost:9200/_cat/nodes?v&pretty'	查看集群节点
curl -XGET 'localhost:9200/_cat/indices?v&pretty'	列出所有索引
curl -XPUT 'localhost:9200/customer?pretty&pretty'	创建索引customer
在customer索引中创建external类型的id为1的文档（其实就是索引文档的意思），注意新版本的需要加header
curl -H "Content-Type: application/json" -XPUT 'localhost:9200/customer/external/1?pretty&pretty' -d'
{
  "name": "John Doe"
}'
查看文档
curl -XGET 'localhost:9200/customer/external/1?pretty&pretty'
curl -XDELETE 'localhost:9200/customer?pretty&pretty' 删除索引
修改文档
curl -H "Content-Type: application/json" -XPUT 'localhost:9200/customer/external/1?pretty&pretty' -d'
{
  "name": "lafeier"
}'
```

