# Snippet to calculate distance between two point
###### This snippet calculate distance between two points (latitude and longitude)
-------------------
### Latitude Longitude
```
37, -122 
(lat,lng)
```

### Km or Miles ?
```
In Below Query 3959 is form calculating distance in miles and 6371 is for kilometers
for Miles = 3959
for KM = 6371
```

### Database structure
 ```
CREATE TABLE `markers` (
  `id` INT NOT NULL AUTO_INCREMENT PRIMARY KEY ,
  `name` VARCHAR( 60 ) NOT NULL ,
  `address` VARCHAR( 80 ) NOT NULL ,
  `lat` FLOAT( 10, 6 ) NOT NULL ,
  `lng` FLOAT( 10, 6 ) NOT NULL
) ENGINE = MYISAM ;
 ```
 
 
### MySQL Query for calculating distance
 ```
 SELECT
	id,
	( 3959 * acos( cos( radians(37) ) * cos( radians( lat ) ) * 
		cos( radians( lng ) - 
		radians(-122) ) + 
		sin( radians(37) ) * 
		sin( radians( lat ) ) ) ) 
	AS distance 
	FROM markers 
	HAVING distance < 25 
	ORDER BY distance 
	LIMIT 0 , 20;
 ```
 
### PHP Snippet
 ```
 <?php

$lat = 37;
$lng = -122;

// in Miles
$distanceWithin = 25


$sql = "SELECT
	id,
	( 3959 * acos( cos( radians($lat) ) * cos( radians( lat ) ) * 
		cos( radians( lng ) - 
		radians($lng) ) + 
		sin( radians($lat) ) * 
		sin( radians( lat ) ) ) ) 
	AS distance 
	FROM markers 
	HAVING distance < $distanceWithin
	ORDER BY distance 
	LIMIT 0 , 20";

$resultSet = mysqli_query($sql);
```


### Yii Snippet
```
<?php

$lat = 37;
$lng = -122;

// in Miles
$distanceWithin = 25


$distanceSql = "(3959*acos(cos(radians($lat))*cos(radians(lat))*cos(radians(lng)-radians($lng))+sin(radians($lat))*sin(radians(lat))))";

$markers = Marker::find()->select([
    'id',
    'name',
    'address',
    'lat',
    'lng',
    'distance' => $distance
    ])
->orderBy(['distance' => SORT_ASC])
->having(['<=', 'distance', $distanceWithin])
->offset($varStartLimit)
->limit($varEndLimit)
->asArray()
->all();
```
 Reference : https://developers.google.com/maps/articles/phpsqlsearch_v3?csw=1