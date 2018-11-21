# WPF学习笔记

## WPF概述

* 概念介绍

  GDI：图形设备接口，负责系统和绘图程序之间的信息交换

  DirectX：多媒体编程接口，为在WPF中采用的底层图形技术。DirectX在渲染图形的时候将尽可能多的工作交给图形处理单元GPU进行处理，为硬件加速带来好处。

  WPF现状：主要是用于开发Windows桌面应用

# XAML

### XAML简介

* “zammel”，实例化.NET对象的标记语言，主要用于构造WPF用户界面。

### XAML基础

* xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"

​       WPF核心名称空间，包含了所有WPF类，包括用于构建用户界面的控件。整个文档的默认名称空间，每个元素自动位于这个名称空间。

* xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"

  这个名称空间被映射为前缀x，通过在元素名称之前放置一个名称空间前缀来使用这个名称空间。

* InitializeComponent()方法

  编译程序的时候会自动生成一个部分类，以及默认的构造函数，其中包含此方法。作用为将XAML编译后生成的BAML提取，并用它构建用户界面。

* 命名元素

  \<Grid x:Name="grid1"\>或者通过Properties窗口进行设置。

  提示：WPF中并不要求每个空间都有名称。但如果通过VS设计器表面上拖放元素来创建窗口，将为每个元素自动生成一个名称。

## 控件介绍

* Grid或者StackPanel行列划分

  ```C#
  //将一个名字为GridGame的Grid控件分成七行七列
  for (int i = 0; i < 7; i++)
     { 
          ColumnDefinition colDef=new ColumnDefinition();
          GridGame.ColumnDefinitions.Add(colDef);
          RowDefinition rowDef = new RowDefinition();
          GridGame.RowDefinitions.Add(rowDef);
      }
  ```

* 代码设置控件的边距

  ```C#
  //先声明一个Thickness对象（不止一种构造函数）,然后将对象赋值给变量的属性Margin
  Thickness thick = new Thickness(marginNum);//marginNum是定义的一个整型变量
  btn.Margin = thick;
  ```

* 代码设置字体

  ```C#
   btn.FontSize = fontSize;//fontSzie为声明的一个double型变量
   btn.FontWeight = FontWeights.Bold;
   //通过设置控件的FontWeight属性，来确定控件字体的粗细，FontWeights类为一个含有多个FontWeight类型属性的类
  ```

* 代码设置控件占多行多列

  ```C#
  TextBox txtBox = new TextBox();
  Grid.SetRow(txtBox, 0);
  Grid.SetColumn(txtBox, 0);
  Grid.SetColumnSpan(txtBox, 4);//设置文本框占四列
  Grid.SetRowSpan(txtBox, 2);//设置文本框占两行
  txtBox.Margin = txtThick;
  GridGame.Children.Add(txtBox); //GrideGame为声明的Gride对象
  ```

* 代码控制Window窗口

  * Title属性修改窗口标题

  * ResizeMode可以控制窗口是否可以调整大小。例：ResizeMode="NoResize";

  * WindowStartupLocation可以调整窗口打开的时候的位置。

    例：WindowStartupLocation="CenterScreen"

  * WindowState="Maximized"窗口默认最大化

  * 声明一个WinTest窗口，在另一个窗口上通过点击按钮调用WinTest窗口？

    首先new一个窗口WinTest的对象，然后对象通过调用ShowDialog方法（**阻塞方法**）弹出窗口。如果想关闭WinTest窗口，有两种方法。方法一：点击窗口上的某一按钮，调用Close方法。方法二：点击某一按钮，给DialogResult赋值，true或者false，如果对其赋值就会关闭。

* 可空类型

  引用类型的变量可以赋值为null。其中string类型是比较特殊的引用类型。int和bool型的变量不能赋值null,但是可以通过在类型后面加上？变成可空的数据类型，如int?，bool?。

* 打开文件

  **小贴士：**如果没有声明需要的命名空间的时候，调用OpenFileDialog会报错。这个时候可以在OpenFileDialog关键词上右键-解析-调用相应的命名空间。

  ```C#
  OpenFileDialog ofd=new OpenFileDialog; 
  ofd.Filter="文本文件|*.txt|PBG图片|*.png|所有文件|*.*";//过滤器，两个为一组
  if(ofd.ShowDialog==true)//如果打开文件，则返回值为true
  {
      string fileName=ofd.FileName;//通过FileName属性获得打开的文件名
      image1.Source=new BitmapImage(new Uri(fileName));
      //image1为窗口已经添加的图片控件
  }
  ```

* 保存文件

  SaveFileDialog，保存的时候会自动添加后缀名，其余参考打开文件。

* ComboBox控件

  ## WPF面板布局

  ### 什么是布局？

  所谓布局，即确定所有控件的大小和位置，是一种递归进行的父元素（Panel）和子元素交互的过程，WPF采用了一种包含**测量（Measure）和排列（Arrange）**两个步骤的解决方案。子元素最终所占用的空间和位置是由父元素确定的（RenderSize），但是父元素会先参考子元素的意见（DesiredSize）。

  ### Panel类的公有属性

  WPF的布局控件都继承于Sysrem.Windows.Controls.Panel类。

  * Background 面板背景色的画刷。如果想接受鼠标事件，必须将该属性设置为非空值（如果想接受鼠标事件，又不希望显示一个固定颜色的背景，只需要将背景色设置为透明）
  * Children 面板中存储的条目集合。第一级对象，条目自身可以包含更多的条目。

  ### Canvas布局控件

* Canvas是最轻量级的布局容器，不会自动调整内部元素的排列和大小。元素默认显示在画布的左上方，主要用来画图。默认不会自动剪裁超出自身范围的内容（ClipToBounds属性的默认值是false）。

  ### StackPanel

* StackPanel就是将子元素按照堆栈形式一一排列。可以通过设置**StackPanel的Orientation属性设置两种排列方式：横排（Horizontal，默认值)和竖排（Vertical）。**纵向的StackPanel每个元素默认宽度与面板一样宽，反之横向是高度和面板一样高。如果包含的元素超过了面板控件，它会被截断多出的内容。

  ```C#
  <Window x:Class="WpfApplication4.MainWindow"
          xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
          xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
          Title="MainWindow" Height="350" Width="525">
      <Grid>
          <StackPanel Margin="10,10,10,10" Background="Azure" Orientation="Horizontal"> 
              <Label HorizontalAlignment="Center">A Button Stack</Label>
              <Button Content="Button1" />
              <Button>Button 2</Button>
              <Button>Button 3</Button>
              <Button Content="Button 4"></Button>
          </StackPanel>
      </Grid>
  </Window>
  ```

* 布局属性

  HorizontalAlignment：当水平方向有额外空间的时候可以决定元素在布局容器中如何定位。Center/Left等。

  VerticalAlignment：竖直方向元素定位。

  Margin：在元素的周围添加一定的空间。

  MinWidth和MinHeight：设置元素的最小尺寸。

  MaxWidth和MaxHeight：元素的最大尺寸。


