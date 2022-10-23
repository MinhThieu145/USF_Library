# USF Library
An analysis on the availability of rooms of the USF library. 

# Summary
## The data scrapping process
- I scrap the data at 3 different time stamp over the day (6 am, 12am and 4pm)
- I can only see the availability of the day I check (So if the room available on day October 11, that mean I scrap that data on the same day (October)
- The data was collect over the course of one week
- Since the data was collected in a short amount of time ( 1 week), it may subject to bias, but the week I scrapped data was an ordinary week (not exam week nor holiday)

## A few notes about the dataset:
- The total of available rooms is different throughout the weeks, since library closed at different time throughout the week
- The total of availabe rooms is decreasing throughout the day as the 7am rooms would not available for you to book at 11am (those hour that have already passed will disapear instead of having the status of unavailable, so If I ping the website at 7am, and the room 256 at 8am is available, then when I check again at 12am, it will disapear regardless the availability)

## Explanation of the columns' meaning:  
The data has 6 columns in total, with 1 column will be dropped:
- checking_hour: this is the time that I ping the page ( 6 mean I check the availability of rooms at 6am, 12 means at 12am and 16 means at 4pm)
- hour: the availability of rooms at that time 
- day_of_week: the availability of room at that day of week
- room: the room number
- status: either available or unavailable

# Analysis
An overiew of the raw dataset:  

![image](https://user-images.githubusercontent.com/88282475/197417139-0b498b9f-cabb-43e0-a3ad-373ee736c4ae.png)  
I then dropped the date column, since the data was collected in such a short amount of time that, date will have no impact on the overall trend of the data.

### 1. The availability of rooms at different time:
I first look at the availability of different rooms at 6am:  

![image](https://user-images.githubusercontent.com/88282475/197418592-536afefb-0a74-4881-9c65-834e9e75b1b0.png)  
  
As you can see, there are large differences between days in week and weekday compare to weekend. The difference between available and unavailable rooms slowly rises from Monday to Thursday. On Friday the trend is **reversed**, there is more available rooms than unavailable rooms. On weekend, the available rooms are much more than unavailable (since nobody wants to study of the weekends obvious)   

I then saw if the trend is still the same if I check the availability at 12am and 4pm:  

![image](https://user-images.githubusercontent.com/88282475/197418185-776799aa-76cd-4001-87aa-120c5280eee1.png)  
  
The trend is remain very much the same, except more rooms were booked, so you can see the gap of available and unavailable rises throughout the day (both available and unavailable rooms would surely go down as the total went down)










