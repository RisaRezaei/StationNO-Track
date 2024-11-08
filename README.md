# StationAirTrack: Tracking NO₂ Pollution at Air Quality Stations

## Overview

**StationAirTrack** is a tool that allows users to track nitrogen dioxide (NO₂) pollution levels at specific air quality monitoring station locations using **Sentinel-5P TROPOMI** data within **Google Earth Engine (GEE)**. This tool visualizes NO₂ column density over a user-defined time period and generates a time series chart for pollution trends at different locations.

---

## Code Explanation

### 1. **Define the Point of Interest**

In this step, we define the geographic point (longitude, latitude) for the air quality station you want to monitor:

```javascript
var point = ee.Geometry.Point([-73.817694, 40.739264]);
```
point: This line sets a geographic point of interest with latitude and longitude coordinates. In this case, the coordinates [-73.817694, 40.739264] correspond to a location in New York, USA. You can change these values to the coordinates of your own location.

### 2. Define the Time Range

Next, we define the time period during which we want to analyze the NO₂ data. Here, we are setting the start and end dates:

```javascript

var startDate = ee.Date('2018-07-01'); // Sentinel-5P TROPOMI data starts from July 2018
var endDate = ee.Date(new Date()); // up to the current date
```
startDate: We specify the start date for data collection. In this case, July 1, 2018.

endDate: The end date is set to the current date (new Date()), meaning the most recent available data will be included.

3. Load the Sentinel-5P NO₂ Data

This part of the code loads the Sentinel-5P TROPOMI NO₂ dataset from the Google Earth Engine data catalog. It also applies filters to limit the data to the specific location and time range:

```javascript
var collection = ee.ImageCollection('COPERNICUS/S5P/NRTI/L3_NO2')
  .select('NO2_column_number_density')
  .filterDate(startDate, endDate)
  .filterBounds(point);
```
select('NO2_column_number_density'): This filters the dataset to include only the NO₂ column density band, which measures NO₂ concentration in the vertical column of the atmosphere (mol/m²).

filterDate(startDate, endDate): Limits the data to the defined time range from startDate to endDate.

filterBounds(point): Focuses the data on the geographic area around the defined point of interest.

4. Create a Time Series Chart of NO₂ Column Density

In this step, we create a time series chart that displays the average NO₂ concentration over time for the selected location:

```javascript
var chart = ui.Chart.image.series({
  imageCollection: collection,
  region: point,
  reducer: ee.Reducer.mean(),
  scale: 1000 // Adjust scale if necessary for the specific area
});
```
imageCollection: Specifies the source of the data, which is the NO₂ dataset filtered earlier.

region: Limits the data to the defined geographic point.

reducer: Uses ee.Reducer.mean() to calculate the average NO₂ column density for each image in the collection.

scale: Defines the spatial resolution (in meters) used to aggregate the data. You can adjust the scale depending on the area you're analyzing.

5. Set Chart Options

Now, we customize the appearance of the chart to make it more readable:
```javascript
chart.setOptions({
  title: 'NO2 Column Density Time Series at a Specific Point (mol/m²)',
  vAxis: {title: 'NO2 Column Density (mol/m²)'},
  hAxis: {title: 'Date'},
  lineWidth: 1,
  pointSize: 3,
});
```
title: Specifies the title of the chart.
    
vAxis and hAxis: These define the axis labels for NO₂ column density (mol/m²) and date, respectively.

lineWidth: Controls the width of the line in the chart.
    
pointSize: Adjusts the size of the data points for better visibility.

6. Display the Chart in the Console

Finally, we display the generated time series chart in the Google Earth Engine console:
```javascript

print(chart);
```
print(chart): Outputs the chart to the console, where you can visualize the NO₂ column density trend over time at the specified location.

Summary

The StationAirTrack tool allows users to visualize NO₂ pollution trends over time at specific geographic locations. Using Sentinel-5P TROPOMI data in Google Earth Engine, it generates a time series chart that shows the average NO₂ column density at the selected point of interest. This is helpful for monitoring air pollution and understanding environmental changes at different locations.
Repository Structure

StationAirTrack/
  ├── README.md
  ├── NO2_Tracking_Code.js
  ├── examples/
      └── sample_output_chart.png

    README.md: The documentation explaining the usage and functionality of the tool.
    NO2_Tracking_Code.js: The full Google Earth Engine code for tracking NO₂ pollution.
    examples/sample_output_chart.png: A sample output chart demonstrating the tool's capabilities.

By following this structure, the StationAirTrack tool allows for easy tracking and visualization of NO₂ pollution trends, helping researchers and environmental scientists better understand air quality dynamics at specific locations.


---

This structure ensures that the code is clearly separated from the explanation, making the repository more readable and easier to navigate. Each section is clearly defined with headings, and the code blocks are properly formatted using Markdown syntax, allowing for both explanation and code to coexist in a clean and user-friendly manner.

