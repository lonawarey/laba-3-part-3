 #include <stdio.h>
#include <stdlib.h>

// Процедура для разделения массива A на четные и нечетные элементы
void Split2(int A[], int Na, int B[], int *Nb, int C[], int *Nc) {
    *Nb = 0; // Количество четных элементов
    *Nc = 0; // Количество нечетных элементов
    
    for (int i = 0; i < Na; i++) {
        if (A[i] % 2 == 0) {
            // Четное число - добавляем в массив B
            B[*Nb] = A[i];
            (*Nb)++;
        } else {
            // Нечетное число - добавляем в массив C
            C[*Nc] = A[i];
            (*Nc)++;
        }
    }
}

// Вспомогательная функция для ввода массива
void inputArray(int arr[], int size, const char *name) {
    printf("Введите элементы массива %s (%d элементов):\n", name, size);
    for (int i = 0; i < size; i++) {
        printf("%s[%d] = ", name, i);
        scanf("%d", &arr[i]);
    }
}

// Вспомогательная функция для вывода массива
void printArray(int arr[], int size, const char *name) {
    if (size == 0) {
        printf("Массив %s пуст\n", name);
        return;
    }
    
    printf("Массив %s (размер %d): [", name, size);
    for (int i = 0; i < size; i++) {
        printf("%d", arr[i]);
        if (i < size - 1) {
            printf(", ");
        }
    }
    printf("]\n");
}

int main() {
    int Na;
    
    // Ввод размера исходного массива
    printf("Введите размер исходного массива A (Na): ");
    scanf("%d", &Na);
    
    // Проверка корректности размера
    if (Na <= 0) {
        printf("Ошибка: размер массива должен быть положительным\n");
        return 1;
    }
    
    // Выделение памяти для массивов
    int *A = (int*)malloc(Na * sizeof(int));
    // Максимальный размер для B и C - Na (все элементы могут быть четными или нечетными)
    int *B = (int*)malloc(Na * sizeof(int));
    int *C = (int*)malloc(Na * sizeof(int));
    
    if (A == NULL || B == NULL || C == NULL) {
        printf("Ошибка выделения памяти\n");
        free(A); free(B); free(C);
        return 1;
    }
    
    // Ввод исходного массива A
    inputArray(A, Na, "A");
    
    // Вывод исходного массива
    printf("\nИсходный массив:\n");
    printArray(A, Na, "A");
    
    // Переменные для размеров массивов B и C
    int Nb = 0, Nc = 0;
    
    // Вызов процедуры Split2
    Split2(A, Na, B, &Nb, C, &Nc);
    
    // Вывод результатов
    printf("\nРезультаты разделения:\n");
    printf("---------------------------------\n");
    printArray(B, Nb, "B (четные)");
    printArray(C, Nc, "C (нечетные)");
    
    // Дополнительная статистика
    printf("\nСтатистика:\n");
    printf("Всего элементов: %d\n", Na);
    printf("Четных элементов: %d (%.1f%%)\n", Nb, (float)Nb/Na*100);
    printf("Нечетных элементов: %d (%.1f%%)\n", Nc, (float)Nc/Na*100);
    
    // Подробный вывод с указанием исходных индексов
    printf("\nПодробное распределение:\n");
    printf("Индекс | Значение | Массив\n");
    printf("-------+----------+---------\n");
    
    // Проходим по массиву A и показываем распределение
    int b_idx = 0, c_idx = 0;
    for (int i = 0; i < Na; i++) {
        if (A[i] % 2 == 0) {
            printf("A[%2d]  | %8d | -> B[%d] = %d\n", 
                   i, A[i], b_idx, B[b_idx]);
            b_idx++;
        } else {
            printf("A[%2d]  | %8d | -> C[%d] = %d\n", 
                   i, A[i], c_idx, C[c_idx]);
            c_idx++;
        }
    }
    
    // Проверка правильности распределения
    printf("\nПроверка сохранения порядка:\n");
    printf("Порядок четных элементов сохранен: ");
    int correct_order = 1;
    b_idx = 0;
    for (int i = 0; i < Na; i++) {
        if (A[i] % 2 == 0) {
            if (A[i] != B[b_idx]) {
                correct_order = 0;
                break;
            }
            b_idx++;
        }
    }
    printf(correct_order ? "ДА\n" : "НЕТ\n");
    
    printf("Порядок нечетных элементов сохранен: ");
    correct_order = 1;
    c_idx = 0;
    for (int i = 0; i < Na; i++) {
        if (A[i] % 2 != 0) {
            if (A[i] != C[c_idx]) {
                correct_order = 0;
                break;
            }
            c_idx++;
        }
    }
    printf(correct_order ? "ДА\n" : "НЕТ\n");
    
    // Освобождение памяти
    free(A);
    free(B);
    free(C);
    
    return 0;
}
