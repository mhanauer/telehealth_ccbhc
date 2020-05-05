---
title: "telehealth_noms"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

Stack variables

Post stacking transformations

All other grants
0600 = Baseline
0601 = 6 month resasessment


CCBHC
For assessment see below 3-months is actually vitals and so on
3-month vitals
6 month reassessment
9-month vitals
12 month reassessment

Assessment: 
0600 = Baseline Assessment
0301 = 3 Month Reassessment (vitals)
0302 = 6 Month Reassessment
0303 = 9 Month Reassessment (vitals)
0304 = 12 Month Reassessment
0699 = clincial discharge

Assessment_new
0 = Baseline
1 = 3 month reassessment (vitals)
2 = 6 month reassessment (302 from CCBHC and 601 from all other grants)
3 = 9 month reassessment (vitals)
4 = 12 month reassessment
5 = clinical discharge

Now review the missing data
Only include Baseline and 6-month
telehealth.y means they were in telehealth at 6 months which is what we want

# Data mergeing
For CCBHC IN, IL all the same

telehealth: Telehealth = 1; Pre-telehealth = 0 telehealth defined as those with any assessment date on or after 4-2-2020

### Run this prior to any analysis to load data ####
```{r}

###
setwd("T:/CRI_Research/telehealth_evaluation/data_codebooks")
IL =  read.csv("CCBHC_IL_4_14_20.csv", header = TRUE, na.strings = c(-99, -98, -1, -2, -3, -4, -5, -6, -7, -8, -9))
IN =  read.csv("CCBHC_IN_4_15_20.csv", header = TRUE, na.strings =  c(-99, -98, -1, -2, -3, -4, -5, -6, -7, -8, -9))
FHHC = read.csv("fhhc_noms_4_22_20.csv", header= TRUE, na.strings = c(-99, -98, -1, -2, -3, -4, -5, -6, -7, -8, -9))
ICP = read.csv("ICP_NOMS_Data Download 4.24.2020.csv", header = TRUE, na.strings = c(-99, -98, -1, -2, -3, -4, -5, -6, -7, -8, -9))
SOCAT = read.csv("SOCAT NOMs download 4.27.20.csv", header = TRUE, na.strings = c(-99, -98, -1, -2, -3, -4, -5, -6, -7, -8, -9))
KY_adult = read.csv("4_27_20 Adult_CCBHC_KY.csv", header = TRUE, na.strings = c(-99, -98, -1, -2, -3, -4, -5, -6, -7, -8, -9))
KY_youth = read.csv("4_27_20 Child_CCBHC_KY.csv", header = TRUE, na.strings = c(-99, -98, -1, -2, -3, -4, -5, -6, -7, -8, -9))
FL_ACT = read.csv("FL_ACT_4_30_20.csv", header = TRUE, na.strings = c(-99, -98, -1, -2, -3, -4, -5, -6, -7, -8, -9))

## Now stack them
### Create an empty data and then fill it with NAs.  Keep the first 44 those are correct and match
SOCAT$RespondentType = NULL
SOCAT_matrix = matrix(NA, ncol = 185-43, nrow = dim(SOCAT)[1])
SOCAT_matrix = data.frame(SOCAT_matrix)
colnames(SOCAT_matrix) = colnames(ICP[,44:185])
SOCAT_full = data.frame(SOCAT[,1:43], SOCAT_matrix)
dim(SOCAT_full)
### Change variables that match
SOCAT_full$Nervous = SOCAT$Nervous
SOCAT_full$Hopeless = SOCAT$Hopeless
SOCAT_full$Restless = SOCAT$Restless
SOCAT_full$Depressed = SOCAT$Depressed
SOCAT_full$EverythingEffort = SOCAT$EverythingEffort
SOCAT_full$Worthless = SOCAT$Worthless
SOCAT_full$Tobacco_Use = SOCAT$Tobacco_Use
SOCAT_full$Alcohol_Use = SOCAT$Alcohol_Use
SOCAT_full$StreetOpioids_Use = SOCAT$StreetOpioids_Use
SOCAT_full$RxOpioids_Use = SOCAT$RxOpioids_Use
SOCAT_full$NightsHomeless = SOCAT$NightsHomeless
SOCAT_full$NightsHospitalMHC = SOCAT$NightsHospitalMHC
SOCAT_full$NightsDetox = SOCAT$NightsDetox
SOCAT_full$NightsJail = SOCAT$NightsJail
SOCAT_full$TimesER = SOCAT$TimesER
SOCAT_full$Housing = SOCAT$Housing
SOCAT = SOCAT_full
describe.factor(FHHC$Assessment)

KY_youth$RespondentType = NULL
KY_youth_matrix = matrix(NA, ncol = 185-43, nrow = dim(KY_youth)[1])
KY_youth_matrix = data.frame(KY_youth_matrix)
colnames(KY_youth_matrix) = colnames(ICP[,44:185])
KY_youth_full = data.frame(KY_youth[,1:43], KY_youth_matrix)
dim(KY_youth_full)
### Change variables that match
KY_youth_full$Nervous = KY_youth$Nervous
KY_youth_full$Hopeless = KY_youth$Hopeless
KY_youth_full$Restless = KY_youth$Restless
KY_youth_full$Depressed = KY_youth$Depressed
KY_youth_full$EverythingEffort = KY_youth$EverythingEffort
KY_youth_full$Worthless = KY_youth$Worthless
KY_youth_full$Tobacco_Use = KY_youth$Tobacco_Use
KY_youth_full$Alcohol_Use = KY_youth$Alcohol_Use
KY_youth_full$StreetOpioids_Use = KY_youth$StreetOpioids_Use
KY_youth_full$RxOpioids_Use = KY_youth$RxOpioids_Use
KY_youth_full$NightsHomeless = KY_youth$NightsHomeless
KY_youth_full$NightsHospitalMHC = KY_youth$NightsHospitalMHC
KY_youth_full$NightsDetox = KY_youth$NightsDetox
KY_youth_full$NightsJail = KY_youth$NightsJail
KY_youth_full$TimesER = KY_youth$TimesER
KY_youth_full$Housing = KY_youth$Housing
KY_youth = KY_youth_full


IN_IL_KY_CCBHC = rbind(IN[,1:185], IL[,1:185], KY_youth[,1:185], KY_adult[,1:185])
dim(IN_IL_KY_CCBHC)
FHHC = FHHC[,1:185]
ICP = ICP[,1:185]
FL_ACT = FL_ACT[,1:185]
dim(ICP)
dim(SOCAT)
### Add grant ID
IN_IL_KY_CCBHC$grant = rep("IN_IL_KY_CCBHC", dim(IN_IL_KY_CCBHC)[1])
FHHC$grant = rep("FHHC", dim(FHHC)[1])
ICP$grant = rep("ICP", dim(ICP)[1])
SOCAT$grant = rep("SOCAT", dim(SOCAT)[1])
FL_ACT$grant = rep("FL_ACT", dim(FL_ACT)[1])
dim(SOCAT)
dim(IN_IL_CCBHC)
telehealth_noms = rbind(IN_IL_KY_CCBHC, FHHC, ICP, SOCAT, FL_ACT)
dim(telehealth_noms)
### Create a new ConsumerID that is a mix of grant and ConsumerID
telehealth_noms$ConsumerID_grant = paste0(telehealth_noms$ConsumerID, telehealth_noms$GrantID)

### Figure out how you can stack FHHC data
dim(telehealth_noms)
describe.factor(telehealth_noms$grant) 

describe.factor(telehealth_noms$Assessment)
## Rename to the above

## No one has multiple reassessments
describe.factor(telehealth_noms$ReassessmentNumber_07)

## Create recoded assessment variable
telehealth_noms$Assessment_new = ifelse(telehealth_noms$Assessment == 600, 0, ifelse(telehealth_noms$Assessment == 301, 1, ifelse(telehealth_noms$Assessment == 302, 2, ifelse(telehealth_noms$Assessment == 303, 3, ifelse(telehealth_noms$Assessment == 601,2, NA)))))
telehealth_noms$Assessment_new = as.numeric(telehealth_noms$Assessment_new)
describe.factor(telehealth_noms$Assessment_new, decr.order= FALSE)
### Create full date variable
telehealth_noms$date = paste0(telehealth_noms$FFY, "-", telehealth_noms$Month, "-", "01")
telehealth_noms$date = ymd(telehealth_noms$date)
head(telehealth_noms$date)

telehealth_noms$telehealth = ifelse(telehealth_noms$date >= "2020-04-01", 1, 0)
telehealth_noms[c("date","telehealth")]
### Cannot be greater than 2020-09-30 last day of grant
telehealth_noms = subset(telehealth_noms, date < "2020-09-30")
## Check that all dates post 2014 most grants are for at most five years
telehealth_noms = subset(telehealth_noms, date > "2014-01-01")
telehealth_noms[c("date","telehealth")]
dim(telehealth_noms)
describe.factor(telehealth_noms$grant)
range(telehealth_noms$date)
### Create a NOMS data set  
telehealth_noms_wide = subset(telehealth_noms, Assessment_new == 0 | Assessment_new == 2)
dim(telehealth_noms)[1]
describe.factor(telehealth_noms$Assessment_new)


####################
library(naniar)
miss_var_summary(telehealth_noms)

head(telehealth_noms)
miss_var_summary(subset(telehealth_noms, Assessment_new == 2))
miss_var_summary(subset(telehealth_noms, Assessment_new == 0))

### These people have two baselines delete them 'A00276''A00295''A00298'
telehealth_noms_wide_test = subset(telehealth_noms_wide, ConsumerID == "'A00276'" | ConsumerID == "'A00295'" | ConsumerID == "'A00298'")
telehealth_noms_wide = telehealth_noms_wide[order(telehealth_noms_wide$ConsumerID),]
## If there is no interview then delete the second, if there is only one interview delete the none interview, if there are two interviews for baseline delete the second see conductedinterview variable
telehealth_noms_wide[c(276, 291, 298),]
telehealth_noms_wide = telehealth_noms_wide[-c(276, 291, 298),] 

### These repeating
#Consumergrant ID '169658'SM81849 Consumer ID '169658'  
# Consumergrant ID'180815'SM81849 Consuerm ID '180815'
telehealth_noms_wide_test = subset(telehealth_noms_wide, ConsumerID_grant == "'169658'SM81849" | ConsumerID_grant == "'180815'SM81849")
telehealth_noms_wide_test
telehealth_noms_wide[c(3779:3900),]
#
telehealth_noms_wide = telehealth_noms_wide[-c(3754, 3779),] 
telehealth_noms_base_noms = subset(telehealth_noms_wide,Assessment_new == 0)
telehealth_noms_month6_noms = subset(telehealth_noms_wide,Assessment_new == 2)
describe.factor(telehealth_noms_base_noms$grant)
describe.factor(telehealth_noms_month6_noms$grant)

head(telehealth_noms_base_noms)
dim(telehealth_noms_month6_noms)
telehealth_noms_wide_noms = merge(telehealth_noms_base_noms, telehealth_noms_month6_noms, by = "ConsumerID_grant", all.y = TRUE)
dim(telehealth_noms_wide_noms)
telehealth_noms_wide_noms = telehealth_noms_wide_noms[order(telehealth_noms_wide_noms$ConsumerID_grant),]
telehealth_noms_month6_noms = telehealth_noms_month6_noms[order(telehealth_noms_month6_noms$ConsumerID_grant),]
telehealth_noms_month6_noms$ConsumerID_grant == telehealth_noms_wide_noms$ConsumerID_grant

head(telehealth_noms_month6_noms)
describe.factor(telehealth_noms_month6_noms$telehealth)
describe.factor(telehealth_noms_month6_noms$grant)
describe.factor(telehealth_noms_wide_noms$telehealth.y)
```
Data set descriptives
telehealth_noms_wide_noms = CCBHC IN and IL both adults and youth, FFHC NOMS data for baseline and 6-month matched pairs 
### Finish this later

################################################
Full data set created can start data analysis
################################################




Let's try satisfaction 
c.	how satisfied are you with your ability to perform your daily living activities? PerformDailyActivitiesSatisfaction
d.	how satisfied are you with your health? HealthSatisfaction
how satisfied are you with yourself? SelfSatisfaction
how satisfied are you with your personal relationships? RelationshipSatisfaction

### General instructions ####
1. Grab the variable you want and put into data frame
2. Check the descriptives to make sure everything is in range
3. Conduct psychometrics 



```{r}
### All items should be 1 to 5
telehealth_noms_wide_noms_sat = telehealth_noms_wide_noms[c("telehealth.y", "PerformDailyActivitiesSatisfaction.x", "PerformDailyActivitiesSatisfaction.y", "HealthSatisfaction.x", "HealthSatisfaction.y", "SelfSatisfaction.x", "SelfSatisfaction.y", "RelationshipSatisfaction.x","RelationshipSatisfaction.y")]
apply(telehealth_noms_wide_noms_sat,2, function(x){describe.factor(x)})
library(psych)

### Plug in all the .x variables 
omega_sat_base =  omega(telehealth_noms_wide_noms_sat[c(2,4,6,8)], poly = TRUE)
### Plug in all the .y variables except telehealth.y 
omega_sat_6month =  omega(telehealth_noms_wide_noms_sat[c(3,5,7,9)], poly = TRUE)
omega_sat_6month

### Plug in all the .x variables 
vss(telehealth_noms_wide_noms_sat[c(2,4,6,8)], cor = "poly")
fa(telehealth_noms_wide_noms_sat[c(2,4,6,8)], cor = "poly", correct = 0)

### Plug in all the .y variables except telehealth.y 
vss(telehealth_noms_wide_noms_sat[c(3,5,7,9)], cor = "poly")
fa(telehealth_noms_wide_noms_sat[c(3,5,7,9)], cor = "poly", correct = 0)
```
Create a total score for each 

IN_IL_dat_wide_noms_sat_month6_complete  = satisfaction total scores only with complete data for 6-month
```{r}
### Plug in all .x variables
telehealth_noms_wide_noms_sat$total_base = apply(telehealth_noms_wide_noms_sat[c(2,4,6,8)], 1, mean, na.rm = TRUE)
### Plug in all .y expect for telehealth.y
telehealth_noms_wide_noms_sat$total_month6 = apply(telehealth_noms_wide_noms_sat[c(3,5,7,9)], 1, mean, na.rm = TRUE)
hist(telehealth_noms_wide_noms_sat$total_base)
hist(telehealth_noms_wide_noms_sat$total_month6)

## No data for difference scores so try just 6months
range(telehealth_noms_wide_noms_sat$total_month6, na.rm = TRUE)
dim(telehealth_noms_wide_noms_sat)
head(telehealth_noms_wide_noms_sat)
telehealth_noms_wide_noms_sat
telehealth_noms_wide_noms_sat_month6_complete = na.omit(telehealth_noms_wide_noms_sat[c("total_month6", "telehealth.y")])
dim(telehealth_noms_wide_noms_sat_month6_complete)
telehealth_noms_wide_noms_sat_month6_complete
telehealth_noms_wide_noms_sat_month6_complete$face_to_face = ifelse(telehealth_noms_wide_noms_sat_month6_complete$telehealth.y == 1,0,1)

```
Analysis sat
Percentage change: https://stats.idre.ucla.edu/sas/faq/how-can-i-interpret-log-transformed-variables-in-terms-of-percent-change-in-linear-regression/
  
You need to expondiate the parameter estimate
Doesn't work to well when you get above 20% differences: https://people.duke.edu/~rnau/411log.htm

P-change is bad for regression because of: 
(3-2)/2 
(2-3)/3

Change the telehealth.y to be face to face 1 and telehealth.y  = 0

```{r}
library(rstanarm)
library(descr)
### Scale is .2 which means 20% difference in each direction
my_prior = normal(location = 0, scale = .2, autoscale = FALSE)
describe.factor(telehealth_noms_wide_noms_sat_month6_complete$telehealth.y)
n_total = dim(telehealth_noms_wide_noms_sat_month6_complete)[1]

bayes_p_change_sat = stan_glm(log(total_month6)~ face_to_face, prior = my_prior, data = telehealth_noms_wide_noms_sat_month6_complete, seed = 123)
### You should not need to change this.  We want the mean, sd, 2.5, and 97.5
## check bayes_p_change_sat$stan_summary if you are unsure
bayes_p_change_sat_sum = round(bayes_p_change_sat$stan_summary[,c(1,3,4,10)],4)
## To get percentage change interpretation need to exp the parameter estimates
bayes_p_change_sat_sum = round(exp(bayes_p_change_sat_sum),3)
### Creates a percentage instead 1 + % 
bayes_p_change_sat_sum= bayes_p_change_sat_sum - 1
bayes_p_change_sat_sum

### Grabing the means, sds, and n's for each group
mean_sd_sat= round(compmeans(telehealth_noms_wide_noms_sat_month6_complete$total_month6, telehealth_noms_wide_noms_sat_month6_complete$telehealth.y),2)
mean_sd_sat
### Get freq cohen's D, because I don't know how to get bayes cohen's D
month_6_sat_d =  cohen.d(telehealth_noms_wide_noms_sat_month6_complete$total_month6, group = telehealth_noms_wide_noms_sat_month6_complete$face_to_face)
### Put together the results.  Should not need to change this.  See example telehealth_noms_sat_results in TDrive CRI_Research/telehealth_evaluation/data_codebooks/results
## Change results from results_sat to whatever you are measuring results_(fill in name)
results_sat = data.frame(par_estimate = bayes_p_change_sat_sum[2,1], sd_p_change =  bayes_p_change_sat_sum[2,2], ci_95 = paste0(bayes_p_change_sat_sum[2,3], ",", bayes_p_change_sat_sum[2,4]), n_total = n_total, n_pre_telehealth = mean_sd_sat[1,2], n_post_telehealth = mean_sd_sat[2,2], raw_p_change = round((mean_sd_sat[2,1] -  mean_sd_sat[1,1]) /  mean_sd_sat[1,1],3), telehealth_mean =  mean_sd_sat[2,1], face_to_face_mean =  mean_sd_sat[1,1], telehealth_sd= mean_sd_sat[2,3], face_to_face_sd= mean_sd_sat[1,3],freq_cohen_d = round(month_6_sat_d$cohen.d[2],3))

write.csv(results_sat, "results.csv", row.names = FALSE)
results_sat

```
#################
Code from Github
1. Grab code from master branch


##################### 
Follow the code above and plug in the variables below instead of the variable above
Post to github as your name patch-1,2,3...n (however, many patchs you have).  Let Matt know when you are finished.
Or just save code on telehealth folder on T drive

######################################

######Cat

Next section
Dealing with everyday life with mental health illness
Sat
a.	I deal effectively with daily problems.
b.	I am able to control my life. T\CRI_Research\telehealth_evaluation
c.	I am able to deal with crisis.
d.	I am getting along with my family.
e.	I do well in social situations.
f.	I do well in school and/or work.
g.	My housing situation is satisfactory.
h.	My symptoms are not bothering me.

```{r}
telehealth_noms_wide_noms_deal = telehealth_noms_wide_noms[c("telehealth.y", "HandlingDailyLife.x", "HandlingDailyLife.y", "ControlLife.x", "ControlLife.y", "DealWithCrisis.x", "DealWithCrisis.y", "GetsAlongWithFamily.x","GetsAlongWithFamily.y", "SocialSituations.x", "SocialSituations.y", "SchoolOrWork.x", "SchoolOrWork.y", "FunctioningHousing.x", "FunctioningHousing.y", "Symptoms.x","Symptoms.y")]
apply(telehealth_noms_wide_noms_deal,2, function(x){describe.factor(x)})
library(psych)

### Plug in all the .x variables 
omega_deal_base =  omega(telehealth_noms_wide_noms_deal[c(2,4,6,8,10,12,14,16)], poly = TRUE)
### Plug in all the .y variables except telehealth.y 
omega_deal_6month =  omega(telehealth_noms_wide_noms_deal[c(3,5,7,9,11,13,15,17)], poly = TRUE)
omega_deal_6month

### Plug in all the .x variables 
vss(telehealth_noms_wide_noms_deal[c(2,4,6,8,10,12,14,16)], cor = "poly")
fa(telehealth_noms_wide_noms_deal[c(2,4,6,8,10,12,14,16)], cor = "poly", correct = 0)

### Plug in all the .y variables except telehealth.y 
vss(telehealth_noms_wide_noms_deal[c(3,5,7,9,11,13,15,17)], cor = "poly")
fa(telehealth_noms_wide_noms_deal[c(3,5,7,9,11,13,15,17)], cor = "poly", correct = 0)



### Plug in all .x variables
telehealth_noms_wide_noms_deal$total_base = apply(telehealth_noms_wide_noms_deal[c(2,4,6,8,10,12,14,16)], 1, mean, na.rm = TRUE)
### Plug in all .y expect for telehealth.y
telehealth_noms_wide_noms_deal$total_month6 = apply(telehealth_noms_wide_noms_deal[c(3,5,7,9,11,13,15,17)], 1, mean, na.rm = TRUE)
hist(telehealth_noms_wide_noms_deal$total_base)
hist(telehealth_noms_wide_noms_deal$total_month6)

## No data for difference scores so try just 6months
range(telehealth_noms_wide_noms_deal$total_month6, na.rm = TRUE)
dim(telehealth_noms_wide_noms_deal)
head(telehealth_noms_wide_noms_deal)
telehealth_noms_wide_noms_deal
telehealth_noms_wide_noms_deal_month6_complete = na.omit(telehealth_noms_wide_noms_deal[c("total_month6", "telehealth.y")])
dim(telehealth_noms_wide_noms_deal_month6_complete)
telehealth_noms_wide_noms_deal_month6_complete
telehealth_noms_wide_noms_deal_month6_complete$face_to_face = ifelse(telehealth_noms_wide_noms_deal_month6_complete$telehealth.y == 1,0,1)




library(rstanarm)
library(descr)
### Scale is .2 which means 20% difference in each direction
my_prior = normal(location = 0, scale = .2, autoscale = FALSE)
describe.factor(telehealth_noms_wide_noms_deal_month6_complete$telehealth.y)
n_total = dim(telehealth_noms_wide_noms_deal_month6_complete)[1]

bayes_p_change_deal = stan_glm(log(total_month6)~ face_to_face, prior = my_prior, data = telehealth_noms_wide_noms_deal_month6_complete, seed = 123)
#launch_shinystan(bayes_p_change_deal)
### You should not need to change this.  We want the mean, sd, 2.5, and 97.5
## check bayes_p_change_sat$stan_summary if you are unsure
bayes_p_change_deal_sum = round(bayes_p_change_deal$stan_summary[,c(1,3,4,10)],4)
## To get percentage change interpretation need to exp the parameter estimates
bayes_p_change_deal_sum = round(exp(bayes_p_change_deal_sum[1:4,]),3)
### Creates a percentage instead 1 + % 
bayes_p_change_deal_sum= bayes_p_change_deal_sum - 1
bayes_p_change_deal_sum
### Grabing the means, sds, and n's for each group
mean_sd_deal= round(compmeans(telehealth_noms_wide_noms_deal_month6_complete$total_month6, telehealth_noms_wide_noms_deal_month6_complete$telehealth.y),2)
mean_sd_deal
### Get freq cohen's D, because I don't know how to get bayes cohen's D
month_6_deal_d =  cohen.d(telehealth_noms_wide_noms_deal_month6_complete$total_month6, group = telehealth_noms_wide_noms_deal_month6_complete$face_to_face)
### Put together the results.  Should not need to change this.  See example telehealth_noms_sat_results in TDrive CRI_Research/telehealth_evaluation/data_codebooks/results
## Change results from results_sat to whatever you are measuring results_(fill in name)
results_deal = data.frame(par_estimate = bayes_p_change_deal_sum[2,1], sd_p_change =  bayes_p_change_deal_sum[2,2], ci_95 = paste0(bayes_p_change_deal_sum[2,3], ",", bayes_p_change_deal_sum[2,4]), n_total = n_total, n_pre_telehealth = mean_sd_deal[1,2], n_post_telehealth = mean_sd_deal[2,2], raw_p_change = round((mean_sd_deal[2,1] -  mean_sd_deal[1,1]) /  mean_sd_deal[1,1],3), telehealth_mean =  mean_sd_deal[2,1], face_to_face_mean =  mean_sd_deal[1,1], telehealth_sd= mean_sd_deal[2,3], face_to_face_sd= mean_sd_deal[1,3],freq_cohen_d = round(month_6_deal_d$cohen.d[2],3))

write.csv(results_deal, "results.csv", row.names = FALSE)
results_deal

```

####Jess
####################################################
Feeling in last 30 days
During the past 30 days, about how often did you feel …
a.	nervous?
b.	hopeless?
c.	restless or fidgety?
d.	so depressed that nothing could cheer you up?
e.	that everything was an effort?
f.	worthless?
```{r}

```


#######################

6.	The following questions relate to your experience with alcohol, cigarettes, and other drugs. Some of the substances we’ll talk about are prescribed by a doctor (like pain medications). But I will only record those if you have taken them for reasons or in doses other than prescribed.

In the past 30 days, how often have you used …

a.	tobacco products (cigarettes, chewing tobacco, cigars, etc.)?
Tobacco_Use

b.	alcoholic beverages (beer, wine, liquor, etc.)?
Alcohol_Use

k.	prescription opioids (fentanyl, oxycodone [OxyContin, Percocet], hydrocodone [Vicodin], methadone, buprenorphine, etc.)?
RxOpioids_Use

j.	street opioids (heroin, opium, etc.)?
StreetOpioids_Use

```{r}

### All items should be 1 to 5
### Not enough for drugs
telehealth_alc_tob = telehealth_noms_wide_noms[c("telehealth.y", "Tobacco_Use.x", "Tobacco_Use.y", "Alcohol_Use.x", "Alcohol_Use.y")]
apply(telehealth_alc_tob,2, function(x){describe.factor(x)})
library(psych)
### Only two items not needed.
### Plug in all the .x variables 
#omega_sat_base =  omega(telehealth_noms_wide_noms_sat[c(2,4,6,8)], poly = TRUE)
### Plug in all the .y variables except telehealth.y 
#omega_sat_6month =  omega(telehealth_noms_wide_noms_sat[c(3,5,7,9)], poly = TRUE)
#omega_sat_6month

### Plug in all the .x variables 
#vss(telehealth_noms_wide_noms_sat[c(2,4,6,8)], cor = "poly")
#fa(telehealth_noms_wide_noms_sat[c(2,4,6,8)], cor = "poly", correct = 0)

### Plug in all the .y variables except telehealth.y 
#vss(telehealth_noms_wide_noms_sat[c(3,5,7,9)], cor = "poly")
#fa(telehealth_noms_wide_noms_sat[c(3,5,7,9)], cor = "poly", correct = 0)

########### 
# Create total scores and use sum so you can get counts 
### Plug in all .x variables
telehealth_alc_tob$total_base = apply(telehealth_alc_tob[c(2,4)], 1, mean, na.rm = TRUE)
### Plug in all .y expect for telehealth.y
telehealth_alc_tob$total_month6 = apply(telehealth_alc_tob[c(3,5)], 1, mean, na.rm = TRUE)
hist(log(telehealth_alc_tob$total_month6))

### Make binary anything greater than 1 means you used alcohol or tobacco 
describe.factor(telehealth_alc_tob$total_month6)
hist(telehealth_alc_tob$total_month6)
compmeans(telehealth_alc_tob$total_month6, telehealth_alc_tob$telehealth.y)
describeBy (telehealth_alc_tob$total_month6, telehealth_alc_tob$telehealth.y)


telehealth_alc_tob$total_month6 = ifelse(telehealth_alc_tob$total_month6 == 1, 1,0)

## No data for difference scores so try just 6months
range(telehealth_alc_tob$total_month6, na.rm = TRUE)
dim(telehealth_alc_tob)
head(telehealth_alc_tob)
telehealth_alc_tob
telehealth_alc_tob_complete = na.omit(telehealth_alc_tob[c("total_month6", "telehealth.y")])
dim(telehealth_alc_tob_complete)
telehealth_alc_tob_complete
describe.factor(telehealth_alc_tob_complete$telehealth.y)
############ Run posisson regression
library(rstanarm)
library(descr)
### Scale is exp(.2) which means 20% difference in each direction


my_prior = normal(location = 0, scale = exp(.2), autoscale = FALSE)
my_prior = student_t(4,0,.2)
describe.factor(telehealth_alc_tob_complete$telehealth.y)
n_total = dim(telehealth_alc_tob_complete)[1]
## Take log of outcome to get percentage change interpretation

telehealth_alc_tob_complete$face_to_face = ifelse(telehealth_alc_tob_complete$telehealth.y == 1, 0,1)
bayes_p_change_al_tob = stan_glm(total_month6~ face_to_face, prior = my_prior, family = binomial(link = "logit"), data = telehealth_alc_tob_complete, seed = 123)
bayes_p_change_al_tob
### You should not need to change this.  We want the mean, sd, 2.5, and 97.5
## check bayes_p_change_sat$stan_summary if you are unsure
bayes_p_change_al_tob_sum = round(bayes_p_change_al_tob$stan_summary[,c(1,3,4,10)],4)
## To get percentage change interpretation need to exp the parameter estimates
bayes_p_change_al_tob_sum = round(exp(bayes_p_change_al_tob_sum),3)
### Creates a percentage instead 1 + % 
bayes_p_change_al_tob_sum= bayes_p_change_al_tob_sum - 1
bayes_p_change_al_tob_sum

### Need mean sd change here
telehealth_alc_tob_complete

mean_sd_alc_tob= round(compmeans(telehealth_alc_tob_complete$total_month6, telehealth_alc_tob_complete$telehealth.y),2)
### Put together the results.  Should not need to change this.  See example telehealth_noms_sat_results in TDrive CRI_Research/telehealth_evaluation/data_codebooks/results
## Change results from results_sat to whatever you are measuring results_(fill in name)

results_alc_tob = data.frame(odds_change_alc_tob = bayes_p_change_al_tob_sum[2,1], sd_odds_change =  bayes_p_change_al_tob_sum[2,2], ci_95 = paste0(bayes_p_change_al_tob_sum[2,3], ",", bayes_p_change_al_tob_sum[2,4]), n_total = n_total, n_pre_telehealth = mean_sd_sat[1,2], n_post_telehealth = mean_sd_sat[2,2], p_yes = mean_sd_alc_tob[2,1], p_no = mean_sd_alc_tob[1,1])

write.csv(results_alc_tob, "results_alc_tob.csv", row.names = FALSE)
results_alc_tob
prior_summary(bayes_p_change_al_tob)
exp(.2)


```

Not doing this no 6-month data

##################################
VIOLENCE AND TRAUMA 
10.	Did any of these experiences feel so frightening, horrible, or upsetting that in the past and/or the present you:
a.	Have had nightmares about it or thought about it when you did not want to?
b.	Tried hard not to think about it or went out of your way to avoid situations that remind you of it?
c.	Were constantly on guard, watchful, or easily startled?
d.	Felt numb and detached from others, activities, or your surroundings?
VT_OnGuard

```{r}

telehealth_noms_wide_vt = telehealth_noms_wide_noms[c("telehealth.y","VT_NightmaresThoughts.x", "VT_NightmaresThoughts.y", "VT_NotThinkAboutIt.x", "VT_NotThinkAboutIt.y", "VT_OnGuard.x", "VT_OnGuard.y", "VT_NumbDetached.x", "VT_NumbDetached.y")]

apply(telehealth_noms_wide_vt, 2, function(x){describe.factor(x)})

```


#################
PERCEPTION OF CARE
1.	In order to provide the best possible mental health and related services, we need to know what you think about the services you received during the past 30 days, the people who provided it, and the results. Please indicate your disagreement/agreement with each of the following statements.
a.	Staff here believe that I can grow, change, and recover.
Recover

b.	I felt free to complain.
Complain

c.	I was given information about my rights.
Rights

d.	Staff encouraged me to take responsibility for how I live my life.
Responsibility

e.	Staff told me what side effects to watch out for.
SideEffects

f.	Staff respected my wishes about who is and who is not to be given information about my treatment.
SharingTreatmentInformation

g.	Staff were sensitive to my cultural background (race, religion, language, etc.).
SensitiveToCulture

h.	Staff helped me obtain the information I needed so that I could take charge of managing my illness.
InformationNeeded

i.	I was encouraged to use consumer-run programs (support groups, drop-in centers, crisis phone line, etc.).
ConsumerRunPrograms

j.	I felt comfortable asking questions about my treatment and medication.
ComfortableAskingQuestions

a.	I, not staff, decided my treatment goals.
TreatmentGoals

b.	I like the services I received here.
LikeServices

c.	If I had other choices, I would still get services from this agency.
Choices

d.	I would recommend this agency to a friend or family member.
RecommendAgency

```{r}
telehealth_noms_wide_pc = telehealth_noms_wide_noms[c("telehealth.y", "Recover.x", "Recover.y", "Rights.x", "Rights.y", "Responsibility.x", "Responsibility.y", "SideEffects.x", "SideEffects.y", "SharingTreatmentInformation.x", "SharingTreatmentInformation.y", "SensitiveToCulture.x", "SensitiveToCulture.y", "InformationNeeded.x", "InformationNeeded.y", "ConsumerRunPrograms.x", "ConsumerRunPrograms.y", "ComfortableAskingQuestions.x", "ComfortableAskingQuestions.y", "TreatmentGoals.x", "TreatmentGoals.y", "LikeServices.x", "LikeServices.y", "Choices.x", "Choices.y", "RecommendAgency.x", "RecommendAgency.y")]

apply(telehealth_noms_wide_pc, 2, function(x){describe.factor(x)})

telehealth_noms_wide_pc$total_month6 = apply(telehealth_noms_wide_pc[,c(3,5,7,9,11,13,15,17,19,21,23,25,27)], 1, mean, na.rm = TRUE)


### Psychometrics
### Plug in all the .x variables 
omega_pc_6month =  omega(telehealth_noms_wide_pc[,c(3,5,7,9,11,13,15,17,19,21,23,25,27)], poly = TRUE)
omega_pc_6month

### Plug in all the .y variables except telehealth.y 
vss(telehealth_noms_wide_pc[,c(3,5,7,9,11,13,15,17,19,21,23,25,27)], cor = "poly")
fa(telehealth_noms_wide_pc[,c(3,5,7,9,11,13,15,17,19,21,23,25,27)], cor = "poly", correct = 0)

telehealth_noms_wide_pc_complete = na.omit(telehealth_noms_wide_pc[c("telehealth.y", "total_month6")])
n_total =  dim(telehealth_noms_wide_pc_complete)[1]
### Just do a wilcox test for now.
mean_sd_pc = compmeans(telehealth_noms_wide_pc_complete$total_month6, telehealth_noms_wide_pc_complete$telehealth.y)
mean_sd_pc

### This is comparing face to face with telehealth
results_pc = wilcox.test(telehealth_noms_wide_pc_complete$total_month6 ~telehealth_noms_wide_pc_complete$telehealth.y, conf.int = TRUE)
round(results_pc$estimate,3)


results_pc = data.frame(diff_location = round(results_pc$estimate,3), ci_95 = paste0(round(results_pc$conf.int[1],3), ",", round(results_pc$conf.int[2],3)), n_total = n_total, n_pre_telehealth = mean_sd_sat[1,2], n_post_telehealth = mean_sd_sat[2,2])

results_pc
write.csv(results_pc, "results_pc.csv", row.names = FALSE)

#### What percent of staff have a positive perception of care with centertone 
telehealth_only_positive = subset(telehealth_noms_wide_pc_complete, telehealth.y == 1)
telehealth_only_positive$positive = ifelse(telehealth_only_positive$total_month6 > 3, 1, 0)

agree_p_n =  describe.factor(telehealth_only_positive$positive)
agree_p_n = data.frame(agree_p_n)
agree_p_n$total = apply(agree_p_n, 1, sum)
colnames(agree_p_n) = c("agree_or_greater", "less_than_agree", "total")
agree_p_n = round(agree_p_n,2)
write.csv(agree_p_n, "agree_p_n.csv")

#### Recomend
recommend_agree = subset(telehealth_noms_wide_pc, telehealth.y == 1)
recommend_agree = recommend_agree[c("telehealth.y", "RecommendAgency.y")]
recommend_agree_complete = na.omit(recommend_agree)
recommend_agree_complete$recommend_agree = ifelse(recommend_agree_complete$RecommendAgency.y > 3, 1, 0)
recommend_agree_results =  data.frame(describe.factor(recommend_agree_complete$recommend_agree))
recommend_agree_results$total = apply(recommend_agree_results, 1, sum)
colnames(recommend_agree_results) = c("agree_or_greater", "less_than_agree", "total")
recommend_agree_results = round(recommend_agree_results,2)
recommend_agree_results
write.csv(recommend_agree_results, "recommend_agree_results.csv")
```




