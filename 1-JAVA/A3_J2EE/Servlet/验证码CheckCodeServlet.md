```java
package com.servlet;

import java.io.IOException;
import java.util.*;
import java.awt.*;
import java.awt.image.BufferedImage;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@WebServlet("/CheckCodeServlet")
public class CheckCodeServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    Random random = new Random(); //产生随机数	
    //随机颜色
    public Color getTandomColor(){
        int r = random.nextInt(255);//随机产生一个0-255的数
        int g = random.nextInt(255);
        int b = random.nextInt(255);
        return new Color(r,g,b);
    }

    //拼接验证码上的字符
    public String getRandomStr(){
        StringBuilder sb = new StringBuilder();
        for(int i = 0;i<4;i++){
            int cmd = random.nextInt(3); //0	1	2
            switch(cmd){
                case 0:
                    sb.append((char)(random.nextInt(25)+98));//所有的小写字母从98开始
                    break;
                case 1:
                    sb.append((char)(random.nextInt(25)+65));//所有的大写字母从65开始
                    break;
                case 2:
                    sb.append(random.nextInt(10));
                    break;
            }
        }
        return sb.toString();
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //定验证码所在矩形的宽度、高度
        int width = 120;
        int height = 50;

        //设置验证码字体大小
        int fontSize = 30;

        //设置干扰线条数
        int grx = 15;

        //将矩形区域定格为画布或图片,第三个参数代表画布样式
        BufferedImage image = new BufferedImage(width,height,BufferedImage.TYPE_INT_BGR);

        Graphics g = image.getGraphics();//画笔Graphics

        g.setColor(Color.WHITE);  
        g.fillRect(0, 0, width-1, height-1);	//白色的填充

        g.setColor(Color.BLACK);
        g.drawRect(0, 0, width, height);	//黑色的矩形边框

        //把随机字符串写入矩形中
        String msg = getRandomStr();

        //**把自动生成的验证码放入session域中
        //**用于Servlet验证登录
        HttpSession session = request.getSession();
        session.setAttribute("autoCode",msg);

        if(msg != null){
            for(int i = 0;i<msg.length();i++){
                char x = msg.charAt(i);
                g.setColor( getTandomColor() );//设置随机颜色
                int size = fontSize + random.nextInt(15); //设置字体大小
                g.setFont(new Font("黑体",Font.BOLD,size)); //设置字体
                g.drawString(x+" ",10+(i*25) ,size); //将字符写在矩形中
            }
        }

        //绘制干扰线
        for(int i = 0;i<grx;i++){
            //随机一个颜色
            g.setColor(getTandomColor());
            //随机位置
            g.drawLine(random.nextInt(width),random.nextInt(height) ,random.nextInt(width),random.nextInt(height));
        }

        //将image对象显示在页面中
        response.setContentType("image/png");
        ServletOutputStream os = response.getOutputStream();
        ImageIO.write(image, "png", os);
        os.flush();
        os.close();
    }


    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // TODO Auto-generated method stub
        doGet(request, response);
    }
}
```

