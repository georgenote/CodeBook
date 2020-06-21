#  零、前情提要

日常工作中因为本地使用的是window10系统, 而crontab使用的是linux系统, 想使用Python3搭建impyla链接公司Hive库, 但一直出现安装问题, 因此在这里记录下相关安装过程已被之后使用.

Python连接Hive的方法有:
* ThriftHive  ## Thrift是Hive连接外部的一个组件
* pyhs2 driver ## 需开启hiveserver2服务
* PyHive ## Linux推荐
* impyla ## Windows推荐

## 一、安装方法
曾经由此安装时使用的各最新版第三方库导致安装失败, 因此不建议所有库都使用最新第三方库.

### 1.1 Windows10

```shell
# 安装依赖 
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple bit_array==0.1.0 ## -i 使用清华源进行下载, 国内还是清华源会快很多.
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple thrift==0.9.3
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple thriftpy==0.3.9
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pure_sasl==0.6.1
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --no-deps thrift-sasl==0.2.1

# 安装impyla
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple impyla==0.14.1
```

```python
# 连接Hive
from impala.dbapi import connect
conn = connect(host=host, port=port, database=database, user=user, password=password, auth_mechanism="PLAIN") ## auth_mechanism是Hive的配置设置
cur = conn.cursor()
cur.execute('show tables')
print(cur.fetchall())
```

#### 注意事项
1. 重点不要安装`sasl`, 否则会提示报错.卸载方法: `pip uninstall sasl`

2. 在安装过程中, 如果出现包安装失败的情况, 可以下载whl包进行安装, 下载链接:https://www.lfd.uci.edu/~gohlke/pythonlibs/ 安装方式: `pip install 绝对地址.whl`

3. 如果在安装过程中, 出现任何包安装失败的问题, 可以先将之前所有安装过的包统统卸载, 再按顺序依次安装一遍.

4. `Linux`建议采用`pyhive`形式连接
```shell
sudo yum install cyrus-sasl-devel
sudo yum install gcc-c++
pip3 install sasl
pip3 install thrift
pip3 install thrift-sasl
pip3 install PyHive
```

#### 问题集锦
1. `impyla (0.14.0) ERROR - 'TSocket' object has no attribute 'isOpen'`

这个问题的原因是`thrift-sasl`版本过高导致的，将其换成0.2.1的版本即可`pip install thrift-sasl==0.2.1`

2. `thriftpy2.protocol.exc.TProtocolException: TProtocolException(type=4)
`

这是由于`auth_mechanism`设置的问题导致的，加上或将其改为`auth_mechanism="PLAIN"`即可

3. `TypeError: can’t concat str to bytes`

修改`thrift-sasl`的源代码文件:`thrift-sasl init.py`，在第94行之前加上以下语句即可：
```python
if (type(body) is str):
    body = body.encode()
```

4. `thrift.transport.TTransport.TTransportException: Could not start SASL: b'Error in sasl_client_start (-4) SASL(-4): no mechanism available: Unable to find a callback: 2'`

这是`Windows`下采用`pyhive`连接方式提出的错误，正如前言所述，可能需要修改对应的配置文件，也可能`sasl`根本就不支持`Windows`，建议改用`impyla`形式连接

5. `thriftpy.parser.exc.ThriftParserError: ThriftPy does not support generating module with path in protocol 'c'`
修改`thriftpy`包下`\parser\parser.py`中第488行代码，将`if url_scheme == '':`修改为`if len(url_scheme) <=1:`即可

### 1.2 Linux
`Linux`包主要安装`impyla`相对容易, 但是需要安装几个依赖环境.
```shell
# 安装系统依赖
apt-get install -y --no-install-recommends gcc libsasl2-dev python3.x-dev ##其中python3.x-dev需要看在Linux上运行相关程序的具体python版本
```

```python
# 安装impyla
pip install bre bit_array==0.1.0
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple thrift==0.9.3
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple thriftpy==0.3.9
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pure_sasl==0.6.1
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --no-deps thrift-sasl==0.2.1
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple impyla==0.14.1
```
