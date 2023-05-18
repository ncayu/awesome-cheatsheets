## Vim 编译器
Vim是从vi发展出来的一个文本编辑器。其代码补完、编译及错误跳转等方便编程的功能特别 丰富，在程序员中被广泛使用。

### 如何安装vim

```bash
yum -y install vim
```

### 编译安装

```bash
wget ftp://ftp.vim.org/ftp/pub/vim/unix/vim-6.4-src2.tar.gz
tar xvf vim-6.4-src2.tar.gz
cd vim-6.4-src2
./configure --disable-selinux --enable-cscope
make
make install
```

### Windows安装（GVIM）
```bash
https://www.vim.org/download.php#pc
https://github.com/vim/vim-win32-installer/releases
```

## 如何使用Vim

### vim常用操作

```bash
:set nu			设置行号
:set nonumber		取消显示行数
:=				查看总行数
ctrl+g			显示文件名、当前的行号、文件的总行数和文件位置的百分比
:wq				保存文件并退出,`还可以使用 ZZ 或 :x`
:q!				不保存文件,强制退出
i				在光标前插入文本
n%:				到文件n%的位置
gg				移到文件的首行
G				移到文件的末行
^				移到当前行首
$				移到当前行尾
/something:		在后面的文本中查找something
:n,md			删除第n行到第m行的数据（删除数据包含n行和m行）
:%s/old/new/g	用new替换文件中所有的old。
```
一般情况下掌握以上的命令就满足日常使用啦,vim命令是不是很简单呀！

### 模式切换

正常模式：可以使用快捷键命令，或按:输入命令行。
插入模式：在正常模式下，按i、a、o等都可以进入插入模式。
可视模式：正常模式下按v可以进入可视模式， 在可视模式下，移动光标可以选择文本。总是整行整行的选中。ctrl+v进入可视块模式。
替换模式：正常模式下，按R进入。

### 文档操作

```bash
:e file			关闭当前编辑的文件，并开启新的文件
:e! file			放弃对当前文件的修改，编辑新的文件
:e+file			开始新的文件，并从文件尾开始编辑
:e+n file			开始新的文件，并从第n行开始编辑
:enew			编译一个未命名的新文档
:e				重新加载当前文档
:e!				重新加载当前文档，并丢弃已做的改动
:e#或ctrl+^		回到刚才编辑的文件，很实用
:f或ctrl+g			显示文档名，是否修改，和光标位置
gf				打开以光标所在字符串为文件名的文件
:w				保存修改
:n1,n2w filename	选择性保存从某n1行到另n2行的内容
:wq				保存并退出
ZZ				保存并退出
:x				保存并退出
:q[uit]			退出当前窗口
:saveas newfilename另存为
```

### 光标控制

```bash
k			向上移一行
j			向下移一行
h			向左移一个字符
l			向右移一个字符
gg			移到文件的首行	
G			移到文件的末行
H			移到屏幕的第一行
^			移到当前行首
$			移到当前行尾
{			移到上一段开头
}			移到上一段结尾
Enter		移到下一行行首
```

### 翻动屏幕

```bash
ctrl+f:			下翻一屏
ctrl+b:			上翻一屏
ctrl+d:			下翻半屏
ctrl+u:			上翻半屏
ctrl+e:			向下滚动一行
ctrl+y:			向上滚动一行
n%:				到文件n%的位置
zz:				将当前行移动到屏幕中央
zt:				将当前行移动到屏幕顶端
zb:				将当前行移动到屏幕底端
```

### 添加文本

```bash
i			在光标前插入文本
a			在光标后插入文本
o			在当前行的下边插入新行
S(大写字母)	删除光标所在的行，并进入插入模式
:r filename	读入指定文件内容，并插在当前行后
:nr file		读入文件 file 内容，并插在第 n 行后
```

### 复制

```bash
y:			 复制在可视模式下选中的文本
yy			 复制整行文本
y$:			 从光标当前位置复制到行尾
y0:			 从光标当前位置复制到行首
:m,ny<cr>		复制m行到n行的内容
yaw和yas：复制一个词和复制一个句子，即使光标不在词首和句首也没关系
```

### 剪切
```bash
d$ or D:		  删除（剪切）当前位置到行尾的内容
d0:			      删除（剪切）当前位置到行首的内容
[n] dd:			  删除（剪切）1(n)行。
:m,nd<cr>		  剪切m行到n行的内容
:1,10 m 12		剪切1-10行并粘贴到12行里面
daw和das：		剪切一个词和剪切一个句子，即使光标不在词首和句首也没关系
d/f<cr>：		 将删除当前位置 到下一个f之间的内容
```

### 粘贴

```bash
p: 			在光标之后粘贴。
P: 			在光标之前粘贴。
u 			撤消上一次修改
U 			撤消所有修改
```

### 保存并退出

```bash
:w			  保存文件但不退出
:w file		将修改保存在 file 中但不退出
:wq			  保存文件并退出,还可以使用 ZZ 或 :x
:q!			  不保存文件,强制退出
:e!			  放弃所有修改，从上次保存文件开始再编辑
```

### 查找

```bash
/something:			    在后面的文本中查找something
?something:			    在前面的文本中查找something
/pattern/+number:		将光标停在包含pattern的行后面第number行上
/pattern/-number:		将光标停在包含pattern的行前面第number行上
n:				   向后查找下一个
N:				   向前查找下一个
:noh				 清除查找后的高亮
```

### 替换

```bash
:s/old/new			      用new替换当前行第一个old
:s/old/new/g			    用new替换当前行所有的old
:n1,n2s/old/new/g		  用new替换文件n1行到n2行所有的old
:%s/old/new/g			    用new替换文件中所有的old
:%s/^/xxx/g			      在每一行的行首插入xxx，^表示行首
:%s/$/xxx/g			      在每一行的行尾插入xxx，$表示行尾
%s/old/new/c			    每个替换都将需要用户确认
%s/old/new/i			    每个替换忽略大小写
:set fileencoding=utf-8 	设置文件编码
```

### 删除文本

```bash
x			  删除光标处的字符
db			删除光标前面的字
dw			删至下一个字的开头
dd			删除整行
:n,md		删除第n行到第m行的数据（删除数据包含n行和m行）
d$			从光标处删除到行尾
d^			从光标处删除到行首
```

## 写在最后
文本编辑器有很多，一般Windows中使用nodepad++即可满足日常使用。但是有时候你可能会遇到一些大文件，比如50M以上的文件，nodepad++打开就会有一点吃力啦。我尝试过，vim编辑器打开2G的文本依然很流畅，可以正常编辑。我们可以通过vim处理一些大文件，比如数据表，数据库文件。

