# SHAPEFILES

## Contents
- [Shapefiles and macroecology](#shapefiles-and-macroecology)
- [Working with shapefiles in r](#working-with-shapefiles-in-r)
    
- [Contact](#contact)

## Shapefiles and macroecology
Shapefiles (.shp) indeed serve as a foundational tool in macroecological research, enabling researchers to effectively manage, analyze, and visualize geospatial data pertinent to understanding species distributions, ecological dynamics, and environmental patterns across various scales. Their ability to organize data into layers and their compatibility with spatial analysis techniques make them invaluable assets for advancing our knowledge of macroecological processes.

Shapefile files typically come in four different extensions when downloaded because each extension contains a different part of the geographic dataset. These files need to work together for geographic information to be correctly interpreted by GIS (Geographic Information System). 

__.shp__ - This is the main file storing the geometric shapes of features (such as points, lines, or polygons). It contains vector geometry for all features.  
__.shx__ - The shape index file (.shx) stores the index of the geometric record position within the .shp file. This enables quick access to specific records in the main file, enhancing efficiency, especially in large files.  
__.dbf__ - This file contains attributes for each shape, in a standard table format that can be accessed by many database programs. Each row in the .dbf table corresponds to a shape in the .shp file, and each column stores a type of information (such as name, type, area, etc.).  
__.prj__ - The projection file (.prj) stores information about the cartographic projection and coordinate system in which the geographic data is mapped. This is crucial to ensure that GIS software correctly interprets the geographic position of the shapes.  

## Downloading shapefiles  
There are many geospatial image banks with files (.shp) available for download.  
An example of site that we will use:  

Political map of the world  
https://www.naturalearthdata.com/  

## Working with shapefiles in r
Today, install the following packages:    
"sf" - a popular R package for manipulating geospatial data;  

Some of the main commands for working with shapefiles in R

```
# Opening packages
library(sf)

# Opening a .csv file in r
## Change to working directory
setwd("C:/PATH")

## Open and view the table (dataframe) created in the previous tutorial
occs <- read.csv(file = "Potamonautes_obesus.csv", header = T);
head(occs)

# Reading shapefiles
mundo <- read_sf(dsn="C:/PATH/ne_10m_admin_0_countries.shp")
mundo; #Check out what the metadata for the file look like
plot(st_geometry(mundo))

# Evaluating shapefile content
names(mundo)
mundo$NAME
plot(st_geometry(mundo[45,]))
mundo$CONTINENT
mundo$POP_EST
mundo$ECONOMY

# Plotting occurrence points on the map
head(occs)
pontos_sf <- st_as_sf(occs, coords = c("long", "lat"))
crs <- st_crs(mundo)  # Use o CRS do shapefile
st_crs(pontos_sf) <- crs

plot(st_geometry(mundo))
plot(pontos_sf, add = TRUE, col = "red", pch = 16, cex = 0.5)

# Selecting the region of interest
africa<- subset (mundo, mundo$CONTINENT == "Africa")
africa$NAME
plot(st_geometry(africa))

## Save the selected polygon
st_write(south_america,'.','america_sul',driver='ESRI Shapefile',overwrite=T)

# Plotting only the region of interest
head(occs)
pontos_sf <- st_as_sf(occs, coords = c("long", "lat"))
crs <- st_crs(mundo)  # Use o CRS do shapefile
st_crs(pontos_sf) <- crs

plot(st_geometry(africa), axes = TRUE)
plot(pontos_sf, add = TRUE, col = "red", pch = 16, cex = 0.5)


### Cutting the area of interest by the coordinates
africa
plot(st_geometry(africa),xlim = c(20,50), ylim = c(-30, 20), axes = TRUE)
plot(pontos_sf, add = TRUE, col = "red", pch = 16, cex = 0.5)

#pch atributo 
#https://www.r-bloggers.com/2021/06/r-plot-pch-symbols-different-point-shapes-in-r/

# Plotting and save a map
pdf('Map_P_obesus.pdf')
plot(st_geometry(africa), col = "cyan", border = "grey60",xlim = c(20,50), ylim = c(-30, 20), axes = TRUE)
plot(pontos_sf, add = TRUE, col = "red", pch = 16, cex = 0.5)
box()
dev.off()

```
## CRS 
CRS (Coordinate Reference System), in a geospatial context, describes how spatial data is represented in terms of coordinates and units of measurement. It is fundamental to the correct interpretation of geospatial data, ensuring that it is correctly projected into a specific coordinate system.  

## Contact
For more information, please contact me at my email: douglas_biologo@yahoo.com.br
