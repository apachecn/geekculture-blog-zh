# 带回溯的骑士之旅

> 原文：<https://medium.com/geekculture/knights-tour-with-backtracking-6d19a3934c7b?source=collection_archive---------13----------------------->

我有一个问题..我不知道这是一个问题还是缺乏基本的神经元，但是除非我不把问题分解成一个个小问题，然后把它们重新组合起来，否则我愚蠢的大脑什么也理解不了。

哦，好吧，玩你手上的牌。我遇到了一个困扰了我一段时间的问题，所以我想在这里把它说出来。

下面是[维基百科](https://en.wikipedia.org/wiki/Knight%27s_tour)对骑士之旅的描述:

> 骑士之旅是一个[骑士](https://en.wikipedia.org/wiki/Knight_(chess))在[棋盘](https://en.wikipedia.org/wiki/Chessboard)上的一系列移动，使得骑士恰好访问每个方格一次。如果骑士在一个离开始的方格有一步的方格上结束(这样它可以立即沿着相同的路径再次巡视棋盘)，巡视就结束了；否则，它是开放的。

以下是 Java 代码:

```
public class KnightsTour {
     private static int *N* = 8; // set valid squares where the knight can land on the board
     private static boolean isValid (int x, int y, int[][] board) {
         return (x >= 0 && x < *N* && y >= 0 && y < *N* && board[x][y] == -1);
     }

     private static void printBoard(int[][] board) {
         for (int i = 0; i < *N*; i++) {
             for (int j = 0; j < *N*; j++) {
                 System.*out*.print(board[i][j] + "  ");
             }
             System.*out*.println();
         }
     }

     private static boolean knightsTour () { // initiate board
         int[][] board = new int[*N*][*N*]; // populate the board with -1, ie., unused square
         for (int i = 0; i < *N*; i++) {
             for (int j = 0; j < *N*; j++) {
                 board[i][j] = -1;
             }
         } //since we're starting at 0,0
         board[0][0] = 0; // predefined moves for the knight. First one is 2 x moves      and -1 y move. Each one follows that pattern of the L shape Knight   moves.
         int[] xMove = {2, 1, -1, -2, -2, -1, 1, 2};
         int[] yMove = {-1, -2, -2, -1, 1, 2, 2, 1}; // if after running the recursive function, there’s no solutions then
         if (!*tourUtility*(0, 0, 1, board, xMove, yMove)) {
             System.*out*.println("No solutions");
             return false;
         }
         else {
             *printBoard*(board);
             return true;
         }

     }     // recursive function
     private static boolean tourUtility(int x, int y, int move, int[][] board, int[] xMove, int[] yMove) {// since the objective is to visit each square, when the number of moves is n-squared, return true
         if (move == *N* * *N*) {
             return true;
         }

         int next_x, next_y;

         for (int k = 0; k < *N*; k++) { // add the L moves to present position
             next_x = x + xMove[k];
             next_y = y + yMove[k]; if (*isValid*(next_x, next_y, board)) { // if the square hasn’t been visited before, add the    number of move there.
                 board[next_x][next_y] = move; // recursive call to the next move
                 if (*tourUtility*(next_x, next_y, move+1, board, xMove, yMove)) {
                     return true;
                 } else { // backtrack if the recursive call fails and start again
                     board[next_x][next_y] = -1;
                 }
             }
         }
         return false;
     }

     public static void main(String[] args) {
         *knightsTour*();
     }
 }
```

这种回溯方法的问题是它的时间复杂性。每次特定的路径不起作用时，我们就回溯，这意味着每一次错误的移动都会增加步数。对于一个 8 x 8 或 64 x 64 的棋盘来说，这应该不是问题，但是如果它是一个 1，000，000 x 1，000，000 的数据集，计算机可能会不太满意。

为此，我们将使用试探法，然后围绕它们建立一个程序。这就是沃恩斯多夫法则派上用场的地方。但是这个有点长，所以我们将在另一篇文章中讨论。