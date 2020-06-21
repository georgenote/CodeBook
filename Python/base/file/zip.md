# 零、前情提要

最近编写自动化邮件监控时, `google`的自动报表是通过附件`zip`格式传输的,在解压转化成`csv`格式的时候编码有问题,由于源码编译成了`cp437`,导致最终`python`内编码显示会成`\x00`样式的`unicode`编码形式, 无法正常读取. 因此进行记录已被不时之需

# 一、基本用法

```python
# 0. 导入
## pip install zipfile
import zipfile

# 1. 解压缩
zfile = zipfile.ZipFile('report.zip','r')
zfile.extractall(path) # .extractall 解压内部所有文件到特定目录.

# 2. 读取压缩内文件
## 基础准备
zfile = zipfile.ZipFile('report.zip','r')

## 压缩文件内文件名
zfile.namelist ## list类型

## 读取文件内容
zfile.read(zfile.namelist()[0]) ## 读取第一个文件内的内容

# 3. 压缩
## 创建压缩文件
zfile = zipfile.ZipFile('output','w')

## 往压缩文件中写入文件
zfile.write('outputfile.log')
zfile.write('outputfile.csv')

# 4. 编码
## 编码问题是zipfile中一个非常大的问题,由于外部文件的不可预期性
## zipfile源编码很大可能会将文件按`cp437`进行编码
## 很大可能会进行奇怪编码,此时`utf-8`和`gbk`都无法解析.
import pandas as pd
data = pd.read_csv('report.csv',encoding='utf-16-le',sep='\t\n') ## sep按需要使用
other. 其他用法
## 基础准备
zfile = zipfile.ZipFile('report.zip','r')
## 显示文件信息
zfile.infolist()
## output: [<ZipInfo filename='report.txt' compress_type=deflate filemode='-rw-r--r--' file_size=0 compress_size=2>]
## 关闭压缩文件 / 打开后务必关闭
zfile.close()
```
