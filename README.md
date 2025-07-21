# esg-vs-traditional-portfolio
Google Data Analytics Capstone – ESG vs Traditional Investment Analysis
Capstone Project: ESG vs Traditional Portfolio Performance
Arizona State University | [LinkedIn](https://www.linkedin.com/in/sanjay-sankar-7728ba289/) | sasankar@asu.edu
By Sanjay Sankar | Google Data Analytics Certificate Capstone
Objective:
Does an ESG-focused investment portfolio outperform a traditional portfolio in terms of return and risk over a 10-year period (2015–2025)?

Data Set Used:company_esg_financial_dataset.csv (Kaggle)

Conclusion: Based on 10 years of data, ESG portfolios outperformed traditional portfolios both in return and risk, suggesting they are a more efficient investment choice.



#First I’m going to LOAD the dataset using R. 
install.packages("readr")
# Load the necessary libraries
library(readr)
# Read the CSV into a dataframe
esg_data <- read_csv("company_esg_financial_dataset.csv")

#Next I’m going to CLEAN my dataset using R
# Check data structure
str(esg_data)

# Check for missing values
colSums(is.na(esg_data))

# I may need to remove rows with major missing values or fill them
# Since some GrowthRate has many missing let’s get rid of those:
esg_data <- esg_data[!is.na(esg_data$GrowthRate), ]

#Now I want to make sure I compare ESG scores on the same scale as returns, you can standardize scores (z-scores):
esg_data$ESG_Overall_z <- scale(esg_data$ESG_Overall)

#Now let’s split the dataset into two different categories based off of ESG Scores
esg_data$PortfolioType <- ifelse(esg_data$ESG_Overall >= 60, "ESG", "Traditional")
#Check
table(esg_data$PortfolioType)



#Now I will test the portfolio performance metrics:
#I will create a summary table that compares the average annual return and standard deviation (as a proxy for risk) between ESG and Traditional portfolios.
#Install dplyr then run below
library(dplyr)

portfolio_summary <- esg_data %>%
  group_by(PortfolioType, Year) %>%
  summarise(
    AvgGrowth = mean(GrowthRate),
    Risk = sd(GrowthRate)
  ) %>%
  ungroup()

head(portfolio_summary)

Now let’s export the table as a CSV under the name portfolio_summary.csv

My table has been completed. I have 20 rows, 10 for ESG portfolios, 10 for Traditional portfolios. Each row corresponds to its year (from 2015-2025), and also includes Average Risk, and growth for that portfolio that year. 

#Although I’m going to mess around in Tableau for Visualizations, I’m going to Quick Plot in R first. 
#install ggplot2
 install.packages("ggplot2")

#Now run the ggplot codefor a quick visualization. I want a line graph consisting of 2 lines, one for ESG performance, and one for Traditional. X axis will be the years, Y axis can be the AVG growth rate. 

library(ggplot2)

ggplot(portfolio_summary, aes(x = Year, y = AvgGrowth, color = PortfolioType)) +
geom_line(size = 1.2) +
geom_point(size = 3) +
labs(
       title = "Average Growth Rate by Portfolio Type Over Time",
       x = "Year",
       y = "Average Growth Rate (%)"
       ) +
 theme_minimal()


#After running the code, I now have a simple visualization. 
Find my attatched picture of the visualization called:
"Average Growth Rate by Portfolio Over Time"

Next, I took my new portfolio_summary into Tablaeu.
The dashboard includes:
A dual-axis line chart comparing average growth and risk by portfolio type over time.
A side-by-side bar chart showing average performance and risk for ESG vs Traditional.
A scatterplot showing each portfolio’s tradeoff between risk and return.
Check out my Dashboard here: https://public.tableau.com/views/ESGvsTraditionalStocksPerformance/ESGvsTrad_Dashboard?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link
To restate the conclusion for this project; Based on 10 years of data, ESG portfolios outperformed traditional portfolios both in return and risk, suggesting they are a more efficient investment choice.


A scatterplot showing each portfolio’s tradeoff between risk and return.
These visualizations reinforce that ESG portfolios have historically delivered higher returns and lower risk over the last ten years. On the scatterplot, ESG portfolios tend to cluster in the top-left quadrant — the optimal zone of high return and low risk.


