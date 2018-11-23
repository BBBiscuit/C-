[TOC]

# WPF学习笔记

## WPF概述

* 概念介绍

  GDI：图形设备接口，负责系统和绘图程序之间的信息交换

  DirectX：多媒体编程接口，为在WPF中采用的底层图形技术。DirectX在渲染图形的时候将尽可能多的工作交给图形处理单元GPU进行处理，为硬件加速带来好处。

  WPF现状：主要是用于开发Windows桌面应用

## XAML

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

  \<Grid Name="grid1"\>或者通过Properties窗口进行设置。

  提示：WPF中并不要求每个空间都有名称。但如果通过VS设计器表面上拖放元素来创建窗口，将为每个元素自动生成一个名称。

### XAML中的属性和事件

XAML解析器需要将字符串值转换成非字符串属性。

#### 复杂属性

```C#
    <Grid>
        <Grid.Background>
            <LinearGradientBrush>
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0.00" Color="Red" />
                    <GradientStop Offset="0.50" Color="Indigo" />
                    <GradientStop Offset="1.00" Color="Violet" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Grid.Background>
    </Grid>
    //产生一个渐变的背景色
```

#### 标记扩展

非常规方式设置属性的专门语法，例如：将属性值设置为一个已经存在的对象，或者通过一个属性绑定到另一个控件来动态地设置属性值。

```C#
        <Button  HorizontalAlignment="Center" VerticalAlignment="Center" Height="200"                    Width="100" Content="Hello">
            <Button.Foreground>
                <x:Static Member="SystemColors.ActiveCaptionBrush"></x:Static>
                //标记扩展
            </Button.Foreground>
        </Button>
```

#### 附加属性

用于多个控件，但是在另一个类中定义的属性。例如，当在一个容器中放置一个控件的时候，根据容器的类型控件会获得额外的特征。

附加属性总是使用包含两个部分的命名形式：**定义类型.属性名**

附加属性其实不是真正的属性，实际上转换成方法调用。

#### 特殊字符与空白

小于号(<) -----------------&lt；

大于号(>)----------------- & gt; 如果一个控件的名字里面带有<或者>，需要用上面的符号代替，包括“；”

XAML默认折叠所有的空白，可以使用xml:space="preserve" 保存所有空白字符。

#### 事件

语法：事件名=“事件处理程序方法名”

WPF遵循的顺序：首先设置Name属性，然后关联所有的事件处理程序，最后设置其他属性。

因此所有对属性变化做出响应的时间处理程序，在第一次设置属性的时候都会被触发。

#### 完整的实例

```c#
    <Grid Name="grid1">
        <Grid.RowDefinitions>
            <RowDefinition Height="*"></RowDefinition>
            <RowDefinition Height="Auto"></RowDefinition>
            <RowDefinition Height="*"></RowDefinition>
        </Grid.RowDefinitions>
        <TextBox VerticalAlignment="Stretch" HorizontalAlignment="Stretch" 
                 Margin="10,10,13,10" Name="txtQuestion" 
                 TextWrapping="Wrap" FontFamily="Verdana" FontSize="24" 
                 Grid.Row="0">
            [Place question here.]
        </TextBox>
        <Button VerticalAlignment="Top"
                HorizontalAlignment="Left"
                Margin="10,0,0,20"
                Width="127"
                Height="23"
                Name="cmdAnswer"
                Click="cmdAnswer_Click"
                Grid.Row="1">
            Ask the Eight Ball
        </Button>
        <TextBox VerticalAlignment="Stretch"
                 HorizontalAlignment="Stretch"
                 Margin="10,10,13,10"
                 Name="txtAnswer"
                 TextWrapping="Wrap"
                 IsReadOnly="True"
                 FontFamily="Verdana"
                 FontSize="24"
                 Foreground="Green"
                 Grid.Row="2">
            [Answer will appear here.]
        </TextBox>
        <Grid.Background>
            <LinearGradientBrush>
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0.00"
                                  Color="Red" />
                    <GradientStop Offset="0.50"
                                  Color="Indigo" />
                    <GradientStop Offset="1.00"
                                  Color="Violet" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Grid.Background>
    </Grid>
```

![TIM图片20181122222213](F:\C#学习文档\学习图片\TIM图片20181122222213.png)

### 使用其他名称空间中的类型

创建不是用于在XAML窗口显示的对象，比如当使用数据绑定并且希望在一个控件上显示从其他对象提取的信息时，或者希望使用一个非WPF对象为一个WPF对象设置属性时。

语法：xmlns:***Prefix***="clr-namespace:***Namespace***; assembly=***AssemblyName***"

```C#
<Window x:Class="WpfApplication4.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
            //sys前缀映射到System名称空间，并使用System名称空间创建两个DateTime对象，并填充列表
        Title="Eight Ball Answer"
        Height="313"
        Width="457">
    <ListBox>
        <ListBoxItem>
            <sys:DateTime>10/13/2010 4:30 PM</sys:DateTime>           
        </ListBoxItem>
        <ListBoxItem>
            <sys:DateTime>11/23/2018 2:43 PM</sys:DateTime>
        </ListBoxItem>
    </ListBox>
</Window>
```

### 加载和编译XAML

可以使用三种编码方式来创建WPF应用程序：只是用代码，使用代码和未编译的标记（XAML），使用代码和编译过的标记（BAML）。



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

  ### Canvas面板

* Canvas是最轻量级的布局容器，使用固定的坐标绝对定位元素。不会自动调整内部元素的排列和大小。元素默认显示在画布的左上方，主要用来**画图**。默认不会自动剪裁超出自身范围的内容（ClipToBounds属性的默认值是false）。

  #### Z顺序

  如果在Canvas面板中有多个相互重叠的元素，可以通过设置Canvas.ZIndex附加属性来控制他们的层叠方式。通常所有元素的ZIndex值为0，如果具有相同的Z值，则根据它们在Canvas.Children集合中的顺序进行显示，后声明的元素在先声明的元素上面。Z值大的元素在Z值小的元素上面。

  ```C#
  <Canvas>
  	<Button Canvas.Left="60" Canvas.Top="80" Canvas.ZIndex="1" 
  	        Width="50" Height="50">Button 1</Button>
  	 <Button Canvas.Left="70" Canvas.Top="120"
  	        Width="100" Height="50">Button 2</Button>
  </Canvas>
  ```

  #### InkCanvas元素

  InkCanvas定义了四个附加属性（ToP/left/Bottom/Right），可以将这四个附加属性应用于子元素，异根据坐标进行定位。

  InkCanvas元素的主要目的是用于接收手写笔输入，同时也可以使用鼠标进行工作。

  ```C#
      <Grid >
          <InkCanvas Name="inkCanvas" Background="LightYellow" EditingMode="Ink">
              <Button InkCanvas.Top="10" InkCanvas.Left="10" Width="20"                                 Height="30"></Button>
          </InkCanvas>
      </Grid>
  ```

  InkCanvas.EditingMode属性值为一个枚举类型。

  Ink：默认模式，鼠标或者手写笔绘图

  GestureOnly：不允许绘制笔画批注，但是会关注预先定义的特定姿势。能够识别的姿势的完整列表由System.Windows.Ink.ApplicationGesture枚举给出。

  InkAndGesture

  EraseByStroke:单击画笔，擦除笔画。

  EraseByPoint：擦除点击部分。

  Select：允许用户选择保存在Children集合中的元素，可以通过单击或者拖动套索包含该元素。一旦选择了一个元素，就可以移动该元素，改变其尺寸或者删除。

  None：忽略鼠标和手写笔输入。

  ### StackPanel面板

* StackPanel就是将子元素按照堆栈形式一一排列。通常用于复杂窗口中的一些小区域。

  可以通过设置**StackPanel的Orientation属性设置两种排列方式：横排（Horizontal，默认值)和竖排（Vertical）。**纵向的StackPanel每个元素默认宽度与面板一样宽，反之横向是高度和面板一样高。如果包含的元素超过了面板控件，它会被截断多出的内容。

  ```C#
  <Window x:Class="WpfApplication4.MainWindow"
          xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
          xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
          Title="MainWindow" Height="350" Width="525">
      <Grid>
          <StackPanel Margin="10,10,10,10" Background="Azure" Orientation="Horizontal"> 
              <Label HorizontalAlignment="Center">A Button Stack</Label>
              <Button Content="Button1" HorizontalAlignment="Right"/>//按钮水平方向位置
              <Button>Button 2</Button>
              <Button>Button 3</Button>
              <Button Content="Button 4"></Button>
              //如果想重新排列元素，可以拖动元素到新位置
          </StackPanel>
      </Grid>
  </Window>
  ```

  #### 布局属性

  HorizontalAlignment：当水平方向有额外空间的时候可以决定元素在布局容器中如何定位。Center/Left等。

  VerticalAlignment：竖直方向元素定位。

  Margin：在元素的周围添加一定的空间。

  MinWidth和MinHeight：设置元素的最小尺寸。

  MaxWidth和MaxHeight：元素的最大尺寸。

  Width和Height：显式地设置元素的尺寸。

  **注意：**以上布局属性可以应用于许多布局面板的通用属性，因此这些属性被定义为FrameworkElement基类的一部分。除此之外，不同的布局容器可以为他们的子元素提供**附加属性**。例如，Grid对象的所有子元素可以获得Row和Column属性。

  **StackPanel面板也有自己的HorizontalAlignment和VerticalAlignment属性，默认为Stretch，故StackPanel面板充满容器。如果使用不同设置，尺寸将容纳最宽的控件。**

  #### 边距

  设置边距时可以令所有边设置为相同宽度。

  ```C#
  <Button Margin="5">Button 3</Button>
  ```

  也可以令控件的每个边以**左、上、右、下**的顺序设置不同的边距

  ```
  <Button Margin="5,10,5,10">Button 3</Button>
  ```

  代码中边距使用Thickness结构设置：

  ```C#
  cmd.Margin=new Thickness(5);
  ```

  **注意：**因为需要考虑相邻控件边距设置的相互影响，应该避免为不同的边设置不同的值。例如，在StackPanel中，按钮和面板本身使用相同的边距是比较合适的。

  #### 尺寸

  Width和Height：显式地设置元素的尺寸，但是如果有必要，应该使用**最大尺寸和最小尺寸**，把控件限制在正确的范围内。在一个良好的布局设计中，显式地设置尺寸是不需要的。

  ```C#
  <StackPanel Margin="3"> 
       <Label HorizontalAlignment="Center">A Button Stack</Label>
       <Button Margin="3" MaxWidth="200" MinWidth="100">Button 2</Button>
       <Button Margin="3" MaxWidth="200" MinWidth="100">Button 3</Button>
   </StackPanel>
  ```

  可以通过**ActualHeight属性和ActualWidth属性**得到用于渲染元素的**实际尺寸**，当窗口大小发生变化或其中的内容改变的时候，这些值可能会改变。

  **顶级窗口的尺寸：**为了使窗口能够自动改变大小，需要删除Height和Width属性，并将Window.SizeToContent属性设置为WidthAndHeight，窗口就会调整自身的尺寸，从而足以容纳所包含的内容。还可以通过将SizeToContent属性设置为Width或Height，可以让窗口在一个方向上改变自身尺寸。

  #### Border控件

  Border类只能包含**一段**嵌套内容（通常是布局面板），并且为其添加背景或者边框。

  ```c#
          <Border Margin="5"
                  Padding="5"
                  //边框和内部的内容之间添加空间
                  Background="LightYellow"
                  //使用Brush设置边框在所有内容后面的背景，固定颜色或者更特殊的背景
                  BorderBrush="SteelBlue"
                  BorderThickness="3,5,3,5"
                  //设置边框颜色以及宽度，为了显示边框必须设置这两个属性
                  CornerRadius="9"
                  //使边框具有优美的圆角
                  VerticalAlignment="Top">
              <StackPanel>
                 <Button Margin="3" Content="Button 4" />
                 <Button Content="Button 2" />        
              </StackPanel>
          </Border>
  ```

  ### WrapPanel面板和DockPanel面板

  #### WrapPanel面板

  在可能的空间中，一次以一行或者一列的方式布置控件。默认WrapPanel.Orientation属性设置为Horizontal。控件从左向右排列，然后再下一行中排列。

  WrapPanel面板水平地创建了一系列假象的行，每一行的高度为所包含最高元素的高度。可以通过VerticalAlignment属性设置进行对齐。

  提示：实际应用的时候主要用来控制一小部分布局细节，比如以类似工具栏空间的方式将所有的按钮保持在一起。

  ```C#
          <WrapPanel Margin="3,3,92,144">
              <Button VerticalAlignment="Top">Top Button</Button>
              <Button MinHeight="60">Tall Button</Button>
              <Button VerticalAlignment="Bottom">Bottom Button</Button>
              <Button>Stretch Button</Button>
              <Button VerticalAlignment="Center">Centered Button</Button>
          </WrapPanel>
  ```

  #### DockPanel面板

  如果一个按钮停靠在DockPanel面板顶部，该按钮将会被拉伸至面板的整个宽度，但是根据内容和MinHeight属性为其设置所需的**高度**。显然停靠顺序也很重要。

  ```C#
         //通过Dock的附加属性设置子元素停靠的边
  		<DockPanel LastChildFill="True">//LastChildFill属性令最后一个元素占满剩余空间
              <Button DockPanel.Dock="Top">Top Button</Button>
              <Button DockPanel.Dock="Bottom">Bottom Button</Button>
              <Button DockPanel.Dock="Left">Left Button</Button>
              <Button DockPanel.Dock="Right">Right Button</Button>
              <Button >Remaining Space</Button>
          </DockPanel>
  ```

  嵌套布局容器

  ```C#
          <DockPanel LastChildFill="True">
          //StackPanel面板放置两个Button，并通过对面板属性 HorizontalAlignment赋值，令其水平放置，并处于右边。由于开始只在DockPanel中声明了一个StackPanel元素，他将占满整个DockPanel面板，之后声明TextBox后，StackPanel将会根据按钮元素内容调整大小
              <StackPanel DockPanel.Dock="Bottom"
                          Orientation="Horizontal"
                          HorizontalAlignment="Right">
                  <Button Margin="10,10,2,10" Padding="3">OK</Button>
                  <Button Margin="2,10,10,10" Padding="3">Cancel</Button>
              </StackPanel>
              //TextBox框将处于DockPanel面板的上方，并充满剩余空间
              <TextBox DockPanel.Dock="Top"
                       Margin="10">This is a test.</TextBox>
          </DockPanel>Grid面板
  ```

  ### Grid面板

  #### 三种设置尺寸的方式

  绝对设置尺寸方式`<ColumnDefinition Width="100"/>

  自动设置尺寸方式`<ColumnDefinition Width="Auto"/>

  按比例设置尺寸方式`<ColumnDefinition Width="*"/>

  ​                                    `<ColumnDefinition Width="2*"/>

* ```C#
      <Grid ShowGridLines="True">//ShowGridLines可以显示每行每列的分割线
          <Grid.RowDefinitions>
              <RowDefinition Height="*"></RowDefinition>
              <RowDefinition Height="Auto"></RowDefinition>
          </Grid.RowDefinitions>
          <TextBox Margin="10"
                   Grid.Row="0">This is a test</TextBox>
          <StackPanel Grid.Row="1"
                      Orientation="Horizontal"
                      HorizontalAlignment="Right">
              <Button Margin="10,10,2,10" 
          			Padding="3">OK</Button>
              <Button Margin="2,10,10,10"
                      Padding="3">Cancel</Button>
          </StackPanel>
      </Grid>
  ```

  #### 布局舍入

  如果Grid面板的宽度为175像素，就不能很清晰的分割成两列，并且每列为87.5像素。可以通过布局容器的UseLayoutRounding属性设置为true解决此问题。

  #### 跨越行列

  ```C#
      <Grid ShowGridLines="True">
          <Grid.RowDefinitions>//将面板分为两行三列
              <RowDefinition Height="*"></RowDefinition>
              <RowDefinition Height="Auto"></RowDefinition>
          </Grid.RowDefinitions>
          <Grid.ColumnDefinitions>
              <ColumnDefinition Width="*"></ColumnDefinition>
              <ColumnDefinition Width="Auto"></ColumnDefinition>
              <ColumnDefinition Width="Auto"></ColumnDefinition>
          </Grid.ColumnDefinitions>
          <TextBox Margin="10"
                   Grid.Row="0"
                   Grid.Column="0"
                   Grid.ColumnSpan="3">This is a test</TextBox>//TextBox占三列
              <Button Margin="10,10,2,10" 
                      Padding="3"
                      Grid.Row="1"
                      Grid.Column="1">OK</Button>
              <Button Margin="2,10,10,10"
                      Padding="3"
                      Grid.Row="1"
                      Grid.Column="2">Cancel</Button>
      </Grid>
  ```

  注意：这种**占多行多列的布局并不好**，列宽由位于窗口底部的两个按钮的尺寸决定，继续添加新的内容变得困难。所以，对于一次性的布局任务，例如安排一组按钮，可以使用更小的布局容器。

  #### 分割窗口

  分隔条由GridSplitter类表示，是Grid面板的一个特性。

  使用分隔条的指导原则：

  * GridSplitter必须放在Grid单元格中，可以和已经存在的内容一起放到一个单元格，通过调整边距使它们不重叠。更好的方法是预留一行或者一列用于专门放置GridSplitter对象，并把预留的行或者列的Heigh属性或者Width属性的值设置为Auto。

  * GridSplitter总是改变整行或者整列的尺寸，而不是单个单元格，所以需要拉伸GridSplitter对象使其穿越整行或者整列，为此可以使用RowSpan或者ColumnSpan属性。

  * GridSplitter对象尺寸很小，需要设置尺寸。竖直分隔条需要将VerticalAlignment属性设置为Stretch，并且将其Width设置为一个固定的值。水平分隔条，将HorizontalAlignment属性设置为Stretch，Height设置为一个固定值。

  * 水平分隔条，需要将VerticalAlignment属性设置为Center，竖直分隔条需要将HorizontalAlignment属性设置为Center。

    ```C#
        <Grid ShowGridLines="True">
            <Grid.RowDefinitions>
                <RowDefinition ></RowDefinition>
                <RowDefinition ></RowDefinition>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition MinWidth="100"></ColumnDefinition>
                <ColumnDefinition Width="Auto"></ColumnDefinition>
                //放置分隔条的列设置为Auto宽度
                <ColumnDefinition MinWidth="50"></ColumnDefinition>
            </Grid.ColumnDefinitions>
                <Button Margin="3" Grid.Row="0"  Grid.Column="0">Left</Button>
                <Button Margin="3" Grid.Row="0" Grid.Column="2">Right</Button>
                <Button Margin="3" Grid.Row="1" Grid.Column="0">Left</Button>
                <Button Margin="3" Grid.Row="1" Grid.Column="2">Right</Button>
            <GridSplitter Grid.Row="0" Grid.Column="1" Grid.RowSpan="2" //分隔条占两列
                          Width="3" VerticalAlignment="Stretch" //设置宽度		                                   HorizontalAlignment="Center" 
                          ShowsPreview="False"></GridSplitter>
         //ShowPreview属性为False的时候，只要拖动分隔条就会立即改变尺寸
         //如果设置为True，拖动分隔条就会看到一个灰色的阴影随着鼠标移动，释放鼠标之后才会改变尺寸
        </Grid>
    ```

  #### 共享尺寸组

  Grid面板中设置两列的ShareSizeGroup属性即可。

  例：<ColumnDenifition Width="Auto"  ShareSizeGroup="TextLable”/>


### 布局示例

```C#
    <Grid Margin="3,3,10,3" ShowGridLines="True">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" ></RowDefinition>
            <RowDefinition Height="Auto"></RowDefinition>
            <RowDefinition Height="Auto"></RowDefinition>
            <RowDefinition Height="Auto" ></RowDefinition>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"></ColumnDefinition>
            <ColumnDefinition Width="*"></ColumnDefinition>
            <ColumnDefinition Width="Auto"></ColumnDefinition>
        </Grid.ColumnDefinitions>
        //重点注意以下控件的放置位置属性
        <Label Margin="3" Grid.Row="0" Grid.Column="0"
               VerticalAlignment="Center">Home:</Label>
        <TextBox Margin="3" Grid.Row="0" Grid.Column="1" Height="Auto" 
                 VerticalAlignment="Center">c:\</TextBox>
        <Button Margin="3" Grid.Row="0" Grid.Column="2" 
                Padding="2">Browse</Button>
        //以下三组控件放置基本重复上面
        <Label Margin="3" Grid.Row="1" Grid.Column="0"                                                VerticalAlignment="Center">Network:</Label>
        <TextBox Margin="3" Grid.Row="1" Grid.Column="1" Height="Auto"                                  VerticalAlignment="Center">e:\Shared</TextBox>
        <Button Margin="3" Grid.Row="1" Grid.Column="2" Padding="2">Browse</Button>

        <Label Margin="3" Grid.Row="2" Grid.Column="0"
               VerticalAlignment="Center">Web:</Label>
        <TextBox Margin="3" Grid.Row="2" Grid.Column="1" Height="Auto"                                  VerticalAlignment="Center">c:\</TextBox>
        <Button Margin="3" Grid.Row="2" Grid.Column="2" Padding="2">Browse</Button>
        
        <Label Margin="3" Grid.Row="3" Grid.Column="0"                                                VerticalAlignment="Center">Secondary:</Label>
        <TextBox Margin="3" Grid.Row="3" Grid.Column="1" Height="Auto"                                  VerticalAlignment="Center">c:\</TextBox>
        <Button Margin="3" Grid.Row="3" Grid.Column="2" 
                Padding="2">Browse</Button>
    </Grid>
```

注意：放置到WPF窗口中的任何内容都支持**Visibility**属性，其值为一个枚举类型。

​           Visible ：元素在窗口正常显示。

​	   Collapsed：元素不显示，也不占用任何空间。

​           Hidden：元素不显示，但是仍然为它保留空间。会在元素可能显示的地方保留空白空间。

​           **如果需要隐藏和显示元素，而且又不希望改变窗口布局和窗口中剩余元素的相对位置，使用此设置。**

## 3D绘图

### 视口

包含3D内容的容器为Viewport3D类，继承自FrameworkElement类，能够放到放置正常元素的任何地方。

Viewport3D类只应用于复杂的3D编程，只增加了两个属性：Camera和Children，前者定义了3D场景的观察者，后者包含了希望放置在场景中的所有3D对象。

注意：Viewport3D的属性ClipToBounds,默认值为true，超出视口边界的内容将被剪裁掉。如果为false，内容将会显示在相邻元素上面。如果为false，渲染复杂场景时可以明显提高性能。

## WPF编程过程涉及的零散知识点

### WPF中的URI

* URI为统一资源标识符

* 作用

  指定当应用程序第一次启动时显示的用户界面

  加载图像

  导航到页

  加载不可执行的数据文件

  使用URI标识和加载位于各种位置的文件，包括当前程序集，所引用的程序集，相对于程序集的某个位置，应用程序的源站点。

  #### 利用URI加载图像

  * Visual Studio中，可以通过将一个文件添加到项目，并将其BuildAction（汉化名字为：生成操作）设置为Resource来创建资源文件

  * 也可以直接通过Resources来添加需要的文件

  * URI加载图像的一般形式：协议+授权+路径

    **协议**：pack://

    **授权**：第一种，访问编译时已知的文件，用application:///；第二种，访问编译时不知道，运行时才知道的文件，用siteoforigin:///。加载图片时选择前者，但是一般写成application:,,,形式

    **路径**：绝对路径和相对路径

  * 在XAML中指定路径

    ```c#
        <Window.Background>
            <ImageBrush ImageSource="pack://application:,,,/Resources/海洋上位机界面最                     最终版.png"></ImageBrush>
        </Window.Background>
    ```

  * 在代码中指定路径

    ```C#
    ImageBrush b = new ImageBrush();
    b.ImageSource = new BitmapImage(new                                                           Uri("pack://application:,,,/Resources/海洋上位机界面最最终版.png"));
    b.Stretch = Stretch.Fill;
    this.Background = b;
    ```


