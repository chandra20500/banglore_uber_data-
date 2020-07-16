# banglore_uber_data-
$ each 'delete DISPLAY_NAME' target=bangalore_wards

$ each 'this.properties = {WARD_NAME:WARD_NAME, WARD_NO:WARD_NO, 
  MOVEMENT_ID:MOVEMENT_ID, AREA_KM2:this.area/1e6}' 
  target=bangalore_wards

$ dissolve sum-fields=AREA_KM2 name=dissolved +
 target=bangalore_wards

$ join bangalore_wards target=map

$ filter 'sourceid == 94' name=filtered + 
 target=bangalore-wards-2019-3-All-HourlyAggregate

$ join filtered keys=MOVEMENT_ID,dstid 
 calc='median_time = median(mean_travel_time), join_count = count()'
 target=bangalore_wards

$ filter 'median_time <= 1800' name=30mins + target=bangalore_wards

$ dissolve target=30mins

$ style fill=#f0f0f0 stroke=#bdbdbd target=bangalore_wards
style stroke=#2b8cbe target=dissolved
style label-text=name dx=15 target=map
style stroke=red fill=#fee8c8 opacity=0.5 target=30mins

$ style stroke=#2b8cbe target=dissolved

$ style fill=#f0f0f0 stroke=#bdbdbd target=bangalore_wards

$ style label-text=name dx=15 target=map

$ style stroke=red fill=#fee8c8 opacity=0.5 target=30mins

$ colorizer name=traveltime colors='#f0f9e8,#bae4bc,#7bccc4,#2b8cbe' breaks=900,1800,2700

$ style fill=traveltime(median_time) target=bangalore_wards
