* 如何让TextBox只能输入整型格式的字符串？

  **解决方法1**：利用KeyPress事件

  KeyDown  在首次按下某个键时发生

  KeyPress 在控件具有焦点并且用户按下并释放某个键后发生

  KeyUp 在释放键时发生

  KeyPress主要用来捕获数字、字母，只能捕获单个字符，不区分小键盘和主键盘的数字字符

  KeyDown和KeyUp可以捕获组合键，可以捕获除PrScrn之外的所有按键

  **解决方法2：**利用int.Parse()

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

* 