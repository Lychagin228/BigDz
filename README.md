# BigDz

#include <stdio.h> 

#include <stdlib.h> 

#include <time.h>

#include <locale.h>

#include <math.h>

#include <limits.h>



double* full_elements(double* ptr_array, int size);

double* calc_elements(double* ptr_array, int size);

int put_elements(double* ptr_array, int size);

int delete_k(double* ptr_arr, int size, int k);

double* insert(double* ptr_arr, int* size, int index, double num);

double* insert_after_each_k(double* ptr_arr, int* size, int k, double num);


int main() {


    setlocale(LC_CTYPE, "RUS");
    

    srand(time(NULL));
    

    double* ptr_array = NULL;
    
    int size;


    printf("Введите размер массива: ");
    
    if (scanf("%d", &size) != 1) {
    
        puts("Ошибка ввода размера массива!");
        
        return -1;
        
    }

    if (size <= 0) {
    
        puts("Размер массива должен быть положительным!");
        
        return -1;
        
    }


    ptr_array = (double*)malloc(size * sizeof(double));
    
    if (ptr_array == NULL) {
    
        puts("Ошибка выделения памяти!");
        
        return -1;
        
    }


    printf("Исходный массив:\n");
    
    full_elements(ptr_array, size);
    
    put_elements(ptr_array, size);


    printf("Обработанный массив:\n");
    
    double* new_array = calc_elements(ptr_array, size);
    
    if (new_array != NULL) {
    
        put_elements(new_array, size);
        
    }


    printf("\n=== Демонстрация удаления элемента ===\n");
    
    int del_index;
    
    if (size > 0) {
    
        del_index = rand() % size;
        
        printf("Удаляем элемент с индексом %d\n", del_index);
        
        int new_size = delete_k(ptr_array, size, del_index);
        
        printf("Массив после удаления (%d элементов):\n", new_size);
        
        put_elements(ptr_array, new_size);


        printf("\n=== Демонстрация вставки элемента ===\n");
        
        int insert_index;
        
        if (new_size > 0) {
        
            insert_index = rand() % new_size;
            
            printf("Вставляем -999 после элемента с индексом %d\n", insert_index);
            
            double* temp_array = insert(ptr_array, &new_size, insert_index + 1, -999.0);
            
            if (temp_array != NULL) {
            
                ptr_array = temp_array;
                
                printf("Массив после вставки (%d элементов):\n", new_size);
                
                put_elements(ptr_array, new_size);
                
            }
            
        }
        
    }
    


    if (ptr_array != NULL) {
    
        free(ptr_array);
        
        ptr_array = NULL;
    }
    
    if (new_array != NULL) {
    
        free(new_array);
        
        new_array = NULL;
        
    }

    printf("Программа завершена успешно.\n");
    
    return 0;
    
}



double* full_elements(double* ptr_array, int size) {

    if (ptr_array == NULL || size <= 0) {
    
        return NULL;
        
    }

    for (int i = 0; i < size; i++) {
    
        ptr_array[i] = (double)rand() / RAND_MAX * 2.0 - 1.0;
        
    }
    
    return ptr_array;
    
}



double* calc_elements(double* ptr_array, int size) {

    int n;

    printf("Введите количество элементов массива: ");
    
    scanf("%d", &n);

    if (n <= 0) {
    
        printf("Ошибка: количество элементов должно быть положительным числом.\n");
        
        return 1;
        
    }


    int* arr = (int*)malloc(n * sizeof(int));
    
    if (arr == NULL) {
    
        printf("Ошибка выделения памяти!\n");
        
        return 1;
        
    }


    printf("Введите %d целых чисел (положительных и отрицательных):\n", n);
    
    for (int i = 0; i < n; i++) {
    
        scanf("%d", &arr[i]);
        
    }

    int max_value = INT_MIN;
    
    int max_index = 0;

    for (int i = 0; i < n; i++) {
    
        if (arr[i] > max_value) {
        
            max_value = arr[i];
            
            max_index = i;
            
        }
        
    }

    int count_positive = 0;
    
    for (int i = 0; i < max_index; i++) {
    
        if (arr[i] > 0) {
        
            count_positive++;
            
        }
        
    }
    
    printf("Количество положительных элементов до максимального: %d\n", count_positive);


    return 0;
    
}



int put_elements(double* ptr_array, int size) {

    if (ptr_array == NULL) {
    
        puts("Массив пуст!");
        
        return -1;
        
    }

    if (size <= 0) {
    
        puts("Неверный размер массива!");
        
        return -1;
        
    }

    for (int i = 0; i < size; i++) {
    
        printf("[%d] = %.4f\n", i, ptr_array[i]);
        
    }
    
    return 0;
    
}


int delete_k(double* ptr_arr, int size, int k) {

    if (ptr_arr == NULL) {
    
        return 0;
        
    }
    

    if (k < 0 || k >= size) {
    
        printf("Некорректный индекс для удаления: %d\n", k);
        
        return size;
        
    }

    int n = size - 1;
    
    for (int i = k; i < n; i++) {
    
        ptr_arr[i] = ptr_arr[i + 1];
        
    }
    
    return n;
    
}



double* insert(double* ptr_arr, int* size, int index, double num) {

    if (ptr_arr == NULL || size == NULL) {
    
        printf("Некорректные параметры для вставки!\n");
        
        return NULL;
        
    }

    if (index < 0 || index > *size) {
    
        printf("Некорректный индекс для вставки: %d (размер: %d)\n", index, *size);
        
        return ptr_arr;
        
    }

    int size_n = (*size) + 1;
    
    double* ptr_arr_n = (double*)realloc(ptr_arr, size_n * sizeof(double));
    
    if (ptr_arr_n == NULL) {
    
        printf("Ошибка перевыделения памяти!\n");
        
        return ptr_arr;
        
    }

    for (int i = size_n - 1; i > index; i--) {
    
        ptr_arr_n[i] = ptr_arr_n[i - 1];
        
    }

    ptr_arr_n[index] = num;
    
    *size = size_n;
    
    return ptr_arr_n;
    
}


double* insert_after_each_k(double* ptr_arr, int* size, int k, double num) {

    if (ptr_arr == NULL || size == NULL || k <= 0) {
    
        printf("Некорректные параметры для вставки после каждого k-го элемента!\n");
        
        return ptr_arr;
        
    }

    if (*size == 0) {
    
        printf("Массив пуст, вставка невозможна!\n");
        
        return ptr_arr;
        
    }

    double* current_array = ptr_arr;
    
    int current_size = *size;

    for (int i = current_size - 1; i >= 0; i--) {
    
        if ((i + 1) % k == 0) {
        
            current_array = insert(current_array, &current_size, i + 1, num);
            
            if (current_array == NULL) {
            
                printf("Ошибка при вставке после элемента %d\n", i);
                
                return ptr_arr;
                
            }
            
        }
        
    }

    *size = current_size;
    return current_array;
}
