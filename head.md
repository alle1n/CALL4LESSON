```
//заголовки библиотек
#pragma once //определение заголовочного файла
#pragma comment(lib, "audiere.lib") //работа с библиотекой Audiere 
#include <iostream> //ввод вывод
#include <ctime> //время
#include <Windows.h> //функции Windows
#include <vector> //динамический массив
#include "audiere.h" //заголовочный файл библиотеке Audiere

using namespace audiere; //определение пространства имен Audiere

//структуры и классы
struct Call //структура звонка
{
	int hour; //звонок производится в определенный час
	int minute; //и в определенную минуту
};

class Lesson //главный класс 
{
public:
	Call set; //время звонка
	double timedif(Call T); //дельта текущего и установленного времени 
	Lesson input(); //ввод установленного врем
	void print(); //метод для вывода установленного времени (для разработчиков)
};

//прототипы функций
std::vector<Lesson> makevector(); //создание вектора
void sortvector(std::vector<Lesson>& S); //сортировка вектора
void call(std::vector<Lesson> S); //звонок
```
