# ENVIRONMENTAL LAYERS

## Contents
- [Environmental layers and macroecology](#environmental-layers-and-macroecology)
- [Downloading environmental layers](#downloading-environmental-layers)
- [Working with environmental layers in r](#working-with-environmental-layers-in-r)
    
- [Contact](#contact)

## Environmental layers and macroecology
Interpreting these environmental layers in macroecological studies enables the identification of ecological patterns and understanding how these environmental factors influence species distribution. For example, some species may be restricted to certain temperature or altitude ranges, while others may be more generalists and adapt to a variety of environmental conditions. Furthermore, the interaction between different environmental layers can create ecological gradients that influence community diversity and composition.

Environmental layers can be obtained from a variety of sources, including field observations, satellite-collected data, and simulation models. Here are some of the most common file formats: 

__.tif__ - GeoTIFF - Satellite images are an important source of data for environmental layers (Remote Sensing Data), providing information on vegetation cover, surface temperature, soil moisture, among others. Common file formats for this data include __GeoTIFF__, __NetCDF__, and HDF.  
__.nc__ - NetCDF - NetCDF files are a common format for storing climatic and environmental data, especially those derived from global or regional climate models, satellite observations, and meteorological stations. NetCDF (Network Common Data Form) is an efficient format for storing large multidimensional datasets, including climate data with multiple variables and spatial and temporal dimensions.  
__.asc__ - .asc files are often used to store elevation data, such as digital elevation models (DEM), but can also be used for other types of raster data, such as precipitation, temperature, land cover data, and more.  


## Downloading environmental layers  
There are many databases on environmental layers, and we can create layers, depending on our objective. 
Some examples of sites and files that we will use:  
Marine environmental layers  
https://bio-oracle.org/  

Terrestrial layers  
https://www.worldclim.org/  


## Extension and resolution
__Extension__   
When dealing with environmental layers in geospatial studies, it's important to understand both the geographical (latitude and longitude) and environmental extents (e.g., marine or terrestrial) of these layers.  
It's important to consider which environmental variables are relevant to your research and whether the available data covers those variables within the desired geographical extent.  

__Resolution__  
The resolution of environmental layers is another crucial aspect to consider in geospatial analysis.

__Spatial Resolution:__  
1- Spatial resolution refers to the size of the smallest discernible unit on the Earth's surface represented by each pixel or cell in the raster dataset. It determines the level of detail captured by the data.  
2- Higher spatial resolution datasets have smaller pixel sizes and can capture finer details, while lower spatial resolution datasets have larger pixel sizes and provide a more generalized view of the landscape.  

<img src="chequered map.jpg" width="750">

__Temporal Resolution:__  
1- Temporal resolution refers to the frequency at which data is collected over time. It's particularly relevant for dynamic environmental variables such as temperature, precipitation, and vegetation cover.  
2- Temporal resolution is important for understanding seasonal variations, detecting trends, and assessing the impacts of short-term events such as storms or droughts.  
When working with environmental layers, it's essential to consider both spatial and temporal resolution to ensure that the data's level of detail and frequency of observation match the requirements of your analysis or research objectives. 

## Working with environmental layers in r
Opening and cutting environmental layers in R

```
#install.packages("ncdf4")

# Opening packages
library(sf)
library(raster)

# Opening a .csv file in r
## Change to working directory
setwd("C:/PATH")

# Reading shapefiles
mundo <- read_sf(dsn="C:/PATH/ne_10m_admin_0_countries.shp")

## Open and view the table (dataframe) with occs points
occs <- read.csv(file = "Potamonautes_obesus.csv", header = T);
head(occs)
pontos_sf <- st_as_sf(occs, coords = c("long", "lat"))
crs <- st_crs(mundo)  # Use o CRS do shapefile
st_crs(pontos_sf) <- crs  

#####Marine environmental layers
depth<-raster('C:/PATH/terrain_characteristics_6d78_4f59_9e1f_U1714656179666.nc')
depth
plot(depth)


temp<-raster('C:/PATH/thetao_baseline_2000_2019_depthmin_74ff_39fa_9adc_U1714656171112.nc')
temp
plot(temp)


#####Terrestrial environmental layers
temp_terr<-raster('C:/PATH/wc2.1_10m_bio/wc2.1_10m_bio_1.tif')
temp_terr
plot(temp_terr)

precip<-raster('C:/PATH/wc2.1_10m_bio/wc2.1_10m_bio_12.tif')
precip
plot(precip)

#########cropping layers
temp_terr_crop <- crop(temp_terr, extent(10,60,-30,20))
plot(temp_terr_crop)
                       
# Plotting and save a map
pdf('Map_P_obesus_temp.pdf')
plot(temp_terr_crop)
plot(st_geometry(mundo), add=TRUE, border = "grey60",xlim = c(20,50), ylim = c(-30, 20), axes = TRUE)
plot(pontos_sf, add = TRUE, col = "black", pch = 16, cex = 0.5)
box()
dev.off()
```


## Contact
For more information, please contact me at my email: douglas_biologo@yahoo.com.br

