---
title: "NYC Taxi demand prediction"
header:
  teaser: /images/projects/Taxi-demand-Prediction/nyc_taxi_logo.jpeg
date: 2018-07-18
categories:
  - Project
tags: 
  - Python
  - Clustering
  - Machine Learning
  - EDA
excerpt: "Predict number of pickups in a region at a given time."
---

# Problem statement
To find number of pickups, given location coordinates (latitude and longitude) and time, in the query region and surrounding regions.

### Data sources
Get the 2016 data from [NYC.GOV](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml).   
In the given project we are considering only the yellow taxis for the time period between Jan - Mar 2015 & Jan - Mar 2016 with given features. 

<table border="1">
	<tr>
		<th>Field Name</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>VendorID</td>
		<td>
		A code indicating the TPEP provider that provided the record.
		<ol>
			<li>Creative Mobile Technologies</li>
			<li>VeriFone Inc.</li>
		</ol>
		</td>
	</tr>
	<tr>
		<td>tpep_pickup_datetime</td>
		<td>The date and time when the meter was engaged.</td>
	</tr>
	<tr>
		<td>tpep_dropoff_datetime</td>
		<td>The date and time when the meter was disengaged.</td>
	</tr>
	<tr>
		<td>Passenger_count</td>
		<td>The number of passengers in the vehicle. This is a driver-entered value.</td>
	</tr>
	<tr>
		<td>Trip_distance</td>
		<td>The elapsed trip distance in miles reported by the taximeter.</td>
	</tr>
	<tr>
		<td>Pickup_longitude</td>
		<td>Longitude where the meter was engaged.</td>
	</tr>
	<tr>
		<td>Pickup_latitude</td>
		<td>Latitude where the meter was engaged.</td>
	</tr>
	<tr>
		<td>RateCodeID</td>
		<td>The final rate code in effect at the end of the trip.
		<ol>
			<li> Standard rate </li>
			<li> JFK </li>
			<li> Newark </li>
			<li> Nassau or Westchester</li>
			<li> Negotiated fare </li>
			<li> Group ride</li>
		</ol>
		</td>
	</tr>
	<tr>
		<td>Store_and_fwd_flag</td>
		<td>This flag indicates whether the trip record was held in vehicle memory before sending to the vendor,<br\> aka “store and forward,” because the vehicle did not have a connection to the server.
		<br\>Y= store and forward trip
		<br\>N= not a store and forward trip
		</td>
	</tr>
	<tr>
		<td>Dropoff_longitude</td>
		<td>Longitude where the meter was disengaged.</td>
	</tr>
	<tr>
		<td>Dropoff latitude</td>
		<td>Latitude where the meter was disengaged.</td>
	</tr>
	<tr>
		<td>Payment_type</td>
		<td>A numeric code signifying how the passenger paid for the trip.
		<ol>
			<li> Credit card </li>
			<li> Cash </li>
			<li> No charge </li>
			<li> Dispute</li>
			<li> Unknown </li>
			<li> Voided trip</li>
		</ol>
		</td>
	</tr>
	<tr>
		<td>Fare_amount</td>
		<td>The time-and-distance fare calculated by the meter.</td>
	</tr>
	<tr>
		<td>Extra</td>
		<td>Miscellaneous extras and surcharges. Currently, this only includes. the $0.50 and $1 rush hour and overnight charges.</td>
	</tr>
	<tr>
		<td>MTA_tax</td>
		<td>0.50 MTA tax that is automatically triggered based on the metered rate in use.</td>
	</tr>
	<tr>
		<td>Improvement_surcharge</td>
		<td>0.30 improvement surcharge assessed trips at the flag drop. the improvement surcharge began being levied in 2015.</td>
	</tr>
	<tr>
		<td>Tip_amount</td>
		<td>Tip amount – This field is automatically populated for credit card tips.Cash tips are not included.</td>
	</tr>
	<tr>
		<td>Tolls_amount</td>
		<td>Total amount of all tolls paid in trip.</td>
	</tr>
	<tr>
		<td>Total_amount</td>
		<td>The total amount charged to passengers. Does not include cash tips.</td>
	</tr>
</table>

### Mapping to a machine learning problem

We need to predict the demand at a given time and region. So, we will treat it as a time-series forecasting problem keeping metric as MSE.

# Exploratory Data Analysis

### Pickup and Dropoff coordinates
New York is bounded by the location cordinates(lat,long) - (40.5774, -74.15) & (40.9176,-73.7004), however there are many coordinates which lie outside the bounded region for pickups and dropoffs (see image below). So, we removed those data points.  

![](/images/projects/Taxi-demand-Prediction/1.JPG)  

### Trip duration
Moving onto the `Trip` duration, according to NYC Taxi & Limousine Commision Regulations the maximum allowed trip duration in a 24 hour interval is 12 hours. So selecting only the data points whose trip times is greater than 1 and less than 720 minutes (12 hours). Given below is the PDF of log-trip-times along with its Normal Q-Q plot which says that the data follows a good normal distribution.  

![](/images/projects/Taxi-demand-Prediction/2.png)  

![](/images/projects/Taxi-demand-Prediction/3.png)  

### Speed
Taking the 99th percentile, replacing all the outliers by `45.31` (99th percentile). The average speed of taxis came out to be `12.45` miles/hr.

### Distance
Replacing outliers by 23 miles (99th percentile).

### Total Fare
Replacing outliers with 1000 (99th percentile)  

*Note - While removing outliers, everytime we suppose a value greater than 0*

# Data preparation

Creating clusters across the NY city. Keeping minimum inter-cluster distance as 0.5 mile and maximum inter-cluster distance as 2 mile, since 2 miles can be covered in 10 minutes.  
We choose the optimum number of clusters as 40 because on choosing a cluster size of  40 we have -  
```python
Avg. Number of Clusters within the vicinity (i.e. intercluster-distance < 2): 9.0   
Avg. Number of Clusters outside the vicinity (i.e. intercluster-distance > 2): 31.0   
Min inter-cluster distance =  0.5064095487015858  (as desired)  
```

**Cluster centers**  

![](/images/projects/Taxi-demand-Prediction/4.JPG)  
![](/images/projects/Taxi-demand-Prediction/5.JPG)  


So finally, we have cluster IDs along with 10 minutes time bins. We see in many 10 minute time bins that number of pickups are zero. Such missing value is filled with interpolating and exterpolating. 

# References
* [Applied AI course](https://www.appliedaicourse.com)
* [nyc.gov](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml)

