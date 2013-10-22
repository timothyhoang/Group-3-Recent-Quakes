plot_quakes plots the earthquakes in a rectangular region around coordinates in latitude and longitude from the earthquake \n
data in quakes. The rectangular region is defined by dimensions (latitude x longitude), where:

      llcrnrlon = longitude - dimensions[2]/2 (longitude of lower left corner of region)
      
      urcrnrlon = longitide + dimensions[2]/2 (longitude of upper right corner of region)
      
      llcrnrlat = latitude - dimensions[1]/2 (latitude of lower left corner of region)
      
      urcrnrlat = latitude + dimensions[1]/2 (latitude of upper right corner of region)
      
The resolution and area threshold (area_thresh) of the map is handled based on the area of the dimensions provided.

@param quakes - a data frame containing the earthquake data (need to document proper formatting of data frame)

@param longitude - the longitude of the center of the region to be plotted

@param latitude - the latitude of the center of the region to be plotted

@param dimensions - the dimensions of the region to be plotted (latitude x longitude) as a list [degrees in latitude, degrees in longitude]

@param projection - the type of projection ('cyl', 'merc', 'mill', 'cea' or 'gall')

```python
def plot_quakes(quakes, longitude, latitude, dimensons, projection):
    
    # Resolution, stored in res, will be narrowed down based on the area of the dimensions provided
    threshold = [1,10,100,1000,10000]
    res = ['f','h','i','l','c']
    area = dimensions[0]*dimensions[1]
    
    for i in range(5):
        if area <= threshold[i]:
            res = res[i]
            break
        if i == 5:
            res = res[5]

    # Plot of the region in the map 
    m = Basemap(llcrnrlon=longitude-dimensions[1]/2,
                llcrnrlat=latitude-dimensions[0]/2,
                urcrnrlon=longitide+dimensions[1]/2,
                urcrnrlat=latitude+dimensions[0]/2,
                resolution=res,
                projection=projection,
                lat_0=latitude,lon_0=longitude)
				
    m.drawcoastlines()
    m.drawcountries()
    m.fillcontinents(color='coral',lake_color='blue')
    m.drawmapboundary(fill_color='aqua')
	
    for i in range(10):
        color = str((0,0,.3+i*.05))
        x, y = m(quakes.Longitude[quakes.Magnitude <= i], quakes.Magnitude[quakes.Depth <= i])	
        m.plot(x, y, color = col, marker = '.')
	
    return m
```    

If this code does not work, try:
```python
def plot_quakes(quakes, longitude, latitude, dimensons, projection):
    
    # Resolution, stored in res, will be selected based on the area of the dimensions provided
    threshold = [1,10,100,1000,10000]
    res = ['f','h','i','l','c']
    area = dimensions[0]*dimensions[1]
    
    for i in range(5):
        if area <= threshold[i]:
            res = res[i]
            break
        if i == 5:
            res = res[5]

    # Plot of the region in the map 
    m = Basemap(llcrnrlon=longitude-dimensions[1]/2,
                llcrnrlat=latitude-dimensions[0]/2,
                urcrnrlon=longitide+dimensions[1]/2,
                urcrnrlat=latitude+dimensions[0]/2,
                resolution=res,
                projection=projection,
                lat_0=latitude,lon_0=longitude)
				
    m.drawcoastlines()
    m.drawcountries()
    m.fillcontinents(color='coral',lake_color='blue')
    m.drawmapboundary(fill_color='aqua')
	
    x, y = m(quakes.Longitude, quakes.Latitude)
    m.plot(x, y, 'k.')
    return m
```
    
Example:
To plot the earthquakes in California, for a data frame for earthquakes structured from our code:
```
quakes_df = pd.DataFrame({"Source":listPropCode,"Network":listPropNet,"Time":listPropTime,"Longitude":listGeoLongitude,"Latitude":listGeoLatitude, "Depth":listGeoDepth,"NST":listPropNst,"Place":listPropPlace,"Magnitude":listPropMag})
```
Run:
```python
 California = quakes_df[quakes_df.Source=='ci']
 Longitude = -119.5
 Latitude = 37
 dimensions = [11,10]
 projection = 'merc'
 
 plot_quakes(California, Longitude, Latitude, dimensions, projection)
```

Here is the simplified coe, which simple takes in a data frame of earthquake data and plots all of the points:

```python
from mpl_toolkits.basemap import Basemap

# plot_quakes plots the earthquakes as circles in a rectangular region around the mean of the coordinates where the size of the 
# circle represents the magnitude of the earthquake and light the earthquake is represents the magnitude 

def plot_quakes_simple(quakes):
    
    # Resolution, stored in res, will be selected based on the area of the dimensions provided
    
    lat_0 = quakes['Latitude'].mean()
    lon_0 = quakes['Longitude'].mean()

    max_lat = quakes['Latitude'].max()
    min_lat = quakes['Latitude'].min()
    max_lon = quakes['Longitude'].max()
    min_lon = quakes['Longitude'].min()
    dimensions = [1.2*(max_lat-min_lat), 1.2*(max_lon-min_lon)]

    threshold = [1,10,100,1000,10000]
    res = ['f','h','i','l','c']
    area = dimensions[0]*dimensions[1]
    
    depth_level = ('(0,0,80)','(0,0,120)','(0,0,160)','(0,0,200)','(0,0,240)')

    for i in range(5):
        if area <= threshold[i]:
            res = res[i]
            break
        if i == 5:
            res = res[5]

    # Plot of the region in the map 
    m = Basemap(llcrnrlon=longitude-dimensions[1]/2,
                llcrnrlat=latitude-dimensions[0]/2,
                urcrnrlon=longitide+dimensions[1]/2,
                urcrnrlat=latitude+dimensions[0]/2,
                resolution=res,
                projection='nsper',
                lat_0=latitude,lon_0=longitude)
				
    m.drawcoastlines()
    m.drawcountries()
    m.fillcontinents(color='coral',lake_color='blue')
    m.drawmapboundary(fill_color='aqua')
	
    x, y = m(quakes.Longitude, quakes.Latitude)

    for i in range(len(x)-1):
        if quakes.Depth[i] < 100:
            depth_col = depth_level[0]
        elif 100 <= quakes.Depth[i] < 200:
            depth_col = depth_level[0]
        elif 200 <= quakes.Depth[i] < 300:
            depth_col = depth_level[0]
        elif 300 <= quakes.Depth[i] < 400:
            depth_col = depth_level[0]
        else:
            depth_col = depth_level[0]
            
        m.plot(x[i], 
               y[i],
               alpha = 0.5,
               color = depth_col, 
               marker = 'o', 
               markersize = (quakes.Magnitude[i]**2)*pi)

    return m
```
