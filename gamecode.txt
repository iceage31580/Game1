#include <fstream>
#include <iostream>
#include <conio.h>
#include <dos.h>
#include <windows.h>
#include <time.h>

#define SCREEN_WIDTH 100
#define SCREEN_HEIGHT 30
#define WIN_WIDTH 80


using namespace std;

HANDLE console = GetStdHandle(STD_OUTPUT_HANDLE);
COORD CursorPosition;

int coinY[3];
int coinX[3];
int coinFlag[3];
int spikeY[3];
int spikeX[3];
int spikeFlag[3];
char warrior [6][6]  {'|','*','0','*',' ', ' ',
                      '|' , '/' ,'|','\\',' ', ' ',
                      '|','/' ,'|','\\',' ', ' ',
                      '*', '|','|','|',' ', ' ',
                      ' ','-',' ','-',' ', ' ',
                       '_'  , '_' , '_'  , '_'  ,'_', ' ',
                      };
int warriorPos = WIN_WIDTH/2;
int score = 0;

//Declarations

void gotoxy(int x, int y) {
    CursorPosition.X = x;
    CursorPosition.Y = y;
    SetConsoleCursorPosition(console, CursorPosition);
}

//Positions

void setcursor (bool visible, DWORD size) {
    if (size == 0)
        size = 20;

        CONSOLE_CURSOR_INFO lpCursor;
        lpCursor.bVisible = visible;
        lpCursor.dwSize = size;
        SetConsoleCursorInfo(console,&lpCursor);
}

//cursor

void drawBorder () {
    for(int i=0; i<SCREEN_HEIGHT; i++) {
        for (int j = 0; j<13; j++){
            gotoxy(0+j,i); cout << "+";
            gotoxy(WIN_WIDTH-j,i); cout << "+";
            }
        }
        for (int i=0; i<SCREEN_HEIGHT; i++){
            gotoxy(SCREEN_WIDTH,i); cout << "+";
        }
}

//Border

void genCoin (int ind){
    coinX[ind] = 17 + rand()%(33);
}

//random coin generator

void drawCoin (int ind) {
    if(coinFlag[ind] == true){
        SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), FOREGROUND_GREEN);
        gotoxy(coinX[ind],  coinY[ind]);   cout << "0";
    }
}

//outputs coin

void eraseCoin (int ind) {
    if(coinFlag[ind] == true){
        gotoxy(coinX[ind], coinY[ind]);   cout << "    ";
    }
}

//outputs coin erasure

void resetCoin(int ind) {
    eraseCoin(ind);
    coinY[ind] = 1;
    genCoin(ind);
}

//restarts cycle

void genSpike (int ind){
    spikeX[ind] = 17 + rand()%(33);
}

//random spike generator

void drawSpike (int ind) {
    if(spikeFlag[ind] == true){
        gotoxy(spikeX[ind], spikeY[ind]);   cout << "  _ ";
        gotoxy(spikeX[ind], spikeY[ind]+1); cout << " |_|";
        gotoxy(spikeX[ind], spikeY[ind]+2); cout << "  V ";
        gotoxy(spikeX[ind], spikeY[ind]+3); cout << "    ";
    }
}

//outputs spike

void eraseSpike (int ind) {
    if(spikeFlag[ind] == true){
        gotoxy(spikeX[ind], spikeY[ind]);   cout << "    ";
        gotoxy(spikeX[ind], spikeY[ind]+1); cout << "    ";
        gotoxy(spikeX[ind], spikeY[ind]+2); cout << "    ";
        gotoxy(spikeX[ind], spikeY[ind]+3); cout << "    ";
    }
}

//outputs coin erasure

void resetSpike(int ind) {
    eraseSpike(ind);
    spikeY[ind] = 1;
    genSpike(ind);
}

//restarts cycle


void drawWarrior() {
    for(int i=0; i<6; i++){
        for(int j=0; j<6; j++){
            gotoxy(j+warriorPos, i+22); cout << warrior[i][j];
        }
    }
}

//outputs 2d array

void eraseWarrior() {
    for(int i=0; i<6; i++) {
        for(int j=0; j<6; j++){
            gotoxy(j+warriorPos, i+22); cout << " ";
        }
    }
}

//deletes output array


int pierce ()
{
    if(spikeY[0]+4 >= 23){
        if (spikeX[0] + 6 - warriorPos >= 0 && spikeX[0] + 6 - warriorPos < 9 ) {
            return 1;

        }
    }
    return 0;
}

//gameover condition created

int collect()
{
    if(coinY[0]+4 >= 23){
        if (coinX[0] + 6 - warriorPos >= 0 && coinX[0] + 6 - warriorPos < 9 ) {
            return 1;

        }
    }
    return 0;
}

//point creator

void music () {
//130 BPM
//1 quarter note = 462 ms
//1 half note = 924ms
//C4 261
//E4 329
//G4 392
//G#4 415
//A4 440
//A#4 466
//B4 493
//beep(hertz, milliseconds)

//C4 E4 G4
_beep(261,462);
_beep(329,462);
_beep(392,924);

_beep(261,462);
_beep(329,462);
_beep(392,924);

//C4 E4 G#4
_beep(261,462);
_beep(329,462);
_beep(415,924);

_beep(261,462);
_beep(329,462);
_beep(415,924);

//C4 E4 A4
_beep(261,462);
_beep(329,462);
_beep(440,924);

_beep(261,462);
_beep(329,462);
_beep(440,924);

//C4 E4 A#4
_beep(261,462);
_beep(329,462);
_beep(466,924);

_beep(261,462);
_beep(329,462);
_beep(466,924);

//C4 E4 B4
_beep(261,462);
_beep(329,462);
_beep(493,924);

_beep(261,462);
_beep(329,462);
_beep(493,924);

//C4 E4 A#4
_beep(261,462);
_beep(329,462);
_beep(466,924);

_beep(261,462);
_beep(329,462);
_beep(466,924);

//C4 E4 A4
_beep(261,462);
_beep(329,462);
_beep(440,924);

_beep(261,462);
_beep(329,462);
_beep(440,924);

//C4 E4 G#4
_beep(261,462);
_beep(329,462);
_beep(415,924);

_beep(261,462);
_beep(329,462);
_beep(415,924);

//C4 E4 G4
_beep(261,462);
_beep(329,462);
_beep(392,924);

_beep(261,462);
_beep(329,462);
_beep(392,924);

}

//play game over theme

void gameover(){
    system("cls");

    cout << endl;
    cout << "\t\t---------------------------------"<< endl;
    cout << "\t\t---------------------------------"<< endl;
    cout << "\t\t----------- Game Over -----------"<< endl << endl;
    cout << "\t\t Press any key to go back to menu.";



    music();
    getch();
}

//gameover screen

void updateScore(){
    gotoxy(WIN_WIDTH + 7, 5); cout << "Score: "<<score<<endl;
}

//score function

void instruction () {

system ("cls");
cout << "Instructions";
cout << "\n----------";
cout << "\n Avoid the spikes and collect dimensionium by moving left or right. ";
cout << "\n\n Press 'a' to move left";
cout << "\n Press 'd' to move right";
cout << "\n Press 'escape' to exit";
cout << "\n\n Press any key to go back to menu";
getch();
}

//instructions

void saveFile(){
    ofstream outputFile;
    outputFile.open("savefile.txt");

     if(outputFile.fail())
    {
        cout << "Cannot Open Output File!" << endl;
        exit(1);
    }

    outputFile << score;
}

//save file, outputs score to output File


void play() {
    warriorPos = -1 + WIN_WIDTH/2;
    score = 0;
    coinFlag[0] = 1;
    coinFlag[1] = 0;
    coinY[0] = coinY[1] = 1;
    spikeFlag[0] = 1;
    spikeFlag[1] = 0;
    spikeY[0] = spikeY[1] = 1;

    system("cls");
    drawBorder();
    updateScore();
    genCoin(0);
    genCoin(1);
    genSpike(0);
    genSpike(1);

    gotoxy(WIN_WIDTH + 7,2); cout << "     ALIEN WARRIOR     ";
    gotoxy(WIN_WIDTH + 6,4); cout << "- - - - - - - - - - - -";
    gotoxy(WIN_WIDTH + 6,6); cout << "- - - - - - - - - - - -";
    gotoxy(WIN_WIDTH + 7,12);cout << "Controls";
    gotoxy(WIN_WIDTH + 7,13);cout << "- - - - - - - - - - - -";
    gotoxy(WIN_WIDTH + 2,14);cout << " A Key - Left";
    gotoxy(WIN_WIDTH + 2,15);cout << " D Key - Right";

    gotoxy(18, 5); cout << "Press any key to start";
    getch();
    gotoxy(18, 5); cout << "                      ";

    while(1){
        if(kbhit()){ //kbhit listens to key strokes. When you hit any key, kbhit gets involved
            char ch = getch();
            if( ch == 'a' || ch == 'A') {
                if ( warriorPos > 18 )
                     warriorPos -= 4;
            }
            if( ch == 'd' || ch == 'D') {
                if( warriorPos < 50 )
                    warriorPos +=4;
            }
        if (ch == 27) {
            break;
        }
    }

    drawWarrior();
    drawCoin(0);
    drawCoin(1);
    drawSpike(0);
    drawSpike(1);
    //music();
    if ( collect() == 1) {
        score += 4;
    if(pierce () == 1){
        gameover();
        saveFile();
        return;
    }

    }
    Sleep(50);
    eraseWarrior();
    eraseCoin(0);
    eraseCoin(1);
    eraseSpike(0);
    eraseSpike(1);
    //music();

    if( spikeY[0] == 10 )
        if ( spikeFlag[1] == 0 )
             spikeFlag[1] =1;

        if ( spikeFlag[0] == 1 )
            spikeY[0] += 1;

        if ( spikeFlag[1] == 1 )
            spikeY[1] += 1;

        if( spikeY[0] > SCREEN_HEIGHT-4) {
            resetSpike(0);
            score++;
            updateScore();
        }
        if( spikeY[1] > SCREEN_HEIGHT-4){
            resetSpike(1);
            score++;
            updateScore();
        }

        if( coinY[0] == 10 )
        if ( coinFlag[1] == 0 )
             coinFlag[1] =1;

        if ( coinFlag[0] == 1 )
            coinY[0] += 1;

        if ( coinFlag[1] == 1 )
            coinY[1] += 1;

        if( coinY[0] > SCREEN_HEIGHT-4) {
            resetCoin(0);
            score++;
            updateScore();
        }
        if( coinY[1] > SCREEN_HEIGHT-4){
            resetCoin(1);
            score++;
            updateScore();
        }


    }

}

//culmination to create game play

void story(){
SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), FOREGROUND_BLUE);
//SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), FOREGROUND_RED);\
////The year is 2091. A profession called coin running has become popular. It involves running through a vacuum dodging spikes and collecting a rare precious metal named dimensionium for its use in interdimensional travel. Good Luck to all runners!

system("cls");

gotoxy(0,0);
cout <<"\n\n\n\n";
cout << "T"; Sleep(650);
cout << "h"; Sleep(650);
cout << "e"; Sleep(650);
cout << " "; Sleep(650);
cout << "y"; Sleep(650);
cout << "e"; Sleep(650);
cout << "a"; Sleep(650);
cout << "r"; Sleep(650);
cout << " "; Sleep(650);
cout << "i"; Sleep(650);
cout << "s"; Sleep(650);
cout << " "; Sleep(650);
cout << "2"; Sleep(650);
cout << "0"; Sleep(650);
cout << "9"; Sleep(650);
cout << "1"; Sleep(650);
cout << "."; Sleep(650);
cout << endl; Sleep(700);
cout << "A"; Sleep(650);
cout << " "; Sleep(650);
cout << "p"; Sleep(650);
cout << "r"; Sleep(650);
cout << "o"; Sleep(650);
cout << "f"; Sleep(650);
cout << "e"; Sleep(650);
cout << "s"; Sleep(650);
cout << "s"; Sleep(650);
cout << "i"; Sleep(650);
cout << "o"; Sleep(650);
cout << "n"; Sleep(650);
cout << " "; Sleep(650);
cout << "c"; Sleep(650);
cout << "a"; Sleep(650);
cout << "l"; Sleep(650);
cout << "l"; Sleep(650);
cout << "e"; Sleep(650);
cout << "d"; Sleep(650);
cout << " "; Sleep(650);
cout << "c"; Sleep(650);
cout << "o"; Sleep(650);
cout << "i"; Sleep(650);
cout << "n"; Sleep(650);
cout << " "; Sleep(650);
cout << "r"; Sleep(650);
cout << "u"; Sleep(650);
cout << "n"; Sleep(650);
cout << "n"; Sleep(650);
cout << "i"; Sleep(650);
cout << "n"; Sleep(650);
cout << "g"; Sleep(650);
cout << " "; Sleep(650);
cout << "h"; Sleep(650);
cout << "a"; Sleep(650);
cout << "s"; Sleep(650);
cout << " "; Sleep(650);
cout << "b"; Sleep(650);
cout << "e"; Sleep(650);
cout << "c"; Sleep(650);
cout << "o"; Sleep(650);
cout << "m"; Sleep(650);
cout << "e"; Sleep(650);
cout << " "; Sleep(650);
cout << "p"; Sleep(650);
cout << "o"; Sleep(650);
cout << "p"; Sleep(650);
cout << "u"; Sleep(650);
cout << "l"; Sleep(650);
cout << "a"; Sleep(650);
cout << "r"; Sleep(650);
cout << "."; Sleep(650);
cout << endl;Sleep(700);
cout << "I"; Sleep(650);
cout << "t"; Sleep(650);
cout << " "; Sleep(650);
cout << "i"; Sleep(650);
cout << "n"; Sleep(650);
cout << "v"; Sleep(650);
cout << "o"; Sleep(650);
cout << "l"; Sleep(650);
cout << "v"; Sleep(650);
cout << "e"; Sleep(650);
cout << "s"; Sleep(650);
cout << " "; Sleep(650);
cout << "r"; Sleep(650);
cout << "u"; Sleep(650);
cout << "n"; Sleep(650);
cout << "n"; Sleep(650);
cout << "i"; Sleep(650);
cout << "n"; Sleep(650);
cout << "g"; Sleep(650);
cout << " "; Sleep(650);
cout << "t"; Sleep(650);
cout << "h"; Sleep(650);
cout << "r"; Sleep(650);
cout << "o"; Sleep(650);
cout << "u"; Sleep(650);
cout << "g"; Sleep(650);
cout << "h"; Sleep(650);
cout << " "; Sleep(650);
cout << "a"; Sleep(650);
cout << " "; Sleep(650);
cout << "v"; Sleep(650);
cout << "a"; Sleep(650);
cout << "c"; Sleep(650);
cout << "u"; Sleep(650);
cout << "u"; Sleep(650);
cout << "m"; Sleep(650);
cout << " "; Sleep(650);
cout << "d"; Sleep(650);
cout << "o"; Sleep(650);
cout << "d"; Sleep(650);
cout << "g"; Sleep(650);
cout << "i"; Sleep(650);
cout << "n"; Sleep(650);
cout << "g"; Sleep(650);
cout << " "; Sleep(650);
cout << "s"; Sleep(650);
cout << "p"; Sleep(650);
cout << "i"; Sleep(650);
cout << "k"; Sleep(650);
cout << "e"; Sleep(650);
cout << "s"; Sleep(650);
cout << " "; Sleep(650);
cout << "a"; Sleep(650);
cout << "n"; Sleep(650);
cout << "d"; Sleep(650);
cout << " "; Sleep(650);
cout << "c"; Sleep(650);
cout << "o"; Sleep(650);
cout << "l"; Sleep(650);
cout << "l"; Sleep(650);
cout << "e"; Sleep(650);
cout << "c"; Sleep(650);
cout << "t"; Sleep(650);
cout << "i"; Sleep(650);
cout << "n"; Sleep(650);
cout << "g"; Sleep(650);
cout << " "; Sleep(650);
cout << "a"; Sleep(650);
cout << " "; Sleep(650);
cout << "r"; Sleep(650);
cout << "a"; Sleep(650);
cout << "r"; Sleep(650);
cout << "e"; Sleep(650);
cout << " "; Sleep(650);
cout << "p"; Sleep(650);
cout << "r"; Sleep(650);
cout << "e"; Sleep(650);
cout << "c"; Sleep(650);
cout << "i"; Sleep(650);
cout << "o"; Sleep(650);
cout << "u"; Sleep(650);
cout << "s"; Sleep(650);
cout << " "; Sleep(650);
cout << "m"; Sleep(650);
cout << "e"; Sleep(650);
cout << "t"; Sleep(650);
cout << "a"; Sleep(650);
cout << "l"; Sleep(650);
cout << " "; Sleep(650);
cout << "n"; Sleep(650);
cout << "a"; Sleep(650);
cout << "m"; Sleep(650);
cout << "e"; Sleep(650);
cout << "d"; Sleep(650);
cout << " "; Sleep(650);
cout << "d"; Sleep(650);
cout << "i"; Sleep(650);
cout << "m"; Sleep(650);
cout << "e"; Sleep(650);
cout << "n"; Sleep(650);
cout << "s"; Sleep(650);
cout << "i"; Sleep(650);
cout << "o"; Sleep(650);
cout << "n"; Sleep(650);
cout << "u"; Sleep(650);
cout << "m"; Sleep(650);
cout << " "; Sleep(650);
cout << "f"; Sleep(650);
cout << "o"; Sleep(650);
cout << "r"; Sleep(650);
cout << " "; Sleep(650);
cout << "i"; Sleep(650);
cout << "t"; Sleep(650);
cout << "s"; Sleep(650);
cout << " "; Sleep(650);
cout << "u"; Sleep(650);
cout << "s"; Sleep(650);
cout << "e"; Sleep(650);
cout << " "; Sleep(650);
cout << "i"; Sleep(650);
cout << "n"; Sleep(650);
cout << " "; Sleep(650);
cout << "i"; Sleep(650);
cout << "n"; Sleep(650);
cout << "t"; Sleep(650);
cout << "e"; Sleep(650);
cout << "r"; Sleep(650);
cout << "d"; Sleep(650);
cout << "i"; Sleep(650);
cout << "m"; Sleep(650);
cout << "e"; Sleep(650);
cout << "n"; Sleep(650);
cout << "s"; Sleep(650);
cout << "i"; Sleep(650);
cout << "o"; Sleep(650);
cout << "n"; Sleep(650);
cout << "a"; Sleep(650);
cout << "l"; Sleep(650);
cout << " "; Sleep(650);
cout << "t"; Sleep(650);
cout << "r"; Sleep(650);
cout << "a"; Sleep(650);
cout << "v"; Sleep(650);
cout << "e"; Sleep(650);
cout << "l"; Sleep(650);
cout << "."; Sleep(650);
cout << endl;Sleep(700);
cout << "G"; Sleep(650);
cout << "o"; Sleep(650);
cout << "o"; Sleep(650);
cout << "d"; Sleep(650);
cout << " "; Sleep(650);
cout << "L"; Sleep(650);
cout << "u"; Sleep(650);
cout << "c"; Sleep(650);
cout << "k"; Sleep(650);
cout << " "; Sleep(650);
cout << "t"; Sleep(650);
cout << "o"; Sleep(650);
cout << " "; Sleep(650);
cout << "a"; Sleep(650);
cout << "l"; Sleep(650);
cout << "l"; Sleep(650);
cout << " "; Sleep(650);
cout << "r"; Sleep(650);
cout << "u"; Sleep(650);
cout << "n"; Sleep(650);
cout << "n"; Sleep(650);
cout << "e"; Sleep(650);
cout << "r"; Sleep(650);
cout << "s"; Sleep(650);
cout << "!"; Sleep(650);
cout << endl;
cout << "\n Press 'escape' to exit";
cout << "\n\n Press any key to go back to menu";
getch();

}

//story

int main()
{
    setcursor(0,0);
    srand((unsigned)time(NULL));



    do {
        system("cls");
        SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), FOREGROUND_RED);
        gotoxy(10,5); cout << "------------------------";
        gotoxy(10,6); cout << " |    Alien Warrior    |";
        gotoxy(10,7); cout << "------------------------";
        gotoxy(10,9); cout << "1. Start Game ";
        gotoxy(10,10); cout <<"2. Instructions";
        gotoxy(10,11); cout <<"3. Story";
        gotoxy(10,12); cout <<"4. Quit";
        char op = getche();

        if( op == '1') play();
        else if( op == '2') instruction();
        else if( op == '3') story();
        else if( op == '4') exit(0);


        } while(1);


        return 0;

    }

//Works Cited
//https://onecompiler.com/cpp/3whnsgbmf
//https://forums.hak5.org/topic/9298-music-in-c/

