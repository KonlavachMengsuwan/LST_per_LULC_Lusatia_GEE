# LST_per_LULC_Lusatia_GEE

# Land Surface Temperature (LST) and Land Use/Land Cover (LULC) Analysis with Google Earth Engine

This repository contains the Google Earth Engine (GEE) scripts and related files for analyzing Land Surface Temperature (LST) and Land Use/Land Cover (LULC) data for the Lusatia region. The project uses data from Landsat 8/9 and the ESRI LULC dataset.

## Overview

The primary objectives of this project are to:
1. Analyze the LST for different land cover types in the Lusatia region.
2. Visualize the LULC and LST data.
3. Generate histograms of LST for each LULC class.
4. Export the LULC and LST data for further analysis.

## Data Sources

- **Landsat 8/9**: Provides Land Surface Temperature (LST) data.
- **ESRI Global Land Cover (10m)**: Provides Land Use/Land Cover (LULC) data.

## Files

- `LST_Mosaic_20230815_0957`: Land Surface Temperature mosaic image.
- `Lausitz_ESRI_LULC_2023`: ESRI LULC image for the Lusatia region.
- `Lusatia_Administrative_Boundary_EPSG4326_Dissolved`: Administrative boundary of the Lusatia region.

## Google Earth Engine Script

The GEE script performs the following tasks:
1. Loads the LST and ESRI LULC images.
2. Clips the LULC image into separate rasters based on each LULC class.
3. Clips the LST image based on the same regions.
4. Adds LULC and LST layers to the map.
5. Generates histograms of LST for each LULC class.
6. Exports the LULC and LST images for each LULC class to Google Drive.

## Usage

To run the script, follow these steps:

1. **Google Earth Engine Setup**:
   - Go to the [Google Earth Engine Code Editor](https://code.earthengine.google.com/).
   - Create a new script and paste the GEE script from this repository.
   - Modify the paths to the assets if needed.

2. **Run the Script**:
   - Click the "Run" button in the GEE Code Editor to execute the script.
   - The script will load the LST and LULC images, perform the analysis, and export the results.

## Exported Data

The script exports the following data to Google Drive:
- LULC images for each land cover class.
- LST images for each land cover class.

## Visualization

The script also visualizes the LULC and LST data on the map with appropriate legends.

![image](https://github.com/user-attachments/assets/9289789a-df1a-463e-9101-9ef6a2a78479)


## Histogram Generation

Histograms of LST for each LULC class are generated and displayed in the GEE console.
![image](https://github.com/user-attachments/assets/6ab08063-f890-4b73-8e22-132a6cc542b8)
![image](https://github.com/user-attachments/assets/939e95b0-e840-48af-bd91-73b807257840)
![image](https://github.com/user-attachments/assets/630db5dc-3073-45e4-8a96-2792e21e70ff)
![image](https://github.com/user-attachments/assets/adcfb08e-765d-4345-a371-84f1956b073e)
![image](https://github.com/user-attachments/assets/f00a6cda-e08e-4a4b-960c-13861b3130df)
![image](https://github.com/user-attachments/assets/43982063-1ca4-479a-a792-9ea784191f15)
![image](https://github.com/user-attachments/assets/577c3c56-ab31-4e48-a8bf-ead6d4c2d028)

## Code
```
// Set the basemap to the default map
Map.setOptions('ROADMAP');

// Define the study area using your uploaded boundary data
var boundary = ee.FeatureCollection('projects/ee-my-konlavach/assets/Lusatia_Administrative_Boundary_EPSG4326_Dissolved');

// Load the LST and ESRI LULC images from your assets
var lstImage = ee.Image('projects/ee-my-konlavach/assets/LST_Mosaic_20230815_0957');
var lulcImage = ee.Image('projects/ee-my-konlavach/assets/Lausitz_ESRI_LULC_2023');

// Define the LULC classes and their corresponding values
var lulcClasses = {
  'Water': 1,
  'Trees': 2,
  'Flooded Vegetation': 3,
  'Crops': 4,
  'Built Area': 5,
  'Bare Ground': 6,
  'Snow/Ice': 7,
  'Clouds': 8,
  'Rangeland': 9
};

// Define visualization parameters for each LULC class
var lulcVisParams = {
  'Water': {palette: ['#1A5BAB'], min: 1, max: 1},
  'Trees': {palette: ['#358221'], min: 2, max: 2},
  'Flooded Vegetation': {palette: ['#87D19E'], min: 3, max: 3},
  'Crops': {palette: ['#FFDB5C'], min: 4, max: 4},
  'Built Area': {palette: ['#ED022A'], min: 5, max: 5},
  'Bare Ground': {palette: ['#EDE9E4'], min: 6, max: 6},
  'Snow/Ice': {palette: ['#F2FAFF'], min: 7, max: 7},
  'Clouds': {palette: ['#C8C8C8'], min: 8, max: 8},
  'Rangeland': {palette: ['#C6AD8D'], min: 9, max: 9}
};

// Define visualization parameters for LST
var lstVisParams = {
  min: 20,
  max: 40,
  palette: ['blue', 'cyan', 'green', 'yellow', 'red']
};

// Function to clip and mask the LULC image by class
function clipLULCByClass(image, classValue) {
  return image.updateMask(image.eq(classValue)).clip(boundary);
}

// Function to add LULC and LST layers to the map
function addLayers(className, classValue) {
  var clippedLULC = clipLULCByClass(lulcImage, classValue);
  var clippedLST = lstImage.updateMask(clippedLULC).clip(boundary);
  
  Map.addLayer(clippedLULC, lulcVisParams[className], className + ' LULC', false);
  Map.addLayer(clippedLST, lstVisParams, className + ' LST', false);

  // Export the LULC image
  Export.image.toDrive({
    image: clippedLULC,
    description: className + '_LULC',
    folder: 'GEE_Exports',
    fileNamePrefix: className + '_LULC',
    region: boundary.geometry(),
    scale: 10,
    maxPixels: 1e13
  });

  // Export the LST image
  Export.image.toDrive({
    image: clippedLST,
    description: className + '_LST',
    folder: 'GEE_Exports',
    fileNamePrefix: className + '_LST',
    region: boundary.geometry(),
    scale: 30,
    maxPixels: 1e13
  });
}

// Create a panel to hold the LULC legend widget
var lulcLegend = ui.Panel({
  style: {
    position: 'bottom-left',
    padding: '8px 15px'
  }
});

// Create a panel to hold the LST legend widget
var lstLegend = ui.Panel({
  style: {
    position: 'bottom-right',
    padding: '8px 15px'
  }
});

// Function to generate the legend
function addCategoricalLegend(panel, dict, title) {
  // Create and add the legend title.
  var legendTitle = ui.Label({
    value: title,
    style: {
      fontWeight: 'bold',
      fontSize: '18px',
      margin: '0 0 4px 0',
      padding: '0'
    }
  });
  panel.add(legendTitle);

  var loading = ui.Label('Loading legend...', {margin: '2px 0 4px 0'});
  panel.add(loading);

  // Creates and styles 1 row of the legend.
  var makeRow = function(color, name) {
    // Create the label that is actually the colored box.
    var colorBox = ui.Label({
      style: {
        backgroundColor: color,
        // Use padding to give the box height and width.
        padding: '8px',
        margin: '0 0 4px 0'
      }
    });

    // Create the label filled with the description text.
    var description = ui.Label({
      value: name,
      style: {margin: '0 0 4px 6px'}
    });

    return ui.Panel({
      widgets: [colorBox, description],
      layout: ui.Panel.Layout.Flow('horizontal')
    });
  };

  // Get the list of palette colors and class names from the image.
  var palette = dict['colors'];
  var names = dict['names'];
  loading.style().set('shown', false);

  for (var i = 0; i < names.length; i++) {
    panel.add(makeRow(palette[i], names[i]));
  }
}

// Define the legend dictionary for ESRI LULC
var lulcDict = {
  "names": [
    "Water",
    "Trees",
    "Flooded Vegetation",
    "Crops",
    "Built Area",
    "Bare Ground",
    "Snow/Ice",
    "Clouds",
    "Rangeland"
  ],
  "colors": [
    "#1A5BAB",
    "#358221",
    "#87D19E",
    "#FFDB5C",
    "#ED022A",
    "#EDE9E4",
    "#F2FAFF",
    "#C8C8C8",
    "#C6AD8D"
  ]
};

// Define the legend dictionary for LST
var lstDict = {
  "names": [
    "20°C",
    "25°C",
    "30°C",
    "35°C",
    "40°C"
  ],
  "colors": [
    "blue",
    "cyan",
    "green",
    "yellow",
    "red"
  ]
};

// Add the legend for ESRI LULC to the map
addCategoricalLegend(lulcLegend, lulcDict, 'ESRI Land Use/Land Cover');
Map.add(lulcLegend);

// Add the legend for LST to the map
addCategoricalLegend(lstLegend, lstDict, 'Land Surface Temperature (°C)');
Map.add(lstLegend);

// Add the original LULC and LST layers to the map
Map.addLayer(lulcImage.clip(boundary), {min: 1, max: 9, palette: [
  "#1A5BAB", "#358221", "#87D19E", "#FFDB5C", "#ED022A", "#EDE9E4", "#F2FAFF", "#C8C8C8", "#C6AD8D"
]}, 'Original ESRI LULC 2023', false);

Map.addLayer(lstImage.clip(boundary), lstVisParams, 'Original LST 15.08.2023', true);

// Iterate over each LULC class, clip the images, and add layers to the map
for (var className in lulcClasses) {
  var classValue = lulcClasses[className];
  addLayers(className, classValue);
}

// Center the map on the boundary
Map.centerObject(boundary, 7);

// Generate histograms of LST for each LULC class
for (var className in lulcClasses) {
  var classValue = lulcClasses[className];
  var clippedLULC = clipLULCByClass(lulcImage, classValue);
  var clippedLST = lstImage.updateMask(clippedLULC).clip(boundary);
  
  // Calculate histogram
  var histogram = ui.Chart.image.histogram({
    image: clippedLST,
    region: boundary,
    scale: 30,
    maxPixels: 1e13, // Set maxPixels to a high value
    minBucketWidth: 0.5
  }).setOptions({
    title: 'LST Histogram for ' + className,
    hAxis: {title: 'Temperature (°C)'},
    vAxis: {title: 'Frequency'},
    series: {
      0: {color: lstVisParams.palette[4]} // Color of the histogram bars
    }
  });
  
  // Print histogram
  print(histogram);
}
```

## Acknowledgements

- **Google Earth Engine**: For providing a powerful platform for geospatial analysis.
- **USGS**: For providing Landsat data.
- **ESRI**: For providing the global land cover dataset.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please fork this repository and submit pull requests.

## Contact

For any questions or feedback, please contact [your-email@example.com].

