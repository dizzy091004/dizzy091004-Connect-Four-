






using System;

namespace ConnectFour
{
    class Program
    {
        static int ROWS = 6;
        static int COLS = 7;
        static char[,] board = new char[6, 7];
        static char currentPlayer = 'R'; // R: Red, Y: Yellow

        static void Main(string[] args)
        {
            InitializeBoard();
            while (true)
            {
                Console.Clear();
                DrawBoard();
                Console.WriteLine($"플레이어 {currentPlayer}의 차례입니다. 열 번호(0-6)를 입력하세요:");

                if (int.TryParse(Console.ReadLine(), out int col) && col >= 0 && col < COLS)
                {
                    if (DropChip(col))
                    {
                        if (CheckWin())
                        {
                            Console.Clear();
                            DrawBoard();
                            Console.WriteLine($"축하합니다! 플레이어 {currentPlayer}가 승리했습니다!");
                            break;
                        }
                        currentPlayer = (currentPlayer == 'R') ? 'Y' : 'R';
                    }
                    else
                    {
                        Console.WriteLine("해당 열이 가득 찼습니다. 다시 시도하세요. (아무 키나 누르세요)");
                        Console.ReadKey();
                    }
                }
            }
        }

        static void InitializeBoard()
        {
            for (int r = 0; r < ROWS; r++)
                for (int c = 0; c < COLS; c++)
                    board[r, c] = '.';
        }

        static void DrawBoard()
        {
            for (int r = 0; r < ROWS; r++)
            {
                for (int c = 0; c < COLS; c++)
                    Console.Write(board[r, c] + " ");
                Console.WriteLine();
            }
            Console.WriteLine("0 1 2 3 4 5 6");
        }

        static bool DropChip(int col)
        {
            for (int r = ROWS - 1; r >= 0; r--)
            {
                if (board[r, col] == '.')
                {
                    board[r, col] = currentPlayer;
                    return true;
                }
            }
            return false;
        }

        static bool CheckWin()
        {
            // 가로, 세로, 대각선 체크 로직 (단순화 버전)
            for (int r = 0; r < ROWS; r++)
            {
                for (int c = 0; c < COLS; c++)
                {
                    if (board[r, c] == '.') continue;
                    if (CheckDirection(r, c, 0, 1) || CheckDirection(r, c, 1, 0) || 
                        CheckDirection(r, c, 1, 1) || CheckDirection(r, c, 1, -1))
                        return true;
                }
            }
            return false;
        }

        static bool CheckDirection(int r, int c, int dr, int dc)
        {
            char player = board[r, c];
            int count = 0;
            for (int i = 0; i < 4; i++)
            {
                int nr = r + dr * i;
                int nc = c + dc * i;
                if (nr >= 0 && nr < ROWS && nc >= 0 && nc < COLS && board[nr, nc] == player)
                    count++;
                else break;
            }
            return count == 4;
        }
    }
}
