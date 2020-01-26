```
//библиотеки
#pragma comment(lib, "audiere.lib") //подключение библиотеки Audiere
#include <iostream> //ввод\вывод 
#include <ctime> //время
#include <Windows.h> //функции Windows
#include <vector> //массив звонков
#include "head.h" //заголовочный файл
#include "audiere.h" //заголовочный файл библиотеки Audiere

//определения
#define NUMBER_OF_LESSONS 20 //количество звонков в день

//пространства имен
using namespace audiere; //

//функции
double Lesson::timedif(Call T) //
{
	time_t now; //настоящее время
	struct tm Set; //структура времени
	double s; //количество секунд

	time(&now); //количество секунд с 1 января 1970 года

	localtime_s(&Set, &now); //время в регионе

	Set.tm_hour = T.hour;
	Set.tm_min = T.minute; //присваеваем время структуре
	Set.tm_sec = NULL;

	s = (now > mktime(&Set) ?
		86400 - difftime(now, mktime(&Set)) //дельта настоящего и установленного времени
		: s = difftime(mktime(&Set), now));

	return s; //возврат дельты
}

Lesson Lesson::input() //ввод времени
{
	Lesson L;
	std::cin
		>> L.set.hour
		>> L.set.minute; //ввод кол-ва часов и минут
	return L;
}

void Lesson::print() //метод для разработчиков 
{
	std::cout 
		<< set.hour << " "  //печать введенного времени
		<< set.minute << "\n";
}

std::vector<Lesson> makevector() //вектор
{
	std::vector<Lesson> L; //определение вектора
	for (int i = 0; i < NUMBER_OF_LESSONS; i++)
	{
		Lesson Element;
		L.push_back(Element.input()); //помещение введенного времени в вектор
	}
	return L;
}

void sortvector(std::vector<Lesson>& S) //сортировка вектора
{
	int wall = 0; //"стенка"

	for (int i = 0; i < S.size() - 1; i++) //алгоритм сортировки выбором
	{
		int max = 0; //максимальное число

		for (int j = 1; j < S.size() - wall; j++) //поиск максимума 
		{
			if (S[j].timedif(S[j].set) 
			> S[max].timedif(S[max].set)) 
			//сравнение дельт установленного и текущего времени
			{
				max = j; //передача индекса максимуму 
			}
		}

		std::swap(S[max], S[S.size() - ++wall]); //перемещение 
		//последнего и максимального числа местами + отодвигание "стенки"
	}
}

void call(std::vector<Lesson> L) //звонок 
{
	while (true) //бесконечный цикл
	{
		for (int i = 0; i < L.size(); i++) //все звонки
		{
			//сон программы до посталенного времени
			Sleep(DWORD(L[i].timedif(L[i].set) * 1000)); 

			AudioDevicePtr device = OpenDevice(); //проигрыватель

			if (!device)
			{
				std::cout << "error no device"; //если программа не нашла проигрыватель
				exit(0);					//принудительный выход и сообщение об ошибке
			}

			OutputStreamPtr sound = OpenSound(device, "audio.mp3", true); //аудио файл

			if (!sound)
			{
				std::cout << "error no sound"; //если программа не нашла звуковой файл
				exit(0);					//принудительный выход и сообщение об ошибке
			}

			sound->setVolume(1.0); //громкость
			sound->reset(); //начало аудио-файла
			sound->play(); //воспроизведение аудио-файла
			Sleep(15000); //сон программы во время воспроизведения
			sound->stop(); //остановка воспроизведения
		}
	}
}
```
