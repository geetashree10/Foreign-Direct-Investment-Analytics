# Foreign-Direct-Investment-Analytics


```
install.packages(c("tidyverse", "lubridate", "ggplot2", "dplyr", "readr", "reshape2"))
library(tidyverse)
file_path <- "C:/Users/geeta/Downloads/fdidata.csv"
data <- read_csv(file_path)
```

# Sector-wise total investment over the years
```
sector_total <- data %>% 
  gather(Year, Investment, -Sector) %>%
  group_by(Sector) %>%
  summarise(Total_Investment = sum(Investment))
sector_total
 ```

# Year-wise total investment

```
year_total <- data %>%
  gather(Year, Investment, -Sector) %>%
  group_by(Year) %>%
  summarise(Total_Investment = sum(Investment))
year_total
```

![image](https://github.com/geetashree10/Foreign-Direct-Investment-Analytics/assets/158750505/891a41cf-7a83-4257-badc-917333f4a554)


#visualising the data
```
ggplot(year_total, aes(x = Year, y = Total_Investment)) +
  geom_line(group = 1) +
  labs(title = "Total Investment by Year", x = "Year", y = "Total Investment (in crores)")
```

# sector-wise total and average investments
```
sector_analysis <- data %>%
  gather(Year, Investment, -Sector) %>%
  group_by(Sector) %>%
  summarise(Total_Investment = sum(Investment), Average_Investment = mean(Investment))
sector_analysis
```
# Sectors with highest and lowest investments
```
top_sectors <- sector_analysis %>% arrange(desc(Total_Investment))
top_sectors
bottom_sectors <- sector_analysis %>% arrange(Total_Investment)
bottom_sectors
```
# year-wise total and average investments
```
year_analysis <- data %>%
  gather(Year, Investment, -Sector) %>%
  group_by(Year) %>%
  summarise(Total_Investment = sum(Investment), Average_Investment = mean(Investment))
year_analysis
```
# Plotting trends over the years
```
ggplot(year_analysis, aes(x = Year, y = Total_Investment)) +
  geom_line(group = 1) +
  labs(title = "Year-wise Total Investment", x = "Year", y = "Total Investment (in crores)")
```
# Correlation analysis
```investment_matrix <- data %>% select(-Sector) %>% as.matrix()
cor_matrix <- cor(investment_matrix, use = "pairwise.complete.obs")
install.packages("corrplot")
library(corrplot)
corrplot(cor_matrix, method = "circle")
```

![image](https://github.com/geetashree10/Foreign-Direct-Investment-Analytics/assets/158750505/22db400c-a8c0-4843-b468-2b1a05b71636)

```
install.packages("forecast")
library(forecast)
```

# Converting the data into time series object
```
ts_data <- ts(year_analysis$Total_Investment, start = c(2000, 1), frequency = 1)
```
# Forecasting
```
fit <- auto.arima(ts_data)
forecasted <- forecast(fit, h = 5)
forecasted
```
# Plot the forecast
```
autoplot(forecasted) +
  labs(title = "Forecasted Total Investment", x = "Year", y = "Investment (in crores)")
```
![image](https://github.com/geetashree10/Foreign-Direct-Investment-Analytics/assets/158750505/cc4ffb9d-8d69-4457-a1de-e89204d906ff)

# descriptive statistics
```
summary(sector_analysis)
summary(year_analysis)

```



