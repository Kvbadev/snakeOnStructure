#include <unistd.h>
#include <iostream>
#include <time.h>
#include <vector>
#include <iterator>
#include "../kbhit/kbhit.h"


struct point{
    int x; //coordinate x
    int y; //coordinate y
    char ch; //symbol
    char d='0'; //direction
};

typedef std::vector<point> ssnake;
bool isSnake(ssnake s,int num, int width, bool isIncluded);
void show(unsigned int width, unsigned int height, std::vector<point>&snake, point apple);
void cord(ssnake s, point apple);
void add(std::vector<point>&s);
void move(std::vector<point>&s, char dir);
point genFood(ssnake s, int width, int height);
void scan(ssnake &s, int width, int height, point &apple);
void gameOver(int variant);
char changeDirection(ssnake s);

int main(){

    srand(time(NULL));

    //board parameters
    int width=20;
    int height=10;

    std::cout<<"Welcome in simple snake game!"<<std::endl;
    std::cout<<"Choose your layout!"<<std::endl;
    std::cout<<"\tWIDTH: ";
    std::cin>>width;
    std::cout<<"\tHEIGHT: ";
    std::cin>>height;

    //snake structure and vector initialization
    point snakeHead = {5,5,'@','R'};
    std::vector<point> ss;
    ss.push_back(snakeHead);

    //generate food
    point apple = genFood(ss,width, height);

    while(true){
        move(ss,changeDirection(ss)); //logic of snake
        show(width,height,ss,apple); //display game
        cord(ss,apple); //shows info about snake
        scan(ss,width,height,apple); //checks if snake hit border or apple
        usleep(300000); //0.5 second
    }

    std::cout<<'\n';
    return 0;
}

void scan(ssnake &s, int width, int height, point &apple){
    if(s[0].x == width || s[0].x==0 || s[0].y==height || s[0].y<1){
        gameOver(1);
    }
    else if(apple.x==s[0].x&&apple.y==s[0].y){
        add(s);
        apple = genFood(s,width,height);
    }
    /* for(int i=0;i<width*height;i++) */
    else if(isSnake(s,(s[0].x+(s[0].y*width)),width,1))
        gameOver(2);

}

point genFood(ssnake s, int width, int height){
    int x,y;
    do{
         x = rand()%width-1;
         y = rand()%height-1;
    }while(isSnake(s,(x+(y*height-1)),width,0) || x<=1 || y<=1);
    return {x,y,'O'};
}



bool isSnake(ssnake s,int num, int width, bool isIncluded){
    ssnake::iterator it;
    if(!isIncluded)
        it = s.begin();
    else
        it = s.begin()+1;
    for(it; it!=s.end(); it++){
        if((it->x+(it->y)*width)==num){
            return true;
        }
    }
    return false;

}


void show(unsigned int width, unsigned int height, std::vector<point>&snake, point apple){
    system("clear");

    for(int i=1;i<=width*height;i++){

        if(i>width-1&&i%width==0) std::cout<<'|';
        if(i<width) std::cout<<'-';
        else if(i>(width*height)-width&&i<width*height) std::cout<<'-';

        else{
            if(i%width==0&&i<width*height){
                std::cout<<'\n';
            }
            else{
                if(i%width==0&&i<width*height)
                    std::cout<<'\n';

                else if(isSnake(snake, i, width,0))
                    std::cout<<snake[0].ch;

                else if(apple.x+(apple.y*width)==i)
                    std::cout<<apple.ch;

                else std::cout<<' ';
            }
        }
    }
}

void cord(ssnake s, point apple){
    int j=0;
    std::cout<<std::endl<<"size: "<<s.size()<<std::endl;
    if(s[0].d=='R')
        std::cout<<"direction: "<<s[0].d<<"(ight)"<<std::endl;
    else if(s[0].d=='L')
        std::cout<<"direction: "<<s[0].d<<"(eft)"<<std::endl;
    else if(s[0].d=='T')
        std::cout<<"direction: "<<s[0].d<<"(op)"<<std::endl;
    else if(s[0].d=='B')
        std::cout<<"direction: "<<s[0].d<<"(ottom)"<<std::endl;
    for(auto i=s.begin();i<s.end();i++){

        if(j==0){
            std::cout<<"Snake head: "<<"( "<<i->x<<","<<i->y<<" )"<<std::endl;
            j++;
        }
        else
            std::cout<<"element["<<j++<<"]  = "<<"( "<<i->x<<","<<i->y<<")"<<std::endl;
    }
    std::cout<<std::endl<<"apple: "<<"( "<<apple.x<<','<<apple.y<<" )"<<std::endl;
}

void add(std::vector<point>&s){
    s.push_back({s.at(s.size()-1).x,s.at(s.size()-1).y,'@'});
}

char changeDirection(ssnake s){
    keyboard kb;
    if(kb.kbhit()){
        char key = (char)kb.getch();
switch(key){
            case 'w':
                if(s[0].d!='B')
                    return 'T';
                break;
            case 's':
                if(s[0].d!='T')
                    return 'B';
                break;
            case 'd':
                if(s[0].d!='L')
                    return 'R';
                break;
            case 'a':
                if(s[0].d!='R')
                    return 'L';
                break;
            default:
                return s[0].d;
        }
        return s[0].d;
    }
    else
        return s[0].d;
}

void move(std::vector<point>&s, char dir){
    int i=s.size();
    for(i;i>1;i--){
        s.at(i-1).x = s.at(i-2).x;
        s.at(i-1).y = s.at(i-2).y;
    }
    switch(dir){
        case 'T':
            s[0].y-=1;
            break;
        case 'B':
            s[0].y+=1;
            break;
        case 'R':
            s[0].x+=1;
            break;
        case 'L':
            s[0].x-=1;
            break;
    }
    s[0].d = dir;
}

void gameOver(int variant){
    switch(variant){

        case 1:
            system("clear");
            std::cout<<"\n\n\t\tBorder has been crossed!";
            std::cout<<"\n\n\t\t\tGame Over!"<<std::endl;
            break;
        case 2:
            system("clear");
            std::cout<<"\n\n\t\tSnake's head has touched snake's body!";
            std::cout<<"\n\n\t\t\tGame Over!"<<std::endl;
            break;
    }
    exit(0);

                                                                
