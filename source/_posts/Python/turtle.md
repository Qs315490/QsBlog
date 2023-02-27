---
title: turtle
date: 2023-02-27 12:36:25
tags: [Python,模块,Turtle]
categories: [Python,模块]
---
# 常用
## forward(x)
向当前画笔方向移动x个像素长度

## backward(x)
向当前画笔背面移动x像素长度

## pensize()
设置画笔的宽度(像素单位)

## right(x)
顺时针旋转x度

## left(x)
逆时针旋转x度

## pendown()
落笔，移动时绘制图形

## goto(x,y)
turtle(画笔)移动到坐标为x,y的位置

## penup()
抬笔，不绘制图形

## circle(r,e,s)
r表示半径，正值逆时针旋转，负值顺时针旋转  
e表示度数，绘制曲线  
s表示边数，可用于绘制正多边形  
{% p yellow::r是必要的；e和s参数可有可无 %}

## setx(a)
将当前x轴移动到a位置

## sety(a)
将当前y轴移动到a位置

## setheading(x)
设置当前朝向为x角度

## speed(x)
设置当前速度为x
效果|x的值
-|-
极快|0
快|10
正常|6
慢|3
极慢|1

## dot(d,color)
绘制一个指定直径和颜色的圆点，颜色是可选项

## fillcolor(color)
图形的填充颜色

## color(1, 2)
同时设置pencolor=1, fillcolor=2

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