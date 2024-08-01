## Code for practice in r  

```
###### Using shapefiles to categorize occurrence records
## Opening packages
library(sf)
library(raster)
library(dplyr) 

## Load working directory
setwd('PATH')

# Load the raster (replace with your raster file)
temp_biooracle<-raster('thetao_baseline_2000_2019_depthmin_74ff_39fa_9adc_U1720639852268.nc')
temp_biooracle

res(temp_biooracle)

# Load the raster (replace with your raster file)
port_gmed<-raster('port_distance.asc')
port_gmed

res(port_gmed)

# Saving temp_biooracle in .asc format
writeRaster(temp_biooracle, "temperature_biooracle.asc")


## Combine all rasters in your given directory into a stack object
predictors <- stack(list.files(path = "PATH", pattern='.asc', full.names=T))
###Error : different extent

# Load the raster (replace with your raster file)
temp_biooracle<-raster('temperature_biooracle.asc')
plot(temp_biooracle)

# Some ways to change the resolution of environmental layers

# Decrease the raster resolution (aggregation)
temp_biooracle_menor <- aggregate(temp_biooracle, fact=2, method='bilinear')
plot(temp_biooracle_menor)

### Check the resolution, the number of columns, rows, and cells
temp_biooracle
temp_biooracle_menor

# Increase the raster resolution (disaggregation)
temp_biooracle2 <- disaggregate(temp_biooracle_menor, fact=2, method='bilinear')

### Check the resolution, the number of columns, rows, and cells
temp_biooracle
temp_biooracle2
temp_biooracle_menor

res(temp_biooracle)
res(port_gmed)

## Making everything the same size 
temp_biooracle_res<-resample(temp_biooracle,port_gmed,'bilinear')
temp_biooracle_res
plot(temp_biooracle_res)

res(temp_biooracle_res)
res(port_gmed)


####### Visualizing the difference in cell sizes in the plot

## open .shp file
cities <- read_sf("BR_Municipios_2022.shp")
names(cities)

fn<-subset(cities, cities$NM_MUN=="Fernando de Noronha")
fn

# Define the new extent (bbox) of the plot
new_bbox <- st_bbox(c(xmin = -32.8, ymin = -4, xmax = -32.2, ymax = -3.6), crs = st_crs(fn))

# Crop the raster to the new extent
temp_biooracle_crop <- crop(temp_biooracle, new_bbox)
temp_biooracle_res_crop <- crop(temp_biooracle_res, new_bbox)

# Plot the cropped raster and the geometry of Fernando de Noronha
plot(temp_biooracle_crop)
plot(st_geometry(fn), add=TRUE)

# Plot the cropped raster and the geometry of Fernando de Noronha
plot(temp_biooracle_res_crop)
plot(st_geometry(fn), add=TRUE)

# Saving temp_biooracle_res in .asc format
writeRaster(temp_biooracle_res, "temperature_biooracle2.asc")

## Combine all rasters in your given directory into a stack object
predictors <- stack(list.files(path = "PATH/resolution", pattern='.asc', full.names=T))
predictors

```

