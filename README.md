# Data Engineering Project Weather Overview

Gans is a startup developing an e-scooter-sharing system. It aspires to operate in the most populous cities all around the world. In each city, the company will have hundreds of e-scooters parked in the streets and allow users to rent them by the minute.

The purpose of this project is to gather information related to cities such as population, weather, airports and flights information to upload to MySQL, then connecting MySQL to AWS in order to automate a process which will return flights, city, and weather info for a selected city.

## Code and resources used:

**MySQL:** MySQL Workbench, version 8.0.32

**AWS**

**Python version:** 3.8

**Packages:** pandas, BeautifulSoup, request, SQLAlchemy, PyMySQL, re

**APIs:** Open weather map, rapid API (Aero data box)

**Data:** Obtained from [WBS coding school](https://www.wbscodingschool.com/)

## Web Scraping

BeautifulSoup was used to scrap wikipedia in order to get some information regarding the required cities:

* City Name
* Latitude and Longitude
* Population
* Timestamp of the population

## APIs

Also, some API were used to get information regarding weather events and flight information:

* Forecast time
* Temperature
* Feels like
* Wind speed
* Country Code
* flight_id
* arrival_ICAO
* departure_ICAO
* departure_name
* departure_sch_time

Among some

## Data Cleaning

After scraping the data, I needed to clean it up so that it was usable. I made the following changes and created the following variables and dataframes:

* **For cities:**
  - A function was created to scrap the necessary info from Wikipedia
  - The coordinates in Wikipedia are given in GPS, so they had to be changed to lat and lon
  - The datatype of lat and lon was set to float64
* **For population:**
  - Two columns were created, timestamp_population and population
* **For weather:**
  - Five columns were creted for the API call
  - city_id column was created to make associations between tables and also for MySQL
* **For airports:**
  - Lat and lon from cities were taken and added to a list
  - Columns ICAO and city_name were created
* **For flights:**
  - ICAO from the previous dataframe in airports was taken
  - 9 columns were created and populated with information from the API
  - Departures and arrivals datatypes were set to_datetime

## MySQL - Setting up a local databse:

Once all the necessary information was collected, the local database was created to store the information. In the database, a table for each dataframe was created and the datatypes properly changed. A primary key was set for each city_id, flight_id and airport_ICAO. 


![1_p9AhbvUq7Dmg3dJKn7alsA (1)](https://user-images.githubusercontent.com/99658869/216339683-d3c79086-4895-4d6a-bc6d-40f71e5e167d.png)


## Local pipelines:

Two libraries were used to create pipes to make the gathered data flow to MySQL:

* **sqlalchemy:**
  - To facilitate the communication between python and MySQL
* **pymysql:**
  - To connect python to MySQL schema

## Cloud pipelines:

For the could pipelines, AWS was used:

* **Could database:**
  - RDS was used to set up MySQL database
* **Lambda functions:**
  - A lambda funciton was created to move my data collection scripts from Jupyter notebooks into AWS lambda function
* **Automate the process:**
  - Used CloudWatch Events / EventBridge to create rules that will trigger the execution of the data collection scripts


![data_engineering](https://user-images.githubusercontent.com/99658869/216336064-5393286c-69a6-4135-a623-8c809834f988.png)


## EDA:

I checked that everything was working as intended. First, a called the function and passed Barcelona as a parameter to see all the arrivals and departures on a certain date:

![Screenshot 2023-02-02 at 14 41 26](https://user-images.githubusercontent.com/99658869/216340660-fc063107-124b-43da-92a8-e822da5fd2af.png)

Next, I checked to see if the airport table was also working correctly:

![Screenshot 2023-02-02 at 14 45 23](https://user-images.githubusercontent.com/99658869/216341493-78f1e15b-a745-4f1e-8641-3776a67ac172.png)

Finally, the weather table was also checked:

![Screenshot 2023-02-02 at 14 46 17](https://user-images.githubusercontent.com/99658869/216341692-05702bfb-7b5a-484f-8736-a57425ab7426.png)





Some changes need to be done to some of the notebooks to make the automatization better.


