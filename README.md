# Gussing-the-number-game
This game is meant for two players. Each of the players will try to get the closest to the number 42. The winner will be decided by his score and the lowest number of turns. Each player starts off with an initial score of 0.


#include <stdio.h>
#include <time.h>
#include <stdlib.h>

#define HOLY_SCORE 42
// global variables
int p1Score = 0, p2Score = 0, p1Playing = 1, p2Playing = 1, p1TurnesPlayed = 0, p2TurnesPlayed = 0, turn = 0;

//functions 
void printStatus(int player, int score){
    printf("It's player %d turn, previous score: %d\n", player, score);
}

int getRandomNumber(){
    return (rand() %10 ) + 1;
}

int playerDecision(){
    char decision = 0;
    printf("Random number of points were added, do you want to continue? (y/n)\n");
    scanf(" %c",&decision);
    if (decision == 'y') return 1;
    return 0;
}

int calcActualScore(int score){
    return abs(HOLY_SCORE - score);
}

int checkWinner(int p1Score, int p2Score){
    printf("Player 1 score: %d, Player 2 score: %d\n",p1Score,p2Score);

    int p1ActualScore = calcActualScore(p1Score);
    int p2ActualScore = calcActualScore(p2Score);

    if (p1ActualScore < p2ActualScore){
        printf("Player 1 is the winner!!!\n");
        return 1;
    } else if (p1ActualScore > p2ActualScore) {
         printf("Player 2 is the winner!!!\n");
        return 1;
    }
    return 0; // in a case of a tie
}

int calcPointsRemoval(int turns){
    int pointsToRemove = 0;
    for (int i = 0; i < turns; ++i){
        pointsToRemove += getRandomNumber();

    }
    return pointsToRemove;
}


int main()
{
printf("Welcome to BlackJack! Get as close to 42 as you can without going over!\n");

srand(time(NULL));

while (p1Playing == 1 || p2Playing == 1){
    if (turn % 2 == 0 && p1Playing == 1){ // player 1 turn on even numbers
        p1TurnesPlayed++;
        printStatus (1,p1Score);
        p1Score += getRandomNumber();
        p1Playing = playerDecision();
    }
    else if (turn % 2 == 1 && p2Playing == 1){ // player 2 turn on odd numbers
        p2TurnesPlayed++;
        printStatus (2,p2Score);
        p2Score += getRandomNumber();
        p2Playing = playerDecision();
    }
    turn++; //increase the turn by 1 to swich player's turn
}
    if (checkWinner(p1Score,p2Score) == 0){
        printf("There's a tie! removing score by number of turns played.\n");
        p1Score -= calcPointsRemoval(p1TurnesPlayed);
        p2Score -= calcPointsRemoval(p2TurnesPlayed);
        if (checkWinner(p1Score,p2Score) == 0){
            printf("There's another tie! WOW!\n");
        }
    }
    return 0;
}
