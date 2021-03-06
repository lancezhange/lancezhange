title: "Intellij IDEA 基础"
date: 2016-07-31 15:46:05
tags: tool
category: tech
comments: true
---

记录一下一些　Intellij IDEA 的常用快捷键。<!--more-->

当然，使用快捷键的前置任务是，先将配色，字体等进行配置，为自己打造一个舒服的代码环境。

首先，如果只能记住一个快捷键，那就选择**Ctrl Shift A**, 它是其他所有快捷键的超级入口，可以很方便地查询到相应的命令，例如，如果忘记打开终端的快捷键，只要按**Ctrl Shift A**然后输入*terminal*即可看到对应的快捷键(ALT F12).

### 基础
一波选择移动快捷键。

-  Ctrl Y 删除当前行，这个比较反人类。

-  如果要复制当前行，不用先选中，只要光标在目标行，**Ctrl C**即可拷贝。

-  **Ctrl W** 逐级扩张选择，这个是　Intellij IDEA 比较特殊的一个。如果想逐级收缩选择，用**Ctrl Shift W**.
 
-  **Alt J**, 就是sublime text3中的**Ctrl D**.　**Shif Alt J** 可以回退一个选择。

-  多个光标，**Shift Alt + 鼠标左键**

-  **Ctrl [** 跳至括号的左半个，**Ctrl ]**跳至右半个。检查括号匹配时很有用。

-  跳至给定行，用　**Ctrl g**, 'goto' 之意。

-  竖向选择，按住Alt然后鼠标左键下拉即可，这个也是非常有用

-  **Alt + <-**, 返回上次光标所在处

-  **Ctrl Shift J** 连接当前行和下一行

-  **Alt 1** 打开和关闭工程项目树。另外提一下，如果用"project" 视图查看项目树，嵌套比较深的时候很不方便，建议采用"project files"视图。

### 高级基础

-  **Ctrl B** 跳至定义处

-  **Ctrl ALt B** 跳至实现处

-  **Ctrl Alt L** 格式化代码

-  **Alt F7** 查看当前方法被谁调用

-  **Shift F6** 重命名一个方法或变量名，代码重构的利器。

-  **Alt Enter** 自动修复一些问题。


### 其他常用

-  想象一下这个场景：写好一坨代码之后，发现这一坨代码需要被放到if条件句中，于是只能先在这一坨代码之前加上if，然后挪到最后补上大括号，非常烦恼。这时可以用**Ctrl Shift T**,　实现*Surrond With*的功能。除了if,还支持*while*,*try/catch*等一堆，非常实用。

-  书签　
    1. 加书签　**Ctrl F11**
    2. 查看所有书签　**Shift F11**

-  实时模板，例如，输入**pvsm** 然后按Tab键，即可打出　*public void static main*函数雏形。**Ctrl J** 可以查看所有模板。

## 最后的唠叨
Intellij IDEA 确实是比较好用的一个集成开发环境。一般非工程化的代码我都用sublime text3,　工程化的就用　Intellij IDEA. 二者的一些快捷键并不相同，但只要熟练了，也不是问题。我很喜欢这些工具。

