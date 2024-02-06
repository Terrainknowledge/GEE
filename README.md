## Script 01

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

Map.addLayer(ndvi.clip(roi), visualization1, 'Vegetation Index');

//Afficher la zone d'étude

var vis_params = {
    'color': 'red', 
    'width': 2,
    'lineType': 'solid',
    'fillColor': '00000000',
}

Map.addLayer(roi.style(vis_params), {}, "Study Area")

Map.centerObject(roi)
```

