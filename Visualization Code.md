# plot_quakes plots the earthquakes in a rectangular region around coordinates in latitude and longitude from the earthquake 
# data in quakes. The rectangular region is defined by dimensions (latitude x longitude), where:
#       llcrnrlon = longitude - dimensions[2]/2 (longitude of lower left corner of region)
#       urcrnrlon = longitide + dimensions[2]/2 (longitude of upper right corner of region)
#       llcrnrlat = latitude - dimensions[1]/2 (latitude of lower left corner of region)
#       urcrnrlat = latitude + dimensions[1]/2 (latitude of upper right corner of region)
# the resolution and area threshold (area_thresh) of the map is handled based on the area of the dimensions provided and
#
# @param quakes - a data frame containing the earthquake data (need to document proper formatting of data frame)
# @param longitude - the longitude of the center of the region to be plotted
# @param latitude - the latitude of the center of the region to be plotted
# @param dimensions - the dimensions of the region to be plotted (latitude x longitude) as a list [degrees in latitude, degrees in longitude]
# @param projection - the type of projection ('cyl', 'merc', 'mill', 'cea' or 'gall')

```
def plot_quakes(quakes, longitude, latitude, dimensons, projection):
    
    # Resolution, stored in res, will be narrowed down based on the area of the dimensions provided
    threshold = [1,10,100,1000,10000]
    res = ['f','h','i','l','c']
    area = dimensions[1]*dimensions[2]
    
    for i in range(5):
        if area <= threshold[i]:
            res = res[i]
            break
        if i == 5:
            res = res[5]

    # Plot of the region in the map 
    m = Basemap(llcrnrlon=longitude-dimensions[2]/2,
                llcrnrlat=latitude-dimensions[1]/2,
                urcrnrlon=longitide+dimensions[2]/2,
                urcrnrlat=latitude+dimensions[1]/2,
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
    
# Example:
# To plot the earthquakes in California, for a data frame for earthquakes structured from our code:
#	quakes_df = pd.DataFrame({"Source":listPropCode,"Network":listPropNet,"Time":listPropTime,"Longitude":listGeoLongitude,"Latitude":listGeoLatitude,
#	"Depth":listGeoDepth,"NST":listPropNst,"Place":listPropPlace,"Magnitude":listPropMag})
# Run:
# California = quakes_df[quakes_df.Source=='ci']
# Longitude = -119.5
# Latitude = 37
# dimensions = [11,10]
# projection = 'merc'
# 
# plot_quakes(California, Longitude, Latitude, dimensions, projection)
