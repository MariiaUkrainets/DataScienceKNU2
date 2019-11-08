Лабораторна робота № 4. Зчитування даних з реляційних баз даних.
Дані для цієї лабораторної роботи взяті з
«https://www.kaggle.com/benhamner/nips-2015-papers»
Для виконання лабораторної необхідно завантажити файл бази даних SQLite за
посиланням: «https://www.dropbox.com/s/pf2htfcrdoqh3ii/database.sqlite?dl=0».
В цьому файлі містяться дані по доповідям на Neural Information Processing
Systems (NIPS) яка є однією з ведучих конференцій по машинному навчанню в
світі. База даних складається з наступних таблиць:
  
Для роботи з базою даних SQLite в R можна використовувати бібліотекі DBI та
RSQLite.
Спочатку необхідно створити з’єдання з базою даних
conn <- dbConnect(RSQLite::SQLite(), "Шлях до БД")
Функція dbSendQuery(conn, "Запит») формує запит і виконує його в БД. Для
отримання результатів у вигляді фрейму даних необхідно використати функцію
dbFetch(res, n), де n – кількість строк, які будуть отримані. Для обмеження виводу
даних, в подальших завданнях встановіть n = 10.

```R

library(RSQLite)
library(DBI)
conn <- dbConnect(RSQLite::SQLite(), "E:\\R\\database.sqlite")
res <- dbSendQuery(conn, "SELECT Name as Author FROM Authors ORDER BY Name")
df <- dbFetch(res, n=10)
dbClearResult(res)
dbDisconnect(conn)
df

```

В результаті виконання лабораторної роботи необхідно створити фрейми даних:
1. Назва статті (Title), тип виступу (EventType). Необхідно вибрати тільки статті
с типом виступу Spotlight. Сортування по назві статті.
```R
conn <- dbConnect(RSQLite::SQLite(), "E:\\R\\database.sqlite")
res1 <- dbSendQuery(conn, "SELECT title, eventtype FROM Papers where eventtype='Spotlight' ORDER by title;")
df1 <- dbFetch(res1,15)
dbClearResult(res1)
dbDisconnect(conn)
df1
```
                                                                                           Title EventType
1  A Tractable Approximation to Optimal Point Process Filtering: Application to Neural Encoding Spotlight
2                                    Accelerated Mirror Descent in Continuous and Discrete Time Spotlight
3                        Action-Conditional Video Prediction using Deep Networks in Atari Games Spotlight
4                                                                      Adaptive Online Learning Spotlight
5                          Asynchronous Parallel Stochastic Gradient for Nonconvex Optimization Spotlight
6                                                 Attention-Based Models for Speech Recognition Spotlight
7                                                       Automatic Variational Inference in Stan Spotlight
8                                   Backpropagation for Energy-Efficient Neuromorphic Computing Spotlight
9                       Bandit Smooth Convex Optimization: Improving the Bias-Variance Tradeoff Spotlight
10                         Biologically Inspired Dynamic Textures for Probing Motion Perception Spotlight
11                        Closed-form Estimators for High-dimensional Generalized Linear Models Spotlight
12             Collaborative Filtering with Graph Information: Consistency and Scalable Methods Spotlight
13                           Color Constancy by Learning to Predict Chromaticity from Luminance Spotlight
14                                                Data Generation as Sequential Decision Making Spotlight
15                      Decoupled Deep Neural Network for Semi-supervised Semantic Segmentation Spotlight



2. Ім’я автора (Name), Назва статті (Title). Необхідно вивести всі назви статей
для автора «Josh Tenenbaum». Сортування по назві статті.
```R
conn <- dbConnect(RSQLite::SQLite(), "E:\\R\\database.sqlite")
res2 <- dbSendQuery(conn, "SELECT a.name, p.title FROM authors a JOIN PaperAuthors pa ON a.Id=pa.AuthorId JOIN Papers p ON p.Id=pa.paperid WHERE Name='Josh Tenenbaum' ORDER by Title;")
df2 <- dbFetch(res2)
dbClearResult(res2)
dbDisconnect(conn)
df2
```
            Name                                                                                             Title
1 Josh Tenenbaum                                                       Deep Convolutional Inverse Graphics Network
2 Josh Tenenbaum Galileo: Perceiving Physical Object Properties by Integrating a Physics Engine with Deep Learning
3 Josh Tenenbaum                                                Softstar: Heuristic-Guided Probabilistic Inference
4 Josh Tenenbaum                                                        Unsupervised Learning by Program Synthesis




3. Вибрати всі назви статей (Title), в яких є слово «statistical». Сортування по
назві статті.

```R
conn <- dbConnect(RSQLite::SQLite(), "E:\\R\\database.sqlite")
res3 <- dbSendQuery(conn, "SELECT title from Papers where title LIKE '%statistical%' ORDER by Title;")
df3 <- dbFetch(res3)
dbClearResult(res3)
dbDisconnect(conn)
df3

                                                                                 Title
1 Adaptive Primal-Dual Splitting Methods for Statistical Learning and Image Processing
2                                Evaluating the statistical significance of biclusters
3                  Fast Randomized Kernel Ridge Regression with Statistical Guarantees
4     High Dimensional EM Algorithm: Statistical Optimization and Asymptotic Normality
5                Non-convex Statistical Optimization for Sparse Tensor Graphical Model
6            Regularized EM Algorithms: A Unified Framework and Statistical Guarantees
7                            Statistical Model Criticism using Kernel Two Sample Tests
8                         Statistical Topological Data Analysis - A Kernel Perspective




```


4. Ім’я автору (Name), кількість статей по кожному автору (NumPapers).
Сортування по кількості статей від більшої кількості до меньшої.

```R
conn <- dbConnect(RSQLite::SQLite(), "E:\\R\\database.sqlite")
res4 <- dbSendQuery(conn, "SELECT a.name, count(p.title) NumPapers FROM Authors a JOIN PaperAuthors pa ON a.id=pa.authorid JOIN papers p ON p.id=pa.PaperId GROUP by name ORDER by NumPapers DESC;")
df4 <- dbFetch(res4, 15)
dbClearResult(res4)
dbDisconnect(conn)
df4
```
                   Name NumPapers
1  Pradeep K. Ravikumar         7
2               Han Liu         6
3        Lawrence Carin         6
4   Inderjit S. Dhillon         5
5               Le Song         5
6     Zoubin Ghahramani         5
7        Christopher Re         4
8      Csaba Szepesvari         4
9           Honglak Lee         4
10       Josh Tenenbaum         4
11    Michael I. Jordan         4
12       Percy S. Liang         4
13         Prateek Jain         4
14        Ryan P. Adams         4
15          Shie Mannor         4

