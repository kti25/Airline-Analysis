# Airline-Analysis

library(dplyr)
library(tidyverse)

#import files
getwd()
setwd("C:/Users/Desktop/R/")

q1 = read.csv("q1_16_sample.csv", sep = " ", header = TRUE)
q11<-data.frame(q1)
head(q11)
names(q11)
dim(q11)
q2 = read.csv("q2_16_sample.csv", sep = " ", header = TRUE)
q22<-data.frame(q2)
head(q22)
names(q22)
dim(q22)
q3 = read.csv("q3_16_sample.csv", sep = " ", header = TRUE)
q33<-data.frame(q3)
head(q33)
names(q33)
dim(q33)
q4 = read.csv("q4_15_sample.csv", sep = " ", header = TRUE)
q44<-data.frame(q4)
head(q44)
names(q44)
dim(q44)

#combine files
new<-rbind.data.frame(q11,q22,q33,q44)
names(new)
dim(new)
head(new)
attach(new)

#create new feature: origin and destination state and city names
state = read.csv("states.csv", header = TRUE)
city = read.csv("airportcity.csv", header = TRUE)
aero = read.csv("aerodata.csv", header = TRUE)

air1<-mutate(new, ORIGIN_STATE = aero$state[match(ORIGIN_AIRPORT_ID, aero$id)])
air2<-mutate(air1, ORIGIN_CITY = aero$city[match(ORIGIN_AIRPORT_ID, aero$id)])
air3<-mutate(air2, DEST_STATE = aero$state[match(DEST_AIRPORT_ID, aero$id)])
air4<-mutate(air3, DEST_CITY = aero$city[match(DEST_AIRPORT_ID, aero$id)])
names(air4)
head(air4)
summary(air4)
dim(air4)

#new feature: categorical (4 cat.) price features - MARKET_FARE
air4$PRICE_CAT <- cut(air4$MARKET_FARE,
                     breaks=c(-Inf, 125, 250, 375, Inf),
                     labels=c("Lowest","Low","Medium","High"))
summary(air4)

# how many fairs were one way?
x = count(air4, c('ITIN_ID'))
dim(x)
f<-filter(x, x$freq> 1)
dim(f)

#how often did ticket carrier change?
w = table(air4$OP_CARRIER_CHANGE)
w

