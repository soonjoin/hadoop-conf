hadoop三个节点集群的最小配置

集群说明:
hadoop-a: master节点
    [HDFS]NameNode
    [HDFS]SecondaryNameNode
    [YARN]ResourceManager
hadoop-b: worker节点1
    [HDFS]DataNode
    [YARN]NodeManager
hadoop-c: worker节点2
    [HDFS]DataNode
    [YARN]NodeManager

配置步骤: 
    1. 配置hosts,使3个节点能通过主机名互相访问。
        例如：
        192.168.106.25 testserver-25
        192.168.106.27 testserver-10627
        192.168.106.28 testserver-10628

        192.168.106.25 hadoop-a
        192.168.106.27 hadoop-b
        192.168.106.28 hadoop-c
        注意，其中testserver-开头的是这几台服务器的hostname，因为没有修改上述服务器的hostname则需按上述配置。如果直接将几台服务器的hostname修改为hadoop-a/b/c则不可能需要（或者把hadoop-a/b/c的hosts记录放到hosts文件的开头估计也行）。注意查看log就能很快定位这类问题。
    2. 安装pdsh，并配置各主机能够通过ssh免密码访问(包括ssh localhost也要求能免密码访问)
    3. 设置环境变量（在/etc/profile.d/下新建.sh文件配置或~/.bash_profile中配置均可），我只设置了HADOOP_HOME，然后在PATH中增加了hadoop的bin目录集群就能正常运行mapreduce-examples的wordcount了。
        export HADOOP_HOME="/opt/hadoop-3.2.1"
        export PATH="$HADOOP_HOME/bin:$PATH"

注意事项：
    如果每个机器的JDK安装不一样，则要注意每个节点的hadoop-env.sh中的JAVA_HOME配置。在JDK和hadoop的安装路径一致情况下，各节点的配置文件相同。