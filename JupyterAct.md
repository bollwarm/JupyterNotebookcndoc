#  装扮你的Jupyter
又到摆脱重复工作，换个心情，然而并没有软用的时间了。这次，教大家如何搭建一个好看的jupyter环境。
## 安装Jupyter
先来展示一下我的环境
```
python: 3.5.*
macos: 10.12.4
```
安装Jupyter的过程只需安装`Anaconda`即可。
测试一下初始设置：
```
jupyter notebook
```
## 配置ipython
首先，如果每次你打开一个`nb（notebook）`时，如果都需要载入一些模块，一个很好地方法就是配置ipython的配置文件，可以直接使用以下命令创建配置文件：
```
ipython profile create
```
此时你会在~/.ipython/profile_default/目录中获得下面两个文件：
ipython_config.py：打开任意ipython kernel时都会运行
ipython_notebook_config.py：打开notebook时会运行
配置方式是在所需要的配置文件中先键入：
```
c = get_config()
```
然后就可以通过修改c的属性来控制所有的配置。
显然，对大多数分析场景，numpy, scipy, pandas是肯定要载入的，因此，写到配置中即可：
`c.InteractiveShellApp.exec_lines = [
        "import pandas as pd",
        "import numpy as np",
        "import scipy.stats as spstats",
        "import scipy as sp",
        ]`
## 配置matplotlib
还有一个常用功能就是matplotlib。matplotlib在notebook中需要使用
```
%matplotlib inline
```
才可默认在notebook中显示图像，一个简单地方法就是在配置文件中加入，
c.IPKernelApp.matplotlib = 'inline'
当然，默认也需要载入matplotlib
```
c.InteractiveShellApp.exec_lines = [
        "import pandas as pd",
        "import numpy as np",
        "import scipy.stats as spstats",
        "import scipy as sp",
        "import matplotlib.pyplot as plt"
        ]
```
当然，也可以更多。但这样可能会影响初始化notebook和ipython shell的速度，这个请大家自己权衡。
## matplotlib显示中文
此外，单独拎matplotlib出来的另一个原因是，matplotlib还有一个中文显示的问题。
### 首先，解决编码问题。
python 2.7.*的解决方案是，在配置中加入：
```
import seaborn as sns
import sys# print sys.getdefaultencoding()# ipython notebook中默认是ascii编码 
reload(sys)
sys.setdefaultencoding('utf8')
```
python 3.*出于某些原因，不建议通过sys模块修改编码。
解决方案是，在shell的配置中重新设置配置变量（bash的话设置文件.bashrc，zsh则设置文件.zshrc）。方法是末尾添加：
```
export PYTHONIOENCODING="utf8"
```
当然另一个方法是在启动notebook时使用
```
PYTHONIOENCODING="utf8" & jupyter notebook
```
### 第二个是修改matplotlib的默认字体。
首先我们来看可以使用的字体
```
import matplotlib.font_manager
fonts = matplotlib.font_manager.findSystemFonts()
l = []
for f in fonts:
    try:
        font =matplotlib.font_manager.FontProperties(fname=f)
        #print(font.get_family())
        l.append((f, font.get_name(), font.get_family(), font.get_weight()))
    except:
        pass
df = pd.DataFrame(l, columns=['path', 'name', 'family', 'weight'])
df
```
你应该看到下面这样的表格：
![](_v_images/1553686725_8473.png)

然后找到支持中文的字体名，然后设置matplotlib的默认字体：
```
import matplotlib as mpl
mpl.rc('font', family='Noto Sans CJK SC') 
```
当然，你可以添加到刚才的配置中，或者采用这个博客的方法。
此外，如果你使用seaborn的话，seaborn在设置配置时可能会覆盖掉matplotlib，此时采用以下代码即可：
```
import seaborn as sns
sns.set_style('ticks',
              {
                    'font.family': ['Noto Sans CJK SC'],
    })
```
但是，该语句不建议写在配置中，因为经常需要修改，可能会覆盖之前的配置。
### matplotlib在Retina屏幕中显示模糊问题
直接使用下面语句即可，
```
%config InlineBackend.figure_format = 'retina'
```
当然也可在配置中直接加入
```
c.InlineBackend.figure_format = 'retina'
```
## 修改notebook样式
默认的notebook可以逼你心中大喊WTF，这时候你需要一点CSS技能，修改`~/.jupyter/custom/custom.css`的内容。
个人认为最需要修改的内容包括
notebook的默认宽度：notebook默认比较宽，markdown文字会显得比较少，如果需要对外展示，文字部分会过少。
notebook的代码字体
修改规则是：

```
pre.CodeMirror-line {
    font-family: 'BlinkMacSystemFont', 'Lucida Grande', 'Segoe UI', Ubuntu, Cantarell, sans-serif
}
.output_subarea.output_text.output_result>pre {
    font-family: 'BlinkMacSystemFont', 'Lucida Grande', 'Segoe UI', Ubuntu, Cantarell, sans-serif
}

.output_subarea.output_text.output_stream.output_stdout>pre {
    font-family: 'BlinkMacSystemFont', 'Lucida Grande', 'Segoe UI', Ubuntu, Cantarell, sans-serif
}

#notebook-container {
    max-width: 830px;
    padding: 40px;
}
```

## 安装Jupyter常用插件
这里推荐两个jupyter插件:
插件管理器`jupyter notebook extensions`

然后你就可以在jupyter主页里找到下面的标签页管理插件了：
![](_v_images/1553686750_14006.png)
## jupyter Dashboard
如果你的jupyter服务是搭建在主机上，并且平时和业务人员想用notebook地址的方式交付，jupyter dashboard插件是一个不错的选择。
安装方法和github地址在这里。
原本效果如下：
![](_v_images/1553686763_21444.png)
点击如下红色设置，并点击黄色按钮后
![](_v_images/1553686783_12701.png)
就可得到如下的报告形式（删去了业务人员不想查看的代码），然后就可以粘贴连接交付报告了：
![](_v_images/1553686796_19703.png)
切换成dashboard模式可以拖拽相关方格来设置位置。
## 安装R kernel
R kernel安装方式有两个：
### 通过conda安装
```
conda create -n R-Env -c r r-essentials
source activate R-Env
conda install jupyter
```
然后在R中配置
```
install.packages(c('repr', 'IRdisplay', 'evaluate', 'crayon', 'pbdZMQ', 'devtools', 'uuid', 'digest'))
devtools::install_github('IRkernel/IRkernel')
IRkernel::installspec()
```
建议一定要新建环境，不然会和你之前安装的R冲突。
当然，我不建议这种安装方式，原因是：
1.	不是很多人想在电脑里有两个R环境
2.	在jupyter notebook中不配置默认镜像，是没法选择镜像的，这导致没法再notebook中直接安装R包，当然你也可以配置好默认CRAN镜像，但这样显然很麻烦，切换网络环境后也很难调整
3.	可能你在旧环境中已经安装了大量包，这样子迁移成为问题
4.	你必须在这个新环境中启动jupyter
### 直接使用原本安装
直接在R环境中使用以下语句
```
install.packages(c('repr', 'IRdisplay', 'evaluate', 'crayon', 'pbdZMQ', 'devtools', 'uuid', 'digest'))
devtools::install_github('IRkernel/IRkernel')
IRkernel::installspec()
IRkernel::installspec(user = FALSE)
```
## 设置Jupyter服务配置
这里请做个区别：ipython是负责和python交互的部分，jupyter是作为服务的部分。因此所有服务配置都要在~/.jupyter中进行，而和python、模块相关的配置都要在~/.ipython中。
这里主要配置的有ip和默认文件夹。
首先，生成配置文件：
```
jupyter notebook --generate-config
```
现在~/.jupyter/内就生成jupyter_notebook_config.py文件。
再次我们设置ip，在其中添加，这样就可以外网访问。
```
NotebookApp.ip = '127.0.0.1'
```
最后，加上默认启动位置，这样，在任何工作目录下都能保证，notebook的启动位置一致。
```
NotebookApp.notebook_dir = '/jupyter'
```
