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

