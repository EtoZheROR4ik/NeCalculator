# NeCalculator
отиджер приём проверка приём
Связь есть
в одномерном массиве вставить новый элемент после всех элементов, которые заканчиваются на данную цифру (цифра вводится с клавиатуры)

#include <iostream>
using namespace std;
int main() {
    setlocale(LC_ALL, "Ru");
    const int maxSize = 100;  // Максимальный размер массива
    int numbers[maxSize];     // Одномерный массив чисел
    int size;                 // Размер массива
    int digit;                // Цифра, вводимая с клавиатуры
    int newElement;           // Новый элемент, который нужно вставить

    // Ввод размера массива
    cout << "Введите размер массива (не более " << maxSize << "): ";
    cin >> size;

    // Ввод массива
    cout << "Введите элементы массива: ";
    for (int i = 0; i < size; i++) {
        cin >> numbers[i];
    }

    // Ввод цифры и нового элемента
    cout << "Введите цифру: ";
    cin >> digit;

    cout << "Введите новый элемент: ";
    cin >> newElement;

    // Вставка нового элемента после чисел, оканчивающихся на заданную цифру
    for (int i = 0; i < size; i++) {
        if (numbers[i] % 10 == digit) {
            // Сдвиг всех элементов справа от текущего
            for (int j = size; j > i + 1; j--) {
                numbers[j] = numbers[j - 1];
            }

            // Вставка нового элемента
            numbers[i + 1] = newElement;

            // Увеличение размера массива
            size++;
        }
    }

    // Вывод результата
    cout << "Результат:\n";
    for (int i = 0; i < size; i++) {
        cout << numbers[i] << " ";
    }

    return 0;
}

пздц конешн задание 
поменяй названия переменных
а то слишком по умному вышло
На почту зайди
ты еблан
не всё скопировал
исправил в компиляторе
