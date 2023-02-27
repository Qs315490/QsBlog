---
title: turtle
date: 2023-02-27 12:36:25
tags: [Python,模块,Turtle]
categories: [Python,模块]
---
# 备忘
默认画笔方向是右  
先设置属性, 再调用命令

# 画布
## screensize(x,y,color)
设置画布大小和背景色(单词或16进制)  
默认 `800*600` 白色

## setup(windowX,windowY,x,y)
设置画布大小和窗口起始位置的坐标  
默认 `800*600` (0,0)

# 移动
## forward(x) fd(x)
向当前画笔方向移动x个像素长度

## backward(x)
向当前画笔背面移动x像素长度

## pensize(px)
设置画笔的宽度(像素单位)

## goto(x,y)
turtle(画笔)移动到坐标为 (x,y) 的位置

## right(x)
顺时针旋转x度

## left(x)
逆时针旋转x度

## pendown()
落笔，移动时绘制图形

## penup()
抬笔，不绘制图形

## setx(a)
将当前x轴移动到a位置

## sety(a)
将当前y轴移动到a位置

## setheading(x)
设置当前朝向为x角度

## speed(x)
设置当前速度为x, 默认为5
效果|x的值
-|-
极快|0
快|10
正常|6
慢|3
极慢|1

## shape(type)
更改光标外形, 默认箭头  
`turtle` 是海龟

# 绘画
## dot(d,color)
绘制一个指定直径和颜色的圆点, 颜色是可选项

## circle(r,e,s)
r表示半径，正值逆时针旋转，负值顺时针旋转  
e表示度数，绘制曲线  
s表示边数，可用于绘制正多边形  
{% p yellow::r是必要的; e和s参数可有可无 %}

## pencolor()
设置画笔颜色

## fillcolor(color)
画出图形的内部填充颜色

## color(1, 2)
同时设置pencolor=1, fillcolor=2(内部的填充颜色)

## filling()
返回当前是否在填充状态

## begin_fill()
准备填充图形

## end_fill()
填充结束(完成)

## hideturtle()
隐藏画笔的turtle形状

## showturtle()
显示画笔的turtle形状

## clear()
清空窗口(turtle的位置和状态不会改变)

## reset()
清空窗口，(重置turtle状态为起始状态)

## undo()
撤销上一个动作

## isvisible()
返回当前turtle是否可见

## stamp()
复制当前图形

## done()
turtle中的最后一个语句

## write(x [,font=("name",size,"type")])
写出文本，x为文本内容，字体名称是name，大小是size，类型是type(在线执行无法执行英文或汉字属于正常现象)