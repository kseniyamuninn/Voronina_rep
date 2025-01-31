# План

1.  Установить программный пакет nyclights13
2.  Проанализировать набор данных
3.  ответить на вопросы

## Порядок действий

#### 1. Установка пакета nycflights13

```{r}
install.packages("nycflights13")
```

#### 2. Загрузка библиотеки

```{r}
library(nycflights13)
library(dplyr)
```
#### 3. Выполнение заданий

##### а)Сколько встроенных в пакет nycflights13 датафреймов?

```{r}
length(data(package = "nycflights13")$results[, "Item"])
```
##### б) Сколько строк в каждом датафрейме?

```{r}
list(
  flights = nrow(flights),
  airlines = nrow(airlines),
  airports = nrow(airports),
  planes = nrow(planes),
  weather = nrow(weather)
)
```
##### в) Сколько столбцов в каждом датафрейме?

```{r}
list(
  flights = ncol(flights),
  airlines = ncol(airlines),
  airports = ncol(airports),
  planes = ncol(planes),
  weather = ncol(weather)
)
```

##### г) Как просмотреть примерный вид датафрейма?

```{r}
flights %>% glimpse()
```

##### д) Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных (представлено в наборах данных)?

```{r}
flights %>% filter(!is.na(carrier)) %>% distinct(carrier) %>% nrow()

```

##### е) Сколько рейсов принял аэропорт John F Kennedy Intl в мае?

```{r}
flights %>% filter(origin == "JFK", month == 5) %>% nrow()

```
##### ж) Какой самый северный аэропорт?

```{r}
airports %>% arrange(desc(lat)) %>% slice(1)
```
##### з) Какой аэропорт самый высокогорный (находится выше всех над уровнем моря)?

```{r}
airports %>% arrange(desc(alt)) %>% slice(1)
```

##### и)Какие бортовые номера у самых старых самолетов?

```{r}
planes %>% arrange(desc(year)) %>% select(tailnum)
```

##### к)  Какая средняя температура воздуха была в сентябре в аэропорту John F Kennedy Intl (в градусах Цельсия).

```{r}
weather %>% filter(origin == "JFK", month == 9) %>% summarise(avg_temp = mean((temp - 32) * 5 / 9, na.rm = TRUE))
```

##### л) Самолеты какой авиакомпании совершили больше всего вылетов в июне?

```{r}
flights %>% filter(month == 6) %>% count(carrier, sort = TRUE)
```

#####  м) Самолеты какой авиакомпании задерживались чаще других в 2013 году?

```{r}
flights %>% filter(dep_delay > 0 & year == 2013) %>% count(carrier, sort = TRUE)
```

# Вывод

По результатам проделанной работы был изучен пакет nycflights13 и отработаны навыки по работе с наборами данных
