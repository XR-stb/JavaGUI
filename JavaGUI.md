# Java GUI 

## 1. awt

### 1.1 简单的窗口实现

![image-20211005165107862](https://i.loli.net/2021/10/06/1VDfzAJlhU2dpS8.png)

```java
//MyJFrame
//MainClass
import javax.swing.*;
import java.awt.*;
import java.io.File;
import java.util.*;
import java.io.*;
public class MainClass {
    public static void main(String [] args){
         //MyWindow my_window = new MyWindow();
         ManyFrame manyFrame_1 = new ManyFrame(200,200,200,200,Color.green);
         ManyFrame manyFrame_2 = new ManyFrame(200,400,200,200,Color.red);
         ManyFrame manyFrame_3 = new ManyFrame(400,200,200,200,Color.pink);
         ManyFrame manyFrame_4 = new ManyFrame(400,400,200,200,Color.yellow);
    }
}
```

```java
//MyWindowClass
import java.awt.*;

public class MyWindow {
    public MyWindow(){
        Frame frame = new Frame("my first gui");
        frame.setVisible(true);
        frame.setSize(400,400);
        frame.setBackground(Color.pink);
        frame.setLocation(200,200);
        frame.setResizable(false);
    }
}

class ManyFrame extends Frame{
    static int id = 0;
    public ManyFrame(int x,int y,int w,int h,Color color){
        //实现一个自己的Frame类 便于生成窗口 可以一次传入多个参数初始化
        super("many frame"+(++id));
        setVisible(true);
        setBounds(x,y,w,h);
        setBackground(color);
        setResizable(false);
        //Frame 没有 setDefaultExitOnClose()
    }
}
```

### 1.2 Panel

![image-20211005172657087](https://i.loli.net/2021/10/06/GtpoOvMf4ZSwzHc.png)

可以用循环生成多个 panel

![image-20211005173750454](https://i.loli.net/2021/10/06/8fnTNptyKjaJoSv.png)

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

public class MyPanel {
    public static void main(String[] args) {
        Frame frame = new Frame();
        Panel panel = new  Panel();
        frame.setLayout(null);
        frame.setBounds(200,100,400,400);
        frame.setBackground(new Color(0x11BB2B));
        frame.setVisible(true);

        //panel 坐标相对于frame
        panel.setBounds(50,50,50,50);
        panel.setBackground(new Color(0x0A0AB9));

        //把panel 放入frame
        frame.add(panel);

        //监听事件 关闭frame
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);//关闭程序
            }
        });
    }
}
```

### 1.3 FlowLayout

![image-20211005175650939](https://i.loli.net/2021/10/06/MdRqELcOCli138G.png)

```java
import jdk.management.jfr.FlightRecorderMXBean;

import java.awt.*;

public class MyLayout {
    public static void main(String[] args) {
        Frame frame = new Frame();
        Button button1 = new Button("button1");
        Button button2 = new Button("button2");
        Button button3 = new Button("button3");

        //流式布局 默认center
        frame.setLayout(new FlowLayout(FlowLayout.LEFT));//修改成左对齐

        frame.setSize(300,300);
        frame.setVisible(true);

        frame.add(button1);
        frame.add(button2);
        frame.add(button3);
    }
}
```

### 1.4 BorderLayout 

![image-20211005181352576](https://i.loli.net/2021/10/06/9K2wgiqpMRCvFWE.png)

```java
import java.awt.*;

public class MyBorder {
    public static void main(String[] args) {
        Frame frame = new Frame();

        Button east = new Button("East");
        Button west = new Button("West");
        Button south = new Button("South");
        Button north = new Button("North");
        Button center = new Button("Center");

        frame.add(east,BorderLayout.EAST);
        frame.add(west,BorderLayout.WEST);
        frame.add(south,BorderLayout.SOUTH);
        frame.add(north,BorderLayout.NORTH);
        frame.add(center,BorderLayout.CENTER);

        frame.setVisible(true);
        frame.setSize(400,400);
    }
}
```

### 1.5 GridLayout

![image-20211005182033140](https://i.loli.net/2021/10/06/5aRbIrx4HW3utS7.png)

```java
import java.awt.*;

public class MyGrid {
    public static void main(String[] args) {
        Frame frame = new Frame();

        Button button1 = new Button("button1");
        Button button2 = new Button("button2");
        Button button3 = new Button("button3");
        Button button4 = new Button("button4");
        Button button5 = new Button("button5");
        Button button6 = new Button("button6");

        frame.setLayout(new GridLayout(3,2,20,20));//三行两列 还有边距

        frame.add(button1);
        frame.add(button2);
        frame.add(button3);
        frame.add(button4);
        frame.add(button5);
        frame.add(button6);

        frame.setVisible(true);
        frame.setSize(400,400);
    }
}
```

* frame.pack() 自适应布局 填充满

![image-20211005182510981](https://i.loli.net/2021/10/06/2g4nLBotFyDisfQ.png)

### 1.6 提高题

![image-20211005191632913](https://i.loli.net/2021/10/06/Rq6XTJu5CtzIE9Z.png)

> 思路：
>
> 1. frame 里面套两个 panel （上下分布）
> 2. up_panel 使用**BorderLayout**布局,东西放两个button， center 里面再套一个panel（用GridLayout即可）
> 3. down_panel 同上
> 4. 最后把上下的panel add到frame 即可看到效果

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

public class Test {
    public static void main(String[] args) {
        Frame frame = new Frame();
        frame.setLayout(new GridLayout(2,1));

        Panel p_up = new Panel(new BorderLayout());
        p_up.add(new Button("east-1"),BorderLayout.EAST);
        p_up.add(new Button("west-1"),BorderLayout.WEST);

        Panel up_cen = new Panel(new GridLayout(2,1));
        up_cen.add(new Button("up_cen-1"));
        up_cen.add(new Button("up_cen-2"));

        p_up.add(up_cen,BorderLayout.CENTER);

        //下半部分______________________________________________________

        Panel p_down = new Panel(new BorderLayout());
        p_down.add(new Button("east-2"),BorderLayout.EAST);
        p_down.add(new Button("west-2"),BorderLayout.WEST);

        Panel down_cen = new Panel(new GridLayout(2,2));
        for(int i = 1; i <= 4; i++){
            down_cen.add(new Button("down_cen"+i));
        }

        p_down.add(down_cen, BorderLayout.CENTER);

        //两部分完______________________________________________________
        frame.add(p_up);
        frame.add(p_down);

        frame.setSize(500,400);
        frame.setVisible(true);
    }
}
```

### 1.7 事件监听

* 点击button 输出 aaa

![image-20211006130644313](https://i.loli.net/2021/10/06/7gL4cNeMV6ETBjG.png)

```java
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TestActionEvent {
    public static void main(String[] args) {
       Button button =  new Button("button");
       Frame frame = new Frame();

       //因为addActionListener需要一个ActionListener
       MyActionListener myActionListener = new MyActionListener();
       button.addActionListener(myActionListener);

       frame.add(button,BorderLayout.CENTER);
       //frame.pack();
       frame.setVisible(true);
       frame.setSize(400,400);
       windowClose(frame);
    }
    private static void windowClose(Frame frame){
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }
}

class  MyActionListener implements ActionListener{
    @Override
    public void actionPerformed(ActionEvent e){
        System.out.println("aaa");
    }
}
```

![image-20211006132355292](https://i.loli.net/2021/10/06/K12GORBomrSjqns.png)

![image-20211006133143423](https://i.loli.net/2021/10/06/JUfDYy7pI4jWZXq.png)

```java
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TwoActionButton {
    public static void main(String[] args) {
        Frame frame = new Frame();
        Button btn1 = new Button("start");
        Button btn2 = new Button("stop");

        btn1.setActionCommand("b1");
        btn2.setActionCommand("b2");

        MyActListener myActListener = new MyActListener();

        btn1.addActionListener(myActListener);
        btn2.addActionListener(myActListener);

        frame.add(btn1,BorderLayout.SOUTH);
        frame.add(btn2,BorderLayout.NORTH);

        frame.pack();
        frame.setVisible(true);
        frame.setSize(400,400);
        closeWindow(frame);
    }
    private static void closeWindow(Frame frame){
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }
}

class MyActListener implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent e){
        //System.out.println(e.getActionCommand() + "按钮被点击了！！！" );
        if(e.getActionCommand().equals("b1")){
            System.out.println("you have click btn1");
        }else{
            System.out.println("you have click btn2");
        }
    }
}
```

