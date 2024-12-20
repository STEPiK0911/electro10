# Работу выполнил Погребняк Степан Сиваселвамович

# Цель работы

![image](https://github.com/user-attachments/assets/518133e6-fc70-4e6f-9199-df6806f0b41e)




```
#include <reg51.h>

// Определяем порты для управления сегментами и разрядами
#define SEGMENTS P1  // Порт для сегментов
sbit DIGIT1 = P2^0;  // Разряд 1
sbit DIGIT2 = P2^1;  // Разряд 2

// Битовые маски сегментов для цифр 0-9
unsigned char segment_map[] = {
    0b00111111, // 0
    0b00000110, // 1
    0b01011011, // 2
    0b01001111, // 3
    0b01100110, // 4
    0b01101101, // 5
    0b01111101, // 6
    0b00000111, // 7
    0b01111111, // 8
    0b01101111  // 9
};

// Задержка (приблизительная)
void delay(unsigned int ms) {
    unsigned int i, j;
    for (i = 0; i < ms; i++) {
        for (j = 0; j < 1275; j++);
    }
}

// Функция для отображения числа в динамическом режиме
void displayNumber(unsigned char number) {
    unsigned char tens = number / 10; // Десятки
    unsigned char units = number % 10; // Единицы

    // Отображаем десятки
    SEGMENTS = segment_map[tens];
    DIGIT1 = 1; // Включаем первый разряд
    delay(5);   // Небольшая задержка для переключения
    DIGIT1 = 0; // Выключаем первый разряд

    // Отображаем единицы
    SEGMENTS = segment_map[units];
    DIGIT2 = 1; // Включаем второй разряд
    delay(5);   // Небольшая задержка для переключения
    DIGIT2 = 0; // Выключаем второй разряд
}

void main() {
    unsigned char year = 23; // Пример: текущий год (23)
    unsigned char mode = 0; // Режим отображения: 0 — текущий год, 1 — день рождения

    // Инициализация разрядов как выходов
    DIGIT1 = 0;
    DIGIT2 = 0;

    while (1) {
        // Чередуем режимы отображения каждые 2 секунды
        if (mode == 0) {
            displayNumber(year); // Отображаем текущий год
        } else {
            displayNumber(99); // Пример: отображение года рождения
        }

        delay(200); // Задержка 200 мс (динамическая индикация)

        // Смена режима каждые 10 итераций
        static unsigned int counter = 0;
        if (++counter >= 10) {
            mode = !mode; // Переключаем режим
            counter = 0;
        }
    }
}

```
