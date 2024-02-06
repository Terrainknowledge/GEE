![https://terrainknowledge.com/](https://github.com/Terrainknowledge/GEE/assets/7762172/048febda-61a3-4eef-95fb-da8d6b6b8531)



# Sensibilité à la dégradation des sols 



Appui à la mise en œuvre de l’accord de Paris sur le climat en l'Algérie


## Script 01

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
## Script 02

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

## Script 03

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

## Script 04

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
