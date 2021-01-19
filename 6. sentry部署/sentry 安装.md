1. 安装机器内存需要>2.4G
2.环境要求
Docker 19.03.6+
Compose 1.24.1+

2.1安装必要的工具包
yum install net-tools vim lrzsz unzip wget -y

2.2 移除系统中存在的Docker
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
				  
3.安装docker-ce
yum localinstall docker-ce-19.03.6-3.el7.x86_64.rpm \
	docker-ce-cli-19.03.6-3.el7.x86_64.rpm \
	containerd.io-1.2.10-3.2.el7.x86_64.rpm -y

3.1 启动Docker服务
chkconfig docker on
service docker start
service docker status

3.2 配置镜像加速器(所有node节点)
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://plqjafsr.mirror.aliyuncs.com"]
}
EOF

#启动服务
systemctl daemon-reload
systemctl restart docker


4 安装python3
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
yum install python36 -y

5 移除之前的python
mv /usr/bin/python /usr/bin/python2_bak


5.1 修改yum版本为2.7
vi /usr/bin/yum 
vi /usr/libexec/urlgrabber-ext-down


#建立软链接
ln -s /usr/bin/python3.6 /usr/bin/python
ln -s /usr/bin/pip3 /usr/bin/pip

pip3 install cryptography -i https://pypi.tuna.tsinghua.edu.cn/simple


6 安装 docker-compose
pip install docker-compose -i https://pypi.tuna.tsinghua.edu.cn/simple
#pip3 install sentry-plugin-dingding -i https://pypi.tuna.tsinghua.edu.cn/simple

6.1 查看docker-compose 版本
docker-compose version


7 安装Sentry
yum install git -y

7.1 下载版本库
cd /opt
git clone https://github.com/getsentry/onpremise.git




#安装
cd onpremise

#启动安装
./install.sh

#创建用户与密码


#注意内存大小
[root@demo onpremise]# ./install.sh
Checking minimum requirements...
FAIL: Expected minimum RAM available to Docker to be 2400 MB but found 1991 MB

8启动
通过 IP:9000 即可成功访问，用之前创建的账号即可登陆。
docker-compose up

#查看端口
[root@demo onpremise]# netstat -anltup | grep 9000
tcp6       0      0 :::9000                 :::*                    LISTEN      12446/docker-proxy

8.1 docker-compose 命令帮助
docker-compose build	构建容器
docker-compose restart	重启
docker-compose ps		查看当前服务
docker-compose up		以日志方式启动服务
docker-compose up -d	后台启动服务
docker-compose run --rm web upgrade	运行新的迁移


#集成springcloud
首先选择一个项目类型，这里选择Java
接着，选择项目名称，我们这里和微服务的名字保持一致passport

Install the SDK via Maven or Gradle:

<dependency>
    <groupId>io.sentry</groupId>
    <artifactId>sentry</artifactId>
    <version>3.1.0</version>
</dependency>


#main 集成
import io.sentry.Sentry;
public class MyClass {
  public static void main(String... args) {
    Sentry.init(options -> {
      options.setDsn("http://360422b677ea4bb190f2c93d17befe59@192.168.91.13:9000/2");
    });
  }
}

#异常
try {
  System.out.println(1/0);
} catch (Exception e) {
  Sentry.captureException(e);
}


配置钉钉通知
 vim sentry/requirements.txt 

# Add plugins here
sentry-plugin-dingding

重建下

docker-compose build
docker-compose up -d
重新构建docker时，可以在日志中看到钉钉插件相关的日志：

#修改dsn地址
passport-demo/src/main/java/com/exa/de/PassportDemoApplication.java



复制代码
[root@localhost onpremise]# docker-compose build

配置邮件通知
将邮件的相关信息配置到config.yml文件中