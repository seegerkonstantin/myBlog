---
title:       "Assessment: Lidar forest indices"
subtitle:    ""
description: ""
date:        2021-12-05
author:      "Konstantin Seeger"
categories: ["aGIS"]
tags: ["task"]
toc: true
weight: 100
image: "/img/IMG-20190924-WA0117.jpg"
---




# Task 
Our research question is:
Is it possible to derive a suitable set of predictor variables from LiDAR data to obtain a reliable prediction of the microclimate parameters temperature and humidity?

The following Task had to be performed: 

- Read the ressources related to forest and identify those which you will use for attempting the task.
- Decide which algorithms and indices are adequate to answer the research question.
- Apply and document this findings with on base of the scripts of the this unit experiences.

# Possible Predictors 

## LAI

To solve this task I read a few articles and selected "Simulations of Leaf BSDF Effects on Lidar Waveforms" (Roth et al. 2020).
They described the calculation of the Leaf Area Index (LAI). I thought that it might be posible to predict temperature and humidity from the LAI. I did some research and I read that eg. Hardwick et al. (2014) found out that, if the LAI is high, humidity and temperature are low and if the LAI is low, humidity and temperature are high.

Based on these findings we are trying to calculate the LAI onto our research area close to Marburg. Roth et al. (2010) described that he used the formula of Kamoske et al. (2019) to calculate the LAI. To calculate it Kamoske build a R-Packege called "canopyLazR" which I used for this assignment. The following code does describe the calculation of the LAI. 





```r
# Convert .laz or .las file into a voxelized lidar array
#laz.data <- laz.to.array(laz.file.path = file.path(envrmt$path_lidar_org,"las_mof.las"), 
 #                        voxel.resolution = 10, 
  #                       z.resolution = 1,
   #                      use.classified.returns = TRUE)

# Level the voxelized array to mimic a canopy height model
#level.canopy <- canopy.height.levelr(lidar.array = laz.data)

# Estimate Leaf Area Density (LAD) for each voxel in leveled array
#lad.estimates <- machorn.lad(leveld.lidar.array = level.canopy, 
 #                            voxel.height = 1, 
  #                           beer.lambert.constant = NULL)

# Convert the LAD array into a single raster stack
#lad.raster <- lad.array.to.raster.stack(lad.array = lad.estimates, 
 #                                       laz.array = laz.data, 
  #                                      epsg.code = 25832)


# Create a single LAI raster from the LAD raster stack
#lai.raster <- raster::calc(lad.raster, fun = sum, na.rm = TRUE)
```


![](images/lai_1.png) 

It is visibly that the LAI has the value zero at few areas that are in the east. These are most likely open areas. Higher values up to five occur at a few points in all directions of the research area. 

## Digital Elevation Model

An pretty simple indicator could be a digital elevation model (DGM). Temperatur increses with height by 0.98 K for every 100 meters (Bendix & Lutherbacher 2019). Humidity on the other side decreses with higher elevation (Duane et al. 2008). The dgm and the rest of the predictors have been calculated with the lidR or the raster package (Hijmans 2018; Roussel 2021). 


```r
#dgm <- grid_terrain(lidar_file, res = 1, algorithm = tin())
```


![](images/dgm2.png)

The highest elevation can be found in the southwestern part of our research area. In the north-west the elevation is the lowest. 


## Slope, Aspect, Topographic postion index (TPI)

Calculated from the digital elevation model slope, aspect and the TPI could be three other predictors. 

```r
# Slope
#slope <- raster::terrain(dgm,opt="slope", unit="degrees")

# Aspect 
#aspect <- raster::terrain(dgm,opt = "aspect",unit = "degrees")

# TPI
#tpi <- raster::terrain(dgm,opt = "TPI")
```

![](images/Aspect.png)
![](images/Slope.png)
![](images/TPI2.png)

## Canopy Height Model

A canopy height model (chm) is another potential predictor. A canopy height model has also been used in the study of Carrasco et al. (2019). 


```r
#chm = lidR::grid_canopy(norm_las, 1, pitfree(c(0,2,5,10,15), c(0,1), subcircle = 0.2))
```

![](images/canopy_height_model_chm2.png)

## Height first returns standard deviation

The first returns of the height standard deviation had in the study of Carrasco et al. (2019) one of the highest correlations with temperature. Therefore it is a must use to predict temperature. Additionally you could also use the maximum and mean height first returns but I expect them to be very similar to the canopy height model itself.  


```r
#first_return_sd <- grid_metrics(lidar_file, func = ~sd(Z), res = 1, by_echo = "first")
```

![](images/First_returns_height_sd2.png)


## Point density
My last predictor is the point density. Point density is defined as the number of points within a pixel divided by the are of the pixel (m²) (Roussel et al. 2021). 
With higher forest density radiation can't get through the canopy which leads to lower temperatures compared to the areas that solar radiation can reach (Boehnke 2021 & Arx et al. 2012). In the study from Arx et al. (2012) they also found out that density lead to an increased humidity. 


```r
#density <- grid_metrics(lidar_file, ~length(Z)/1, 1)
```

![](images/Point_density.png)

Obviously the dark stripes in the plot are not really looking natural and making me question if point density can be a reliable predictor. 



# References

- Arx, v.G., Dobbertin, M. & M.  Rebetez (2012): Spatio-temporal effects of forest canopy on understory microclimate in a long-term experiment in Switzerland. Agricultural and Forest Meteorology. 166-167. 144-155. <https://doi.org/10.1016/j.agrformet.2012.07.018> 
- Bendix J. & J. Lutherbacher (2019): Klimatologie. 3. Auflage. Westermann. Braunschweig. 
- Boehnke D. (2021): Exploring the Thermal Microcosms at the Forest Floor—A Case Study of a Temperate Forest. Atmosphere. 12. 503. <https://doi.org/10.3390/atmos12040503> 
- Carrasco, L., Giam, X., Papes, M. & K.S. Sheldon (2019): Metrics of Lidar-Derived 3D Vegetation Structure Reveal Contrasting Effects of Horizontal and Vertical Forest Heterogeneity on Bird Species Richness. Remote Sensing. 11. 743. <https://doi:10.3390/rs11070743>
- Duane, W.J., Pepin, N.C., Losleben, M.L. & D.R. Hardy (2008): General Characteristics of Temperature and Humidity Variability on Kilimanjaro, Tanzania. Arctic, Antarctic, and Alpine Research. 40:2. 323-334. <https://doi.org/10.1657/1523-0430(06-127)[DUANE]2.0.CO;2> 
- Hardwick, S.R., Toumi, R., Pfeifer, M., Turner, E.C., Nilius, R. & R.M. Ewers (2014): The relationship between leaf area index and microclimate in tropical forest and oil palm plantation: Forest disturbance drives changes in microclimate. Agricultural and Forest Meteorology. 201. 187-195. <https://doi.org/10.1016/j.agrformet.2014.11.010> 
- Kamoske, A.G. (2020): canopyLazR. <https://github.com/akamoske/canopyLazR> 
- Hijmans R.J. (2018): Spatial Data Science with R. <https://rspatial.org/raster/index.html>
- Roth, D.B., Goodenough, A.A.,Brown, S.D., van AArdt, J.A., Saunders, M.G. & K. Krause (2020): Simulations of Leaf BSDF Effects on Lidar Waveforms. Remote Sensing. 12. 2909. <https://doi.org/10.3390/rs12182909>
- Roussel, J.R., Goodbody, T.R.H. & P. Tompalski (2021): The lidR package. <https://jean-romain.github.io/lidRbook/index.html> 
