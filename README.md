# coronavirus
# RMarkdown - Flex Dashboards for Tracking nCoronavirus Outbreak

![alt text](https://github.com/SaiRamakuru/coronavirus/blob/master/Trends.png "Coronavirus")


Coronaviruses (CoV) are a large family of viruses that cause illness ranging from the common cold to more severe diseases such as Middle East Respiratory Syndrome (MERS-CoV) and Severe Acute Respiratory Syndrome (SARS-CoV).

Coronavirus disease (COVID-19) is a new strain that was discovered in 2019 and has not been previously identified in humans.

Common signs of infection include respiratory symptoms, fever, cough, shortness of breath and breathing difficulties. In more severe cases, infection can cause pneumonia, severe acute respiratory syndrome, kidney failure and even death.

Standard recommendations to prevent infection spread include regular hand washing, covering mouth and nose when coughing and sneezing, thoroughly cooking meat and eggs. Avoid close contact with anyone showing symptoms of respiratory illness such as coughing and sneezing..
 
![alt text](https://github.com/SaiRamakuru/coronavirus/blob/master/download.jfif "Coronavirus")


# Links: 
CSV is  available here [LINK](https://github.com/RamiKrispin/coronavirus-csv)

Medium Articles are availbe here [LINK](https://link.medium.com/BcvGJbh504)

Live Dashboard is availbe here [LINK](https://rpubs.com/YesKay/Covidv3)



# Contributors: 
Rami Krispin: [LINK](https://github.com/RamiKrispin)



# Other Sourcers: 
Tomas Pueyo: [LINK](https://medium.com/@tomaspueyo)

WHO: [LINK](https://www.who.int/)


# 1. Getting the Data: 

coronavirus <- read.csv("https://raw.githubusercontent.com/RamiKrispin/coronavirus-csv/master/coronavirus_dataset.csv", header = TRUE)

# 2. Prepping the Data: 
df <- coronavirus %>% 
  dplyr::filter(date == max(date)) %>%
  
  dplyr::group_by(Country.Region, type) %>%
  
  dplyr::summarise(total = sum(cases)) %>%
  
  tidyr::pivot_wider(names_from =  type,

                     values_from = total) %>%
                     
  dplyr::mutate(unrecovered = confirmed - ifelse(is.na(recovered), 0, recovered) - ifelse(is.na(death), 0, death)) %>%
  
  dplyr::arrange(-confirmed) %>%
  
  dplyr::ungroup() %>%
  
  dplyr::mutate(country = Country.Region) %>%
  
  dplyr::mutate(country = trimws(country)) %>% 
  
  dplyr::mutate(country = factor(country, levels = country))
  
  
  
# 3. Sample Plotting: 

daily_confirmed <- coronavirus %>%

  dplyr::filter(type == "confirmed") %>%
  
  dplyr::mutate(country = Country.Region) %>%
  
  dplyr::group_by(date, country) %>%
  
  dplyr::summarise(total = sum(cases)) %>% 
  
  dplyr::ungroup() %>%
  
  tidyr::pivot_wider(names_from = country, values_from = total) 
  
  
  
daily_confirmed %>%

  plotly::plot_ly() %>% 
  
  plotly::add_trace(x = ~ date, 
  
                    y = ~ US, 
                    
                    type = "scatter", 
                    
                    mode = "lines+markers",
                    
                    name = "US") %>% 
                    
  plotly::layout(title = "",
  
                 legend = list(x = 0.1, y = 0.9),
                 
                 yaxis = list(title = "Daily Cases"),
                 
                 xaxis = list(title = ""),
                 
                 # paper_bgcolor = "black",
                 
                 plot_bgcolor = "#ffcccc",
                 
                 # font = list(color = 'white'),
                 
                 hovermode = "compare",
                 
                 margin =  list(
                   # l = 60,
                   # r = 40,
                   b = 10,
                   t = 10,
                   pad = 2
                 ))
# 4. ML 
To be updated.....


