# 零、前情提要
日常工作中一般会使用os库进行一些基础配置, 因此将os库的基础操作进行记录.

# 一、基础用法
```python
## 1. 改变当前工作目录
os.chdir(path)

## 2. 查看当前工作目录
os.getcwd()

## 3. 返回指定文件夹包含的文件或者文件夹的名字的列表
os.listdir(path)

## 4. 创建文件夹
os.mkdir(path)

## 5. 打开文件
os.open()

## 6. 删除文件
os.removed()

## 7. 重命名
os.rename(src,dst)

## 8. 删除空目录
os.rmdir(path)
```

# 二、path模块
```python
## 1. 返回绝对路径
os.path.abspath(path)

## 2. 返回文件名
os.path.basename(path)

## 3. 返回文件路径
os.path.dirname(path)

## 4. 判断路径存在
os.path.exists(path)

## 5. 返回真实路径
os.path.realpath(path)

## 6. 把路径分割成dirname和basename, 返回一个元组
os.path.split(path)
```
