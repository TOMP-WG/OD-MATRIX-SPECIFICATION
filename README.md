# OD-MATRIX-SPECIFICATION

Exchange format for exchanging OD matrices (.odz)

## In this github

* a describing MS Word document, explaining the files in the archive '.odz' file.
  * origin and destination geometries: files for a shape file (.dbf, .shp, .shx) OR a geojson file (.geojson)
  * description file (.odd)
  * value file(s) (.odv)
* examples
* an OpenAPI 3.0 specification (for an exact definition), containing the object model.

## To do

* create a tool that consumes an ODZ file and a CSV containing movements and produces a ODZ file with value files inside

## Learning by doing

This example contains the .odd file, explaining that this exchange contains a trip count between origins and destinations over the a complete period (first of June until the first of July 2017).

### Description file

```json
{
  "unit": "TRIPS",
  "geography_id": "id",
  "aggregation_period": {
    "start": "2017-06-01T00:00:00.000",
    "end": "2017-07-01T00:00:00.000"
  },
  "generation_date": "2017-07-01T03:00:00.000Z",
  "value_files": [
    {
      "file_name": "example_odmatrix.odv",
      "purpose": ["ALL"],
      "mode": ["ALL"],
      "aggregation_function": ["COUNT"],
      "aggregation_date_bucket": "ALL",
      "aggregation_time_bucket": "ALL"
    }
  ]
}
```

### Value file

```csv
TRIPS-ALL-ALL-COUNT-ALL-ALL;324AC234;349AB347
324AC234;2;342
349AB347;94;9
```

## How to

* Exchange data for a specific mode

> Specify the mode you need in the field `mode`. Per mode you can create a separate file. It is also possible to combine multiple modes in one value file. See `Advanced` in the word document.

* Report per hour for a whole day

> You can create a separate value file per hour, in the `aggregation_time_bucket` the value 'HOUR' must be specified and in the `time_bucket` field the corresponding hour must be specified. It is also possible to exchange data for the whole day in one file, using the `Advanced` setup (look in the word document).

This partial json describes the .odd file needed:

```json
{
  ...
  "value_files": [
    {
      "file_name": "hourly_matrix-ALL-ALL-COUNT-DAY-HOUR.odv",
      "purpose": ["ALL"],
      "mode": ["ALL"],
      "aggregation_function": ["COUNT"],
      "aggregation_date_bucket": "ALL",
      "aggregation_time_bucket": "HOUR",
      "time_bucket": [0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23]
    }
  ]
  ...
}
```

The corresponding .odv file has has this format:

```csv
TRIPS-ALL-ALL-COUNT-DAY-HOUR#0|#1|#2|#3|#4|#5|#6|#7|#8|#9|#10|#11|#12|#13|#14|#15|#16|#17|#18|#19|#20|#21|#22|#23;324AC234;349AB347
324AC234;0|1|2|3|4|5|6|7|8|9|0|1|2|3|4|5|6|7|8|9|0|1|2|3;0|1|2|3|4|5|6|7|8|9|0|1|2|3|4|5|6|7|8|9|0|1|2|3
349AB347;0|1|2|3|4|5|6|7|8|9|0|1|2|3|4|5|6|7|8|9|0|1|2|3;0|1|2|3|4|5|6|7|8|9|0|1|2|3|4|5|6|7|8|9|0|1|2|3
```
