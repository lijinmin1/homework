# Greedy Snake 实验报告  
## 一、实验目的
### 1.了解字符游戏的表示
### 2.体验自顶向下的设计方法实现问题求解
### 3.使用伪代码表示算法
### 4.使用函数抽象过程  
## 二、游戏要求与表示
###  1、玩法
贪吃蛇游戏是一款经典的益智游戏，有PC和手机等多平台版本。既简单又耐玩。该游戏通过控制蛇头方向吃蛋，从而使得蛇变得越来越长。百度百科  
### 2、游戏表示
给定一个10*10的字符矩阵表示蛇的生存空间,其中有一条长度5的蛇(HXXXX), “H”表示蛇头,“X”表示蛇身体。空间中可能有食物（“$”表示）和障碍物（“*”表示）  
你可以使用“ADWS”按键分别控制蛇的前进方向“左右上下”, 当蛇头碰到自己的身体或走出边界,游戏结束,否则蛇按你指定方向前进一步。  
## 三、编程要求
### 1、任务1：会动的蛇       

   

     #include <stdio.h>  
     #include <stdlib.h>  
     #include <time.h>  

     #define SNAKE_MAX_LENGTH 20            //蛇的最大长度   
     #define SNAKE_HEAD 'H'   
     #define SNAKE_BODY 'X'    
     #define	BLANK_CELL ' '  
     #define SNAKE_FOOD '$'  
     #define WALL_CELL '*'  

      //snake stepping:dy=-1(up),1(down); dx=-1(left),1(right);       
      dx/dy = 0(no move);   
      void snakeMove(int ,int );  
      //put a food randomized on a blank cell  
      void putMoney(void);  
      //out cells of the grid  
      void output();  
      //outs when gameover  
      void gameover(int ,int );  
      //determine whether the snake get the money or not  
      void getMoney(int ,int );   

      char map[12][12]=  
	  {"************",  
	   "*XXXXH       *",  
	   "*            *",                   //初始状态   
	   "*            *",                   //直接打表   
	   "*            *",    
	   "*            *",  
	   "*            *",  
	   "*            *",  
	   "*            *",  
       "*            *",  
	   "*            *",  
	    "************"};  

      //define vars for snake, notice name of vars in C	   
       int snakeX[SNAKE_MAX_LENGTH]={1,2,3,4,5};  
       int snakeY[SNAKE_MAX_LENGTH]={1,1,1,1,1};                           //蛇头和蛇身的坐标       
        int snakeLength = 5;                                          //蛇的初始长度   

     int isAlive = 1;                                                  // 等于 1 时表示蛇还存活，等于 0 时表示蛇已死亡     
      int flag=0;                                                       //等于 1 时表示吃到了 $ ,等于 0 时表示没吃到 $     

     int main(){  
	      char ch;  
	       int i,j;  

	for(i=0;i<12;i++){
		for(j=0;j<12;j++){
			if(j<11)
				printf("%c",map[i][j]);
			else if(j==11)
				printf("%c\n",map[i][j]);
		}
	}                                                                    //打印初识蛇的位置与地图 
	
	while(isAlive!=0){
		scanf("%c",&ch);
		switch(ch){
			case 'A': gameover(-1,0);                       //先判断是否触发gameover的条件   
			snakeMove(-1,0);                                  //往左移动一格 
			utput();break;                                   //打印 
			case 'D': gameover(1,0);
			 snakeMove(1,0);                                 //往右移动一格 
					  output();break;              
			case 'W': gameover(0,-1);
					  snakeMove(0,-1);               //往上移动一格 
					  output();break;
			case 'S': gameover(0,1);
					  snakeMove(0,1);                //往下移动一格 
					  output();break;
		}
		
	}
	
	    printf("Game Over!!!\n");
	
	    return 0;
     } 

    void snakeMove(int dx,int dy){
	int tmpX1=0,tmpY1=0,tmpX2=0,tmpY2=0,i,j;
	tmpX1=snakeX[snakeLength-1];
	tmpY1=snakeY[snakeLength-1];
	
	for(i=snakeLength-2;i>=0;i--){
			tmpX2=snakeX[i];
			tmpY2=snakeY[i];               //tmpX2、tmpY2用于记录蛇当前部分的坐标 
			snakeX[i]=tmpX1;
			snakeY[i]=tmpY1;              //tmpX1、tmpY1用于记录蛇上一部分的坐标 
			tmpX1=tmpX2;
			tmpY1=tmpY2;
		}                                 //将后面的蛇身往前挪一位 
	
	map[tmpY1][tmpX1]=' ';                //将末尾改为 空格 
	
	if(dx==-1&&dy==0)
		snakeX[snakeLength-1]-=1;
	else if(dx==1&&dy==0)
		snakeX[snakeLength-1]+=1;
	else if(dx==0&&dy==-1)
		snakeY[snakeLength-1]-=1;
	else if(dx==0&&dy==1)
		snakeY[snakeLength-1]+=1;		  //将蛇头按方向行进一位
			                                   
	for(i=0;i<snakeLength;i++){
		if(i<snakeLength-1)
			map[snakeY[i]][snakeX[i]]='X';
		else if(i==snakeLength-1)
			map[snakeY[i]][snakeX[i]]='H';
	}									  //将蛇头和蛇身赋值相应字符  
      }

    void gameover(int dx,int dy){  
	      if(map[snakeY[snakeLength-1]+dy][snakeX[snakeLength-1]  +dx]=='X'||map[snakeY[snakeLength-1]+dy][snakeX  [snakeLength-1]+dx]=='*')  
		    isAlive=0;  
    }                                                                //判断游戏是否结束，若结束，将 0 赋值给 isAlive 

      void output(){
	   int i=0,j=0;
	    for(i=0;i<12;i++){
		for(j=0;j<12;j++){
			if(j<11)
				printf("%c",map[i][j]);
			else if(j==11)
				printf("%c\n",map[i][j]);
		}
	}
    }




###   2、任务2：会吃的蛇  


    //snake_eat.c

    //simple snake game v0.2

    //Created by 16307015 on 18-11-29.

    //Copyright (c) 2018年 Sun Yat-Sen University.All rights reserved.



    #include<stdio.h>

    #include<stdlib.h>

    #include<time.h>



    #define SNAKE_MAX_LENGTH 20

    #define SNAKE_HEAD 'H'

    #define SNAKE_BODY 'X'

    #define BLANK_CELL ' '

    #define SNAKE_FOOD '$'

    #define WALL_CELL '*'



    //传入垂直和水平方向，蛇根据该方向上的下一物体做出不同反应

    void snakeMove(int, int);

    //上一个食物被吃后，在正确的位置上随机放置一个食物

    void put_money(void);

    //输出二维数组地图

    void output(void);

    //置alive为0杀死蛇并输出Game Over！

    void gameover(void);

    //初始化参数、蛇和地图状态

    void init(void);

    //延迟,使用time.h文件以兼容不同平台或编译器

    void delay(int);



    char map[12][12] =

    {

	"************",

	"*XXXXH     *",

	"*          *",

	"*          *",

	"*          *",

	"*          *",

	"*          *",

	"*          *",

	"*          *",

	"*          *",

	"*          *",

	"************"

    };



    int snakeX[SNAKE_MAX_LENGTH] = { 1,2,3,4,5 };

    int snakeY[SNAKE_MAX_LENGTH] = { 1,1,1,1,1 };

    //蛇的长度

    int snakeLength = 5;

    //记录当前蛇的运动方向

    int curv = 0, curh = 1;

    //蛇是否存活

    int alive = 1;

    //玩家得分

    int score = 0;



     int main() {

	     while (1) {

		   printf("按下S开始游戏:");

		   char st = getch();

		   while (st != 'S' && st != 's')

			    st=getch();

		  //初始化参数

		   init();

		//清空界面

		system("cls");

		//输出界面

		output();

		while (alive) {	

		//若键盘有输入，根据输入值进行移动并更新蛇的状态和地图

			if (kbhit()) {

				char dir = getch();

				switch (dir) {

					case 'w':

					case 'W':

					snakeMove(-1, 0);

					break;

					case 's':

					case 'S':

					snakeMove(1, 0);

					break;

					case 'a':

					case 'A':

					snakeMove(0, -1);

					break;

					case 'd':

					case 'D':

					snakeMove(0, 1);

					break;

					default:

					snakeMove(curv, curh);

				}

			}

			//否则蛇按当前方向移动

			else snakeMove(curv, curh);

			//输出界面

			system("cls");

			output();

			//延迟

			delay(150);

		}

	

	 }

    }



    //初始化游戏各参数、蛇和地图

    void init() {

	alive = 1;

	score = 0;

	curv = 0;

	curh = 1;

	snakeLength = 5;

	for (int i = 1; i < 11; ++i)

		for (int j = 1; j < 11; ++j)

			map[i][j] = ' ';

	map[1][1] = map[1][2] = map[1][3] = map[1][4] = SNAKE_BODY;

	map[1][5] = SNAKE_HEAD;

	for (int i = 0; i < 5; ++i) {

		snakeX[i] = i + 1;

		snakeY[i] = 1;

	  }

	   put_money();

    }



    //输出界面

    void output() {

	    printf("·Score:%d·\n", score);

	   for (int i = 0; i < 12; ++i) {
   
		   for (int j = 0; j < 12; ++j)

			    printf("%c", map[i][j]);

		   printf("\n");

	    }

    }



    //延迟

     void  delay(int time)//time单位为ms

     {

	   clock_t now = clock();

	   while (clock() - now < time);

     }



    //snake移动，v：垂直方向，h：水平方向

     void snakeMove(int v, int h) {

	   int newrow = snakeY[snakeLength - 1] + v, newcol = snakeX[snakeLength - 1] + h;

	   //若下一步为墙则失败，游戏结束

	    if (map[newrow][newcol] == WALL_CELL)

		    gameover();

	    //若下一步为蛇身且方向不是当前方向的反方向则失败，游戏结束

	     else if (map[newrow][newcol] == SNAKE_BODY) {

		 if (v == -curv && h == -curh)return;

		 else gameover();

	    }

	     else {

		//长度未到最大长度时继续游戏

		if (snakeLength < SNAKE_MAX_LENGTH) {

			//修改当前移动方向

			curv = v;

			curh = h;

			//如果下一步为空格时正常移动

			if (map[newrow][newcol] == BLANK_CELL) {

				map[snakeY[snakeLength - 1]][snakeX[snakeLength - 1]] = SNAKE_BODY;

				map[newrow][newcol] = SNAKE_HEAD;

				map[snakeY[0]][snakeX[0]] = BLANK_CELL;

				for (int i = 0; i < snakeLength - 1; ++i) {

					snakeX[i] = snakeX[i + 1];

					snakeY[i] = snakeY[i + 1];

				}

				snakeX[snakeLength - 1] = newcol;

				snakeY[snakeLength - 1] = newrow;

			}

			//若下一步为食物，蛇长度加一

			else if (map[newrow][newcol] == SNAKE_FOOD) {

				map[snakeY[snakeLength - 1]][snakeX[snakeLength - 1]] = SNAKE_BODY;

				map[newrow][newcol] = SNAKE_HEAD;

				map[snakeY[0]][snakeX[0]] = SNAKE_BODY;

				snakeX[snakeLength] = newcol;

				snakeY[snakeLength] = newrow;

				snakeLength += 1;

				score += 10;

				put_money();

			}

		}

		//否则停止游戏

		else {

			gameover();

			printf("Well Done But Snake Is Too Long.\n");

		   }

	     }

     }



     //随机在合适的区域放置食物

    void put_money(void) {

	    int r = rand() % 10 + 1, c = rand() % 10 + 1;

	   while (1) {

		   if (map[r][c] != SNAKE_BODY && map[r][c] != SNAKE_HEAD)break;

		   r = rand() % 10 + 1, c = rand() % 10 + 1;

	    }

	    map[r][c] = SNAKE_FOOD;

    }



    //结束游戏

     void gameover(void) {

	    alive = 0;

	    system("cls");

	    printf("Game Over!\nYou got %d.\n", score);

     }



## 四、实验总结  
总体上完成了实验目的：了解字符游戏的表示，
体验自顶向下的设计方法实现问题求解，
使用伪代码表示算法，
使用函数抽象过程。