# Let's move your stick man

---

## Experiment contents

###### 1.利用长方体组合出一个3D小人，将3D小人投影到2D平面上。
###### 2.将组合出来的小人进行走路的移动，要求小人各部分之间不脱离。

###### ps:高级要求小人走路符合人的形态。


## Experiment Environment
##### win10+vs2017  opengl

## Experimental procedure
##### 1.小人的实现：

###### A.将身体分成各个部分，每个部分单独画，首先将人体          分为，头，脖子，上体，胳膊和大腿。其中最重要的          要实现，即能实现出走路有人的形态，最重要的是将          胳膊和大腿分为大小臂。

###### B.因为我们知道在OPenGL 中画图的时候，是有坐标原点的，我们必须相对于坐标原点来进行画图，这时候对于每一个部分分开画图，最大的一个挑战性就是需要判断每一个部分的相对位置。而这种相对位置的设定，需要一点点的试。

### body的代码（其余部分基本相同） 
```

void DrawBody() {
	glColor3f(0.0f, 1.0f, 1.0f);
	glBegin(GL_QUADS);
	//forward           
	glVertex3f(-0.5f, 1.0f, 0.25f);
	glVertex3f(0.5f, 1.0f, 0.25f);
	glVertex3f(0.5f, -1.0f, 0.25f);
	glVertex3f(-0.5f, -1.0f, 0.25f);
	//left  
	glVertex3f(0.5f, 1.0f, 0.25f);
	glVertex3f(0.5f, 1.0f, -0.25f);
	glVertex3f(0.5f, -1.0f, -0.25f);
	glVertex3f(0.5f, -1.0f, 0.25f);
	//back  
	glVertex3f(0.5f, 1.0f, -0.25f);
	glVertex3f(-0.5f, 1.0f, -0.25f);
	glVertex3f(-0.5f, -1.0f, -0.25f);
	glVertex3f(0.5f, -1.0f, -0.25f);
	//right  
	glVertex3f(-0.5f, 1.0f, 0.25f);
	glVertex3f(-0.5f, 1.0f, -0.25f);
	glVertex3f(-0.5f, -1.0f, -0.25f);
	glVertex3f(-0.5f, -1.0f, 0.25f);
	//top  
	glVertex3f(0.5f, 1.0f, 0.25f);
	glVertex3f(0.5f, 1.0f, -0.25f);
	glVertex3f(-0.5f, 1.0f, -0.25f);
	glVertex3f(-0.5f, 1.0f, 0.25f);
	//bottom  
	glVertex3f(0.5f, -1.0f, 0.25f);
	glVertex3f(0.5f, -1.0f, -0.25f);
	glVertex3f(-0.5f, -1.0f, -0.25f);
	glVertex3f(-0.5f, -1.0f, 0.25f);
	glEnd();
}

```

### ！！虽然有了画各部分的函数，那么如何在什么时候调用这些函数即坐标轴原点在什么位置的时候调用这个函数呢？

###### 在这里代码有点多，就不粘贴了，简单而言，就是有一个转换坐标轴的堆栈，里面放着对坐标轴的操作。其中包括 translatef 以及 rotatef两个函数，分别平移与旋转坐标轴从而达到各种物体的运动，注意，后入栈的实在先入栈的坐标轴运算的基础之上。


### 小人的运动

##### 小人的转动以及平移
```
  glLoadIdentity();
	        glTranslatef(0.0f, 0.0f, -40.0f);//调整深度
	        glRotatef(angle, 0, 1, 0);    //让小人绕Y转动
	        glPushMatrix();

        	glTranslatef(0.0f, 0.0f, -20.0f);  //去掉不移动，我也不知道为什么(现在知道了，改变坐标轴的位置，方便旋转)
        	glRotatef(-90, 0, 1, 0);     //让小人转动
	        glPushMatrix();
```

#### 小人大小腿的旋转

```
glTranslatef(0.0, -0.8, 0.0);
	glTranslatef(0.0, -0.8, 0.0);
	//glRotatef(-angleofrleg, 1, 0, 0);
	if (angleofrleg <- 10)
	{

	}
	else
	{
		glRotatef(angleofrlegs, 1, 0, 0);
	}

	DrawLegS();          //大腿加小腿
	glPopMatrix();
	DrawLeg();

	glPopMatrix();
	
	
	DrawWaise();
	glPopMatrix();
	DrawBody();
	glPopMatrix();
	glPopMatrix();


	angleoflarma += a;
	angleofrarma -= a;
	angleofrarmb += b;
	angleoflarmb -= b;
	angleoflleg -= b;
	angleofrleg += b;
	angleofrlegs += c;
	angleofllegs -= c;
	angle += 0.1;

	if (angleoflarma > 50) {
		a = -1.0;
		b = -0.5;
	}
	if (angleoflarma < -50) {
		a = 1.0;
		b = 0.5;
	}

	if (angleofrlegs > 25)
	{
		c = -0.5;
	}

	if (angleofrlegs <-25)
	{
		c = 0.5;
	}
	glutSwapBuffers();
}

```


### 如何让一个小人走起来有人的形态呢？

##### 1.其实在代码中可以看出，我在小人的大臂和小臂的旋转的时候，进行了控制，以前小人的大小臂的旋转是自由旋转，在此基础上，在手臂后后甩时，控制小臂的旋转幅度让其小于大臂的旋转幅度；在腿前身时控制小腿的旋转幅度小于大腿，这样人走起来与人的形态相似。


### 小人图片

[/images/smallman1.png]






