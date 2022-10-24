# USF Library
An analysis on the availability of rooms of the USF library. 

# Summary
## The data scrapping process
- The data was from the USF library room reservation website: https://calendar.lib.usf.edu/spaces
- Tools use to scrape: Selenium with AWS server for schedule scraping and S3 for data storage.
- I scrap the data at 3 different time stamp over the day (6 am, 12am and 4pm)
- I can only see the availability of the day I check (So if the room available on day October 11, that mean I scrap that data on the same day (October)
- The data was collect over the course of one week
- Since the data was collected in a short amount of time ( 1 week), it may subject to bias, but the week I scrapped data was an ordinary week (not exam week nor holiday)

## A few notes about the dataset:
- The total of available rooms is different throughout the weeks, since library closed at different time throughout the week
- The total of availabe rooms is decreasing throughout the day as the 7am rooms would not available for you to book at 11am (those hour that have already passed will disapear instead of having the status of unavailable, so If I ping the website at 7am, and the room 256 at 8am is available, then when I check again at 12am, it will disapear regardless the availability)
- The availability of rooms are counted in 15 mins block (so if room 256 avaialbe from 8:15 to 8:30 is 1 available row, available from 8:30 to 8:45 is another available row)

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

### 1. The availability of rooms at different days:
I first look at the availability of different rooms at 6am:  

![image](https://user-images.githubusercontent.com/88282475/197418592-536afefb-0a74-4881-9c65-834e9e75b1b0.png)  
  
As you can see, there are large differences between days in week and weekday compare to weekend. The difference between available and unavailable rooms slowly rises from Monday to Thursday. On Friday the trend is **reversed**, there is more available rooms than unavailable rooms. On weekend, the available rooms are much more than unavailable (since nobody wants to study of the weekends obvious)   

I then saw if the trend is still the same if I check the availability at 12am and 4pm:  

![image](https://user-images.githubusercontent.com/88282475/197418185-776799aa-76cd-4001-87aa-120c5280eee1.png)  
  
The trend is remain very much the same, except more rooms were booked, so you can see the gap of available and unavailable rises throughout the day (both available and unavailable rooms would surely go down as the total went down).    
   
To make things more intuitive, I put all 3 hour onto 1 graph, and change the metrics to percentage. So now it will show how many percent of hour block is available.   
   
![image](https://user-images.githubusercontent.com/88282475/197418831-bdd70d15-0047-4998-a9f0-faf8c51d18b4.png)  
  
The percentage of availability went down for weekdays and Sunday, with an exception of Saturday.   
### 2. The availability of rooms at different hours:
I then moved on to the availability of rooms at different hours.    
   
![image](https://user-images.githubusercontent.com/88282475/197419159-37ed4abb-bf39-4e5e-bb66-52fa83184740.png)   
  
The trend was as expected, when not many students study at 6am and 7am ( I'm not sure why there's still 3% unavailable. It could be a mistake).  
There was still quite alot of rooms at 8am, before dropping drastically at 9am. From 10am to 9pm was the library's busiest hours. After that, the percent went up again.
   
### 3.The percentage of availability of each room
Then I of course tried to see which room is the most popular room. Below is the graph that visualize the percentage of total availability of each room. So across the dataset, which rooms are the most popular.   
   
![image](https://user-images.githubusercontent.com/88282475/197419558-fc79b073-819d-45d2-9ccd-df956a8bf022.png)
   
So, we have a list of most popular rooms. But I want to know the reasons why, and so I went to the website and scrap a few more feautures.   
   
<p align="center">
  <img src="https://user-images.githubusercontent.com/88282475/197419667-5a670350-e9f6-412d-ad10-3756a3f0233b.png" alt="Sublime's custom image"/>
</p>

So now we have more features:
- capacity: the capacity of that room, 8 means that the room is for 8 people
- quiet_room: whether that room was on the quiet floor or not
- floor: the floor of the room

Now let merge these with our original dataset, and we have this:   

<p align="center">
  <img src="https://user-images.githubusercontent.com/88282475/197419898-2d7f0ba1-9089-47fb-8ec9-c628777443f1.png" alt="Sublime's custom image"/>
</p>

Overall, two most popular rooms have the capacity of 8. Followed by mostly the quiet room (These quiet rooms also have the capacity of 2, but I think quiet is the determined factor here, since you can always get a 4 people rooms and sit alone or with a friends).     
The others are rooms with the capacity of 4 but are not quiet.

### 4. Will the room be occupied?
So, I want to know whether the room would be occiped or not. So I compare the avaialbility at 6am and 12am, to see the percent of room that are still available and rooms that are unavailable.   
    
 ![image](https://user-images.githubusercontent.com/88282475/197420424-cbd035f1-e443-4718-8b8b-c59b6fb47712.png)
- Available - Available: Available at 6am and still available at 12am
- Available - Unavailable: Available at 6am but not available at 12am
    
Note: Because I check the web at 6am and 12am, I will only count from 1pm. Since all the room in the morning would be gone after 12am.  
    
So, most rooms have the counts of Available - Unavailable much higher than the Available - Available. Especially those popular rooms like 257, 258, 514C, 514A, etc have the counts of Available - Unavailable significantly higher than Available - Available.    
The **ONLY** exception is the room 438, which is the least popular room according to the table above (the popular table).  
    
Since the graph above is a little difficult to see which rooms will run out the fastest, I draw another graph below.    
    
![image](https://user-images.githubusercontent.com/88282475/197421149-f78543f2-c97f-4941-b51b-61862d012489.png)   
So room 257, 514C, 258 and a few popular rooms are those room that you want to book early. But if you want the room 438, you're in luck, cause it will available.    
     
Again, just to compare the popular rooms and rooms that run out early, I put a table of top 5 of them:    
    
<p align="center">
  <img src="https://user-images.githubusercontent.com/88282475/197421234-ddd7a7ac-df2f-48c7-9211-05aea0ebeb53.png" alt="Sublime's custom image"/>
</p>
    
If you ever wonder how many percent of them are still available compare to the total(total of available and unavailable of that room), here it is:   
    
![image](https://user-images.githubusercontent.com/88282475/197421650-ab526c14-aab4-4916-a3bc-a1d7052dd238.png)    
The trend remain the same. The room 257 has only 14% that is still available, while room 438 has over 50% that is still available.    
Finally, just for fun, I put the count and the percentage side by side for comparasion.    
     
![image](https://user-images.githubusercontent.com/88282475/197421849-59d3fe32-b0f5-4852-82ae-ef35e36a69f6.png)

# Modelling 
I used a variety of models to train the dataset. Here is the result:   
    
<p align="center">
  <img src="https://user-images.githubusercontent.com/88282475/197426870-d9476231-5561-4223-ba42-2d430288012c.png" alt="Sublime's custom image"/>
</p>

Random forest is the model with the best result, so I tuned it with GridSearchCV. Here is the best parameter:

<p align="center">
  <img src="https://user-images.githubusercontent.com/88282475/197427161-d01f7e5a-02b4-47d2-8674-ae37af5c7d0c.png" alt="Sublime's custom image"/>
</p>
    
Finally, put the model to predict the test set.    

<p align="center">
  <img src="https://user-images.githubusercontent.com/88282475/197427227-cca57453-ea2d-4ac9-9b9b-a97b73647a79.png" alt="Sublime's custom image"/>
</p>

    
With the accuracy score: **0.943**
