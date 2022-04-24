## docker-compose
Elasticsearch从6.8开始允许免费用户使用X-Pack的安全功能，从而实现基础的安全认证，我们基于docker-compose来搭建该环境：
相关文件已在项目中配置，部分设置需要容器中进行：

- 启动容器
    ```bash
    $ docker-compose -f docker-compose-elasticsearch-secured.yml up -d
    ```

- 生成密码  
1. 进入elasticsearch容器(多节点的话，任意一台都是可以的)
    ```bash
    $ docker exec -it elasticsearch /bin/bash
    ```
2. 可以通过-h查看相关帮助
    ```bash
    $ ./bin/elasticsearch-setup-passwords -h
    ```
3. 允许使用`auto`来自动生成密码和`interactive`指定密码：
    ```bash
    ./bin/elasticsearch-setup-passwords interactive
    ```


- 修改kibana的配置文件  
修改`./kibana.yml`文件, 将`elasticsearch.password`替换成上一步设置elastic密码，重启kibana
    ```bash
    $ docker-compose restart kibana
    # 重启多个节点
    # docker-compose -f docker-compose-elasticsearch-secured.yml restart
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
    
- Dashboard  
 推荐使用[kibiter](https://github.com/chaoss/grimoirelab-kibiter)构建你的数据看板：  
 Kibiter is a custom soft fork of Kibana which empowers GrimoireLab Panels with metrics & data visualizations.
