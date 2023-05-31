# lab07
Данная лабораторная работа посвещена изучению систем автоматизации развёртывания и управления приложениями на примере Docker

# Task 1



Давайте создадим исполняемые файлы для этой лабораторной работы. Они будут находиться в отдельных папках, которые затем будут связаны с помощью :CMakeLists.txt

Делаем print.hpp

![image](https://github.com/lepeha81/lab07/blob/main/1.PNG)

print.hpp содержание:

![image](https://github.com/lepeha81/lab07/blob/main/2.PNG)

Делаем print.cpp

print.cpp содержание:

![image](https://github.com/lepeha81/lab07/blob/main/3.PNG)

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

![image](https://github.com/lepeha81/lab07/blob/main/10.PNG)


После сделаем файл.yml

![image](https://github.com/lepeha81/lab07/blob/main/11.PNG)

![image](https://github.com/lepeha81/lab07/blob/main/12.PNG)

![image](https://github.com/lepeha81/lab07/blob/main/13.PNG)


Теперь мы будем тестировать наш проект в локальных репозиториях

Загружать:Docker

![image](https://github.com/lepeha81/lab07/blob/main/14.PNG)


And test:

![image](https://github.com/lepeha81/lab07/blob/main/15.PNG)

![image](https://github.com/lepeha81/lab07/blob/main/16.PNG)
