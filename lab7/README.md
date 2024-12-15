# Анализ данных сетевого трафика при помощи библиотеки Arrow

## Цель работы

1. Изучить возможности технологии Apache Arrow для обработки и анализ больших данных
2. Получить навыки применения Arrow совместно с языком программирования R
3. Получить навыки анализа метаинфомации о сетевом трафике
4. Получить навыки применения облачных технологий хранения, подготовки и анализа данных: Yandex Object Storage, Rstudio Server.

## Исходные данные

1. Программное обеспечение Windows 10
2. Rstudio Desktop и библиотека Dplyr
3. Интерпретатор яз/*ыка R 4.1
4. Сервис Yandex DataLens
5. Apache Arrow

## Задание
Используя язык программирования R, библиотеку arrow и облачную IDE Rstudio Server, развернутую в Yandex Cloud, выполнить задания и составить отчет.

## План

1. Выполнение практического задания
2. Составление отчета

## Ход работы

### Импорт данных

```{r}
library(arrow)
```

```{r}
library(tidyverse)
```
```{r}
#download.file("https://storage.yandexcloud.net/arrow-datasets/tm_data.pqt",destfile = "tm_data.pqt")
df <- arrow::open_dataset(sources = "tm_data.pqt", format = "parquet")
glimpse(df)
```
### Найдите утечку данных из вашей сети

Важнейшие документы с результатами нашей исследовательской деятельности в
области создания вакцин скачиваются в виде больших заархивированных дампов.
Один из хостов в нашей сети используется для пересылки этой информации – он
пересылает гораздо больше информации на внешние ресурсы в Интернете, чем
остальные компьютеры нашей сети. Определите его IP-адрес.

```{r}
task1 <- df %>% filter(str_detect(src, "^12.") | str_detect(src, "^13.") | str_detect(src, "^14."))  %>% filter(!str_detect(dst, "^12.") & !str_detect(dst, "^13.") & !str_detect(dst, "^14."))  %>% group_by(src) %>% summarise("sum" = sum(bytes)) %>% arrange(desc(sum)) %>% head(1) %>% select(src) 
task1 %>% collect() %>% knitr::kable()
```
### Найдите утечку данных из вашей сети 2

Другой атакующий установил автоматическую задачу в системном планировщике
cron для экспорта содержимого внутренней wiki системы. Эта система генерирует
большое количество трафика в нерабочие часы, больше чем остальные хосты.
Определите IP этой системы. Известно, что ее IP адрес отличается от нарушителя из
предыдущей задачи.

```{r}
task21 <- df %>% select(timestamp, src, dst, bytes) %>% mutate(trafic = (str_detect(src, "^((12|13|14)\\.)") & !str_detect(dst, "^((12|13|14)\\.)")),time = hour(as_datetime(timestamp/1000))) %>% filter(trafic == TRUE, time >= 0 & time <= 24) %>% group_by(time) %>%
summarise(trafictime = n()) %>% arrange(desc(trafictime))
task21 %>% collect() %>% knitr::kable()
```
```{r}
task22 <- df %>% mutate(time = hour(as_datetime(timestamp/1000))) %>% 
filter(!str_detect(src, "^13.37.84.125")) %>%  filter(str_detect(src, "^12.") | str_detect(src, "^13.") | str_detect(src, "^14."))  %>% filter(!str_detect(dst, "^12.") | !str_detect(dst, "^13.") | !str_detect(dst, "^14."))  %>% filter(time >= 1 & time <= 15) %>%  group_by(src) %>% summarise("sum" = sum(bytes)) %>% arrange(desc(sum)) %>% head(1) %>% select(src) 
task22 %>% collect() %>% knitr::kable()
```

### Найдите утечку данных в вашей сети 3

Еще один нарушитель собирает содержимое электронной почты и отправляет в
Интернет используя порт, который обычно используется для другого типа трафика.
Атакующий пересылает большое количество информации используя этот порт,
которое нехарактерно для других хостов, использующих этот номер порта.
Определите IP этой системы. Известно, что ее IP адрес отличается от нарушителей
из предыдущих задач.

```{r}
task31 <- df %>% filter(!str_detect(src, "^13.37.84.125")) %>% filter(!str_detect(src, "^12.55.77.96")) %>% filter(str_detect(src, "^12.") | str_detect(src, "^13.") | str_detect(src, "^14."))  %>% filter(!str_detect(dst, "^12.") & !str_detect(dst, "^13.") & !str_detect(dst, "^14."))  %>% select(src, bytes, port) 


task31 %>%  group_by(port) %>% summarise("mean"=mean(bytes), "max"=max(bytes), "sum" = sum(bytes)) %>% 
  mutate("Raz"= max-mean)  %>% filter(Raz!=0) %>% arrange(desc(Raz)) %>% head(1) %>% collect() %>% knitr::kable()
```
```{r}
task32 <- task31  %>% filter(port==37) %>% group_by(src) %>% summarise("mean"=mean(bytes)) %>% arrange(desc(mean)) %>% head(1) %>% select(src)
task32 %>% collect() %>% knitr::kable()
```

## Оценка результата

При использовании языка программирования R и библиотеки arrow были успешно выполнены задания по анализу трафика и нахождению утечек из сети.

## Вывод

Я ознакомилась с применением облачных технологий хранения, подготовки и анализа данных, а также проанализировала метаинформацию о сетевом трафике.
