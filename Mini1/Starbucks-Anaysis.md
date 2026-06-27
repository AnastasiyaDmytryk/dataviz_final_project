---
title: "Data Visualization - Mini-Project 1"
author: "Anastasiya Dmytryk `admytryk9341@floridapoly.edu`"
output:
  html_document:
    df_print: paged
---

# Mini project topic: Starbucks coffee.

Starbucks is one of the most known coffee chains, so much so that Starbucks invent drinks are now served at other places for example caramel machiatto was invented by Starbucks and is not an authentic Italian drink as many people may get confused. I am one of Starbuckses often costumers and i have noticed that the amount of sugar in my coffee is often more that the caffeine, however that is just a theory. With this mini project I envision to see if the amount of sugar exceeds amount of caffeine in some drinks, What are the top 5 drink that are the healthiest (has less amount of sugars, more protein, less fats). What is the worst drink for your health ( highest caffeine level, sugar, carbs, fats).

##First we read in the data

```{r}
 
library(tidyverse)
library(readr)
#the link provided was not working for me but i found the same data set online 
link <- "https://raw.githubusercontent.com/reisanar/datasets/master/starbucks.csv"
coffee <- read_csv(link)
head(coffee, 10)
```



## Second lets sort the drinks by the highest amount of caffeine

```{r}
library(dplyr)
#after exploring the data set I saw caffein is char so we need to make it a doubel 
coffee$`Caffeine (mg)` <- as.double(coffee$`Caffeine (mg)`)
#this lets me see what cofee has the most cafein, it is a brewed coffee 
coffee <- coffee %>% arrange(desc(`Caffeine (mg)`))
coffee


```

### Now lets look at our first visualization

## Visualization 1. What coffee has more sugar than caffeine ?

```{r}
library(ggplot2)
coffeeVsSugar<-coffee %>% drop_na(`Sugars (g)`, `Caffeine (mg)`) %>% filter(`Sugars (g)` > `Caffeine (mg)`) 

coffeeVsSugar
top5<-head(coffeeVsSugar,5)
top5
top5 %>% ggplot(aes(x = Calories , y =`Sugars (g)`,fill="Sugar" )) +
  geom_col() + geom_text(aes(label = Beverage,),vjust=1.5,  size = 2,color="Black") +
  labs(title = "Top 5 Drinks: Calories vs Sugar",
       x = "Calories", y = "Sugars (g)") 

#this label is confusing because it looks like it is the same drink, I will add another text layer to show that this drinks are prepared differently 

coffeeVsSugar
top5<-head(coffeeVsSugar,5)
top5
top5 %>% ggplot(aes(x = Calories , y =`Sugars (g)`,fill="Sugar" )) +
  geom_col() + geom_text(aes(label = Beverage,),vjust=1.5,  size = 2,color="Black")+ geom_text(aes(label = Beverage_prep,),vjust=4.5,  size = 2,color="Black") +
  labs(title = "Top 5 Drinks: Calories vs Sugar",
       x = "Calories", y = "Sugars (g)") 

#This is better but i want the graph to see the cafeein level too so we will add another layer 


top5 %>% ggplot(aes(x = Calories , y =`Sugars (g)`,fill="Sugar" )) +
  geom_col() + geom_text(aes(label = Beverage,),vjust=1.5,  size = 2,color="Black")+ 
  geom_text(aes(label = Beverage_prep,),vjust=4.5,  size = 2,color="Black") +
    geom_segment(aes(x = Calories - 5, xend = Calories + 5,  y = `Caffeine (mg)`, yend = `Caffeine (mg)`),color = "black", linetype = "solid", size = 1) +
  labs(title = "Top 5 Drinks with more sugar than caffein by calorie level",
       x = "Calories", y = "Sugars (g), Caffein level") 

#After doing the last visualization I realized how to improve this one 

top5%>% mutate(drink_label = paste(Beverage, Beverage_prep, sep = "\n"))  %>% ggplot(aes(x = Calories , y =`Sugars (g)`,fill=drink_label )) +
  geom_col()+ geom_segment(aes(x = Calories - 5, xend = Calories + 5,  y = `Caffeine (mg)`, yend = `Caffeine (mg)`),color = "black", linetype = "solid", size = 1) +
  
  labs(title = "Top 5 Drinks with more sugar than caffein by calorie level",
       x = "Calories", y = "Sugars (g), Caffein level") 
```

## Analysis 1

I was not able to figure out how to add a second bar graph to compare caffeine and sugar levels but I tried going around it in a creative way. From exploring the data set after sorting the drinks by the highest amount of sugar and lowest caffeine I have found out that the most sugary drink is hot chocolate. It was weird because hot chocolate seems to be one drink bu it is in all 5 first places, that is why i looked at the calories and different prep variation and i found out that different milks impact the drinks calories, even thought the caffeine level stays the same. Now there are completely non caffeinated drinks that have more or same amount of sugar however they did not interest me because I was looking specifically at coffee profiled drinks.

# Visualization 2.

## Top 5 drink that are the healthiest (has less amount of sugars, more protein, less fats)

```{r}
noSugarNoFat<-coffee%>%filter(`Sugars (g)`== 0, `Total Fat (g)`== 0, )
noSugarNoFat
healthy<-noSugarNoFat%>%arrange(desc(`Protein (g)`))
top5Healthy<-head(healthy, 5)

top5Healthy%>% ggplot(aes(x=Beverage_prep,y = Calories, fill= Beverage) )+ geom_col() +
  labs(title = "Top 5 Healthiest drinks (high protein, no sugar no fat), by calories",
       x = "Drink", y = "Calories")

```

## Analysis 2.

Coffee American Turned out to be the healthiest drink with the hugest amount of protein, which is weird. The way american is typically made is a shot of espresso and a glass of boiling water, by the idea of it there should not be any protein containing elements in the classic espresso. However Starbucks data set says it contains 1 gram of protein, the big question comes now where does that protein come from? Coffee natural contains protein but an 8 ounce shot would have less than 0.3 grams of protein, so only the Starbucks gods now how protein turned out to be in water.

The graph is colored by drink, and it reasonably shows the largest drink having the most calories, espresso is as healthy as a tall americano which makes sense because espresso served in Starbucks contains two shots of espresso ( a doppio) and a tall american contains 2 shots of espresso and hot water. Interesting fact americano is also a most popular coffee world wide.

# Visualization 3.

## What is the worst drink for your health ( highest sugar level, carbs, fats, caffeine).

```{r}

worst<-coffee%>%arrange( desc(`Sugars (g)`), desc(`Total Carbohydrates (g)`), desc(`Total Fat (g)`),desc(`Caffeine (mg)`),)%>% mutate(drink_label = paste(Beverage, Beverage_prep, sep = "\n")) 
#I kept getting same drinks so to not have to deal with the things like before i decided to combine drink name and prep style to be one drink label and then use that on my x axis 

worst
top5Worst<-head( worst,5)

top5Worst %>% ggplot(aes(x=drink_label,y = Calories, fill= Beverage) )+ geom_col() +
  labs(title = "Top 5 Worst drinks (highest sugar level,  carbs, fats, caffeine), by calories",
       x = "Drink", y = "Calories")+
    theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1, size=5))


```

## Analysis 3 .

From this visualization we found that frappuccinos are the worst drinks, Java chip frappuccinos has the highest amount of calories and it is the 5th worst drink by the amount pf sugar, carbs and total fats, the top 4 worst drinks are also all frappuccinos and contain caramel. Even though top 4 worst drinks have less calories than the java chip they contain more sugars carbs and total fats. This are the drinks I would advise anyone to avoid at Starbucks.

# Report Questions.

1.  What were the original charts you planned to create for this assignments?

    The original charts I have planed to create are my Visualization 1, 2 and 3. They go over the drink with more sugar vs caffeine, the healthiest drink and the worst drink for your health.

2.  What story could you tell with your plots?

    This plots can be used for the Starbuckses dietary report, if the company ever decides to take a turn to a healthy lifestyle company they can use visualization 1 and 3 to see where they can improve. Also the visualizations can be used by any dietitians who want to show their clients that their life style might not be as healthy as they think it is by showing same visualizations 1 and 3 and provide an example of what drink they can have instead visualization 2.

3.  How did you apply the principles of data visualizations and design for this assignment?

    I tried to not over complicate my graphs, I don't think my graph 1 was very good but I tried multiple approaches, I feel like I have definitely improved by the last graph. I made sure my graphs all had labels, and titles, and that colors represent distinct details of the graph not just aesthetics.

## Post analysis.

My top drink of choice is a shot of espresso with a splash of half and half and some simple syrup. It is not on the regular menu but we found out that plain espresso is one of the healthiest drinks you can order. This makes me feel better about my drink order. My other drinks of choice such as lattes were not in the worst or the best categories which also makes me feel better about them. I was surprised to learn that the hot chocolate at Starbucks contains caffeine. When i was doing the visualization for the first idea "what drinks have more sugars than caffeine" I have expected it all to be different kinds of frappuccinos and I was very surprise not to see a single frapp.
