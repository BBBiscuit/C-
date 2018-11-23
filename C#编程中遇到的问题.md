[TOC]

## 一、如何让TextBox只能输入整型格式的字符串？

### 利用KeyPress事件

KeyDown  在首次按下某个键时发生

KeyPress 在控件具有焦点并且用户按下并释放某个键后发生

KeyUp 在释放键时发生

KeyPress主要用来捕获数字、字母，只能捕获单个字符，不区分小键盘和主键盘的数字字符

KeyDown和KeyUp可以捕获组合键，可以捕获除PrScrn之外的所有按键

### 利用int.Parse()

```c#
int i = -1;
bool b = int.TryParse(null, out i);
//执行完毕后，b等于false，i等于0，而不是等于-1.
```

```c#
int i = -1;
bool b = int.TryParse("123", out i);
//执行完毕后，b等于true，i等于123；
```

## 二、如何设置WPF的主界面背景为一张图片

### 代码中实现

```C#
 ImageBrush b = new ImageBrush();
 b.ImageSource = new BitmapImage(new Uri(@"Resources\海洋上位机界面最最终版.png",                                            UriKind.Relative));
//此处通过设置UriKind属性，使用Uri的相对路径
 b.Stretch = Stretch.Fill;
 this.Background = b;
```

### XAML中实现

```C#
ImageBrush b = new ImageBrush();
b.ImageSource = new BitmapImage(new                                                           Uri("pack://application:,,,/Resources/海洋上位机界面最最终版.png"));
b.Stretch = Stretch.Fill;
this.Background = b;
```



