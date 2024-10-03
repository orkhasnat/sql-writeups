# Help Desk
Link: [Help Desk](https://sqlzoo.net/wiki/Help_Desk)    

[Jump directly to Solutions](./help-desk.md#Solutions)

---
# Setup
### ER Diagram
![](../_resources/help-desk.png)
> *For some reason, all tables here needs to be in Title case in the queries.*       
> **Why though??**    #check        
### Issue Table
| Call_date                     | Call_ref    | Caller_id  | Detail                                                                               | Taken_by     | Assigned_to | Status  |
| ----------------------------- | ----------- | ---------- | ------------------------------------------------------------------------------------ | ------------ | ----------- | ------- |
| Sat, 12 Aug 2017 08:16:00 GMT | 1237        | 9          | How can I guarantee a digital communication in Oracle ?                              | AW1          | AE1         | Closed  |
| Sat, 12 Aug 2017 08:24:00 GMT | 1238        | 10         | How can I vanish a task-based documentation in Adobe Acrobat ?                       | AW1          | JE1         | Closed  |
| Sat, 12 Aug 2017 08:29:00 GMT | 1239        | 12         | How can I request a usability in Microsoft Powerpoint ?                              | AW1          | AE1         | Closed  |
| Sat, 12 Aug 2017 08:43:00 GMT | 1240        | 13         | How can I skip a aspect ratio in Oracle ?                                            | AW1          | JE1         | Closed  |
| Sat, 12 Aug 2017 08:48:00 GMT | 1241        | 14         | I'm trying to train a locator in SQL Server but the Information Mapping is too wacky | AW1          | AE1         | Closed  |
| Sat, 12 Aug 2017 08:49:00 GMT | 1242        | 15         | I'm trying to wobble a access key in SQL Server but the XML schema is too reflective | AW1          | AE1         | Closed  |
| Sat, 12 Aug 2017 09:01:00 GMT | 1243        | 16         | I'm trying to scatter a tacit knowledge in Adobe PhotoShop but the CSS is too tawdry | AW1          | JE1         | Closed  |
| Sat, 12 Aug 2017 09:01:00 GMT | 1244        | 17         | How can I remind a vocabulary list in Microsoft Excel ?                              | AW1          | JE1         | Open    |
| ..............                | ........... | .......... | ............                                                                         | ............ | ........... | ....... |
### Staff Table
| Staff_code | First_name | Last_name | Level_code |
| ---------- | ---------- | --------- | ---------- |
| AB1        | Anthony    | Butler    | 1          |
| AB2        | Alexis     | Butler    | 3          |
| AE1        | Ava        | Ellis     | 7          |
| AL1        | Alexander  | Lawson    | 3          |
| AW1        | Alyssa     | White     | 1          |
| BJ1        | Briony     | Jones     | 2          |
| ......     | .......    | .....     | .......    |

### Shift Table
| Shift_date                    | Shift_type | Manager    | Operator     | Engineer1   | Engineer2 |
| ----------------------------- | ---------- | ---------- | ------------ | ----------- | --------- |
| Sat, 12 Aug 2017 00:00:00 GMT | Early      | LB1        | AW1          | AE1         | JE1       |
| Sat, 12 Aug 2017 00:00:00 GMT | Late       | AE1        | IM1          | AL1         | BJ1       |
| Sun, 13 Aug 2017 00:00:00 GMT | Early      | AE1        | MM1          | MW1         |           |
| Sun, 13 Aug 2017 00:00:00 GMT | Late       | AE1        | AE1          | EB1         |           |
| ...............               | .........  | .......... | ............ | ........... | .......   |

### Shift Type Table
|Shift_type|Start_time|End_time|
|---|---|---|
|Early|08:00|14:00|
|Late|14:00|20:00|
### Customer Table
| Company_ref | Company_name       | Contact_id | Address_1          | Address_2 | Town            | Postcode     | Telephone          |
| ----------- | ------------------ | ---------- | ------------------ | --------- | --------------- | ------------ | ------------------ |
| 100         | Haunt Services     | 112        | 53 Finger Gate     |           | Dartford        | DA48 5WU     | 01001722832        |
| 101         | Genus Ltd.         | 33         | 34 Pyorrhea Green  |           | Guildford       | GY34 4ZH     | 01004256920        |
| 102         | Corps Ltd.         | 111        | 67 Napery Green    |           | Harrow          | HA32 6PP     | 01012384042        |
| 103         | Train Services     | 115        | 30 Crizzel Parkway |           | Hemel Hempstead | HP38 6DU     | 01012979358        |
| 104         | Somebody Logistics | 127        | 93 Calculated Oval |           | Hull            | HX16 1IF     | 01013707879        |
| .........   | ............       | .......... | ...........        | ........  | ...........     | ............ | .................. |
### Caller Table
| Caller_id | Company_ref | First_name | Last_name |
| --------- | ----------- | ---------- | --------- |
| 1         | 111         | Ava        | Clarke    |
| 2         | 134         | Ava        | Edwards   |
| 3         | 129         | John       | Green     |
| 4         | 108         | Ryan       | White     |
| 5         | 114         | Noah       | Evans     |
| ....      | .........   | .........  | .......   |
### Level Table
|Level_code|Manager|Operator|Engineer|
|---|---|---|---|
|1||Y||
|2|||Y|
|3||Y|Y|
|4|Y|||
|5|Y|Y||
|7|Y|Y|Y|

---
# Solutions
##  Problem 1
#easy    
 There are three issues that include the words "index" and "Oracle". Find the call_date for each of them.
```sql
select call_date, call_ref from Issue
where detail like "%Oracle%" and detail like "%index%";
```
##  Problem 2
#easy    
Samantha Hall made three calls on 2017-08-14. Show the date and time for each.
```sql
select Issue.call_date, 
	Caller.first_name, Caller.last_name 
from Caller join Issue 
	on Caller.caller_id = Issue.caller_id 
where Issue.call_date like '%2017-08-14%' 
	and first_name = 'samantha' and last_name = 'hall';
```

##  Problem 3
#easy    
{{description}}
```sql

```

##  Problem 4
#easy    
{{description}}
```sql

```


##  Problem 5
#easy    
{{description}}
```sql

```

##  Problem 6
#medium    
{{description}}
```sql

```

##  Problem 7
#medium     
{{description}} 
```sql

```

##  Problem 8
#medium      
{{description}}
```sql

```

##  Problem 9
#medium     
{{description}}
```sql

```

##  Problem 10
#medium    
{{description}}
```sql

```

##  Problem 11
#hard      
{{description}}
```sql

```

##  Problem 12
#hard      
{{description}}
```sql

```

##  Problem 13
#hard    
{{description}}
```sql

```

##  Problem 14
#hard     
{{description}}
```sql

```

##  Problem 15
#hard     
{{description}}
```sql

```
