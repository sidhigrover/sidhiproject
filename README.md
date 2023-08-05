

#include<iostream>
#include<map>
#include <vector>
#include <fstream>
#include <string>
#include <time.h>
#include <cstdlib>
#include <graphics.h>
#include <windows.h>
#include <conio.h>
#include <string>
using namespace std;
class TicTacToeGame
{
private:
char board[3][3];
int turn;
char currentPlayer;
bool won;
int input;
string player1;
string player2;
map<string, char> playerSymbols; // new member variable to store player symbols
public:
// Constructor to initialize the game
TicTacToeGame() {
Reset();
}
// Main game loop
void Play() {
do {
DrawBoard();
GetInput();
CheckWin();
ChangePlayer();
} while (!won && turn <= 9);
DrawBoard();
if (won) {
cout << "SORRY " << currentPlayer << " (" << GetPlayerName() << ") YOU LOST THE GAME!" << endl;
} else {
cout << "The game ended in a tie!" << endl;
}
}
private:
// Method to draw the game board

void DrawBoard() {
cout << " " << board[0][0] << " | " << board[0][1] << " | " << board[0][2] << endl;
cout << " ---|---|---" << endl;
cout << " " << board[1][0] << " | " << board[1][1] << " | " << board[1][2] << endl;
cout << " ---|---|---" << endl;
cout << " " << board[2][0] << " | " << board[2][1] << " | " << board[2][2] << endl;
}
// Method to get input from the current player
void GetInput() {
bool validInput = false;
do {
cout << GetPlayerName() << ", enter a number (1-9) to place your mark (" << playerSymbols[GetPlayerName()] << "): ";
cin >> input;
if (cin.fail()) {
cout << "Invalid input! Please enter a number (1-9) that is not already taken." << endl;
cin.clear();
continue;
}
int row = (input - 1) / 3;
int col = (input - 1) % 3;
if (input >= 1 && input <= 9 && board[row][col] != playerSymbols[player1] && board[row][col] != playerSymbols[player2]) {
board[row][col] = currentPlayer;
validInput = true;
} else {
cout << "Invalid input! Please enter a number (1-9) that is not already taken." << endl;
}
} while (!validInput);
}
// Method to check if the current player has won
void CheckWin() {
// Check rows
for (int row = 0; row < 3; row++) {
if (board[row][0] == currentPlayer && board[row][1] == currentPlayer && board[row][2] == currentPlayer) {
won = true;
return;
}
}
// Check columns
for (int col = 0; col < 3; col++) {
if (board[0][col] == currentPlayer && board[1][col] == currentPlayer && board[2][col] == currentPlayer) {
won = true;
return;
}

}
// Check diagonals
if (board[0][0] == currentPlayer && board[1][1] == currentPlayer && board[2][2] == currentPlayer) {
won = true;
return;
}
if (board[0][2] == currentPlayer && board[1][1] == currentPlayer && board[2][0] == currentPlayer) {
won = true;
return;
}
}
// Method to change the player after each turn
void ChangePlayer() {
if (currentPlayer == playerSymbols[player1]) {
currentPlayer = playerSymbols[player2];
} else {
currentPlayer = playerSymbols[player1];
}
turn++;
}
void Reset() {
for (int row = 0; row < 3; row++) {
for (int col = 0; col < 3; col++) {
board[row][col] = ' ';
}
}
turn = 1;
currentPlayer = playerSymbols[player1]; // start with player 1's symbol
won = false;
input = 0;
cout << "Enter the name of player 1: ";
cin >> player1;
cout << "Enter the symbol of player 1 (one character only): ";
cin >> playerSymbols[player1];
cout << "Enter the name of player 2: ";
cin >> player2;
cout << "Enter the symbol of player 2 (one character only, different from player 1's symbol): ";
cin >> playerSymbols[player2];
while (playerSymbols[player2] == playerSymbols[player1]) {
cout << "Invalid symbol! Please enter a symbol different from player 1's symbol: ";
cin >> playerSymbols[player2];
}
}

string GetPlayerName() {
if (currentPlayer == playerSymbols[player1]) {
return player1;
} else {
return player2;
}
}
};
void PrintMessage(string message, bool printTop = true, bool printBottom = true)
{
if (printTop)
{
cout << "+---------------------------------+" << endl;
cout << "|";
}
else
{
cout << "|";
}
bool front = true;
for (int i = message.length(); i < 33; i++)
{
if (front)
{
message = " " + message;
}
else
{
message = message + " ";
}
front = !front;
}
cout << message.c_str();
if (printBottom)
{
cout << "|" << endl;
cout << "+---------------------------------+" << endl;
}
else
{
cout << "|" << endl;
}
}
void DrawHangman(int guessCount = 0)
{
if (guessCount >= 1)
PrintMessage("|", false, false);
else
PrintMessage("", false, false);

if (guessCount >= 2)
PrintMessage("|", false, false);
else
PrintMessage("", false, false);
if (guessCount >= 3)
PrintMessage("O", false, false);
else
PrintMessage("", false, false);
if (guessCount == 4)
PrintMessage("/ ", false, false);
if (guessCount == 5)
PrintMessage("/| ", false, false);
if (guessCount >= 6)
PrintMessage("/|\\", false, false);
else
PrintMessage("", false, false);
if (guessCount >= 7)
PrintMessage("|", false, false);
else
PrintMessage("", false, false);
if (guessCount == 8)
PrintMessage("/", false, false);
if (guessCount >= 9)
PrintMessage("/ \\", false, false);
else
PrintMessage("", false, false);
}
void PrintLetters(string input, char from, char to)
{
string s;
for (char i = from; i <= to; i++)
{
if (input.find(i) == string::npos)
{
s += i;
s += " ";
}
else
s += " ";
}
PrintMessage(s, false, false);
}
void PrintAvailableLetters(string taken)

{
PrintMessage("Available letters");
PrintLetters(taken, 'A', 'M');
PrintLetters(taken, 'N', 'Z');
}
bool PrintWordAndCheckWin(string word, string guessed)
{
bool won = true;
string s;
for (int i = 0; i < word.length(); i++)
{
if (guessed.find(word[i]) == string::npos)
{
won = false;
s += "_ ";
}
else
{
s += word[i];
s += " ";
}
}
PrintMessage(s, false);
return won;
}
string LoadRandomWord(string path)
{
int lineCount = 0;
string word;
vector<string> v;
ifstream reader(path);
if (reader.is_open())
{
while (std::getline(reader, word))
v.push_back(word);
int randomLine = rand() % v.size();
word = v.at(randomLine);
reader.close();
}
return word;
}
int TriesLeft(string word, string guessed)
{
int error = 0;
for (int i = 0; i < guessed.length(); i++)
{
if (word.find(guessed[i]) == string::npos)
error++;
}

return error;
}
//SNAKE GAME
class SnakeGame {
private:
enum Direction {
STOP = 0,
LEFT,
RIGHT,
UP,
DOWN
};
Direction SnakeDirection;
int SnakeBodyCount;
bool GameOver;
bool BodyPrintFlag;
const int Width;
const int Height;
int SnakeHeadX, SnakeHeadY, FruitX, FruitY;
int SnakeX[100], SnakeY[100];
static void DisplayMainMenu();
static void DisplayGameOver();
static void DisplayInstructions();
void ControllerInput();
void SetupGame();
void PrintStage();
void PlayStage();
void PlayGame();
public:
SnakeGame();
void StartGame();
};
SnakeGame::SnakeGame() :Height(22), Width(52) {
SnakeBodyCount = 0;
GameOver = false;
BodyPrintFlag = false;
SnakeHeadX = 0;
SnakeHeadY = 0;
FruitX = 0;
FruitY = 0;
for (int i = 0; i < 100; i++) {
SnakeX[i] = 0;
SnakeY[i] = 0;
}
}
void SnakeGame::DisplayMainMenu() {
cout << " ===========================================================================\n";
cout << " || ||\n";
cout << " || *SNAKE GAME* ||\n";
cout << " || ||\n";

cout << " || __ __ __ __ ||\n";
cout << " || / \\ / \\ / \\ / \\ ||\n";
cout << " ||____________________/ __\\/ __\\/ __\\/ __\\___________________________||\n";
cout << " ||___________________/ /__/ /__/ /__/ /______________________________||\n";
cout << " || | / \\ / \\ / \\ / \\ \\____ ||\n";
cout << " || |/ \\_/ \\_/ \\_/ \\ o \\ ||\n";
cout << " || \\_____/--< ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || *Menu* ||\n";
cout << " || ||\n";
cout << " || -> 1. Play Game ||\n";
cout << " || -> 2. Instructions ||\n";
cout << " || -> 3. Exit ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " ===========================================================================\n";
}
void SnakeGame::DisplayGameOver() {
cout << " ===========================================================================\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || ____ _ __ __ _____ _____ _______ ____ ||\n";
cout << " || / ___| / \\ | \\/ | ____/ _ \\ \\ / / ____| _ \\ ||\n";
cout << " || | | _ / _ \\ | |\\/| | _|| | | \\ \\ / /| _| | |_) | ||\n";
cout << " || | |_| |/ ___ \\| | | | |__| |_| |\\ V / | |___| _ < ||\n";
cout << " || \\____/_/ \\_\\_| |_|_____\\___/ \\_/ |_____|_| \\_\\ ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || PRESS ANY KEY TO RETURN TO MENU ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " ===========================================================================\n";
}
void SnakeGame::DisplayInstructions() {
cout << " ===========================================================================\n";
cout << " || ||\n";
cout << " || *Instructions* ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || ____ ||\n";
cout << " || ________________________/ O \\___/ ||\n";
cout << " || <_____________________________/ \\ ||\n";

cout << " || ||\n";
cout << " || ||\n";
cout << " || 1. W,A,S,D to change direction of the Snake. ||\n";
cout << " || 2. Eat the Fruit to Make the Snake Grow. With ||\n";
cout << " || each fruit 10 Points will be Added to the ||\n";
cout << " || score. ||\n";
cout << " || 3. If Snake eats itself, game will be over. ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " || PRESS ANY KEY TO RETURN TO MENU ||\n";
cout << " || ||\n";
cout << " || ||\n";
cout << " ===========================================================================\n";
}
void SnakeGame::ControllerInput() {
if (_kbhit())
{
switch (_getch())
{
case 'a':
SnakeDirection = LEFT;
break;
case 'd':
SnakeDirection = RIGHT;
break;
case 'w':
SnakeDirection = UP;
break;
case 's':
SnakeDirection = DOWN;
break;
case 'A':
SnakeDirection = LEFT;
break;
case 'D':
SnakeDirection = RIGHT;
break;
case 'W':
SnakeDirection = UP;
break;
case 'S':
SnakeDirection = DOWN;
break;
default:
break;
}
}
}
void SnakeGame::SetupGame() {
GameOver = false;
SnakeDirection = STOP;


SnakeHeadX = Width / 2;
SnakeHeadY = Height / 2;
FruitX = rand() % Width;
FruitY = rand() % Height;
}
void SnakeGame::PrintStage() {
cout << flush;
system("cls");
for (int i = 0; i < Width + 4; i++)
{
printf("=");
}
cout << "\n";
for (int i = 0; i < Height; i++)
{
for (int j = 0; j < Width; j++)
{
if (j == 0)
{
printf("||");
}
if (i == SnakeHeadY && j == SnakeHeadX)
{
cout << "O";
}
else if (i == FruitY && j == FruitX)
{
cout << "#";
}
else
{
BodyPrintFlag = false;
for (int k = 0; k < SnakeBodyCount; k++)
{
if (SnakeX[k] == j && SnakeY[k] == i)
{
cout << "o";
BodyPrintFlag = true;
}
}
if (!BodyPrintFlag)
{
printf(" ");
}
}
if (j == Width - 1)
{
printf("||");
}
}

cout << "\n";
}
for (int i = 0; i < Width + 4; i++)
{
printf("=");
}
}
void SnakeGame::PlayStage() {
int prevX = SnakeX[0];
int prevY = SnakeY[0];
int prev2X, prev2Y;
SnakeX[0] = SnakeHeadX;
SnakeY[0] = SnakeHeadY;
for (int i = 1; i < SnakeBodyCount; i++)
{
prev2X = SnakeX[i];
prev2Y = SnakeY[i];
SnakeX[i] = prevX;
SnakeY[i] = prevY;
prevX = prev2X;
prevY = prev2Y;
}
switch (SnakeDirection)
{
case 1:
SnakeHeadX--;
break;
case 2:
SnakeHeadX++;
break;
case 3:
SnakeHeadY--;
break;
case 4:
SnakeHeadY++;
break;
default:
break;
}
if (SnakeHeadX >= Width)
{
SnakeHeadX = 0;
}
else if (SnakeHeadX < 0)
{
SnakeHeadX = Width - 1;
}
if (SnakeHeadY >= Height)
{
SnakeHeadY = 0;
}

else if (SnakeHeadY < 0)
{
SnakeHeadY = Height - 1;
}
for (int i = 0; i < SnakeBodyCount; i++)
{
if (SnakeX[i] == SnakeHeadX && SnakeY[i] == SnakeHeadY)
{
GameOver = true;
}
}
if (SnakeHeadX == FruitX && SnakeHeadY == FruitY)
{
FruitX = rand() % Width;
FruitY = rand() % Height;
SnakeBodyCount += 1;;
}
}
void SnakeGame::PlayGame() {
SetupGame();
while (!GameOver)
{
PrintStage();
ControllerInput();
PlayStage();
Sleep(100);
}
}
void SnakeGame::StartGame() {
char opt = '\0';
while (true) {
system("cls");
DisplayMainMenu();
opt = _getch();
system("cls");
switch (opt) {
case '1':
PlayGame();
system("cls");
DisplayGameOver();
_getch();
break;
case '2':
DisplayInstructions();
_getch();
break;
case '3':
cout << "Closing Snake Game";
for (int i = 0; i < 4; i++) {
Sleep(600);
cout << ".";

exit(-1);
}
break;
default:
break;
}
}
}
const int WIDTH = 800;
const int HEIGHT = 600;
const int PADDLE_WIDTH = 20;
const int PADDLE_HEIGHT = 100;
const int BALL_SIZE = 10;
const int BALL_SPEED = 5;
const int PADDLE_SPEED = 10;
// Global variables
int paddleAY = HEIGHT / 2 - PADDLE_HEIGHT / 2;
int paddleBY = HEIGHT / 2 - PADDLE_HEIGHT / 2;
int ballX = WIDTH / 2 - BALL_SIZE / 2;
int ballY = HEIGHT / 2 - BALL_SIZE / 2;
int ballDX = BALL_SPEED;
int ballDY = BALL_SPEED;
int scoreA = 0;
int scoreB = 0;
// Draw paddle A
void drawPaddleA() {
setcolor(WHITE);
setfillstyle(SOLID_FILL, WHITE);
rectangle(0, paddleAY, PADDLE_WIDTH, paddleAY + PADDLE_HEIGHT);
floodfill(1, paddleAY + 1, WHITE);
}
// Draw paddle B
void drawPaddleB() {
setcolor(WHITE);
setfillstyle(SOLID_FILL, WHITE);
rectangle(WIDTH - PADDLE_WIDTH, paddleBY, WIDTH, paddleBY + PADDLE_HEIGHT);
floodfill(WIDTH - 1, paddleBY + 1, WHITE);
}
// Draw ball
void drawBall() {
setcolor(WHITE);
setfillstyle(SOLID_FILL, WHITE);
circle(ballX + BALL_SIZE / 2, ballY + BALL_SIZE / 2, BALL_SIZE / 2);
floodfill(ballX + BALL_SIZE / 2, ballY + BALL_SIZE / 2, WHITE);
}
// Update ball position

void updateBall() {
ballX += ballDX;
ballY += ballDY;
if (ballY <= 0 || ballY + BALL_SIZE >= HEIGHT) {
ballDY = -ballDY;
}
if (ballX <= 0) {
ballX = WIDTH / 2 - BALL_SIZE / 2;
ballY = HEIGHT / 2 - BALL_SIZE / 2;
ballDX = -ballDX;
scoreB++;
}
if (ballX + BALL_SIZE >= WIDTH) {
ballX = WIDTH / 2 - BALL_SIZE / 2;
ballY = HEIGHT / 2 - BALL_SIZE / 2;
ballDX = -ballDX;
scoreA++;
}
if (ballX <= PADDLE_WIDTH && ballY + BALL_SIZE >= paddleAY && ballY <= paddleAY + PADDLE_HEIGHT) {
ballDX = -ballDX;
}
if (ballX + BALL_SIZE >= WIDTH - PADDLE_WIDTH && ballY + BALL_SIZE >= paddleBY && ballY <= paddleBY + PADDLE_HEIGHT) {
ballDX = -ballDX;
}
}
// Update paddle A position
void updatePaddleA() {
if (GetAsyncKeyState('W') & 0x8000) {
paddleAY -= PADDLE_SPEED;
}
if (GetAsyncKeyState('S') & 0x8000) {
paddleAY += PADDLE_SPEED;
}
if (paddleAY < 0) {
paddleAY = 0;
}
if (paddleAY + PADDLE_HEIGHT > HEIGHT) {
paddleAY = HEIGHT - PADDLE_HEIGHT;
}
}
// Update paddle B position

void updatePaddleB() {
if (GetAsyncKeyState(VK_UP) & 0x8000) {
paddleBY -= PADDLE_SPEED;
}
if (GetAsyncKeyState(VK_DOWN) & 0x8000) {
paddleBY += PADDLE_SPEED;
}
if (paddleBY < 0) {
paddleBY = 0;
}
if (paddleBY + PADDLE_HEIGHT > HEIGHT) {
paddleBY = HEIGHT - PADDLE_HEIGHT;
}
}
// Draw score
void drawScore() {
setcolor(WHITE);
settextstyle(DEFAULT_FONT, HORIZ_DIR, 2);
char score[10];
sprintf(score, "%d - %d", scoreA, scoreB);
outtextxy(WIDTH / 2 - textwidth(score) / 2, 50, score);
}

int main()
{
cout<<" C++ GROUP PROJECT SEMESTER 2 "<<endl;
cout<<"__________________________________________________________________________________________________________________________
"<<endl;
cout<<" WELCOME TO THE GAMING ARCADE "<<endl;
int ch;
cout<<"________________________________________________"<<endl;
cout<<"PRESS 1:TO PLAY MULTIPLAYER TIC-TAC-TOE |"<<endl;
cout<<"PRESS 2:TO PLAY HANGMAN |"<<endl;
cout<<"PRESS 3:TO PLAY MULTIPLAYER PONG |"<<endl;
cout<<"PRESS 4:TO PLAY SNAKE GAME |"<<endl;
cout<<"PRESS 5:TO EXIT |"<<endl;
cout<<"________________________________________________|"<<endl;
do
{
cout<<"ENTER YOUR CHOICE:";
cin>>ch;
if(ch==1)
{
cout<<"YOU HAVE SELECTED MULTIPLAYER TIC-TAC-TOE........LETS START"<<endl;
TicTacToeGame game;

game.Play();
}
else if(ch==2)
{
cout<<"YOU HAVE SELECTED HANGMAN....LETS GO.."<<endl;
srand(time(0));
string guesses;
string wordToGuess;
wordToGuess = LoadRandomWord("words.txt");
int tries = 0;
bool win = false;
do
{
system("cls");
PrintMessage("WELCOME FROM HANGMAN");
DrawHangman(tries);
PrintAvailableLetters(guesses);
PrintMessage("Guess the word");
win = PrintWordAndCheckWin(wordToGuess, guesses);
if (win)
break;
char x;
cout << ">"; cin >> x;
if (guesses.find(x) == string::npos)
guesses += x;
tries = TriesLeft(wordToGuess, guesses);
} while (tries < 10);
if (win)
PrintMessage("YOU WON!");
else
PrintMessage("GAME OVER");
system("pause");
getchar();
}
else if(ch==3)
{
cout<<"YOU HAVE SELECTED 3 TO PLAY MULTILPAYER PONG"<<endl;
// Initialize graphics
initwindow(WIDTH, HEIGHT, "LETS PLAY PONG....5 POINTS TO WIN", -3, -3);
// Game loop
while (true)
{
// Check if either player has reached a score of 10

if (scoreA == 5 || scoreB == 5) {
break;
}
// Clear screen
cleardevice();
// Draw objects
drawPaddleA();
drawPaddleB();
drawBall();
drawScore();
// Update objects
updatePaddleA();
updatePaddleB();
updateBall();
// Pause for a moment
delay(10);
}
// Close graphics
closegraph();
}
else if(ch==4)
{
cout<<"YOU HAVE SELECTED 4 TO PLAY SNAKE GAME"<<endl;
system("cls");
SnakeGame Snake;
Snake.StartGame();
}
}
while(ch!=5);
cout<<"YOU HAVE SELECTED 5 TO EXIT......................." <<endl<<endl;
cout<<"THANK YOU FOR PLAYING OUR GAMES DO VISIT AGAIN..............................................................."<<endl;
return 0;
}
