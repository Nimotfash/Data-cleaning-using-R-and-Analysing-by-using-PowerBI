Aim: To analyze the performance of Hollywood movies 

Data: Title, genre, studio, profitability and ratings for movies released 2007-2012. Source: InformationIsBeautiful.net Download data from this link: 
https://public.tableau.com/app/sample-data/HollywoodsMostProfitableStories.csv
![image](https://github.com/Nimotfash/POWERBI/assets/136696913/fda2db23-24fa-4152-8975-e68e75e38a89)

Load data 

df<- read.csv("/cloud/project/HollywoodsMostProfitableStories.csv")

#Take a look at the data: 

View(df)

#Load library: 

install.packages("tidyverse")
  
![image](https://github.com/Nimotfash/POWERBI/assets/136696913/4f735224-4a5c-445d-8716-c7926c754a02)

#Import library 
  
library(tidyverse)

# Check data types: 
  
str(df)![image](https://github.com/Nimotfash/POWERBI/assets/136696913/c8f357ab-678e-4e1e-affe-10a4d2e865a2)


# Check for missing values: 
  
colSums(is.na(df))
  
#Drop missing values 
  
df <- na.omit(df) or df <- df %>% drop_na()

# check to make sure that the rows have been removed 

colSums(is.na(df))
![image](https://github.com/Nimotfash/POWERBI/assets/136696913/23a8598b-0118-42d5-95a8-4f583f7a7efc)

#Check for duplicates 
dim(df[duplicated(df$Film),])

Or 

df <- df[!duplicated(df$Film), ]
  
#round off values to 2 places 
  
df$Profitability <- round(df$Profitability ,digit=2)
  
df$Worldwide.Gross <- round(df$Worldwide.Gross ,digit=2)
![image](https://github.com/Nimotfash/POWERBI/assets/136696913/f5344ed0-c22a-4db8-afe4-449efac37766)


#Check for outliers using a boxplot 
  
library(ggplot2)

#Create a boxplot that highlights the outliers   
ggplot(df, aes(x=Profitability, y=Worldwide.Gross)) + geom_boxplot(outlier.colour = "red", outlier.shape = 1)+ scale_x_continuous(labels = scales::comma)+coord_cartesian(ylim = c(0, 1000))
  
 ![image](https://github.com/Nimotfash/POWERBI/assets/136696913/50f1b01b-3eb6-4d3a-9220-7754953bcff8)

#Remove outliers in 'Profitability' 
Q1 <- quantile(df$Profitability, .25)
Q3 <- quantile(df$Profitability, .75)
IQR <- IQR(df$Profitability)
  
no_outliers <- subset(df, df$Profitability> (Q1 - 1.5*IQR) & df$Profitability< (Q3 + 1.5*IQR)) 
  
dim(no_outliers)
![image](https://github.com/Nimotfash/POWERBI/assets/136696913/93fc3539-26b5-4b8a-88c8-bd87741a6c9c)


# Remove outliers in 'Worldwide.Gross' 
Q1 <- quantile(no_outliers$Worldwide.Gross, .25)
Q3 <- quantile(no_outliers$Worldwide.Gross, .75)
IQR <- IQR(no_outliers$Worldwide.Gross)
  
df1 <- subset(no_outliers, no_outliers$Worldwide.Gross> (Q1 - 1.5*IQR) & no_outliers$Worldwide.Gross< (Q3 + 1.5*IQR))
  
dim(df1)![image](https://github.com/Nimotfash/POWERBI/assets/136696913/7a5acc11-c0cb-46cc-bcd9-d3dff783dc7c)

#Summary Statistics/Univariate Analysis: 
summary(df1)
  
#bivariate analysis 
  
#scatterplot 
ggplot(df1, aes(x=Lead.Studio, y=Rotten.Tomatoes..)) + geom_point()+ scale_y_continuous(labels = scales::comma)+coord_cartesian(ylim = c(0, 110))+theme(axis.text.x = element_text(angle = 90))
  
 #bar chart 
ggplot(df1, aes(x=Year)) + geom_bar()
![image](https://github.com/Nimotfash/POWERBI/assets/136696913/615e2836-cacd-4f9f-9166-b81031d732dc)

#Export clean data 
write.csv(df1, "clean_df.csv") 
![image](https://github.com/Nimotfash/POWERBI/assets/136696913/0a725463-eb3a-418f-b993-be6b1a4405b2)

In your power BI Dashboard, the client would like to see: 

The average Rotten Tomatoes ratings of each genre
The number of movies produced per year 
The audience score for each film  
The profitability per studio 
The worldwide gross per genre 
![image](https://github.com/Nimotfash/POWERBI/assets/136696913/445aa4d2-8667-4132-82cd-539fa8a86766)

Link
![Capture power BI](https://github.com/Nimotfash/POWERBI/assets/136696913/347adb99-9495-46cd-a854-8e5ee6232f5f)









  









