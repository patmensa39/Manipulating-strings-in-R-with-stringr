# Manipulating-strings-in-R-with-stringr# Manipulating strings in R with stringr
library(tidyverse)
library(stringr)
### Using your own names for the columns 

names <- c('ID', 'DBName', 'AKName', 'License', 'FacilityType', 'Address', 
           'City', 'State', 'ZipCode', 'InspectionDate', 'InspectionType', 
           'Results', 'Violations', 'Latitute', 'Longitude', 'Location') 

### Supplying this to the new inspection data 
inspections <- read_csv('http://594442.youcanlearnit.net/inspections.csv', 
                        col_names = names)

glimpse(inspections) 
### you can see that all the variable names can be seen in the first line
### so we use the skip command to remove the first column or variable
inspections <- read_csv('http://594442.youcanlearnit.net/inspections.csv', 
                        col_names = names, skip = 1)

glimpse(inspections)

### Creating a column region the combines city and state together
Regional_inspection<- unite(inspections,Region, City, State, sep = ", ", remove = FALSE)
glimpse(Regional_inspection)

### Looking at the regions 
unique(Regional_inspection$Region)

### Changing all the cases in Region to upper case leter
Regional_inspection<- Regional_inspection %>% 
  mutate(Region = str_to_upper(Region))

### Checking to see 
unique(Regional_inspection$Region)

### there some of the CHICAGO that is spelt wrongly
wrongchicago <- c("CHICAGO, IL", "CHCICAGO, IL", "CHICAGICHICAGO, IL", "CHCHICAGO, IL",
                 "CHICAGOI, IL")

### Checking the correct spellings of CHICAGO 
Regional_inspection <- Regional_inspection %>% 
  mutate(Region = ifelse(Region %in% wrongchicago, "CHICAGO, IL", Region))

unique(Regional_inspection$Region)


### Another way 
Regional_inspection <- Regional_inspection %>% 
  mutate(Region = ifelse(Region == "CHICAGO, NA", "CHICAGO,IL", Region))

unique(Regional_inspection$Region)  

nachicago <- c("NA, IL", "NA, NA", "INACTIVE, IL")  
Regional_inspection <- Regional_inspection %>% 
  mutate(Region = ifelse(Region %in% nachicago, NA, Region))

unique(Regional_inspection$Region)  


# THE MEDICARE DATASET 
### Using your own names for the columns 
names <- c('DRG', 'ProviderID', 'Name','Address', 'City', 'State', 'ZipCode',
           'Region', 'Discharges', 'AverageCharges', 'AverageTotalPayments', 
           'AverageMedicarePayments')
types <- 'ccccccccinnn'
inpatient <- read_tsv('http://594442.youcanlearnit.net/inpatient.tsv', 
                      col_names = names, skip = 1)
inpatient_separate <- separate(inpatient, DRG, c('DRGCode', 'DRGDescription'), sep = 4)
glimpse(inpatient_separate)  

### You realize that DRG code has space behind it so we have to remove the space 
inpatient_separate <- inpatient_separate %>% 
  mutate(DRGCode = str_trim(DRGCode))

glimpse(inpatient_separate)

### You realize that DRGDescription has - and space infornt so everything starts with the 
### 3rd letter
inpatient_separate <- inpatient_separate %>% 
  mutate(DRGDescription = str_sub(DRGDescription, start = 3))
glimpse(inpatient_separate)

