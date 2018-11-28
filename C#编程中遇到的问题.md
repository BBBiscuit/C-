[TOC]

## 一、如何让TextBox只能输入整型格式的字符串？

### 利用KeyPress事件

KeyDown  在首次按下某个键时发生

KeyPress 在控件具有焦点并且用户按下并释放某个键后发生

KeyUp 在释放键时发生

KeyPress主要用来捕获数字、字母，只能捕获单个字符，不区分小键盘和主键盘的数字字符

KeyDown和KeyUp可以捕获组合键，可以捕获除PrScrn之外的所有按键

```C#
//对应TextBox控件的KeyPress事件
private void DiyDutyCycleNum_KeyPress(object sender, KeyPressEventArgs e)
{
    //如果输入的数字是允许的则不进行任何处理
    //如果输入的数字是不允许的，则设置e.Handled=true，则输入无效。
	if (e.KeyChar != '\b')//这是允许输入退格键  
	{
        int len = DiyCycleNum.Text.Length;//获得当前控件文本长度
        if (len < 1 && e.KeyChar == '0') //文本首位不能为0
        {
            e.Handled = true;
        }
        else if ((e.KeyChar < '0') || (e.KeyChar > '9'))//这是允许输入0-9数字  
        {
            e.Handled = true;
        }
	}	
}
```



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
   <Window.Background>
        <ImageBrush ImageSource="pack://application:,,,/Resources/海洋上位机界面最最终                                      版.png"></ImageBrush>
    </Window.Background>
```

## WPF中控制窗口显示位置

首先新建一个WPF工程，在主界面添加一个按钮，并给按钮添加点击事件button1_Click，然后新建一个用于测试弹出位置的窗口TestWindow。
1、在屏幕中间显示，设置window.WindowStartupLocation = WindowStartupLocation.CenterScreen;

```C#
private void button1_Click(object sender, RoutedEventArgs e)
{
    TestWindow window = new TestWindow();
    window.WindowStartupLocation = WindowStartupLocation.CenterScreen;
    window.ShowDialog();
}
```

2、在父窗口中间显示，设置window.WindowStartupLocation = WindowStartupLocation.CenterOwner;，并指定Owner。

```C#
private void button1_Click(object sender, RoutedEventArgs e)
{
    TestWindow window = new TestWindow();
    window.WindowStartupLocation = WindowStartupLocation.CenterOwner;
    window.Owner = this;
    window.ShowDialog();
}
```



3、在任意位置显示，设置window.WindowStartupLocation = WindowStartupLocation.Manual;并制定窗口的Left和Top坐标。

```C#
private void button1_Click(object sender, RoutedEventArgs e)
{
    TestWindow window = new TestWindow();
    window.WindowStartupLocation = WindowStartupLocation.Manual;
    window.Left = 0;
    window.Top = 0;
    window.ShowDialog()；
}
```



## 3. 让按钮变成圆角

```C#
        <Button Content="Button"
                HorizontalAlignment="Left"
                Margin="185,122,0,0"
                VerticalAlignment="Top"
                Width="75">
            <Button.Template>
                <ControlTemplate TargetType="{x:Type Button}">
                    <Border BorderBrush="{TemplateBinding Control.BorderBrush}"
                            BorderThickness="0"
                            CornerRadius="10,10,10,10">
                        <Border.Background>
                            <LinearGradientBrush EndPoint="0,1"
                                                 StartPoint="0,0">
                                <GradientStop Color="White"
                                              Offset="0.0" />
                                <GradientStop Color="Silver"
                                              Offset="0.5" />
                                <GradientStop Color="White"
                                              Offset="0.0" />
                            </LinearGradientBrush>
                        </Border.Background>
                        <ContentPresenter Content="{TemplateBinding ContentControl.Content}"
                                          HorizontalAlignment="Center"
                                          VerticalAlignment="Center">                            
                        </ContentPresenter>
                    </Border>
                </ControlTemplate>
            </Button.Template>
        </Button>
```

## 如何让Lable换行

```C#
            <Label Content="    输入 &#13;三相电流" //&#13;相当于\r\n
                   HorizontalAlignment="Left"
                   VerticalAlignment="Top"
                   FontWeight="Bold"
                   Margin="68,20,0,0"
                   Height="45" />
```

## WPF中的Lable实时显示时间

```C#
//在WPF中使用DispatcherTimer定时器     
private static DispatcherTimer dateTimer = new DispatcherTimer();
void InitialTimer()
{ 
    dateTimer.Interval = TimeSpan.FromMilliseconds(1000); //设置定时器间隔
    dateTimer.Tick += dateTimer_Tick; //1秒计时完成触发该事件
    dateTimer.Start(); //启动定时器
}
//注意不要将定时器出发事件声明为静态方法，否则无法使用控件，如timeShow/dateShow等Lable
void dateTimer_Tick(object sender, EventArgs e)//定时器触发事件
{
    timeShow.Content = DateTime.Now.ToString("HH:mm:ss");
    dateShow.Content = DateTime.Now.ToString("yyyy/MM/dd");
}
public MainWindow()
{
    InitializeComponent();
    InitialTimer();//主窗口构造函数中调用定时器的初始化函数（配置并启动定时器）
}
```

