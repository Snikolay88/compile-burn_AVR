**Установка avr-gcc и инструментов для компиляции исходного кода прошивки на C и/или C ++**

Понадобится компилятор gcc-avr, библиотека C avr-libc и avrdude. 

```bash
$ sudo apt-get install gcc-avr avr-libc avrdude
```

Скомпилировать простой исходный код  --> в файл .BIN, затем сгенерировать файл --> .HEX и, наконец, записать этот файл .HEX на микросхему AVR с помощью программатора
***исходный код:***
```c
int
main(void)
{
        while (1);
}
```


### h3 Компиляция
Компиляция с помощью gcc. Указать  опцию -mmcu для конкретного MCU.

```bash
$ avr-gcc -Wall -g -Os -mmcu=atmega328p -o main.bin main.c
```

После успешной компиляции можно проверить размер памяти программы и данных с помощью команды avr-size.
```bash
$ avr-size -C main.bin
AVR Memory Usage
----------------
Device: Unknown
Program:      40 bytes
(.text + .data + .bootloader)
Data:          0 bytes
(.data + .bss + .noinit)
```

Генерация .HEX
Преобразование информационной формы .BIN в файл .HEX. Утилита GNU, которая делает это, называется avr-objcopy.
```bash
$ avr-objcopy -j .text -j .data -O ihex main.bin main.hex
```


### h3 Запись
Утилита под названием avrdude может программировать процессоры, используя содержимое файлов .HEX, указанных в командной строке.
С помощью приведенной ниже команды файл main.hex будет записан во флэш-память.
Параметр -p m328p позволяет avrdude знать, что мы работаем на чипе Atmega328p. 
Параметр -c avrispv2 указывает используемый программатор
Параметр -P /dev/ttyUSB0 Программатр подключен по юсб. (проверить dmesg, lsusb)
Другими словами - эта опция указывает устройство.
```bash
$ avrdude -p m328p -c avrispv2 -U flash:w:main.hex:i -F -P /dev/ttyUSB0
```
