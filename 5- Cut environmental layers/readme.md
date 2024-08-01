## Code for practice in r  

```
###### Using shapefiles to categorize occurrence records
## Opening packages
library(sf)
library(raster)
library(dplyr) 

## Load working directory
setwd('C:/PATH')

## Open .csv file with occurrence records
occs_cleaned<-read.csv("./occs_cleaned.csv",header=TRUE)
dim(occs_cleaned)
names(occs_cleaned)
head(occs_cleaned)

## Check REALM codes in the records dataframe
unique(occs_cleaned$RLM_CODE)
## Create a list of REALM codes (RLM_CODE) that we will use to cut environmental variables
unique_RLM_CODE <- unique(occs_cleaned$RLM_CODE)


## open .shp files containing information to categorize occurrence records
regions <- read_sf("C:/PATH/MEOW/meow_ecos.shp")
names(regions)

## open .shp files to plot occurrence records
world<-read_sf(dsn="C:/PATH/mundo/ne_10m_admin_0_countries.shp")

##################################################################
################## Cut environmental layers ######################
##################################################################

## Combine all rasters in your given directory into a stack object
predictors <- stack(list.files(path = "C:/PATH/predictors", pattern='.nc', full.names=T))

## See names
names(predictors)

## Rename to shorter, simpler names
names(predictors) <- c("oxyg",
                       "ph",
                       "phos",
                       "sili",
                       "sali",
                       "speed",
                       "bathy",
                       "temp")

## Check predictors in stack
predictors

# Plot a layer to check
plot(predictors[[8]])

## Select only the REALMS of interest in the shapefile. Filtered the REALM by RLM_CODE
regions_subset <- regions %>% 
  filter(RLM_CODE %in% unique_RLM_CODE)

## Plot the border of the selected area
plot(st_geometry(regions_subset), border="yellow", add=TRUE)

## Cut the environmental variables (present in the stack) using the area selected above as a mask
predictors_cropped <- mask(predictors,regions_subset)

## Save a pdf map that illustrates the selected background area
pdf("background_area.pdf")
plot(st_geometry(world))
plot(predictors_cropped[[8]], add=TRUE)
plot(st_geometry(occs_cleaned), add=TRUE, cex=0.5, col="blue")
dev.off()

## Create an empty folder called "predictors_cropped" in your directory
dir.create(paste0("C:/PATH/predictors_cropped/")) 

## Let's save the new predictor variable files
writeRaster(predictors_cropped,
            # a series of names for output files
            filename=paste0("C:/PATH/predictors_cropped/",names(predictors),".asc"), 
            format="ascii", ## the output format
            bylayer=TRUE, ## this will save a series of layers
            overwrite=T) 

```
