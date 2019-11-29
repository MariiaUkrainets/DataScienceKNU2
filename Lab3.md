В цій лабораторній роботі необхідно зчитати WEB сторінку з сайту IMDB.com зі 100 фільмами 2017 року виходу за посиланням 
«http://www.imdb.com/search/title?count=100&release_date=2017,2017&title_type=feature».  Необхідно створити data.frame 
«movies» з наступними даними: номер фільму (rank_data), назва фільму (title_data), тривалість (runtime_data). 
Для виконання лабораторної рекомендується використати бібліотеку «rvest». CSS селектори для зчитування необхідних даних: 
rank_data: «.text-primary», title_data: «.lister-item-header a», runtime_data: «.text-muted .runtime». Для зчитування url 
використовується функція read_html, для зчитування даних по CSS селектору – html_nodes і для перетворення зчитаних html 
даних в текст - html_text. Рекомендується перетворити rank_data та runtime_data з тексту в 
числові значення. При формуванні дата фрейму функцією data.frame рекомендується використати параметр «stringsAsFactors = FALSE». 

```R
library(rvest)
library(stringr)

movie_file <- read_html('http://www.imdb.com/search/title?count=100&release_date=2017,2017&title_type=feature')

rank_data <- html_nodes(movie_file, '.text-primary')
rank_text <- html_text(rank_data, trim = TRUE)
rank_data <- as.integer(rank_text)

title_data <- html_nodes(movie_file, '.lister-item-header a')
title_text <- html_text(title_data, trim = TRUE)
title_data <- title_text

runtime_data <- html_nodes(movie_file, '.text-muted .runtime')
runtime_text <- html_text(runtime_data, trim = TRUE)
runtime_data <- str_remove(runtime_text, 'min')
runtime_data <- as.integer(runtime_data)

Sys.setlocale(locale = 'Ukrainian')
movies <- data.frame(rank_data, title_data, runtime_data, stringsAsFactors = FALSE)
head(movies, 10)

   rank_data                              title_data runtime_data
1          1              Джуманджi: Поклик джунглiв          119
2          2 Зорянi вiйни: Епiзод 8 - Останнi Джедаi          152
3          3                                    Воно          135
4          4            Той, хто бiжить по лезу 2049          164
5          5                    Call Me by Your Name          132
6          6                     Рятувальники Малiбу          116
7          7                     Лiга справедливостi          120
8          8                 Kingsman: Золоте кiльце          141
9          9                    Найвеличнiший шоумен          105
10        10                           Тор: Рагнарок          130

````
В результаті виконання лабораторної роботи необхідно відповісти на запитання: 
  
  
1. Виведіть перші 6 назв фільмів дата фрейму. 

```R
head(movies$title_data, 6)

[1] "Джуманджi: Поклик джунглiв"              "Зорянi вiйни: Епiзод 8 - Останнi Джедаi"
[3] "Воно"                                    "Той, хто бiжить по лезу 2049"           
[5] "Call Me by Your Name"                    "Рятувальники Малiбу"                   
```

2. Виведіть всі назви фільмів с тривалістю більше 120 хв.

```R
subset(movies$title_data, runtime_data > 120)

 [1] "Зорянi вiйни: Епiзод 8 - Останнi Джедаi"  "Воно"                                    
 [3] "Той, хто бiжить по лезу 2049"             "Call Me by Your Name"                    
 [5] "Kingsman: Золоте кiльце"                  "Тор: Рагнарок"                           
 [7] "Вартовi Галактики 2"                      "Красуня i Чудовисько"                    
 [9] "Людина-павук: Повернення додому"          "Пiрати Карибського моря: Помста Салазара"
[11] "Логан: Росомаха"                          "Гра Моллi"                               
[13] "Saban's Могутнi рейнджери"                "Диво-Жiнка"                              
[15] "Форма води"                               "Мати!"                                   
[17] "Зменшення"                                "Трансформери: Останнiй лицар"            
[19] "Валерiан i мiсто тисячi планет"           "Джон Уiк 2"                              
[21] "Форсаж 8"                                 "Темнi часи"                              
[23] "Чужий: Заповiт"                           "Вбивство священного оленя"               
[25] "Примарна нитка"                           "Король Артур: Легенда меча"              
[27] "Вороги"                                   "Вiйна за планету мавп"                   
[29] "1+1: Нова iсторiя"                        "Метелик"                                 
[31] "Only the Brave"                           "Сiм сестер"                              
[33] "Усi грошi свiту"                          "Пострiл в безодню"                       
[35] "War Machine"                              "The Glass Castle"   

```

3. Скільки фільмів мають тривалість менше 100 хв. 
```R
length(subset(movies$title_data, runtime_data > 100))

[1] 83
```