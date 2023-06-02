# lab07
Данная лабораторная работа посвещена изучению систем автоматизации развёртывания и управления приложениями на примере Docker

# Task 1



Давайте создадим исполняемые файлы для этой лабораторной работы. Они будут находиться в отдельных папках, которые затем будут связаны с помощью :CMakeLists.txt

Делаем print.hpp

![image](https://github.com/lepeha81/lab07/blob/main/1.PNG)

print.hpp содержание:

![image](https://github.com/lepeha81/lab07/blob/main/2.PNG)

#include <fstream> - подключение библиотеки для работы с файловыми потоками ввода/вывода.

#include <iostream> - подключение библиотеки для работы со стандартными потоками ввода/вывода.

#include <string> - подключение библиотеки для работы со строками.

void print(const std::string& text, std::ofstream& out); - объявление функции для вывода строки в файловый поток.

void print(const std::string& text, std::ostream& out = std::cout); - объявление функции для вывода строки в стандартный поток ввода/вывода с параметром по умолчанию - стандартный поток вывода.
  
Делаем print.cpp

print.cpp содержание:

![image](https://github.com/lepeha81/lab07/blob/main/3.PNG)
  
 #include <print.hpp> - подключение заголовочного файла print.hpp.

void print(const std::string& text, std::ostream& out) - определение функции для вывода строки в стандартный поток ввода/вывода.

{
out << text; - вывод строки в заданный поток.

}

void print(const std::string& text, std::ofstream& out) - определение функции для вывода строки в файловый поток.

{
out << text; - вывод строки в заданный поток.

}


Делаем main.cpp

![image](https://github.com/lepeha81/lab07/blob/main/4.PNG)

main.cpp содержание:
```
#include <print.hpp>
#include <cstdlib>

int main(int argc, char* argv[])
{
  const char* log_path = std::getenv("LOG_PATH");
  if (log_path == nullptr)
  {
    std::cerr << "undefined environment variable: LOG_PATH" << std::endl;
    return 1;
  }
  std::string text;
  while (std::cin >> text)
  {
    std::ofstream out{log_path, std::ios_base::app};
    print(text, out);
    out << std::endl;
  }
}
```
#include <print.hpp> - подключение заголовочного файла print.hpp.

#include <cstdlib> - подключение библиотеки для работы с переменными окружения.

int main(int argc, char* argv[]) - определение функции main.

{

const char* log_path = std::getenv("LOG_PATH"); - получение значения переменной окружения LOG_PATH.

if (log_path == nullptr) - проверка значения переменной окружения.

{

std::cerr << "undefined environment variable: LOG_PATH" << std::endl; - вывод сообщения об ошибке в поток стандартного вывода ошибок.

return 1; - возврат значения 1 (ошибка).

}

std::string text; - объявление переменной типа string.

while (std::cin >> text) - цикл чтения строк из стандартного ввода.

{

std::ofstream out{log_path, std::ios_base::app}; - открытие файла для записи в режиме дополнения.

print(text, out); - вызов функции вывода строки в файловый поток.

out << std::endl; - добавление символа переноса строки в конец файла.

}

}
  
![image](https://github.com/lepeha81/lab07/blob/main/5.PNG)


Также делаем каталог, в котором у нас есть файл logs\.txt

После того, как мы сделаем ссылку на наш проект CMakeLists.txt

![image](https://github.com/lepeha81/lab07/blob/main/6.PNG)

CMakeLists.txt содержание:

![image](https://github.com/lepeha81/lab07/blob/main/6.PNG)

![image](https://github.com/lepeha81/lab07/blob/main/7.PNG)

![image](https://github.com/lepeha81/lab07/blob/main/8.PNG)


И сделать с помощью Docker Dockerfile

Dockerfile содержание:

![image](https://github.com/lepeha81/lab07/blob/main/9.PNG)

FROM ubuntu:20.04 - начало создания Docker-образа на основе образа Ubuntu версии 20.04.

RUN apt update - обновление реестра пакетов Ubuntu.

RUN apt install -yy gcc g++ cmake - установка компиляторов gcc, g++ и сборщика CMake.

COPY . print/ - копирование всех файлов в текущей директории в папку print внутри контейнера.

WORKDIR print - установка рабочей директории контейнера на директорию print.

RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install - запуск CMake для генерации файлов для сборки.

RUN cmake --build _build - запуск команды сборки проекта.

RUN cmake --build _build --target install - запуск команды установки собранного проекта.

ENV LOG_PATH /home/lab-07/logs/log.txt - определение переменной среды LOG_PATH со значением /home/lab-07/logs/log.txt.

VOLUME /home/lab-07/logs - создание тома для директории /home/lab-07/logs.

WORKDIR _install/bin - установка рабочей директории контейнера на директорию _install/bin.

ENTRYPOINT ./demo - установка точки входа для запуска демонстрационного приложения.
![image](https://github.com/lepeha81/lab07/blob/main/10.PNG)


После сделаем файл.yml

![image](https://github.com/lepeha81/lab07/blob/main/11.PNG)

![image](https://github.com/lepeha81/lab07/blob/main/12.PNG)

  - name: docker - название конфигурации.

on: push: - устанавливает условия для запуска на github - при пуше в репозиторий.

jobs: - определяет блок с отдельными задачами.

build: - имя работы.

runs-on: ubuntu-latest - выбор контейнера для выполнения задачи.

steps: - массив шагов, которые должны быть выполнены внутри работы.

- uses: actions/checkout@v3 - клонирование репозитория.

- name: build docker - название шага.

run: docker build -t logger . - создание образа докера с именем logger.

  ![image](https://github.com/lepeha81/lab07/blob/main/13.PNG)


Теперь мы будем тестировать наш проект в локальных репозиториях

Загружать:Docker

![image](https://github.com/lepeha81/lab07/blob/main/14.PNG)


And test:

![image](https://github.com/lepeha81/lab07/blob/main/15.PNG)

![image](https://github.com/lepeha81/lab07/blob/main/16.PNG)
