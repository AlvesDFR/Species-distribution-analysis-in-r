# First steps

Data manipulation skills are indeed crucial across various fields of research, including macroecology. With the increasing availability of large datasets in many scientific disciplines, the ability to efficiently manage, analyze, and interpret data has become indispensable for researchers. Whether it's cleaning messy datasets, performing complex analyses, or visualizing results, proficiency in data manipulation can significantly enhance the quality and efficiency of research outcomes. Moreover, it enables researchers to uncover patterns, relationships, and insights that may not be apparent at first glance, ultimately advancing our understanding of the natural world and informing evidence-based decision-making.


## Contents
- [Instaling R](#instaling-R)
- [Working with .csv files](#working-with-csv)
    - .csv in Excel:
      - How many species?
      - Where do species occur?
    - .csv in R
- [Contact](#contact)

## Instaling R
R download:
https://cran.r-project.org/bin/windows/base/

R Studio download:
https://posit.co/download/rstudio-desktop/

## Comma-separated values .csv
Occurences points of freshwater crabs downloaded from https://www.iucnredlist.org/resources/spatial-data-download  
FW_CRABS_points.csv

### .csv in Excel
1) Open the FW_CRABS_points.csv file in Excel and view the data structure;
2) How many species are there in this database?
3) What is long-lat range of each species?

### .csv in R
Some of the main commands for working with dataframes in R

```
# Opening a .csv file in r
## Change to working directory
setwd("C:/PATH")

## Open and view the table (dataframe) structure
df<-read.csv("FW_CRABS_points.csv", sep=",", header=T)
head(df)
names(df)
str(df)
dim(df)
class(df)

## Checking specific rows and columns
## First row and all columns
df[1,]

## First column and all rows
df[, 1]

## First two rows and all columns
df[1:2,]

## First and third row and all columns
df[ c(1,3), ]

## First row and 2nd and third column
df[1, 2:3]

## First and second row of the second and third column
df[1:2, 2:3]

## Some basic data manipulation functions ##
## unique
unique(df$sci_name)

## Filtering specific data with "subset"
sp<-subset(df, df$sci_name == 'Potamonautes obesus')
dim(sp)
head(sp)
class(sp)

## Merging column and row data with "cbind" and "rbind"
sp<-as.data.frame(cbind(sp$sci_name, sp$longitude , sp$latitude))
dim(sp)
head(sp)
str(sp)

## Editing column names with "colnames"
colnames(sp)<-c('sp','long','lat')
class(sp$long)

## Rounding numbers to specific number of decimal places with "round"
sp$long <- round(as.numeric(sp$long), digits = 2)
class(sp$long)

sp$lat <- round(as.numeric(sp$lat), digits = 2)
head(sp)
str(sp)

## Some basic mathematical functions: "min", "max" and "range"
range(sp$lat)
range(sp$long)
max(sp$lat)
min(sp$lat)

## writing new .csv file
write.csv(sp, "Potamonautes_obesus.csv", row.names = F)
```

## Next meeting topic: working with shapefiles

```
# Installing R packages
library(rgdal)

# Reading and working with shapefiles
world<-readOGR("world.shp")
plot(world, col="grey")
points(sp[,2:3], pch = 3, cex = 0.5, col="red")
```

## Contact
<img src="foto.jpg" width="350">

For more information, please contact me at my email: douglas_biologo@yahoo.com.br
