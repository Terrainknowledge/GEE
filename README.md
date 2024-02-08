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

## Script 5 Landsat 8 NDVI and Elevation Analysis with Legends

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
## Script 6 Multispectral Analysis and Land Characteristics Mapping:

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

## Script 7 Multi-Layer Analysis: NDVI, Elevation, Rainfall, and Land Cover:

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

## Script 8 Multispectral Analysis and Land Cover:

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
# VQI Vegetation Quality Index

## Script 9 Fire risk:


```javascript
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
var roi = ee.FeatureCollection([
  ee.Feature(
    ee.Geometry.Polygon(
      [
        [
          [2.9390799818834257, 36.40214349190413],
          [2.9390799818834257, 35.865329293470964],
          [3.8893973646959257, 35.865329293470964],
          [3.8893973646959257, 36.40214349190413],
        ],
      ],
      null,
      false
    ),
    {
      "system:index": "0",
    }
  ),
]);
var esa2015 = ee.Image("users/djerririk/ESA2015").clip(roi);

print(esa2015);

Map.addLayer(esa2015);

var DR = esa2015.remap(
  [
    62, 121, 30, 70, 100, 10, 11, 20, 40, 60, 110, 120, 122, 50, 61, 71, 80, 81,
    82, 130, 150, 153, 12, 72, 151, 152, 90, 170, 180, 140, 160, 200, 201, 202,
    190,
  ],
  [
    1.7, 1.6, 1.5, 1.5, 1.5, 1.4, 1.4, 1.4, 1.4, 1.4, 1.4, 1.4, 1.4, 1.3, 1.3,
    1.3, 1.3, 1.3, 1.3, 1.3, 1.3, 1.3, 1.2, 1.2, 1.2, 1.2, 1.1, 1.1, 1.1, 1.0,
    1.0, 1.0, 1.0, 1.0, 1.0,
  ]
);

var palette = [
  "#ff6961",
  "#ffb480",
  "#f8f38d",
  "#42d6a4",
  "#08cad1",
  "#59adf6",
  "#9d94ff",
  "#c780e8",
];

var visParams = {
  min: 1.0,
  max: 1.7,
  palette: palette,
};

print(DR);

Map.addLayer(DR, visParams, "Drought Resistance");

Map.centerObject(roi);

// set styling
var styling1 = { color: "red", fillColor: "00000000" };

Map.addLayer(roi.style(styling1));

//_________________LEGEND_____________________________________________

var legend = ui.Panel({
  style: { position: "middle-right", padding: "8px 10px" },
});
var legendTitle = ui.Label({
  value: "Fire Risk",
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
  .multiply((visParams.max - visParams.min) / 100.0)
  .add(visParams.min);

var legendImage = gradient.visualize(visParams);
var panel = ui.Panel({ widgets: [ui.Label(visParams["max"])] });

legend.add(panel);

var thumbnail = ui.Thumbnail({
  image: legendImage,
  params: { bbox: "0,0,10,90", dimensions: "20x70" },
  style: { padding: "1px", position: "bottom-right" },
});

legend.add(thumbnail);

var panel = ui.Panel({ widgets: [ui.Label(visParams["min"])] });

legend.add(panel);

Map.add(legend);

```

## Script 10 Plant cover:
```javascript
var roi = ee.FeatureCollection([
  ee.Feature(
    ee.Geometry.Polygon(
      [
        [
          [2.9390799818834257, 36.40214349190413],
          [2.9390799818834257, 35.865329293470964],
          [3.8893973646959257, 35.865329293470964],
          [3.8893973646959257, 36.40214349190413],
        ],
      ],
      null,
      false
    ),
    {
      "system:index": "0",
    }
  ),
]);

// modis NDVI
var modisNDVI = ee.ImageCollection('MODIS/006/MOD13Q1').filterBounds(roi)
              .select('NDVI')
              .map(function(img){
                        var rescaled_NDVI = img.select('NDVI')
                                                .multiply(0.0001)
                                                .rename('NDVI_rescaled');
                        return img.addBands(rescaled_NDVI);}); 

//make a list of years and filter the modisNDVI  
var ndviMean = ee.List.sequence(2013, 2023, 1).map(function(year){
  var start = ee.Number(year);
  var end = start.add(ee.Number(1));
  var ndviMeanYear = modisNDVI.select('NDVI_rescaled')
                              .filter(ee.Filter.calendarRange(start, end, 'year')).mean();
  return ndviMeanYear.set('year', start)
});

print(ndviMean.get(0))

ndviMean = ee.Image(ndviMean.get(0))


var palette = ['CE7E45', 'DF923D', 'F1B555', 'FCD163', '99B718', '74A901',  '66A000', '529400', '3E8601', '207401', '056201', '004C00', '023B01',     '012E01', '011D01', '011301']

var visParams = {
  min:0.0,
  max: 0.7,
  palette: palette
}

//Map.addLayer(ndviMean, visParams, 'NDVI MODIS')


//------------------------------------------------------------------------------------------------------------------------------------

// Remap values.
var PC = ee.Image(1)
      .where(ndviMean.gt(-1.0).and(ndviMean.lte(0.1)), 2.00)
      .where(ndviMean.gt(0.1).and(ndviMean.lte(0.11)), 1.90)
      .where(ndviMean.gt(0.11).and(ndviMean.lte(0.13)), 1.80)
      .where(ndviMean.gt(0.13).and(ndviMean.lte(0.18)), 1.70)
      .where(ndviMean.gt(0.18).and(ndviMean.lte(0.26)), 1.60)
      .where(ndviMean.gt(0.26).and(ndviMean.lte(0.38)), 1.50)
      .where(ndviMean.gt(0.38).and(ndviMean.lte(0.50)), 1.40)
      .where(ndviMean.gt(0.50).and(ndviMean.lte(0.62)), 1.30)
      .where(ndviMean.gt(0.62).and(ndviMean.lte(0.72)), 1.20)
      .where(ndviMean.gt(0.72).and(ndviMean.lte(0.80)), 1.10)
      .where(ndviMean.gt(0.80).and(ndviMean.lte(1.0)), 1.00)
      .rename('PC')
      

print(PC)


var palette = ['CE7E45', 'DF923D', 'F1B555', 'FCD163', '99B718', '74A901',  '66A000', '529400', '3E8601', '207401', '056201']

var visParams = {
  min:1.0,
  max: 2.0,
  palette: palette
}

Map.addLayer(PC, visParams, 'Plant Cover (PC)')



Map.centerObject(roi)

// set styling
var styling1 = {color: 'red', fillColor: '00000000'};

Map.addLayer(roi.style(styling1))


//_________________LEGEND_____________________________________________

var legend = ui.Panel({style: {position: 'middle-right',padding: '8px 10px'}});

var legendTitle = ui.Label({value: 'mean ndvi 2013-2023',style: {fontWeight: 'bold',fontSize: '15px',margin: '5 0 9px 0',padding: '10'}});

legend.add(legendTitle);

var lon = ee.Image.pixelLonLat().select('latitude');

var gradient = lon.multiply((visParams.max-visParams.min)/100.0).add(visParams.min);

var legendImage = gradient.visualize(visParams);

var panel = ui.Panel({widgets: [ui.Label(visParams['max'])],});

legend.add(panel);

var thumbnail = ui.Thumbnail({image: legendImage,params: {bbox:'0,0,10,90', dimensions:'20x70'},style: {padding: '1px', position: 'bottom-right'}});

legend.add(thumbnail);

var panel = ui.Panel({widgets: [ui.Label(visParams['min'])],});legend.add(panel);

Map.add(legend);

```

## Script 11 Erosion protection:


```javascript
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

var roi = ee.FeatureCollection([
  ee.Feature(
    ee.Geometry.Polygon(
      [
        [
          [2.9390799818834257, 36.40214349190413],
          [2.9390799818834257, 35.865329293470964],
          [3.8893973646959257, 35.865329293470964],
          [3.8893973646959257, 36.40214349190413],
        ],
      ],
      null,
      false
    ),
    {
      "system:index": "0",
    }
  ),
]);

var esa2015 = ee.Image("users/djerririk/ESA2015");
print(esa2015);

Map.addLayer(esa2015);

var DR = esa2015.remap(
  [
    200, 201, 202, 190, 11, 10, 150, 153, 12, 120, 122, 151, 152, 30, 81, 82,
    20, 40, 62, 130, 72, 80, 110,
  ],
  [
    2.0, 2.0, 2.0, 2.0, 1.8, 1.7, 1.7, 1.7, 1.6, 1.6, 1.6, 1.6, 1.6, 1.5, 1.5,
    1.5, 1.4, 1.4, 1.4, 1.4, 1.3, 1.3, 1.3,
  ]
);

var palette = [
  "#ff6961",
  "#ffb480",
  "#f8f38d",
  "#42d6a4",
  "#08cad1",
  "#59adf6",
  "#9d94ff",
];

var visParams = {
  min: 1.3,
  max: 2.0,
  palette: palette,
};

print(DR);

Map.addLayer(DR, visParams, "Erosion Protection");

Map.centerObject(roi);

// set styling
var styling1 = { color: "red", fillColor: "00000000" };

Map.addLayer(roi.style(styling1));

//_________________LEGEND_____________________________________________

var legend = ui.Panel({
  style: { position: "middle-right", padding: "8px 10px" },
});

var legendTitle = ui.Label({
  value: "Erosion Protection",
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
  .multiply((visParams.max - visParams.min) / 100.0)
  .add(visParams.min);

var legendImage = gradient.visualize(visParams);

var panel = ui.Panel({ widgets: [ui.Label(visParams["max"])] });

legend.add(panel);

var thumbnail = ui.Thumbnail({
  image: legendImage,
  params: { bbox: "0,0,10,90", dimensions: "20x70" },
  style: { padding: "1px", position: "bottom-right" },
});

legend.add(thumbnail);

var panel = ui.Panel({ widgets: [ui.Label(visParams["min"])] });

legend.add(panel);

Map.add(legend);

```

## Script 12 Drought resistance:


```javascript
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

var roi = ee.FeatureCollection([
  ee.Feature(
    ee.Geometry.Polygon(
      [
        [
          [2.9390799818834257, 36.40214349190413],
          [2.9390799818834257, 35.865329293470964],
          [3.8893973646959257, 35.865329293470964],
          [3.8893973646959257, 36.40214349190413],
        ],
      ],
      null,
      false
    ),
    {
      "system:index": "0",
    }
  ),
]);

var esa2015 = ee.Image("users/djerririk/ESA2015");
print(esa2015);

Map.addLayer(esa2015);

var DR = esa2015.remap(
  [
    200, 201, 202, 190, 11, 130, 150, 153, 10, 30, 40, 120, 151, 152, 20, 100,
    122, 12, 81, 82, 110, 62, 72, 80, 121, 60, 70, 71, 180, 50, 61, 90, 140,
    160, 170,
  ],
  [
    2.0, 2.0, 2.0, 2.0, 1.6, 1.6, 1.6, 1.6, 1.5, 1.5, 1.5, 1.5, 1.5, 1.5, 1.4,
    1.4, 1.4, 1.3, 1.3, 1.3, 1.3, 1.2, 1.2, 1.2, 1.2, 1.1, 1.1, 1.1, 1.1, 1.0,
    1.0, 1.0, 1.0, 1.0, 1.0,
  ]
);

var palette = [
  "#ff6961",
  "#ffb480",
  "#f8f38d",
  "#42d6a4",
  "#08cad1",
  "#59adf6",
  "#9d94ff",
  "#c780e8",
];

var visParams = {
  min: 1.0,
  max: 2.0,
  palette: palette,
};

print(DR);

Map.addLayer(DR, visParams, "Drought Resistance");

Map.centerObject(roi);

// set styling
var styling1 = { color: "red", fillColor: "00000000" };

Map.addLayer(roi.style(styling1));

//_________________LEGEND_____________________________________________

var legend = ui.Panel({
  style: { position: "middle-right", padding: "8px 10px" },
});

var legendTitle = ui.Label({
  value: "Drought Resistance",
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
  .multiply((visParams.max - visParams.min) / 100.0)
  .add(visParams.min);

var legendImage = gradient.visualize(visParams);

var panel = ui.Panel({ widgets: [ui.Label(visParams["max"])] });

legend.add(panel);

var thumbnail = ui.Thumbnail({
  image: legendImage,
  params: { bbox: "0,0,10,90", dimensions: "20x70" },
  style: { padding: "1px", position: "bottom-right" },
});

legend.add(thumbnail);

var panel = ui.Panel({ widgets: [ui.Label(visParams["min"])] });

legend.add(panel);

Map.add(legend);
```

## Script 13 VQI Vegetation Quality Index:


```javascript
//VQI = (FR*EP*DR*PC).POW(1/4)

var roi = ee.FeatureCollection([
  ee.Feature(
    ee.Geometry.Polygon(
      [
        [
          [2.9390799818834257, 36.40214349190413],
          [2.9390799818834257, 35.865329293470964],
          [3.8893973646959257, 35.865329293470964],
          [3.8893973646959257, 36.40214349190413],
        ],
      ],
      null,
      false
    ),
    {
      "system:index": "0",
    }
  ),
]);

var esa2015 = ee.Image("users/djerririk/ESA2015");

Map.addLayer(esa2015);

//---------------------------------------------------------------------

var DR = esa2015
  .remap(
    [
      200, 201, 202, 190, 11, 130, 150, 153, 10, 30, 40, 120, 151, 152, 20, 100,
      122, 12, 81, 82, 110, 62, 72, 80, 121, 60, 70, 71, 180, 50, 61, 90, 140,
      160, 170,
    ],
    [
      2.0, 2.0, 2.0, 2.0, 1.6, 1.6, 1.6, 1.6, 1.5, 1.5, 1.5, 1.5, 1.5, 1.5, 1.4,
      1.4, 1.4, 1.3, 1.3, 1.3, 1.3, 1.2, 1.2, 1.2, 1.2, 1.1, 1.1, 1.1, 1.1, 1.0,
      1.0, 1.0, 1.0, 1.0, 1.0,
    ]
  )
  .rename("DR");

print(DR);

Map.addLayer(DR, {}, "DR");
//---------------------------------------------------------------------

var EP = esa2015
  .remap(
    [
      200, 201, 202, 190, 11, 10, 150, 153, 12, 120, 122, 151, 152, 30, 81, 82,
      20, 40, 62, 130, 72, 80, 110, 100, 121, 170, 180, 60, 70, 140, 50, 61, 71,
      90, 160,
    ],
    [
      2.0, 2.0, 2.0, 2.0, 1.8, 1.7, 1.7, 1.7, 1.6, 1.6, 1.6, 1.6, 1.6, 1.5, 1.5,
      1.5, 1.4, 1.4, 1.4, 1.4, 1.3, 1.3, 1.3, 1.2, 1.2, 1.2, 1.2, 1.1, 1.1, 1.1,
      1.0, 1.0, 1.0, 1.0, 1.0,
    ]
  )
  .rename("EP");

print(EP);

Map.addLayer(EP, {}, "EP");

//---------------------------------------------------------------------
var FR = esa2015
  .remap(
    [
      62, 121, 30, 70, 100, 10, 11, 20, 40, 60, 110, 120, 122, 50, 61, 71, 80,
      81, 82, 130, 150, 153, 12, 72, 151, 152, 90, 170, 180, 140, 160, 200, 201,
      202, 190,
    ],
    [
      1.7, 1.6, 1.5, 1.5, 1.5, 1.4, 1.4, 1.4, 1.4, 1.4, 1.4, 1.4, 1.4, 1.3, 1.3,
      1.3, 1.3, 1.3, 1.3, 1.3, 1.3, 1.3, 1.2, 1.2, 1.2, 1.2, 1.1, 1.1, 1.1, 1.0,
      1.0, 1.0, 1.0, 1.0, 1.0,
    ]
  )
  .rename("FR");
print(FR);

Map.addLayer(FR, {}, "FR");
//---------------------------------------------------------------------

// modis NDVI
var modisNDVI = ee
  .ImageCollection("MODIS/006/MOD13Q1")
  .filterBounds(roi)
  .select("NDVI")
  .map(function (img) {
    var rescaled_NDVI = img
      .select("NDVI")
      .multiply(0.0001)
      .rename("NDVI_rescaled");
    return img.addBands(rescaled_NDVI);
  });

//make a list of years and filter the modisNDVI
var ndviMean = ee.List.sequence(2013, 2023, 1).map(function (year) {
  var start = ee.Number(year);
  var end = start.add(ee.Number(1));
  var ndviMeanYear = modisNDVI
    .select("NDVI_rescaled")
    .filter(ee.Filter.calendarRange(start, end, "year"))
    .mean();
  return ndviMeanYear.set("year", start);
});

ndviMean = ee.Image(ndviMean.get(0));

//------------------------------------------------------------------------------------------------------------------------------------

// Remap values.
var PC = ee
  .Image(1)
  .where(ndviMean.gt(-1.0).and(ndviMean.lte(0.1)), 2.0)
  .where(ndviMean.gt(0.1).and(ndviMean.lte(0.11)), 1.9)
  .where(ndviMean.gt(0.11).and(ndviMean.lte(0.13)), 1.8)
  .where(ndviMean.gt(0.13).and(ndviMean.lte(0.18)), 1.7)
  .where(ndviMean.gt(0.18).and(ndviMean.lte(0.26)), 1.6)
  .where(ndviMean.gt(0.26).and(ndviMean.lte(0.38)), 1.5)
  .where(ndviMean.gt(0.38).and(ndviMean.lte(0.5)), 1.4)
  .where(ndviMean.gt(0.5).and(ndviMean.lte(0.62)), 1.3)
  .where(ndviMean.gt(0.62).and(ndviMean.lte(0.72)), 1.2)
  .where(ndviMean.gt(0.72).and(ndviMean.lte(0.8)), 1.1)
  .where(ndviMean.gt(0.8).and(ndviMean.lte(1.0)), 1.0)
  .rename("PC");

print(PC);

Map.addLayer(PC, {}, "PC");

var VQI = FR.multiply(EP)
  .multiply(DR)
  .multiply(PC)
  .pow(ee.Image(0.25))
  .rename("VQI");

print(VQI);

// calculate the min and max value of an image
var minMax = VQI.reduceRegion({
  reducer: ee.Reducer.minMax(),
  geometry: roi,
  scale: 300,
  maxPixels: 10e9,
  tileScale: 16,
});

print("minMax", minMax);

var palette = ["#44ce1b", "#bbdb44", "#f7e379", "#f2a134", "#e51f1f"];

var visParams = {
  min: 1.19,
  max: 1.64,
  palette: palette,
};

Map.addLayer(VQI.clip(roi), visParams, "Vegetation Quality Index (VQI)");

Map.centerObject(roi);

// set styling
var styling1 = { color: "red", fillColor: "00000000" };

Map.addLayer(roi.style(styling1));

//_________________LEGEND_____________________________________________

var legend = ui.Panel({
  style: { position: "middle-right", padding: "8px 10px" },
});

var legendTitle = ui.Label({
  value: "VQI",
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
  .multiply((visParams.max - visParams.min) / 100.0)
  .add(visParams.min);

var legendImage = gradient.visualize(visParams);

var panel = ui.Panel({ widgets: [ui.Label(visParams["max"])] });

legend.add(panel);

var thumbnail = ui.Thumbnail({
  image: legendImage,
  params: { bbox: "0,0,10,90", dimensions: "20x70" },
  style: { padding: "1px", position: "bottom-right" },
});

legend.add(thumbnail);

var panel = ui.Panel({ widgets: [ui.Label(visParams["min"])] });
legend.add(panel);

Map.add(legend);

```
