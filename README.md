## System Setting 
Set enough virtual memory to secured ElasticSearch before we started:  
- option 1: `sysctl -w vm.max_map_count=262144` that is only valid for current session.  
- option 2: update `vm.max_map_count` setting in `/etc/sysctl.conf`
- check the result: `sysctl -p`

## Checkout Branch
We have provided three branches now, choose one you needed:
- Kibana(official dashboard of Elasticsearch)
- Kibiter(soft fork of Kibana which provides several features not present in Kibana)
- Grimoirelab(GrimoireLab is a CHAOSS toolset for software development analytics)

## docker-compose
Elasticsearch从6.8开始允许免费用户使用X-Pack的安全功能，从而实现基础的安全认证，我们基于docker-compose来搭建该环境：
相关文件已在项目中配置，部分设置需要容器中进行：

- 启动容器
    ```bash
    $ docker-compose -f docker-compose-elasticsearch-secured.yml up -d
    ```
- 查看节点状态/登陆kibana  
    可以直接访问对应地址查看节点状态：
    ```bash
    $ curl -XGET --user user:pwd 'http://ip_addr:9200/'
    ```
    或者网页登陆 `http://ip_addr:5601`

    状态输出：
    ```bash
    {
    "name" : "Two77A7",
    "cluster_name" : "docker-cluster",
    "cluster_uuid" : "8PuUA-0bTUGbc4wP7cjWFA",
    "version" : {
        "number" : "6.8.6",
        "build_flavor" : "default",
        "build_type" : "docker",
        "build_hash" : "3d9f765",
        "build_date" : "2019-12-13T17:11:52.013738Z",
        "build_snapshot" : false,
        "lucene_version" : "7.7.2",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
        },
    "tagline" : "You Know, for Search"
    }
    ```
