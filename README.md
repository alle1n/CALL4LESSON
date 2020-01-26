# main.cpp
```
//библиотеки
#pragma comment(lib, "audiere.lib") //подключение библиотеки Audiere
#include <iostream> //ввод/вывод  
#include <ctime> //время
#include <Windows.h> //функции Windows
#include <vector> //вектор
#include "audiere.h" //заголовочный файл библиотеки Audiere
#include "head.h" //заголовочный файл 

//пространства имен
using namespace std; //стандартное пространство имен
using namespace audiere; //пространство имен Audiere

//функции
signed int main() //главная функция
{
	vector<Lesson> L = makevector(); //передать обЪекту вектор с временем звонков
	sortvector(L); //отсортировать время по близости к текущему
	call(L); //непосредственно сам звонок
	
	return 0; //завершение программы (можно не писать)
}
```
