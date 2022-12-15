# Urban-Mapping-Fall-2022
# The google earth engine code for the final project:
var dataset00 = ee.ImageCollection('LANDSAT/LE07/C01/T1_32DAY_NDVI')
                  .filterDate('2000-01-01', '2000-12-31').select('NDVI').max().clip(NYC);
var dataset21 = ee.ImageCollection('LANDSAT/LE07/C01/T1_32DAY_NDVI')
                  .filterDate('2021-01-01', '2021-12-31').select('NDVI').max().clip(NYC);

var diff = dataset21.subtract(dataset00).gt(0.05)

var dataset = ee.ImageCollection('USDA/NAIP/DOQQ')
                  .filter(ee.Filter.date('2017-01-01', '2018-12-31'));
var trueColor = dataset.select(['R', 'G', 'B']).max().clip(NYC);
var trueColorVis = {
  min: 0.0,
  max: 255.0,
};

Map.addLayer(trueColor, trueColorVis, 'True Color');

var masked = diff.mask(diff)
 


var colorizedVis = {
  min: 0.0,
  max: 1.0,
  palette: [
    'FFFFFF', 'CE7E45', 'DF923D', 'F1B555', 'FCD163', '99B718', '74A901',
    '66A000', '529400', '3E8601', '207401', '056201', '004C00', '023B01',
    '012E01', '011D01', '011301'
  ],
};


//division
//var division = dataset21.divide(dataset00);
//Map.addLayer(division, {min: 0, max: 3}, 'division');


Map.setCenter(-73.9083, 40.7155, 9);
//Map.addLayer(dataset00, colorizedVis, 'Colorized00');
//Map.addLayer(dataset21, colorizedVis, 'Colorized21');
//Map.addLayer(diff, colorizedVis, 'diff');
Map.addLayer(masked, colorizedVis, 'MaskedDiff');

Export.image.toDrive({
  image: masked, 
  scale: 30,
maxPixels: 892227573612
});



## The document for HW1: https://docs.google.com/document/d/1eiPoy8yAKOGVwSU1PNCt3KAOYoIeJ0YLNyF95-33ikI/edit?usp=sharing


