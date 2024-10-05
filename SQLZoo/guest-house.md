# Guest House
Link: [Guest House](https://sqlzoo.net/wiki/Guest_House)   

[Jump directly to Solutions](./guest-house.md#Solutions)

---
# Setup
### ER Diagram
![](../_resources/guest-house.png)
### Booking Table
The table **booking** contains an entry for every booking made at the hotel. A booking is made by one guest - even though more than one person may be staying we do not record the details of other guests in the same room. In normal operation the table includes both past and future bookings.

### Room Table
Rooms are either single, double, twin or family.

### Rate Table
Rooms are charged per night, the amount charged depends on the "room type requested" value of the booking and the number of people staying:

---
# Solutions
##  Problem 1
#easy    
Give the booking date and the number of nights for guest 1183.
```sql
select booking_date,nights from booking
where guest_id = 1183;
```
##  Problem 2
#easy    
When do they get here? List the arrival time and the first and last names for all guests due to arrive on 2016-11-05, order the output by time of arrival.
```sql
select arrival_time,first_name,last_name from booking
join guest on guest.id = booking.guest_id
where booking_date = '2016-11-05'
order by arrival_time;
```

##  Problem 3
#easy    
Look up daily rates. Give the daily rate that should be paid for bookings with ids 5152, 5165, 5154 and 5295. Include booking id, room type, number of occupants and the amount.
```sql
select booking.booking_id, booking.room_type_requested, 
	booking.occupants, rate.amount
from booking join rate 
	on booking.room_type_requested = rate.room_type 
		and booking.occupants = rate.occupancy
where booking.booking_id in (5152, 5165, 5154, 5295);
```

##  Problem 4
#easy    
Who’s in 101? Find who is staying in room 101 on 2016-12-03, include first name, last name and address
```sql
select first_name,last_name,address from booking 
join guest on guest.id = booking.guest_id
where room_no = 101 and
	booking_date = '2016-12-03';
```


##  Problem 5
#easy    
How many bookings, how many nights? For guests 1185 and 1270 show the number of bookings made and the total number of nights. Your output should include the guest id and the total number of bookings and the total number of nights.
```sql
select guest_id, count(nights), sum(nights) 
from booking
where guest_id in (1185,1270)
group by guest_id;
```

##  Problem 6
#medium    
Ruth Cadbury. Show the total amount payable by guest Ruth Cadbury for her room bookings. You should JOIN to the rate table using room_type_requested and occupants.
```sql
select sum(nights*amount) from rate 
join booking 
	on booking.room_type_requested = rate.room_type 
		and booking.occupants = rate.occupancy
join guest on booking.guest_id = guest.id
where guest.first_name = 'Ruth' 
	and guest.last_name = 'Cadbury';
```

##  Problem 7
#medium       
Including Extras. Calculate the total bill for booking 5346 including extras.
```sql
select (sum(extra.amount) + (rate.amount*nights)) 
from extra join booking 
	on booking.booking_id = extra.booking_id 
join rate 
	on booking.room_type_requested = rate.room_type 
		and booking.occupants = rate.occupancy
where booking.booking_id = 5346;
```

##  Problem 8
#medium      
Edinburgh Residents. For every guest who has the word “Edinburgh” in their address show the total number of nights booked. Be sure to include 0 for those guests who have never had a booking. Show last name, first name, address and number of nights. Order by last name then first name.
```sql
select last_name,first_name,address, 
	coalesce(sum(booking.nights),0) as nights
from guest left join booking 
	on booking.guest_id = guest.id
where guest.address like "%Edinburgh%"
group by last_name,first_name,address
order by last_name,first_name;
```

> `coalesce(val1,...valn)` function returns the first non-null value in a list.       
> So here if the `sum` is `NULL` then `0` is returned.

##  Problem 9
#medium     
How busy are we? For each day of the week beginning 2016-11-25 show the number of bookings starting that day. Be sure to show all the days of the week in the correct order.
```sql
select (booking_date) as i, 
	count(*) as arrivals from booking
where booking_date >= "2016-11-25" 
	and booking_date <"2016-12-02"
group by booking_date
order by booking_date;
```

##  Problem 10
#medium    
How many guests? Show the number of guests in the hotel on the night of 2016-11-21. Include all occupants who checked in that day but not those who checked out.
```sql
select sum(occupants) from booking
where booking_date <= "2016-11-21" and 
	date(booking_date+nights) > "2016-11-21";
```

##  Problem 11
#hard           
Coincidence. Have two guests with the same surname ever stayed in the hotel on the evening? Show the last name and both first names. Do not include duplicates.      
_**N.B.** `last_name` is surname._   
```sql
with guests as(
	select * from guest 
	join booking on booking.guest_id = guest.id
)
select distinct(a.last_name), 
	a.first_name as a_name,
	b.first_name as b_name 
from guests as a
join guests as b 
	on a.last_name = b.last_name 
		and a.first_name != b.first_name
where ( a.booking_date between 
		b.booking_date and 
		date(b.booking_date+b.nights-1)
	) 
	or ( b.booking_date between 
		 a.booking_date and 
		 date(a.booking_date+a.nights-1)
	)
order by a.last_name;
```

> `-1` because `booking_date` itself is a night.

> [!note] #check
> how to filter the duplicates?
##  Problem 12
#hard      
Check out per floor. The first digit of the room number indicates the floor – e.g. room 201 is on the 2nd floor. For each day of the week beginning 2016-11-14 show how many rooms are being vacated that day by floor number. Show all days in the correct order.
```sql
select
	date_add(booking_date, interval nights day) as i,
	sum(case when room_no like '1%' then 1 else 0 end) as 1st,
	sum(case when room_no like '2%' then 1 else 0 end) as 2nd,
	sum(case when room_no like '3%' then 1 else 0 end) as 3rd
from booking
where
	date_add(booking_date, interval nights day) 
		between '2016-11-14' and '2016-11-20'
group by i
order by i;
```

##  Problem 13
#hard    
Free rooms? List the rooms that are free on the day 25th Nov 2016.
```sql
with occupied as (
	select room_no,booking_date, date(booking_date+nights-1)
	from booking
	where '2016-11-25' 
		between booking_date and date(booking_date+nights-1)
)
select distinct room_no as id from booking 
where room_no not in (select room_no from occupied);

```

##  Problem 14
#hard     
Single room for three nights required. A customer wants a single room for three consecutive nights. Find the first available date in December 2016.
```sql
with rooms as (
	select a.room_no, a.nights, a.booking_date, 
		min(b.booking_date) as next_booking
	from booking as a 
	join booking as b 
		on a.booking_date < b.booking_date 
			and a.room_no = b.room_no
	where a.room_no in (101, 201, 301) 
		and a.booking_date like '2016-12%' 
		and b.booking_date like '2016-12%'
	group by a.room_no, a.nights, a.booking_date
	order by a.room_no, a.booking_date
),
timediff as (
	select rooms.*, 
		timestampdiff(
			day, rooms.booking_date, rooms.next_booking
		) - rooms.nights
	from rooms
),
earliest as (
	select timediff.room_no, 
		max(
			date_add(
				timediff.next_booking, 
				interval timediff.nights day
			)
		) as earliest_opening
	from timediff
	group by timediff.room_no
)

select earliest.room_no as id, earliest.earliest_opening
from earliest
order by earliest.earliest_opening
limit 1;
```

##  Problem 15
#hard     
Gross income by week. Money is collected from guests when they leave. For each Thursday in November and December 2016, show the total amount of money collected from the previous Friday to that day, inclusive.
```sql
with weeks as(
	select 
		date_add(booking_date, interval -6 day) as previous_friday, 
		booking_date as thursday 
	from booking
	where dayofweek(booking_date) = 5
	group by booking_date
),
checkout_bill as(
	select booking.*, 
		date_add(booking_date, interval nights day) as checkout_date, 
		(
			(rate.amount * booking.nights) +
			 coalesce(sum(extra.amount), 0)
		) as bill
	from booking join rate 
		on booking.room_type_requested = rate.room_type 
			and booking.occupants = rate.occupancy
	left join extra on booking.booking_id = extra.booking_id
	group by booking.booking_id
)

select weeks.thursday, sum(checkout_bill.bill)
from weeks
join checkout_bill
	on checkout_bill.checkout_date 
		between weeks.previous_friday and weeks.thursday
group by weeks.thursday;
```

> Why the `0.00` are not shown ;(     #check