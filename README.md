# game
扫雷小游戏
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
#define EASY_COUNT 10

#define ROW 9
#define COL 9

#define ROWS ROW+2
#define COLS COL+2

void initboard(char board[ROWS][COLS],int rows,int cols,char set){
    int i,j;
    for(i = 0;i < rows;i++){
        for(j = 0;j < cols;j++){
            board[i][j] = set; 
        }
    }
}

void displayboard(char board[ROWS][COLS],int row,int col){
    int i,j;
    printf("--------------------------------\n");
    for(i = 0;i <= col;i++){
        printf("%d ",i);
    }
    printf("\n");
    for(i = 1;i <= row;i++){
        printf("%d ",i);
        for(j = 1;j <= col;j++){
            printf("%c ",board[i][j]);
        }
        printf("\n");
    }
    printf("--------------------------------\n");
}

void setmine(char mine[ROWS][COLS],int row,int col,int count){
    while(count){
        int x = rand()%row+1;
        int y = rand()%col+1;
        if(mine[x][y] == '0'){//座标处不是雷
            mine[x][y] = '1';
            count--;
        }
    }
}
//统计mine数组x,y坐标周围有几个雷
int getmine(char mine[ROWS][COLS],int x,int y){
    //'1'-'0'= 0(数字)，'0'-'0'=0(数字)，只需将八个周围相加即可
    return mine[x-1][y]+mine[x-1][y-1]+mine[x-1][y+1]+mine[x][y-1]+
    mine[x][y+1]+mine[x+1][y-1]+mine[x+1][y]+mine[x+1][y+1]-8*'0';
}

void findmine(char mine[ROWS][COLS],char show[ROWS][COLS],int row,int col){
    int win = 0;
    while(win < row*col-EASY_COUNT){
        printf("please input the coodinates:>");
        int x = 0;
        int y = 0;
        scanf("%d%d",&x,&y);
        //1.坐标合法性 2.排雷
        if(x >= 1 && x <= row && y >= 1 && y <= col){
            if(mine[x][y] == '1'){
                printf("sorry,you are boom\n");
                displayboard(mine,row,col);
                break;
            }
            else{
                int count = getmine(mine,x,y);
                show[x][y] = count+'0';
                displayboard(show,row,col);
                win++;
            }
        }
        else{
            printf("illegal,please input again:>");
        }
    }
    if(win == row*col-EASY_COUNT){
        printf("congratulate! you win\n");
        displayboard(mine,row,col);
    }
}

void menu(){
    printf("***************************\n");
    printf("**********  1.play ********\n");
    printf("**********  0.exit ********\n");
    printf("***************************\n");
}
void game( ){
    //创建对应数组
    char mine[ROWS][COLS];
    char show[ROWS][COLS];
    initboard(mine,ROWS,COLS,'0');
    initboard(show,ROWS,COLS,'*');
    //打印棋盘
    //displayboard(mine,ROW,COL);
    displayboard(show,ROW,COL);
    //布置雷
    setmine(mine,ROW,COL,EASY_COUNT);
    //排查雷
    findmine(mine,show,ROW,COL);
}
int main( ){
    srand((unsigned int)time(NULL));
    int input = 0;
    do{
        menu();
        printf("please choose:>");
        scanf("%d",&input);
        switch(input){
            case 1:
                game( );//三子棋游戏
                break;
            case 0:
                printf("quit a game\n");//退出游戏
                break;
            default:
                printf("please input again\n");//重新输入
                break;
        }
    }
    while(input);
    return 0;
}
