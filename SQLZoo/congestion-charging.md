# Congestion Charging
Link: [Congestion Charging](https://sqlzoo.net/wiki/Congestion_Charging) 

[Jump directly to Solutions](./congestion-charging.md#Solutions)

---
# Setup
### ER Diagram
![](../_resources/congestion-charging.png)
### Camera Table
| id   | perim |
| ---- | ----- |
| 1    | IN    |
| 2    | IN    |
| 3    | IN    |
| 4    | IN    |
| .... | ....  |
| 11   | OUT   |
| 12   | OUT   |
| 13   | OUT   |
| 14   | OUT   |
### Image Table
| camera | whn                           | reg       |
| ------ | ----------------------------- | --------- |
| 1      | Sun, 25 Feb 2007 06:10:13 GMT | SO 02 ASP |
| 9      | Sun, 25 Feb 2007 06:26:04 GMT | SO 02 ASP |
| 17     | Sun, 25 Feb 2007 06:20:01 GMT | SO 02 ASP |
| 18     | Sun, 25 Feb 2007 06:23:40 GMT | SO 02 ASP |
| ...    |                               |           |
### Keeper Table

| id  | name                | address               |
| --- | ------------------- | --------------------- |
| 1   | Ambiguous, Arthur   | Absorption Ave.       |
| 2   | Inconspicuous, Iain | Interception Rd.      |
| 3   | Contiguous, Carol   | Circumscription Close |
| 4   | Strenuous, Sam      | Surjection Street     |
| 5   | Assiduous, Annie    | Attribution Alley     |
| 6   | Incongruous, Ingrid | Irresolution Pl.      |
### Permit Table
| reg       | sDate                         | chargeType |
| --------- | ----------------------------- | ---------- |
| SO 02 ASP | Sat, 21 Jan 2006 00:00:00 GMT | Weekly     |
| SO 02 ATP | Sun, 21 Jan 2007 00:00:00 GMT | Daily      |
| SO 02 ATP | Mon, 22 Jan 2007 00:00:00 GMT | Daily      |
| SO 02 BSP | Mon, 30 Jan 2006 00:00:00 GMT | Weekly     |
| SO 02 BTP | Mon, 30 Jan 2006 00:00:00 GMT | Daily      |
| SO 02 BTP | Tue, 31 Jan 2006 00:00:00 GMT | Daily      |
| .....     |                               |            |
### Vehicle Table
| id        | keeper |
| --------- | ------ |
| SO 02 MUP |        |
| SO 02 NUP |        |
| SO 02 ASP | 1      |
| SO 02 ESP | 1      |
| SO 02 RSP | 1      |
| SO 02 BTP | 2      |
| SO 02 HTP | 2      |
| ....      |        |

---
# Solutions
##  Problem 1
#easy    
 Show the name and address of the keeper of vehicle SO 02 PSP.
```sql
select keeper.name, keeper.address 
from keeper join vehicle 
	on vehicle.keeper=keeper.id
where vehicle.id="SO 02 PSP";
```
##  Problem 2
#easy    
Show the number of cameras that take images for incoming vehicles.
```sql
select count(*) from camera where perim="IN";
```

##  Problem 3
#easy    
List the image details taken by Camera 10 before 26 Feb 2007.
```sql
select * from image 
where camera=10 and whn < date("2007-02-26");
```

> **DATE datatype format** `YYYY-MM-DD`

##  Problem 4
#easy   
List the number of images taken by each camera. Your answer should show how many images have been taken by camera 1, camera 2 etc. The list must NOT include the images taken by camera 15, 16, 17, 18 and 19.
```sql
select camera,count(*) from image
where camera not between 15 and 19
group by camera;
```

##  Problem 5
#easy      
A number of vehicles have permits that start on 30th Jan 2007. List the name and address for each keeper in alphabetical order without duplication.
```sql
select distinct(keeper.name), keeper.address from keeper 
join vehicle on vehicle.keeper = keeper.id
join permit on permit.reg = vehicle.id
where permit.sDate = date("2007-01-30")
order by keeper.name;
```

##  Problem 6
#medium    
List the owners (name and address) of Vehicles caught by camera 1 or 18 without duplication.
```sql
select distinct(keeper.name), keeper.address from keeper 
join vehicle on vehicle.keeper = keeper.id
join image on vehicle.id = image.reg
where image.camera in (1,18);
```

##  Problem 7
#medium    
Show keepers (name and address) who have more than 5 vehicles.
```sql
select keeper.name, keeper.address, 
	count(vehicle.id) from keeper
join vehicle on vehicle.keeper = keeper.id
group by keeper.id having count(*) >5;
```

##  Problem 8
#medium #check            
For each vehicle show the number of current permits (suppose today is the 1st of Feb 2007). The list should include the vehicles registration and the number of permits. Current permits can be determined based on charge types, e.g. for weekly permit you can use the date after 24 Jan 2007 and before 02 Feb 2007.  
```sql
;(
```

##  Problem 9
#medium               
Obtain a list of every vehicle passing camera 10 on 25th Feb 2007. Show the time, the registration and the name of the keeper if available.
```sql
select image.reg, image.whn, keeper.name from image
join vehicle on vehicle.id = image.reg
join keeper on vehicle.keeper = keeper.id
where image.camera = 10 
	and date(image.whn) = date("2007-02-25");
```

> **How to check data types in MySQL?** #check

##  Problem 10
#medium    
List the keepers who have more than 4 vehicles and one of them must have more than 2 permits. The list should include the names and the number of vehicles.
```sql
select v_count.name, v_count.n_veh as "No. of Vehicles"
from (
	select keeper.id, keeper.name, 
		count(vehicle.id) as n_veh from keeper
	join vehicle on vehicle.keeper = keeper.id 
	group by keeper,id, keeper.name
	having count(vehicle.id) > 4 
) as v_count 
join ( 
	select vehicle.keeper, permit.reg, 
		count(chargetype) from vehicle
	join permit on permit.reg = vehicle.id
	group by permit.reg
	having count(chargeType) > 2 
) as p_count
on v_count.id = p_count.keeper;
```

##  Problem 11
#hard        
When creating a view in scott you must specify the schema name of the sources and the destination.
```sql
-- example view creation
create view scott.tmp as 
select * from gisq.table;
-- view drop
drop view scott.tmp;
```

##  Problem 12
#hard    
There are four types of permit. The most popular type means that this type has been issued the highest number of times. Find out the most popular type, together with the total number of permits issued.
```sql
select chargetype,count(chargetype) as cnt from permit
group by chargetype
order by cnt desc
limit 1;
```

##  Problem 13
#hard #check          
For each of the vehicles caught by camera 19 - show the registration, the earliest time at camera 19 and the time and camera at which it left the zone.
```sql
;(
```

##  Problem 14
#hard #check          
For all 19 cameras - show the position as IN, OUT or INTERNAL and the busiest hour for that camera.
```sql
create view scott.tmp as
select camera.id,
	coalesce(hour(image.whn),-1) as hour,-- do i need coalesce?
	case when camera.perim is null then "INTERNAL"
		else camera.perim
	end as perim 
from gisq.image right join gisq.camera 
	on camera.id = image.camera;
---
select id,perim,max(cnt) 
from (
	select *, count(hour) as cnt
	from scott.tmp
	group by id,hour
) as tt
group by id;
-- but which hour is it?
-- HOW??
---
drop view scott.tmp;
```

> `coalesce(val1,...valn)` function returns the first non-null value in a list.     
> So here if `whn` is `NULL` then `-1` is returned.
##  Problem 15
#hard #check       
Anomalous daily permits. Daily permits should not be issued for non-charging days. Find a way to represent charging days. Identify the anomalous daily permits.
```sql
What are charging days!!
```

##  Problem 16
#hard #check         
Issuing fines: Vehicles using the zone during the charge period, on charging days must be issued with fine notices unless they have a permit covering that day. List the name and address of such culprits, give the camera and the date and time of the first offence.
```sql
Depends on the previous one!!
```