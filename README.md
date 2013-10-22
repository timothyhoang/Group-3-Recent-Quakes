Group-3-Recent-Quakes
=====================
## Members
 Theresa Andrasfay    
  Timothy Hoang  
 Lorraine Hsiao  
 Qi Zhang

## Goals of this project
Clean the USGS earthquake data and visualize the earthquakes on a map.  
The various feeds of earthquakes can be found [here.](http://earthquake.usgs.gov/earthquakes/feed/v1.0/geojson.php "USGS Feeds")

## Virtual Box Initial Setup
Open your virtual machine and log in. 
Then execute the following commands:
```sh
git clone https:github.com/tandrasfay/Group-3-Recent-Quakes
cd Group-3-Recent-Quakes
```
```sh
sudo apt-get install libgeos-3.3.3 python-mpltoolkits.basemap python-mpltoolkits.basemap-data python-mpltoolkits.basemap-doc
```
Then run the notebook from your machine with this command:
```sh
ipython notebook --no-browser --ip=0.0.0.0 --script --pylab=inline
```
You should get a response that the ipython notebook is running at xxxx which you will type into your browser.

Choose *Recent Earthquakes* in the ipython notebook dashboard and then click *cell* --> *run all*
