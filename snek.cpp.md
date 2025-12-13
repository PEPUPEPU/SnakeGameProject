#include <iostream>
#include <fstream>
#include <conio.h>
#include <windows.h>
using namespace std;

bool game_over;
int width = 20;
int height = 20;
int x, y, fruitx, fruity;
int dir;
int tailx[100];
int taily[100];
int ntail;

struct scoreentry{
    string name;
    int score;
};

scoreentry scores[100];
int scorecount;

void setup(int* score_ptr){
    game_over = false;
    dir = 0;
    x = width / 2;
    y = height / 2;
    fruitx = rand() % width;
    fruity = rand() % height;
    *score_ptr = 0;
    ntail = 0;
}

void drawboard(){
    for (int i = 0; i < width + 2; i++) 
        cout << "#";
    cout << "\n";
}

void drawgame(int* score_ptr){
    system("cls");
    drawboard();
    
    for (int i = 0; i < height; i++){
        cout << "#";
        for (int j = 0; j < width; j++){
            if (i == y && j == x)
                cout << "0";
            else if (i == fruity && j == fruitx)
                cout << "*";
            else {
                bool print = false;
                for (int k = 0; k < ntail; k++){
                    if (tailx[k] == j && taily[k] == i){
                        cout << "o";
                        print = true;
                        break;
                    }
                }
                if (!print) 
                    cout << " ";
            }
        }
        cout << "#\n";
    }
    
    drawboard();
    
    cout << "Score: " << *score_ptr << "\n";
    
    loadscore();
    setupscores();
    cout << "Top 3:\n";
    for (int i = 0; i < 3 && i < scorecount; i++){
        cout << scores[i].name << ": " << scores[i].score << "\n";
    }
}

void loadscore(){
    ifstream file("snake_score.txt");
    scorecount = 0;
    if (!file.is_open()) return;
    while (scorecount<100 && file >> scores[scorecount].name >> scores[scorecount].score){
        scorecount++;
    }
    file.close();
}

void savescore(string name, int* score){
    ofstream file("snake_score.txt", ios::app);
    file << name << " " << *score << "\n";
    file.close();
}

void setupscores(){
    for (int i = 0; i < scorecount - 1; i++){
        for (int j = 0; j < scorecount - i - 1; j++){
            if (scores[j].score < scores[j + 1].score){
                scoreentry temp = scores[j];
                scores[j] = scores[j + 1];
                scores[j + 1] = temp;
            }
        }
    }
}

void showscoreboard(){
    loadscore();
    setupscores();
    cout << "\n--TOP SCORES--\n";
    for (int i = 0; i < scorecount && i < 10; i++){
        cout << i + 1 << ") " << scores[i].name << " = " << scores[i].score << "\n";
    }
    cout << "\n";
}

void input(){
    if (_kbhit()){
        switch (_getch()){
            case 'w':
                if (dir != 2) 
                    dir = 1;
                break;
            case 's':
                if (dir != 1) 
                    dir = 2;
                break;
            case 'a':
                if (dir != 4) 
                    dir = 3;
                break;
            case 'd':
                if (dir != 3) 
                    dir = 4;
                break;
            case 'x':
                game_over = true;
                break;
        }
    }
}