#---------------------------HW.5----------------------------------
#Install and Load necessary packages
install.packages("RCurl")
install.packages("sqldf")
install.packages("jsonlite")
install.packages("RJSONIO")

library("RCurl")
library("sqldf")
library("jsonlite")
library("RJSONIO")
#-----------------------------------------------------------------
#Step 1: Load the data
webAddress <- 'https://opendata.maryland.gov/resource/pdvh-tf2u.json'
transition <- fromJSON(webAddress)

#Converting JSON data to a dataframe from a list
myDF <- data.frame(transition, stringsAsFactors = FALSE)

#-----------------------------------------------------------------
#Step 2: Cleaning data
#Remove the tailing whitespace in the day_of_week column.
myDF$day_of_week <- trimws(myDF$day_of_week)

#-----------------------------------------------------------------
#Step 3: Understand the data using SQL (via SQLDF)
#A) How many accidents happen on Sunday?
SQLSunday <- sqldf('SELECT count(day_of_week) FROM myDF WHERE day_of_week = "SUNDAY"')
SQLSunday

#B) How many accidents had injuries?
SQLInjuries <- sqldf('SELECT count(injury) FROM myDF WHERE injury = "YES"')
SQLInjuries

#C)List the Injuries by day
SQLbyDay <- sqldf('SELECT count(injury) FROM myDF GROUP BY "day_of_week"')
SQLbyDay

#-----------------------------------------------------------------
#Step 4:Understand the data using tapply
#D) How many accidents happen on Sunday?
tapply(myDF$acc_date,myDF$day_of_week == "SUNDAY", length)

#E) How many accidents had injuries?
tapply(myDF$injury, myDF$injury, length)

#F) List the injuries by day
tapply(myDF$injury, list(myDF$day_of_week), length)
