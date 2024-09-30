# Congestion Charging
Link: [Congestion Charging](https://sqlzoo.net/wiki/Congestion_Charging)
[Jump directly to Solutions](congestion-charging#Solutions)

---
# Setup
ER Diagram for *Congestion Charging* database
![](../_resources/CongestionCharge.png)
### Camera Table
| id  | perim |
| --- | ----- |
| 1   | IN    |
| 2   | IN    |
| 3   | IN    |
| 4   | IN    |
| 5   | IN    |
| 6   | IN    |
| 7   | IN    |
| 8   | IN    |
| 9   | OUT   |
| 10  | OUT   |
| 11  | OUT   |
| 12  | OUT   |
| 13  | OUT   |
| 14  | OUT   |
### Image Table
| camera | whn                           | reg       |
| ------ | ----------------------------- | --------- |
| 1      | Sun, 25 Feb 2007 06:10:13 GMT | SO 02 ASP |
| 9      | Sun, 25 Feb 2007 06:26:04 GMT | SO 02 ASP |
| 17     | Sun, 25 Feb 2007 06:20:01 GMT | SO 02 ASP |
| 18     | Sun, 25 Feb 2007 06:23:40 GMT | SO 02 ASP |
| ...    |                               |           |
### Keeper Table

|id|name|address|
|---|---|---|
|1|Ambiguous, Arthur|Absorption Ave.|
|2|Inconspicuous, Iain|Interception Rd.|
|3|Contiguous, Carol|Circumscription Close|
|4|Strenuous, Sam|Surjection Street|
|5|Assiduous, Annie|Attribution Alley|
|6|Incongruous, Ingrid|Irresolution Pl.|
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
|....||

---
# Solutions
##  Problem 1
#easy  
Show the name and address of the keeper of vehicle SO 02 PSP.

```sql
select keeper.name, keeper.address from keeper 
join vehicle 
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

```

##  Problem 4
#easy  
List the number of images taken by each camera. Your answer should show how many images have been taken by camera 1, camera 2 etc. The list must NOT include the images taken by camera 15, 16, 17, 18 and 19.
```sql

```


##  Problem 5
#easy  
A number of vehicles have permits that start on 30th Jan 2007. List the name and address for each keeper in alphabetical order without duplication.
```sql

```

##  Problem 6
#medium
A number of vehicles have permits that start on 30th Jan 2007. List the name and address for each keeper in alphabetical order without duplication.
```sql

```