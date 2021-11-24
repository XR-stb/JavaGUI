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

### 1.8 输入框事件监听

![image-20211006141507965](https://i.loli.net/2021/10/06/XnDk1HvO2rYgojt.png)

```java
import org.w3c.dom.Text;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TextActionListener {
    public static void main(String[] args) {
        new MyFrame();
    }
}

class  MyFrame extends Frame{
    public MyFrame(){
        TextField textField = new TextField();
        add(textField);

        MyActionLis2 myActionLis2 = new MyActionLis2();
        //按下回车触发事件
        textField.addActionListener(myActionLis2);

        //设置替换编码  比如输入密码隐藏
        //textField.setEchoChar('*');

        setVisible(true);
        setSize(400,400);
        closeWindow(this);
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

class MyActionLis2 implements ActionListener {

    @Override
    public void actionPerformed(ActionEvent e) {
        TextField field = (TextField) e.getSource();//获得资源， 返回一个对象
        System.out.println("you had input:\n" + field.getText());

        //field.setText("");//再次回车清空文本域 因为默认监听事件是回车
    }
}
```

### 1.9 实现简易加法计算器

* 遵循 oop 原则 组合 > 继承 / 多态

![image-20211006150646733](https://i.loli.net/2021/10/06/vCJi7MjRSa1tLkU.png)

```java
import javax.xml.stream.events.StartDocument;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class Calculate extends Frame {
    TextField textField_1,textField_2,textField_3;

    public static void main(String[] args) {
        new Calculate().loadFrame();
    }

   public void loadFrame(){
        textField_1 = new TextField(10);
        textField_2 = new TextField(10);
        textField_3 = new TextField(20);

        Label label = new Label(" + ");
        Button btn = new Button(" = ");

        btn.addActionListener(new MyListener(this));

        setLayout(new FlowLayout());
        add(textField_1);
        add(label);
        add(textField_2);
        add(btn);
        add(textField_3);

        setVisible(true);
        pack();
        // setSize(400,200);
        closeWindow(this);
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

class MyListener implements ActionListener {
    Calculate calculate = null;
    public MyListener(Calculate calculate){
        this.calculate = calculate;
    }
    @Override
    public void actionPerformed(ActionEvent e){
        int n1 = Integer.parseInt(calculate.textField_1.getText());
        int n2 = Integer.parseInt(calculate.textField_2.getText());

        calculate.textField_3.setText("" + (n1+n2));

        //calculate.textField_1.setText("");//清空前面的两个文本框
        //calculate.textField_2.setText("" );
    }
}
```

### 1.10 画笔 paint

![image-20211007160351648](https://i.loli.net/2021/10/07/ItqhEvliyM2zam1.png)

```java
import java.awt.*;

public class TestPaint {
    public static void main(String[] args) {
        new MyPaint().loadFrame();
    }
}

class MyPaint extends Frame {
    public void loadFrame(){
        setVisible(true);
        setBounds(200,200,500,500);
    }
    // 画笔
    @Override
    public void paint(Graphics g) {
        //super.paint(g);
        g.setColor(Color.red);
        g.drawOval(100,100,100,100);//空心圆
        g.fillOval(150,200,100,100);//实心圆

        g.setColor(Color.green);
        g.drawRect(400,200,80,60);//空心矩形
        g.fillRect(300,200,80,60);//实心矩形

        //养成习惯 画笔用完还原到最初颜色 即 黑色
    }
}
```

### 1.11 鼠标监听 + 画点

* 每点击一次frame产生一个点 但是每次所有的颜色都会变 待解决（-emm...owo

![image-20211007170033804](https://i.loli.net/2021/10/07/kqfzuOZVWBEDtwn.png)

```java
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.Random;

public class TestMouseListener {
    public static void main(String[] args) {
        new MFrame("title");
    }
}

class MFrame extends Frame {
    ArrayList points;

    // 画画需要画笔，需要监听鼠标当前位置 需要使用集合来存储这个点
    public MFrame(String title) {
        super(title);
        setBounds(200, 200, 400, 400);
        setVisible(true);
        //存鼠标点击的点
        points = new ArrayList<>();

        //鼠标监听器 ，正对这个窗口
        this.addMouseListener(new MyMouseListener());
    }

    @Override
    public void paint(Graphics g) {
        //画画需要监听鼠标事件
        Iterator iterator = points.iterator();
        while (iterator.hasNext()) {
            Point point = (Point) iterator.next();
            Random rand = new Random();
            Color color = new Color(rand.nextInt(256),rand.nextInt(256),rand.nextInt(256));
            g.setColor(color);
            g.fillOval(point.x, point.y, 20, 20);
        }
    }

    // 添加一个点到 界面上
    public void addPaint(Point point) {
        points.add(point);
    }

    //适配器模式
    private class MyMouseListener extends MouseAdapter {
        // 鼠标 按下 弹起 按住不放
        @Override
        public void mousePressed(MouseEvent e) {
            MFrame mFrame = (MFrame) e.getSource();
            //这里我们点击的时候在界面上画一个点
            mFrame.addPaint(new Point(e.getX(), e.getY()));
            //Point(e.getPoint())
            //每点击一次，需要重画一次
            mFrame.repaint();
        }
    }
}
```

### 1.12 窗口监听和键盘监听

* 窗口类方法和键盘类方法相似，这里不做过多的赘述

![image-20211007175852043](https://i.loli.net/2021/10/07/ygiG9ZxsT6wIaLA.png)

```java
import java.awt.*;
import java.awt.event.KeyEvent;
import java.awt.event.KeyAdapter;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.util.concurrent.ConcurrentHashMap;

public class TestWindow {
    public static void main(String[] args) {
        new WindowFrame();
    }
}

class  WindowFrame extends Frame {
    public WindowFrame() {
        setBackground(Color.blue);
        setVisible(true);
        setBounds(200,200,400,400);
        this.addKeyListener(new KeyAdapter(){
            @Override
            public void keyPressed(KeyEvent e){
                int keycode = e.getKeyCode();//获取输入的字符
                if(keycode == KeyEvent.VK_UP){
                    System.out.println("你按了上键");
                }
            }
        });
    }
}
```

## 2. Swing 

### 2.1 窗口面板

![image](https://user-images.githubusercontent.com/56108982/143243735-3534d893-107f-4062-bb16-75fc35c48e4a.png)

```java
package com.JFrame;

import javax.swing.*;
import java.awt.*;

public class JFrameDemo {
    public void init(){
        JFrame frame = new  JFrame("title");
        frame.setVisible(true);
        frame.setBounds(200,200,400,400);

        JLabel jLabel = new JLabel("welcome to jlabel",SwingConstants.CENTER);
        //设置label居中 , 两种方式
   	    //jLabel.setHorizontalAlignment(SwingConstants.CENTER); 
        // 设置水平对齐方式 center

        frame.add(jLabel);

        //frame.setBackground(Color.cyan);//顶层容器才可以上色
        frame.getContentPane().setBackground(Color.pink);

        frame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new JFrameDemo().init();
    }
}
```

### 2.2 弹窗 Dialog

![image-20211013190034507](https://i.loli.net/2021/10/13/Pqu1RGUZWgbezo5.png)

```java
package com.Dialog;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Date;

public class Dialog extends JFrame {
    public Dialog(){
        this.setVisible(true);
        this.setSize(700,500);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

        Container container = this.getContentPane();
        // 绝对布局
        this.setLayout(null);

        // 按钮
        JButton button = new JButton("点击弹出对话框");
        button.setBounds(30,30,200,50);

        // 监听按钮 点击就打开弹窗 dialog 即 new 一个dialog对象 即可
        button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                new Mydialog();
            }
        });

        container.add(button);
    }
    public static void main(String[] args) {
        new Dialog();
    }
}


class Mydialog extends JDialog{
    public Mydialog() {
        this.setVisible(true);
        this.setBounds(100,100,500,500);
        //this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
	    // 默认有关闭的 再设置的话会报错
        Container container = this.getContentPane();
        container.setLayout(null);

        container.add(new Label("haohao hao"));
    }
}
```

### 2.3 Icon

![image-20211013200136849](https://i.loli.net/2021/10/13/ZaYUtNbK3BJzqSv.png)

```java
package com.Icon;


import javax.swing.*;
import java.awt.*;

public class IconDemo extends JFrame implements Icon {
    private int height;
    private int width;

    public IconDemo(){}
    public IconDemo(int width, int height){
        this.height = height;
        this.width = width;
    }

    public void init(){
        IconDemo iconDemo = new IconDemo(15, 15);
        //图标可以放在很多地方 我们放在lable

        JLabel label = new JLabel("tu biao",iconDemo,SwingConstants.CENTER);

        Container contentPane = getContentPane();
        contentPane.add(label);

        this.setVisible(true);
        this.setSize(400,400);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }
    public static void main(String[] args) {
        new IconDemo().init();
    }

    @Override
    public void paintIcon(Component c, Graphics g, int x, int y) {
        g.fillOval(x,y,width,height);
    }

    @Override
    public int getIconWidth() {
        return this.width;
    }

    @Override
    public int getIconHeight() {
        return this.height;
    }
}

```

