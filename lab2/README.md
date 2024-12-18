# Анализ встроенного пакета dplyr

## Цель работы
1. Развить практические навыки использования языка программирования R для обработки данных
2. Закрепить знания базовых типов данных языка R
3. Развить практические навыки использования функций обработки данных пакета dplyr – функции select(), filter(), mutate(), arrange(), group_by()

### Установка библиотеки dplyr

```{r}
install.packages("dplyr")
library(dplyr)
```

### Количество строк в датафрейме

```{r}
starwars

```
```{r}
starwars %>% nrow()
```
### Количество столбцов в датафрейме

```{r}
starwars %>% ncol()

```

### Примерный вид датафрейма

```{r}
starwars %>% glimpse()
```
### Уникальные расы персонажей в датафрейме

```{r}
length(unique(starwars$species))  

```
### Самый высокий персонаж

```{r}
starwars %>% arrange(desc(height)) %>% slice_head(n = 1)
```
### Все персонажи ниже 170


```{r}
starwars %>% filter(height < 170) %>% select(name)
```


### Индекс массы тела всех персонажей

```{r}
I <- starwars %>%
  mutate(I = mass / (height ^ 2))
print(I %>% select(name, mass, height, I))
```
### Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по отношению массы (mass) к росту (height) персонажей.

```{r}
starwars %>% mutate(extent = mass / height) %>%  arrange(desc(extent)) %>% slice_head(n = 10) %>%      select(name, extent)
```
### Найти средний возраст персонажей каждой расы вселенной Звездных войн

```{r}
starwars <- starwars %>%  mutate(age = 2024 - birth_year)
average_age <- starwars %>% group_by(species) %>% summarise(average_age = mean(age, na.rm = TRUE))
print(average_age)
```

### Найти самый распространенный цвет глаз
```{r}
popular_eye_color <- starwars %>% group_by(eye_color) %>% summarize(count = n()) %>%      arrange(desc(count)) %>% slice_head(n = 1)                       
popular_eye_color
```
### Подсчитать среднюю длину имени в каждой расе вселенной Звездных войн

```{r}
average_name_length <- starwars %>% mutate(name_length = nchar(name)) %>% group_by(species) %>%   summarize(avg_length = mean(name_length, na.rm = TRUE))  
average_name_length
```

