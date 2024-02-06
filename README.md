![https://terrainknowledge.com/](https://github.com/Terrainknowledge/GEE/assets/7762172/048febda-61a3-4eef-95fb-da8d6b6b8531)



# Sensibilité à la dégradation des sols 



Appui à la mise en œuvre de l’accord de Paris sur le climat en l'Algérie


## Script 1 Loading the Landsat 8 image collection:

```javascript
var dataset = ee
  .ImageCollection("LANDSAT/LC08/C02/T1_L2")
  .filterDate("2021-08-01", "2021-09-01")
  .filterBounds(roi);

// Applies scaling factors.
function applyScaleFactors(image) {
  var opticalBands = image.select("SR_B.").multiply(0.0000275).add(-0.2);
  var thermalBands = image.select("ST_B.*").multiply(0.00341802).add(149.0);
  return image
    .addBands(opticalBands, null, true)
    .addBands(thermalBands, null, true);
}

dataset = dataset.map(applyScaleFactors);

print(dataset);

var visualization = {
  //bands: ['SR_B4', 'SR_B3', 'SR_B2'],
  bands: ["SR_B5", "SR_B4", "SR_B3"],
  min: 0.0,
  max: 0.3,
};

//Map.addLayer(dataset, visualization, 'True Color (432)');

//Map.addLayer(dataset.mean().clip(roi), visualization, 'True Color (432)');

//Map.addLayer(dataset.median().clip(roi), visualization, 'True Color (432)');

var image = dataset.median().clip(roi);

Map.addLayer(dataset.median().clip(roi), visualization, "False Color (543)");

//ndvi = (nir-red)/(nir+red)

var nir = image.select("SR_B5");

var red = image.select("SR_B4");

var ndvi = nir.subtract(red).divide(nir.add(red));

print(ndvi);

ndvi = ndvi.rename("ndvi");

var visualization1 = {
  bands: ["ndvi"],
  min: -1.0,
  max: 1.0,
};

Map.addLayer(ndvi.clip(roi), visualization1, "Vegetation Index");

//Afficher la zone d'étude

var vis_params = {
  color: "red",
  width: 2,
  lineType: "solid",
  fillColor: "00000000",
};

Map.addLayer(roi.style(vis_params), {}, "Study Area");

Map.centerObject(roi);

```
## Script 2 Calculating and visualizing NDVI:

```javascript
var dataset = ee
  .ImageCollection("LANDSAT/LC08/C02/T1_L2")
  .filterDate("2021-08-01", "2021-09-01")
  .filterBounds(roi);

// Applies scaling factors.
function applyScaleFactors(image) {
  var opticalBands = image.select("SR_B.").multiply(0.0000275).add(-0.2);
  var thermalBands = image.select("ST_B.*").multiply(0.00341802).add(149.0);
  return image
    .addBands(opticalBands, null, true)
    .addBands(thermalBands, null, true);
}

dataset = dataset.map(applyScaleFactors);

print(dataset);

var visualization = {
  //bands: ['SR_B4', 'SR_B3', 'SR_B2'],
  bands: ["SR_B5", "SR_B4", "SR_B3"],
  min: 0.0,
  max: 0.3,
};

//Map.addLayer(dataset, visualization, 'True Color (432)');

//Map.addLayer(dataset.mean().clip(roi), visualization, 'True Color (432)');

//Map.addLayer(dataset.median().clip(roi), visualization, 'True Color (432)');

var image = dataset.median().clip(roi);

Map.addLayer(dataset.median().clip(roi), visualization, "False Color (543)");

//ndvi = (nir-red)/(nir+red)

var nir = image.select("SR_B5");

var red = image.select("SR_B4");

var ndvi = nir.subtract(red).divide(nir.add(red));

print(ndvi);

ndvi = ndvi.rename("ndvi");

var visualization1 = {
  bands: ["ndvi"],
  min: -1.0,
  max: 1.0,
};

Map.addLayer(ndvi, visualization1, "Vegetation Index");

var visualization2 = {
  min: 0.082,
  max: 0.384,
  palette:
    "FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301",
};

Map.addLayer(ndvi, visualization2, "Vegetation Index Color");

//Afficher la zone d'étude

var vis_params = {
  color: "red",
  width: 2,
  lineType: "solid",
  fillColor: "00000000",
};

Map.addLayer(roi.style(vis_params), {}, "Study Area");

Map.centerObject(roi);

```
## Script 3 NDVI Visualization with Color Palette and Export to GeoTIFF:

```javascript
var dataset = ee.ImageCollection('LANDSAT/LC08/C02/T1_L2').filterDate('2021-08-01', '2021-09-01').filterBounds(roi);

// Applies scaling factors.
function applyScaleFactors(image) {
  var opticalBands = image.select('SR_B.').multiply(0.0000275).add(-0.2);
  var thermalBands = image.select('ST_B.*').multiply(0.00341802).add(149.0);
  return image.addBands(opticalBands, null, true)
              .addBands(thermalBands, null, true);
}

dataset = dataset.map(applyScaleFactors);

print(dataset)

var visualization = {
  //bands: ['SR_B4', 'SR_B3', 'SR_B2'],
  bands: ['SR_B5', 'SR_B4', 'SR_B3'],
  min: 0.0,
  max: 0.3,
};


//Map.addLayer(dataset, visualization, 'True Color (432)');

//Map.addLayer(dataset.mean().clip(roi), visualization, 'True Color (432)');

//Map.addLayer(dataset.median().clip(roi), visualization, 'True Color (432)');

var image =  dataset.median().clip(roi);

Map.addLayer(dataset.median().clip(roi), visualization, 'False Color (543)');

//ndvi = (nir-red)/(nir+red)

var nir = image.select('SR_B5')

var red = image.select('SR_B4')

var ndvi = nir.subtract(red).divide((nir.add(red)))

print(ndvi)

ndvi = ndvi.rename('ndvi')

var visualization1 = {
  bands: ['ndvi'],
  min: -1.0,
  max: 1.0,
};

Map.addLayer(ndvi, visualization1, 'Vegetation Index');


var visualization2 = {
    min: 0.082,
    max: 0.384,
    palette: 'FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301'
};


Map.addLayer(ndvi, visualization2, 'Vegetation Index Color');


//Afficher la zone d'étude

var vis_params = {
    'color': 'red', 
    'width': 2,
    'lineType': 'solid',
    'fillColor': '00000000',
}

Map.addLayer(roi.style(vis_params), {}, "Study Area")

Map.centerObject(roi)


// Export to GeoTIFF.
Export.image.toDrive(
  {image: ndvi,  
  description: 'ndvi',  
  scale: 30,
  region: roi,
  fileFormat: 'GeoTIFF',
  'maxPixels' : 10000000000000,
  folder: 'LD'
  }); 



//_________________LEGEND_____________________________________________

var legend = ui.Panel({style: {position: 'middle-right',padding: '8px 10px'}});
var legendTitle = ui.Label({value: 'NDVI',style: {fontWeight: 'bold',fontSize: '15px',margin: '5 0 9px 0',padding: '10'}});
legend.add(legendTitle);
var lon = ee.Image.pixelLonLat().select('latitude');
var gradient = lon.multiply((visualization2.max-visualization2.min)/100.0).add(visualization2.min);
var legendImage = gradient.visualize(visualization2);
var panel = ui.Panel({widgets: [ui.Label(visualization2['max'])],});
legend.add(panel);
var thumbnail = ui.Thumbnail({image: legendImage,params: {bbox:'0,0,10,90', dimensions:'20x70'},style: {padding: '1px', position: 'bottom-right'}});
legend.add(thumbnail);
var panel = ui.Panel({widgets: [ui.Label(visualization2['min'])],});legend.add(panel);
Map.add(legend);

```



## Script 4 calculating and displaying NDVI, elevation and slope:

```javascript

var dataset = ee.ImageCollection('LANDSAT/LC08/C02/T1_L2').filterDate('2021-08-01', '2021-09-01').filterBounds(roi);

// Applies scaling factors.
function applyScaleFactors(image) {
  var opticalBands = image.select('SR_B.').multiply(0.0000275).add(-0.2);
  var thermalBands = image.select('ST_B.*').multiply(0.00341802).add(149.0);
  return image.addBands(opticalBands, null, true)
              .addBands(thermalBands, null, true);
}

dataset = dataset.map(applyScaleFactors);

print(dataset)

var visualization = {
  //bands: ['SR_B4', 'SR_B3', 'SR_B2'],
  bands: ['SR_B5', 'SR_B4', 'SR_B3'],
  min: 0.0,
  max: 0.3,
};


//Map.addLayer(dataset, visualization, 'True Color (432)');

//Map.addLayer(dataset.mean().clip(roi), visualization, 'True Color (432)');

//Map.addLayer(dataset.median().clip(roi), visualization, 'True Color (432)');

var image =  dataset.median().clip(roi);

Map.addLayer(dataset.median().clip(roi), visualization, 'False Color (543)');

//ndvi = (nir-red)/(nir+red)

var nir = image.select('SR_B5')

var red = image.select('SR_B4')

var ndvi = nir.subtract(red).divide((nir.add(red)))

print(ndvi)

ndvi = ndvi.rename('ndvi')

var visualization1 = {
  bands: ['ndvi'],
  min: -1.0,
  max: 1.0,
};

Map.addLayer(ndvi, visualization1, 'Vegetation Index');


var visualization2 = {min: 0.082, max: 0.384, palette: 'FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301'};


Map.addLayer(ndvi, visualization2, 'Vegetation Index Color');


//Afficher la zone d'étude

var vis_params = {
    'color': 'red', 
    'width': 2,
    'lineType': 'solid',
    'fillColor': '00000000',
}

Map.addLayer(roi.style(vis_params), {}, "Study Area")

Map.centerObject(roi)


// Export to GeoTIFF.
Export.image.toDrive(
  {image: ndvi,  
  description: 'ndvi', 
  scale: 30,
  region: roi,
  fileFormat: 'GeoTIFF',
  'maxPixels' : 10000000000000,
  folder: 'LD'
  }); 

//---------------------------------------------------------------------

var dataset = ee.Image('CGIAR/SRTM90_V4');

var elevation = dataset.select('elevation');

var visualization3 = {
    min: 0,
    max: 1400, 
    palette: 'FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301'
};

Map.addLayer(elevation.clip(roi), visualization3, 'elevation');

//Map.addLayer(elevation.clip(roi), {min: 0, max: 1400}, 'elevation');

var slope = ee.Terrain.slope(elevation);

//Map.addLayer(slope, {min: 0, max: 60}, 'slope');

Map.addLayer(slope.clip(roi), {min: 0, max: 11}, 'slope');


//_________________LEGEND_____________________________________________


var legend = ui.Panel({style: {position: 'middle-right',padding: '8px 10px'}});
var legendTitle = ui.Label({value: 'Elevation',style: {fontWeight: 'bold',fontSize: '15px',margin: '5 0 9px 0',padding: '10'}});
legend.add(legendTitle);
var lon = ee.Image.pixelLonLat().select('latitude');
var gradient = lon.multiply((visualization3.max-visualization3.min)/100.0).add(visualization3.min);
var legendImage = gradient.visualize(visualization3);
var panel = ui.Panel({widgets: [ui.Label(visualization3['max'])],});
legend.add(panel);
var thumbnail = ui.Thumbnail({image: legendImage,params: {bbox:'0,0,10,90', dimensions:'20x70'},style: {padding: '1px', position: 'bottom-right'}});
legend.add(thumbnail);
var panel = ui.Panel({widgets: [ui.Label(visualization3['min'])],});legend.add(panel);
Map.add(legend);
```

## Script 05

```javascript

var dataset = ee
  .ImageCollection("LANDSAT/LC08/C02/T1_L2")
  .filterDate("2021-08-01", "2021-09-01")
  .filterBounds(roi);

// Applies scaling factors.
function applyScaleFactors(image) {
  var opticalBands = image.select("SR_B.").multiply(0.0000275).add(-0.2);
  var thermalBands = image.select("ST_B.*").multiply(0.00341802).add(149.0);
  return image
    .addBands(opticalBands, null, true)
    .addBands(thermalBands, null, true);
}

dataset = dataset.map(applyScaleFactors);

print(dataset);

var visualization = {
  //bands: ['SR_B4', 'SR_B3', 'SR_B2'],
  bands: ["SR_B5", "SR_B4", "SR_B3"],
  min: 0.0,
  max: 0.3,
};

//Map.addLayer(dataset, visualization, 'True Color (432)');

//Map.addLayer(dataset.mean().clip(roi), visualization, 'True Color (432)');

//Map.addLayer(dataset.median().clip(roi), visualization, 'True Color (432)');

var image = dataset.median().clip(roi);

Map.addLayer(dataset.median().clip(roi), visualization, "False Color (543)");

//ndvi = (nir-red)/(nir+red)

var nir = image.select("SR_B5");

var red = image.select("SR_B4");

var ndvi = nir.subtract(red).divide(nir.add(red));

print(ndvi);

ndvi = ndvi.rename("ndvi");

var visualization1 = {
  bands: ["ndvi"],
  min: -1.0,
  max: 1.0,
};

Map.addLayer(ndvi, visualization1, "Vegetation Index");

var visualization2 = {
  min: 0.082,
  max: 0.384,
  palette:
    "FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301",
};

Map.addLayer(ndvi, visualization2, "Vegetation Index Color");

//Afficher la zone d'étude

var vis_params = {
  color: "red",
  width: 2,
  lineType: "solid",
  fillColor: "00000000",
};

Map.addLayer(roi.style(vis_params), {}, "Study Area");

Map.centerObject(roi);

// Export to GeoTIFF.
Export.image.toDrive({
  image: ndvi,
  description: "ndvi",
  scale: 30,
  region: roi,
  fileFormat: "GeoTIFF",
  maxPixels: 10000000000000,
  folder: "LD",
});

//---------------------------------------------------------------------

var dataset = ee.Image("CGIAR/SRTM90_V4");

var elevation = dataset.select("elevation");

var visualization3 = {
  min: 0,
  max: 1400,
  palette:
    "FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301",
};

Map.addLayer(elevation.clip(roi), visualization3, "elevation");

//Map.addLayer(elevation.clip(roi), {min: 0, max: 1400}, 'elevation');

var slope = ee.Terrain.slope(elevation);

//Map.addLayer(slope, {min: 0, max: 60}, 'slope');

Map.addLayer(slope.clip(roi), { min: 0, max: 11 }, "slope");

//_________________LEGEND_____________________________________________

var legend = ui.Panel({
  style: { position: "middle-right", padding: "8px 10px" },
});
var legendTitle = ui.Label({
  value: "Elevation",
  style: {
    fontWeight: "bold",
    fontSize: "15px",
    margin: "5 0 9px 0",
    padding: "10",
  },
});
legend.add(legendTitle);
var lon = ee.Image.pixelLonLat().select("latitude");
var gradient = lon
  .multiply((visualization3.max - visualization3.min) / 100.0)
  .add(visualization3.min);
var legendImage = gradient.visualize(visualization3);
var panel = ui.Panel({ widgets: [ui.Label(visualization3["max"])] });
legend.add(panel);
var thumbnail = ui.Thumbnail({
  image: legendImage,
  params: { bbox: "0,0,10,90", dimensions: "20x70" },
  style: { padding: "1px", position: "bottom-right" },
});
legend.add(thumbnail);
var panel = ui.Panel({ widgets: [ui.Label(visualization3["min"])] });
legend.add(panel);
Map.add(legend);

```
## Script 06

```javascript
var dataset = ee.ImageCollection('LANDSAT/LC08/C02/T1_L2').filterDate('2021-08-01', '2021-09-01').filterBounds(roi);

// Applies scaling factors.
function applyScaleFactors(image) {
  var opticalBands = image.select('SR_B.').multiply(0.0000275).add(-0.2);
  var thermalBands = image.select('ST_B.*').multiply(0.00341802).add(149.0);
  return image.addBands(opticalBands, null, true)
              .addBands(thermalBands, null, true);
}

dataset = dataset.map(applyScaleFactors);

print(dataset)

var visualization = {
  //bands: ['SR_B4', 'SR_B3', 'SR_B2'],
  bands: ['SR_B5', 'SR_B4', 'SR_B3'],
  min: 0.0,
  max: 0.3,
};


//Map.addLayer(dataset, visualization, 'True Color (432)');

//Map.addLayer(dataset.mean().clip(roi), visualization, 'True Color (432)');

//Map.addLayer(dataset.median().clip(roi), visualization, 'True Color (432)');

var image =  dataset.median().clip(roi);

Map.addLayer(dataset.median().clip(roi), visualization, 'False Color (543)');

//ndvi = (nir-red)/(nir+red)

var nir = image.select('SR_B5')

var red = image.select('SR_B4')

var ndvi = nir.subtract(red).divide((nir.add(red)))

print(ndvi)

ndvi = ndvi.rename('ndvi')

var visualization1 = {
  bands: ['ndvi'],
  min: -1.0,
  max: 1.0,
};

Map.addLayer(ndvi, visualization1, 'Vegetation Index');


var visualization2 = {
    min: 0.082, 
    max: 0.384, 
    palette: 'FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301'
};


Map.addLayer(ndvi, visualization2, 'Vegetation Index Color');


//Afficher la zone d'étude

var vis_params = {
    'color': 'red', 
    'width': 2,
    'lineType': 'solid',
    'fillColor': '00000000',
}

Map.addLayer(roi.style(vis_params), {}, "Study Area")

Map.centerObject(roi)


// Export to GeoTIFF.
Export.image.toDrive(
  {image: ndvi, 
  description: 'ndvi', 
  scale: 30,
  region: roi,
  fileFormat: 'GeoTIFF',
  'maxPixels' : 10000000000000,
  folder: 'LD'
  }); 

//---------------------------------------------------------------------

var dataset = ee.Image('CGIAR/SRTM90_V4');

var elevation = dataset.select('elevation');

var visualization3 = {
    min: 0, 
    max: 1400, 
    palette: 'FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301'
};

Map.addLayer(elevation.clip(roi), visualization3, 'elevation');

//Map.addLayer(elevation.clip(roi), {min: 0, max: 1400}, 'elevation');

var slope = ee.Terrain.slope(elevation);

//Map.addLayer(slope, {min: 0, max: 60}, 'slope');

Map.addLayer(slope.clip(roi), {min: 0, max: 11}, 'slope');

//--------------------------------------------------------------------

var chirps = ee.ImageCollection('UCSB-CHG/CHIRPS/PENTAD')

var year = 2020

var startDate = ee.Date.fromYMD(year, 1, 1)
var endDate = startDate.advance(1, 'year')
var filtered = chirps.filter(ee.Filter.date(startDate, endDate))
var rainfall = filtered.reduce(ee.Reducer.sum())

var palette = ['#ffffcc','#a1dab4','#41b6c4','#2c7fb8','#253494']

var visualization4 = {
  min:0,
  max: 600,
  palette: palette
}

Map.addLayer(rainfall.clip(roi), visualization4, 'Total Precipitation')


//_________________LEGEND_____________________________________________

var legend = ui.Panel({style: {position: 'middle-right',padding: '8px 10px'}});
var legendTitle = ui.Label({value: 'Rainfall',style: {fontWeight: 'bold',fontSize: '15px',margin: '5 0 9px 0',padding: '10'}});
legend.add(legendTitle);
var lon = ee.Image.pixelLonLat().select('latitude');
var gradient = lon.multiply((visualization4.max-visualization4.min)/100.0).add(visualization4.min);
var legendImage = gradient.visualize(visualization4);
var panel = ui.Panel({widgets: [ui.Label(visualization4['max'])],});
legend.add(panel);
var thumbnail = ui.Thumbnail({image: legendImage,params: {bbox:'0,0,10,90', dimensions:'20x70'},style: {padding: '1px', position: 'bottom-right'}});
legend.add(thumbnail);
var panel = ui.Panel({widgets: [ui.Label(visualization4['min'])],});legend.add(panel);
Map.add(legend);
```

## Script 07

```javascript
var dataset = ee
  .ImageCollection("LANDSAT/LC08/C02/T1_L2")
  .filterDate("2021-08-01", "2021-09-01")
  .filterBounds(roi);

// Applies scaling factors.
function applyScaleFactors(image) {
  var opticalBands = image.select("SR_B.").multiply(0.0000275).add(-0.2);
  var thermalBands = image.select("ST_B.*").multiply(0.00341802).add(149.0);
  return image
    .addBands(opticalBands, null, true)
    .addBands(thermalBands, null, true);
}

dataset = dataset.map(applyScaleFactors);

print(dataset);

var visualization = {
  //bands: ['SR_B4', 'SR_B3', 'SR_B2'],
  bands: ["SR_B5", "SR_B4", "SR_B3"],
  min: 0.0,
  max: 0.3,
};

//Map.addLayer(dataset, visualization, 'True Color (432)');

//Map.addLayer(dataset.mean().clip(roi), visualization, 'True Color (432)');

//Map.addLayer(dataset.median().clip(roi), visualization, 'True Color (432)');

var image = dataset.median().clip(roi);

Map.addLayer(dataset.median().clip(roi), visualization, "False Color (543)");

//ndvi = (nir-red)/(nir+red)

var nir = image.select("SR_B5");

var red = image.select("SR_B4");

var ndvi = nir.subtract(red).divide(nir.add(red));

print(ndvi);

ndvi = ndvi.rename("ndvi");

var visualization1 = {
  bands: ["ndvi"],
  min: -1.0,
  max: 1.0,
};

Map.addLayer(ndvi, visualization1, "Vegetation Index");

var visualization2 = {
  min: 0.082,
  max: 0.384,
  palette:
    "FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301",
};

Map.addLayer(ndvi, visualization2, "Vegetation Index Color");

//Afficher la zone d'étude

var vis_params = {
  color: "red",
  width: 2,
  lineType: "solid",
  fillColor: "00000000",
};

Map.addLayer(roi.style(vis_params), {}, "Study Area");

Map.centerObject(roi);

// Export to GeoTIFF.
Export.image.toDrive({
  image: ndvi,
  description: "ndvi",
  scale: 30,
  region: roi,
  fileFormat: "GeoTIFF",
  maxPixels: 10000000000000,
  folder: "LD",
});

//---------------------------------------------------------------------

var dataset = ee.Image("CGIAR/SRTM90_V4");

var elevation = dataset.select("elevation");

var visualization3 = {
  min: 0,
  max: 1400,
  palette:
    "FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301",
};

Map.addLayer(elevation.clip(roi), visualization3, "elevation");

//Map.addLayer(elevation.clip(roi), {min: 0, max: 1400}, 'elevation');

var slope = ee.Terrain.slope(elevation);

//Map.addLayer(slope, {min: 0, max: 60}, 'slope');

Map.addLayer(slope.clip(roi), { min: 0, max: 11 }, "slope");

//--------------------------------------------------------------------

var chirps = ee.ImageCollection("UCSB-CHG/CHIRPS/PENTAD");

var year = 2020;

var startDate = ee.Date.fromYMD(year, 1, 1);
var endDate = startDate.advance(1, "year");
var filtered = chirps.filter(ee.Filter.date(startDate, endDate));
var rainfall = filtered.reduce(ee.Reducer.sum());

var palette = ["#ffffcc", "#a1dab4", "#41b6c4", "#2c7fb8", "#253494"];

var visualization4 = {
  min: 0,
  max: 600,
  palette: palette,
};

Map.addLayer(rainfall.clip(roi), visualization4, "Total Precipitation");

//---------------------------------------------------------------------
/*
0	No data
10	Cropland rainfed
11	Herbaceous cover
12	Tree or shrub cover
20	Cropland irrigated or post-flooding
30	Mosaic cropland (>50%) / natural vegetation (tree shrub herbaceous cover) (<50%)
40	Mosaic natural vegetation (tree shrub herbaceous cover) (>50%) / cropland (<50%)
50	Tree cover broadleaved evergreen closed to open (>15%)
60	Tree cover broadleaved deciduous closed to open (>15%)
61	Tree cover broadleaved deciduous closed (>40%)
62	Tree cover broadleaved deciduous open (15-40%)
70	Tree cover needleleaved evergreen closed to open (>15%)
71	Tree cover needleleaved evergreen closed (>40%)
72	Tree cover needleleaved evergreen open (15-40%)
80	Tree cover needleleaved deciduous closed to open (>15%)
81	Tree cover needleleaved deciduous closed (>40%)
82	Tree cover needleleaved deciduous open (15-40%)
90	Tree cover mixed leaf type (broadleaved and needleleaved)
100	Mosaic tree and shrub (>50%) / herbaceous cover (<50%)
160	Tree cover flooded fresh or brackish water
170	Tree cover flooded saline water
110	Mosaic herbaceous cover (>50%) / tree and shrub (<50%)
130	Grassland
180	Shrub or herbaceous cover flooded fresh/saline/brackish water
190	Urban areas
120	Shrubland
121	Shrubland evergreen
122	Shrubland deciduous
140	Lichens and mosses
150	Sparse vegetation (tree shrub herbaceous cover) (<15%)
151	Sparse tree (<15%)
152	Sparse shrub (<15%)
153	Sparse herbaceous cover (<15%)
200	Bare areas
201	Consolidated bare areas
202	Unconsolidated bare areas
210	Water bodies
220	Permanent snow and ice
*/

var esa2015 = ee.Image("users/djerririk/ESA2015");

//_________________LEGEND_____________________________________________

var legend = ui.Panel({
  style: { position: "middle-right", padding: "8px 10px" },
});
var legendTitle = ui.Label({
  value: "Rainfall",
  style: {
    fontWeight: "bold",
    fontSize: "15px",
    margin: "5 0 9px 0",
    padding: "10",
  },
});
legend.add(legendTitle);
var lon = ee.Image.pixelLonLat().select("latitude");
var gradient = lon
  .multiply((visualization4.max - visualization4.min) / 100.0)
  .add(visualization4.min);
var legendImage = gradient.visualize(visualization4);
var panel = ui.Panel({ widgets: [ui.Label(visualization4["max"])] });
legend.add(panel);
var thumbnail = ui.Thumbnail({
  image: legendImage,
  params: { bbox: "0,0,10,90", dimensions: "20x70" },
  style: { padding: "1px", position: "bottom-right" },
});
legend.add(thumbnail);
var panel = ui.Panel({ widgets: [ui.Label(visualization4["min"])] });
legend.add(panel);
Map.add(legend);

```

## Script 08

```javascript
var dataset = ee
  .ImageCollection("LANDSAT/LC08/C02/T1_L2")
  .filterDate("2021-08-01", "2021-09-01")
  .filterBounds(roi);

// Applies scaling factors.
function applyScaleFactors(image) {
  var opticalBands = image.select("SR_B.").multiply(0.0000275).add(-0.2);
  var thermalBands = image.select("ST_B.*").multiply(0.00341802).add(149.0);
  return image
    .addBands(opticalBands, null, true)
    .addBands(thermalBands, null, true);
}

dataset = dataset.map(applyScaleFactors);

print(dataset);

var visualization = {
  //bands: ['SR_B4', 'SR_B3', 'SR_B2'],
  bands: ["SR_B5", "SR_B4", "SR_B3"],
  min: 0.0,
  max: 0.3,
};

//Map.addLayer(dataset, visualization, 'True Color (432)');

//Map.addLayer(dataset.mean().clip(roi), visualization, 'True Color (432)');

//Map.addLayer(dataset.median().clip(roi), visualization, 'True Color (432)');

var image = dataset.median().clip(roi);

Map.addLayer(dataset.median().clip(roi), visualization, "False Color (543)");

//ndvi = (nir-red)/(nir+red)

var nir = image.select("SR_B5");

var red = image.select("SR_B4");

var ndvi = nir.subtract(red).divide(nir.add(red));

print(ndvi);

ndvi = ndvi.rename("ndvi");

var visualization1 = {
  bands: ["ndvi"],
  min: -1.0,
  max: 1.0,
};

Map.addLayer(ndvi, visualization1, "Vegetation Index");

var visualization2 = {
  min: 0.082,
  max: 0.384,
  palette:
    "FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301",
};

Map.addLayer(ndvi, visualization2, "Vegetation Index Color");

//Afficher la zone d'étude

var vis_params = {
  color: "red",
  width: 2,
  lineType: "solid",
  fillColor: "00000000",
};

Map.addLayer(roi.style(vis_params), {}, "Study Area");

Map.centerObject(roi);

// Export to GeoTIFF.
Export.image.toDrive({
  image: ndvi,
  description: "ndvi",
  scale: 30,
  region: roi,
  fileFormat: "GeoTIFF",
  maxPixels: 10000000000000,
  folder: "LD",
});

//---------------------------------------------------------------------

var dataset = ee.Image("CGIAR/SRTM90_V4");

var elevation = dataset.select("elevation");

var visualization3 = {
  min: 0,
  max: 1400,
  palette:
    "FFFFFF, CE7E45, DF923D, F1B555, FCD163, 99B718, 74A901, 66A000, 529400, 3E8601, 207401, 056201, 004C00, 023B01,012E01, 011D01, 011301",
};

Map.addLayer(elevation.clip(roi), visualization3, "elevation");

//Map.addLayer(elevation.clip(roi), {min: 0, max: 1400}, 'elevation');

var slope = ee.Terrain.slope(elevation);

//Map.addLayer(slope, {min: 0, max: 60}, 'slope');

Map.addLayer(slope.clip(roi), { min: 0, max: 11 }, "slope");

//--------------------------------------------------------------------

var chirps = ee.ImageCollection("UCSB-CHG/CHIRPS/PENTAD");

var year = 2020;

var startDate = ee.Date.fromYMD(year, 1, 1);
var endDate = startDate.advance(1, "year");
var filtered = chirps.filter(ee.Filter.date(startDate, endDate));
var rainfall = filtered.reduce(ee.Reducer.sum());

var palette = ["#ffffcc", "#a1dab4", "#41b6c4", "#2c7fb8", "#253494"];

var visualization4 = {
  min: 0,
  max: 600,
  palette: palette,
};

Map.addLayer(rainfall.clip(roi), visualization4, "Total Precipitation");

//---------------------------------------------------------------------
/*
0	No data
10	Cropland rainfed
11	Herbaceous cover
12	Tree or shrub cover
20	Cropland irrigated or post-flooding
30	Mosaic cropland (>50%) / natural vegetation (tree shrub herbaceous cover) (<50%)
40	Mosaic natural vegetation (tree shrub herbaceous cover) (>50%) / cropland (<50%)
50	Tree cover broadleaved evergreen closed to open (>15%)
60	Tree cover broadleaved deciduous closed to open (>15%)
61	Tree cover broadleaved deciduous closed (>40%)
62	Tree cover broadleaved deciduous open (15-40%)
70	Tree cover needleleaved evergreen closed to open (>15%)
71	Tree cover needleleaved evergreen closed (>40%)
72	Tree cover needleleaved evergreen open (15-40%)
80	Tree cover needleleaved deciduous closed to open (>15%)
81	Tree cover needleleaved deciduous closed (>40%)
82	Tree cover needleleaved deciduous open (15-40%)
90	Tree cover mixed leaf type (broadleaved and needleleaved)
100	Mosaic tree and shrub (>50%) / herbaceous cover (<50%)
160	Tree cover flooded fresh or brackish water
170	Tree cover flooded saline water
110	Mosaic herbaceous cover (>50%) / tree and shrub (<50%)
130	Grassland
180	Shrub or herbaceous cover flooded fresh/saline/brackish water
190	Urban areas
120	Shrubland
121	Shrubland evergreen
122	Shrubland deciduous
140	Lichens and mosses
150	Sparse vegetation (tree shrub herbaceous cover) (<15%)
151	Sparse tree (<15%)
152	Sparse shrub (<15%)
153	Sparse herbaceous cover (<15%)
200	Bare areas
201	Consolidated bare areas
202	Unconsolidated bare areas
210	Water bodies
220	Permanent snow and ice
*/

var esa2015 = ee.Image("users/djerririk/ESA2015");

var freqHist = ee.Dictionary(
  // Calculate frequency histogram
  esa2015.reduceRegion({
    reducer: ee.Reducer.frequencyHistogram(),
    geometry: roi,
    scale: 300,
    maxPixels: 1e13,
  })
);

print(freqHist);

/*
10	Cropland rainfed
11	Herbaceous cover
30	Mosaic cropland (>50%) / natural vegetation (tree shrub herbaceous cover) (<50%)
40	Mosaic natural vegetation (tree shrub herbaceous cover) (>50%) / cropland (<50%)
60	Tree cover broadleaved deciduous closed to open (>15%)
70	Tree cover needleleaved evergreen closed to open (>15%)
100	Mosaic tree and shrub (>50%) / herbaceous cover (<50%)
110	Mosaic herbaceous cover (>50%) / tree and shrub (<50%)
120	Shrubland
150	Sparse vegetation (tree shrub herbaceous cover) (<15%)
153	Sparse herbaceous cover (<15%)
190	Urban areas
200	Bare areas
*/

var classif = esa2015.clip(roi);

classif = classif.remap(
  [10, 11, 30, 40, 60, 70, 100, 110, 120, 150, 153, 190, 200],
  [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
);

Map.addLayer(
  classif,
  {
    min: 0,
    max: 12,
    palette: [
      "f90606",
      "96f906",
      "76f906",
      "f98d06",
      "c1f906",
      "40f906",
      "f9df06",
      "2f6d00",
      "09ee66",
      "c6fd00",
      "bed823",
      "0546f4",
      "fcff02",
    ],
  },
  "LULC Map ESA 2015 (300m)"
);

//_________________LEGEND_____________________________________________
/*
var legend = ui.Panel({style: {position: 'middle-right',padding: '8px 10px'}});
var legendTitle = ui.Label({value: 'Rainfall',style: {fontWeight: 'bold',fontSize: '15px',margin: '5 0 9px 0',padding: '10'}});
legend.add(legendTitle);
var lon = ee.Image.pixelLonLat().select('latitude');
var gradient = lon.multiply((visualization4.max-visualization4.min)/100.0).add(visualization4.min);
var legendImage = gradient.visualize(visualization4);
var panel = ui.Panel({widgets: [ui.Label(visualization4['max'])],});
legend.add(panel);
var thumbnail = ui.Thumbnail({image: legendImage,params: {bbox:'0,0,10,90', dimensions:'20x70'},style: {padding: '1px', position: 'bottom-right'}});
legend.add(thumbnail);
var panel = ui.Panel({widgets: [ui.Label(visualization4['min'])],});legend.add(panel);
Map.add(legend);
*/

// set position of panel
var legend = ui.Panel({
  style: {
    position: "bottom-left",
    padding: "8px 15px",
  },
});

// Create legend title
var legendTitle = ui.Label({
  value: "Classification Legend",
  style: {
    fontWeight: "bold",
    fontSize: "18px",
    margin: "0 0 4px 0",
    padding: "0",
  },
});

// Add the title to the panel
legend.add(legendTitle);

// Creates and styles 1 row of the legend.
var makeRow = function (color, name) {
  // Create the label that is actually the colored box.
  var colorBox = ui.Label({
    style: {
      //backgroundColor: '#' + color,
      backgroundColor: "#" + color,
      // Use padding to give the box height and width.
      padding: "8px",
      margin: "0 0 4px 0",
    },
  });

  // Create the label filled with the description text.
  var description = ui.Label({
    value: name,
    style: { margin: "0 0 4px 6px" },
  });

  // return the panel
  return ui.Panel({
    widgets: [colorBox, description],
    layout: ui.Panel.Layout.Flow("horizontal"),
  });
};

//  Palette with the colors
var palette = [
  "f90606",
  "96f906",
  "76f906",
  "f98d06",
  "c1f906",
  "40f906",
  "f9df06",
  "2f6d00",
  "09ee66",
  "c6fd00",
  "bed823",
  "0546f4",
  "fcff02",
];

// name of the legend
var names = [
  "10	Cropland rainfed",
  "11	Herbaceous cover",
  "30	Mosaic cropland (>50%) / natural vegetation (tree shrub herbaceous cover) (<50%)",
  "40	Mosaic natural vegetation (tree shrub herbaceous cover) (>50%) / cropland (<50%)",
  "60	Tree cover broadleaved deciduous closed to open (>15%)",
  "70	Tree cover needleleaved evergreen closed to open (>15%)",
  "100	Mosaic tree and shrub (>50%) / herbaceous cover (<50%)",
  "110	Mosaic herbaceous cover (>50%) / tree and shrub (<50%)",
  "120	Shrubland",
  "150	Sparse vegetation (tree shrub herbaceous cover) (<15%)",
  "153	Sparse herbaceous cover (<15%)",
  "190	Urban areas",
  "200	Bare areas",
];

// Add color and and names
for (var i = 0; i < 12; i++) {
  legend.add(makeRow(palette[i], names[i]));
}

// add legend to map (alternatively you can also print the legend to the console)
Map.add(legend);

```
