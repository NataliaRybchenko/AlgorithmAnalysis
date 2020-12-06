#include <iostream>
#include <limits>
#include <time.h>


using namespace std;

const int _infinum = numeric_limits<int>::max();
const int _infinum2 = numeric_limits<int>::max()-1;


void coutMatr (int n, int **X) // Функция вывода матрицы
{
    for (int i = 0; i < n; ++i)
    {
        for (int j = 0; j < n; ++j)
        {
            if ((i == 0) && (j == 0))
                cout << " " << '\t';
            else if (X[i][j] == _infinum)
                cout << "inf" << '\t';
            else if (X[i][j] == _infinum2)
                cout << "inf2" << '\t';
            else
                cout <<  X[i][j] << '\t';
        }
        cout << endl << endl;
    }
    cout << endl;
};

int castOnRows (int n, int **X) // Функция преобразования по строкам (возвращает сумму минимальных элементов)
{
    int min, sum = 0;
    for (int i = 1; i < n; ++i)
    {
        min = _infinum;
        for (int j = 1; j < n; ++j)
        {
            if (X[i][j] < min)
                min = X[i][j];
        }
        if (min != 0)
        {
            for (int j = 1; j < n; ++j)
            {
                if (X[i][j] < _infinum2)
                    X[i][j] -= min;
            }
            sum += min;
        }
    }
    return sum;
};

int castOnCols (int n, int **X) // Функция преобразования по столбцам (возвращает сумму минимальных элементов)
{
    int min, sum = 0;
    for (int i = 1; i < n; ++i)
    {
        min = _infinum;
        for (int j = 1; j < n; ++j)
        {
            if (X[j][i] < min)
                min = X[j][i];
        }
        if (min != 0)
        {
            for (int j = 1; j < n; ++j)
            {
                if (X[j][i] < _infinum2)
                    X[j][i] -= min;
            }
            sum += min;
        }
    }
    return sum;
};

void maxDegZero (int n, int **X, int &i_max, int &j_max) // Функция поиска нулевого элемента с максимальной оценкой нуля
{
    int minRow, minCol;
    int k = -1;
    for (int i = 1; i < n; ++i)
    {
        for (int j = 1; j < n; ++j)
        {
            if (X[i][j] == 0)
            {
                minRow = _infinum2;
                minCol = _infinum2;
                for (int h = 1; h < n; ++h)
                {
                    if ((X[i][h] <= minRow) && (h != j)) // ищем минимум в строке
                        minRow = X[i][h];

                    if ((X[h][j] <= minCol) && (h != i)) // ищем минимум в столбце
                        minCol = X[h][j];
                }
                // Чтобы значение суммы минимальных элементов не вышло за пределы типа integer
                if (minRow == _infinum2)
                    minCol = 0;
                else if (minCol == _infinum2)
                    minRow = 0;

                if (minRow + minCol > k) // сравниваем текущую оценку нуля с максимальной на данный момент и запоминаем номера строки и столбца элемента с максимальной оценкой нуля
                {
                    i_max = i;
                    j_max = j;
                    k = minRow + minCol;
                }
            }
        }
    }
};

void deleteRowCol_inf (int n, int **X, int row, int col) // Преобразование матрицы с целью получения новой, соответствующей классу путей, содержащих дугу (row,col)
{
    // заменяем на бесконечность значение соответствующего элемента, чтобы избежать преждевременных циклов
    int i_inf = -1;
    int j_inf = -1;

    for (int i = 1; i < n; ++i)
    {
        if (X[row][i] == _infinum)
            i_inf = i;
        if (X[i][col] == _infinum)
            j_inf = i;
    }
    X[j_inf][i_inf] = _infinum;

    // удаляем строчку и столбец, в которых находится элемент с максимальной оценкой нуля
    for (int i = 0; i < n-1; ++i)
    {
        for (int j = 0; j < n-1; ++j)
        {
            if (i < row)
            {
                if (j >= col)
                    X[i][j] = X[i][j+1];
            } else {
                if (j < col)
                    X[i][j] = X[i+1][j];
                else
                    X[i][j] = X[i+1][j+1];
            }
        }
    }
};

int **Generator (int n)
{
    int** X = new int* [n+1];
    for (int i = 0; i < n+1; ++i)
        X[i] = new int [n+1];
    for (int i = 0; i < n+1; ++i)
    {
        X[0][i] = i-1;
        X[i][0] = i-1;
    }
    for (int i = 1; i < n+1; ++i)
    {
        for (int j = 1; j < n+1; ++j)
        {
            if (i == j)
                X[i][j] = _infinum;
            else
                X[i][j] = rand() % 100 + 1;

        }
    }
    return X;
}

void TestLittleAlgorithm (int n, int **A, int limit, int &record, int **PCSol, int **CSol,  int num) // Функция, реализующая алгоритм Литтла
{
    // Вывод матрицы
    cout << "Current matrix: " << endl;
    coutMatr(n, A);

    // Приведение по строкам
    int sumMinRow;
    sumMinRow = castOnRows(n, A);

    // Приведение по столбцам
    int sumMinCol;
    sumMinCol = castOnCols (n, A);

    // Вывод преобразованной матрицы
    cout << "After transformations:" << endl;
    coutMatr(n, A);

    // Увеличиваем текущую оценку снизу на сумму вычтенных из строк и столбцов элементов
    int newLimit = limit + sumMinRow + sumMinCol;

    // Сравниваем текущую оценку снизу с текущим решением
    if (newLimit < record)
    {
        // Выбор максимальной оценки нуля
        int i_max = 0, j_max = 0;
        maxDegZero (n, A, i_max, j_max);

        // Добавление полученной дуги в цикл, если она была получена
        if ((i_max != 0) && (j_max != 0))
        {
            PCSol[num][0] = A[i_max][0];
            PCSol[num][1] = A[0][j_max];

            // Вывод имеющейся части кандидата на решение
            cout << "Current part of the candidate:" << endl;
            for (int i = 0; i <= num; ++i)
            {
                cout << "(" << PCSol[i][0] << ";" << PCSol[i][1] << ")" << endl;
            }
            cout << endl;

            // Создаем матрицу для класса путей, содержащих найденную дугу
            int** Amn = new int* [n];
            for (int i = 0; i < n; ++i)
                Amn[i] = new int [n];
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    Amn[i][j] = A[i][j];
                }
            }
            deleteRowCol_inf (n, Amn, i_max, j_max);

            if (n > 4) // Рекурсивный вызов функции для этого класса путей (до тех пор, пока порядок матрицы не станет равным двум)
            {
                cout << "Current limit: " << newLimit << endl << endl;
                TestLittleAlgorithm (n-1, Amn, newLimit, record, PCSol, CSol, num+1);
            }
            else // Добавляем оставшиеся дуги, однозначно определяемые получившейся матрицей
            {
                cout << "Current matrix: " << endl;
                coutMatr(n-1, Amn);
                int lastLimit = newLimit;
                for (int i = 1; i < n-1; ++i)
                {
                    for (int j = 1; j < n-1; ++j)
                    {
                        if ((Amn[i][j] != _infinum) && (Amn[i][j] != _infinum2))
                        {
                            ++num;
                            PCSol[num][0] = Amn[i][0];
                            PCSol[num][1] = Amn[0][j];
                            lastLimit += Amn[i][j];
                        }
                    }
                }

                cout << "Candidate:" << endl;
                for (int i = 0; i < num+1; ++i)
                {
                    CSol[i][0] = PCSol[i][0];
                    CSol[i][1] = PCSol[i][1];
                    cout << "(" << CSol[i][0] << ";" << CSol[i][1] << ")" << endl;
                }
                cout << endl;

                if (record == _infinum)
                    cout << "Current limit (" << lastLimit << ") < " << "RECORD (inf) ==> " << endl;
                else
                    cout << "Current limit (" << lastLimit << ") <= " << "RECORD (" << record <<") ==> " << endl;

                record = lastLimit;
                cout << "NEW RECORD: " << record << endl << endl;
                num -= 2;
            }

            // Переход на другую ветвь рекурсии_______________________________________________________
            // Создаем матрицу для класса путей, НЕ содержащих найденную дугу
            int** A_mn = new int* [n];
            for (int i = 0; i < n; ++i)
                A_mn[i] = new int [n];
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    A_mn[i][j] = A[i][j];
                }
            }
            A_mn[i_max][j_max] = _infinum2;


            // Рекурсивный вызов функции для этого класса путей
            TestLittleAlgorithm (n, A_mn, newLimit, record, PCSol, CSol, num);
        }
    }
    else
    {
        cout << "Current limit (" << newLimit << ") >= RECORD (" << record << ") ==> STOP IT" << endl << endl;
    }
};

void LittleAlgorithm (int n, int **A, int limit, int &record, int **PCSol, int **CSol,  int num) // Функция, реализующая алгоритм Литтла
{
    // Приведение по строкам
    int sumMinRow;
    sumMinRow = castOnRows(n, A);

    // Приведение по столбцам
    int sumMinCol;
    sumMinCol = castOnCols (n, A);

    // Увеличиваем текущую оценку снизу на сумму вычтенных из строк и столбцов элементов
    int newLimit = limit + sumMinRow + sumMinCol;

    // Сравниваем текущую оценку снизу с текущим решением
    if (newLimit < record)
    {
        // Выбор максимальной оценки нуля
        int i_max = 0, j_max = 0;
        maxDegZero (n, A, i_max, j_max);

        // Добавление полученной дуги в цикл. если она была найдена
        if ((i_max != 0) && (j_max != 0))
        {
            PCSol[num][0] = A[i_max][0];
            PCSol[num][1] = A[0][j_max];

            // Создаем матрицу для класса путей, содержащих найденную дугу
            int** Amn = new int* [n];
            for (int i = 0; i < n; ++i)
                Amn[i] = new int [n];
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    Amn[i][j] = A[i][j];
                }
            }
            deleteRowCol_inf (n, Amn, i_max, j_max);

            if (n > 4) // Рекурсивный вызов функции для этого класса путей (до тех пор, пока порядок матрицы не станет равным двум)
                LittleAlgorithm (n-1, Amn, newLimit, record, PCSol, CSol, num+1);

            else // Добавляем оставшиеся дуги, однозначно определяемые получившейся матрицей
            {
                int lastLimit = newLimit;
                for (int i = 1; i < n-1; ++i)
                {
                    for (int j = 1; j < n-1; ++j)
                    {
                        if ((Amn[i][j] != _infinum) && (Amn[i][j] != _infinum2))
                        {
                            ++num;
                            PCSol[num][0] = Amn[i][0];
                            PCSol[num][1] = Amn[0][j];
                            lastLimit += Amn[i][j];
                        }
                    }
                }

                for (int i = 0; i < num+1; ++i)
                {
                    CSol[i][0] = PCSol[i][0];
                    CSol[i][1] = PCSol[i][1];
                }

                record = lastLimit;
                num -= 2;
            }

            // Переход на другую ветвь рекурсии_______________________________________________________
            // Создаем матрицу для класса путей, НЕ содержащих найденную дугу
            int** A_mn = new int* [n];
            for (int i = 0; i < n; ++i)
                A_mn[i] = new int [n];
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    A_mn[i][j] = A[i][j];
                }
            }
            A_mn[i_max][j_max] = _infinum2;

            // Рекурсивный вызов функции для этого класса путей
            LittleAlgorithm (n, A_mn, newLimit, record, PCSol, CSol, num);
        }
    }
};

int main() {
    srand( time(0) );
    int n;

    short a;
    cout << "What do you want?" << endl << "1. Test with steps" << endl << "2. Time for n tops" << endl;
    cout << "Enter number: ";
    cin >> a;

    cout << "Enter n: ";
    cin >> n;
    cout << endl;
    if (n < 3)
    {
        cout << "Incorrect data!" << endl;
        return 0;
    }

    int** A = new int* [n+1]; // матрица смежности
    for (int i = 0; i < n+1; ++i)
        A[i] = new int [n+1];

    int** CandidateSolution = new int* [n+1]; // кандидат
    for (int i = 0; i < n+1; ++i)
        CandidateSolution[i] = new int [2];

    int** Solution = new int* [n+1]; // матрица решения
    for (int i = 0; i < n+1; ++i)
        Solution[i] = new int [2];

    int limit, // текущая нижняя граница
            record; // длина текущего цикла минимальной длины

    int number; // указатель на номер первой свободной строки в матрице решения

    switch (a) {
        case 1:
            A = Generator(n);
            number = 0;
            limit = 0;
            record = _infinum;
            for (int i = 0; i < n+1; ++i)
            {
                for (int j = 0; j < 2; ++j)
                {
                    CandidateSolution[i][j] = 0;
                    Solution[i][j] = 0;
                }
            }
            TestLittleAlgorithm (n+1, A, limit, record, CandidateSolution, Solution, number);
            cout << "CYCLE LENGTH: " << record << endl;
            cout << "SOLUTION: " << endl;
            for (int i = 0; i < n; ++i)
            {
               cout << "(" << Solution[i][0] << ";" << Solution[i][1] << ")" << endl;
            }
            break;
        case 2:
            double t = 0; // усредненное время работы алгоритма
            clock_t start, end;

            int q = 100;
            for (int k = 0; k < q; ++k)
            {
                A = Generator(n);
                number = 0;
                limit = 0;
                record = _infinum;
                for (int i = 0; i < n+1; ++i)
                {
                    for (int j = 0; j < 2; ++j)
                    {
                        CandidateSolution[i][j] = 0;
                        Solution[i][j] = 0;
                    }
                }
                start = clock();
                LittleAlgorithm (n+1, A, limit, record, CandidateSolution, Solution, number);
                end = clock();
                t += (double)(end - start) / CLOCKS_PER_SEC;
            }
            t /= q;
            cout << "TIME: " << t << endl;
            break;
    }

    return 0;
}
