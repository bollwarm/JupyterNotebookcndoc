# Sublime Text 3 中使用 Jupyter Notebook 
每当看到人家用 Sublime Text 3 (alias subl) 或 Atom 里华丽多彩的编程界面，在庞大的一套 project 中游刃有余时，再瞅瞅自己总处在边看教程边学用 Jupyter Notebook (alias jpt) 的状态，block by block 运行，不免要妄自菲薄地感慨一番。
`subl` 和` jpt `都是公认的逆天神器， `subl `编辑脚本那是嗖嗖地快，而在 Git 上找到的很多图文并茂的教程都是用`jpt`演示的，可惜很久很久都没有找到一个能在 subl 里直接演示和运行.ipynb的插件，只能列几个稍微能拉近它俩间联系的小工具。
 ![](_v_images/1553679844_13075.png)
## Hermes
这是一个 subl 插件，直接扔给你 jupyter notebook 的高端体验：
 
![](_v_images/1553679854_19046.png)
尽管有一点点“我在用 jpt”的错觉，但还是很感谢开发者的善解人意。在subl中运行`Package Control: Install Package`, 然后选择 Hermes。运行时：
-Hermes: connect kernel --> New kernel -- 选择一个内核
-Hermes: List Kernels -- 有现成的
-Hermes: Execute Block or Hermes: Execute cell -- 运行代码块
-Hermes: Get Object Inspection -- 还可以查看变量
-Hermes: Restart Kernel, Hermes: Shutdown Kernel, Hermes: Interrupt Kernel 
### 优点：
-可以像 jpt 一样实时查看输出，双屏对比
-暗背景的 color theme 和 highlight syntax 搭配，提高注意力！
### 缺点：
-储存和打开的时候仍然是.py文件，不是直接与.ipynb交互，因为很多 tutorials 下载下来仍然是 jpt 的格式，转换不方便
-不能用 jpt 的快捷键 shortcuts
-不能用 jpt 的插件 extensions
## 还是很想很想用jpt怎么办
双击打开.ipynb的“自动操作” 小程序：

打开Automator/自动操作，新建 应用程序 

选取资源库 --> 实用工具 --> 运行AppleScipt，用下面的代码填充内容：

```
on run {input}
   set the_path to POSIX path of input
   set cmd to "jupyter notebook " & quoted form of the_path
   tell application "System Events" to set terminalIsRunning to exists application process "Terminal"
   tell application "Terminal"
      activate
      if terminalIsRunning is true then
         do script with command cmd
      else
         do script with command cmd in window 1
      end if
   end tell
end run
```
取个喜欢的名字，保存成.app文件，拖到Applications里面

找.ipynb文件，在“查看简介”中修改打开方式就可以实现双击打开

习惯问题，也可以把terminal改成iTerm来打开，反正结果都一样

### 缺点：
每次都会从terminal重新启动内核，会不会占用很大的内存呢？（Hermes似乎也是启动新内核，可能需要学习一些新技巧或者培养使用习惯）

## “新手流程” 

### runipy
在 terminal中运行
```
# https://github.com/paulgb/runipy
pip install runipy
runipy mynotebook.ipynb
```
### 优点：
-相比python script.py，在运行代码的时候把代码也打印出来，方便查看运行进度。
当然一个最直接的方法是：-
`ipython notebook notebook_name.ipynb`
### 一条指令把.ipynb转换成.py脚本
```
jupyter nbconvert --to script --execute --stdout mynotebook.ipynb | python
```
### jupyterthemes
大神之作！直接把jpt改造成看起来“高大上”的 editor ！参考点这里
```
pip install jupyterthemes
```
调整时命令行用jt，便捷 ：
```
# dark
jt -t onedork -fs 95 -altp -tfs 11 -nfs 115 -cellw 88% -T
# light
jt -t grade3 -fs 95 -altp -tfs 11 -nfs 115 -cellw 88% -T
```
暗色monokai风格，ubuntu字体，宽带调整为88%的宽屏模式
 
```
jt -t monokai -f ubuntu -cellw 88%
```
```
# 恢复成原有的样式
jt -r
```
### 使用VSCode内置的ipython
在代码的上面一行加上 #%% 就会看到 Run cell的按钮，点击之后就能运行直到下一个#%%的代码了。爽得不行。

 ![](_v_images/1553680173_13272.png)

## 其他
就是用 `Pineapple` 和` nteract `这两个编辑器。


顺便找到一波非常酷炫的 sublime text 3 插件 以及那些在jpt中6到飞起的subl操作！
写几个我自己用的比较多的用法。
```
magic命令：%who str
```

在jupyter下做一些实验的时候，有的时候会定义多个变量，这个命令可用来查看当前所有定义过的变量名。

markdown下可以方便编辑LaTeX公式，如：`\\( P(A \mid B) = \frac{P(B \mid A) \, P(A)}{P(B)} \\)`
  
远程服务器上配置好端口、密码等，开启jupyter服务，在自己本子的浏览器上输入"ip:port"，就可以使用服务器的计算资源了，还是很棒的。
jupyter和pyspark的连接，实现在web端进行一些大数据的分析。

