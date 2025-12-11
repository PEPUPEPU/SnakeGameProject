#include <iostream>
#include <fstream>
#include <conio.h>
#include <windows.h>
using namespace std;

bool game_over;
int width = 20;
int height = 20;
int x, y, fruitx, fruity, score;
int dir;
int tailx[100];
int taily[100];
int ntail;

struct scoreentry {
    string name;
    int score;
};

scoreentry scores[100];
int scorecount = 0;

void setup() {
    game_over = false;
    dir = 0;
    x = width / 2;
    y = height / 2;
    fruitx = rand() % width;
    fruity = rand() % height;
    score = 0;
    ntail = 0;
}

void drawboard() {
    for (int i = 0; i < width + 2; i++) 
        cout << "#";
    cout << "\n";
}
void drawgame() {
    system("cls");
    drawboard();
    for (int i = 0; i < height; i++) {
        cout << "#";
        for (int j = 0; j < width; j++) {
            if (i == y && j == x)
                cout << "O";
            else if (i == fruity && j == fruitx)
                cout << "F";
            else {
                bool print = false;
                for (int k = 0; k < ntail; k++) {
                    if (tailx[k] == j && taily[k] == i) {
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
    cout << "Score: " << score << "\n";
    loadscore();
    setupscores();
    cout << "Top 3:\n";
    for (int i = 0; i < 3 && i < scorecount; i++) {
        cout << scores[i].name << ": " << scores[i].score << "\n";
    }
}
