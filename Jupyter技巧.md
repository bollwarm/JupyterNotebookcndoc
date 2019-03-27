# Jupyter技巧
Jupyter NoteBook 是功能强大的Python交互IDE，深受数据分析师和算法工程师的热爱。Jupyter NoteBook 在综合使用文字、代码、图片等多种元素展示设计者的想法方面有着美妙的用户体验。而其自带的一些常用Magic Command 可以让它变得更加得心应手。
## 目录管理
### 查看当前工作目录
path = %pwd
path
### 更改工作目录
```
%cd .. 
%cd "C:\\\user\\PythonFiles"
```
### 查看目录文件列表
```
%ls
```
## 文件管理
### 写入文件
```
%%writefile helloworld.py
print('Hello world!')
print('Hello China!')
```
### 运行脚本文件
```
%run helloworld.py
```
![](_v_images/1553678272_30066.png) 
### 复制文档内容
执行前：
```
%load helloworld.py
```
执行后：
```
# %load helloworld.py
print('Hello world!')
print('Hello China!')
```
## 变量管理
### 查看当前变量
```
x = 1
y = [1,2,3,4]
%whos
```
### ![](_v_images/1553678415_26100.png)
### 清除变量
清除全部变量
```
%reset
```
 ![](_v_images/1553678478_5502.png)
### 清除某个变量
```
%del x
```
## 测算运行时间
### 测算运行时间
使用`magic command`
%%time
x = [i**2 for i in range(10000)]
y = [i**2 for i in x]
 ![](_v_images/1553678528_16672.png)
使用python代码测算
```
import time
tic = time.time()
x = [i**2 for i in range(10000)]
y = [i**2 for i in x]
toc = time.time()
print('using time:'+str(toc - tic)+'s')
```
![](_v_images/1553678572_5934.png)

