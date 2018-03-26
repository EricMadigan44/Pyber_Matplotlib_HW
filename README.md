# Pyber_Matplotlib_HW

#Eric Madigan - PYBER Matloblib HW 

# Dependencies 
import pandas as pd 
import matplotlib.pyplot as plt
import pandas as pd 
import os 

#Read our csv files city_data.csv and ride_data.csv: 

#Set the path 
city_data = "city_data.csv"

ride_data = "ride_data.csv"

#using pandas to read the csv files 
city_data_df = pd.read_csv(city_data)

ride_data_df = pd.read_csv(ride_data)

#Merge our two data sets 

merged_data = pd.merge(city_data_df, ride_data_df, on="city")

merged_data.head()

# Below we will create our variables and figure out the:
#Avg Fare,Total # of riders,Total Drivers per city and the City Type (Urban, Suburban, Rural)

# Avg Fare per City
avgerage_fare = merged_data.groupby("city")["fare"].mean()
avgerage_fare = pd.DataFrame(avg_fare).reset_index()
avgerage_fare = avgerage_fare.rename(columns = {'fare': 'avg_fare'})

# Total # of Rides per City
total_rides = merged_data.groupby("city")["ride_id"].count()
total_rides = pd.DataFrame(total_rides).reset_index()
total_rides = total_rides.rename(columns = {'ride_id': 'total_rides'})

# Total Drivers per City 

total_drivers = merged_data[["city", "driver_count"]].drop_duplicates("city")

# City Type 
city_type = merged_data[["city", "type"]].drop_duplicates("city")

#Final Data Frame
final_dataframe = pd.merge(pd.merge(pd.merge(avgerage_fare, total_rides, on="city"),
                                   total_drivers, on="city"), city_type, on="city")
final_dataframe.head()

#Data for our Bubble Plot 
urban_group = final_dataframe.loc[final_dataframe['type'] == 'Urban']

suburban_group = final_dataframe.loc[final_dataframe["type"] =='Suburban']

rural_group = final_dataframe.loc[final_dataframe['type'] == 'Rural']

#Bubble Plot
ax1 = urban_group.plot(kind='scatter',x='total_rides', y='avg_fare',
                      color='lightcoral', s=final_dataframe['driver_count']*5, label ='Urban',
                      alpha = 0.4, edgecolor = "firebrick", linewidths = .5)

ax2 = suburban_group.plot(kind='scatter',x='total_rides', y='avg_fare',
                      color='lightskyblue', s=final_dataframe['driver_count']*5, label ='Suburban',
                      alpha = 0.5, edgecolor = "steelblue", linewidths = .5, ax=ax1)

ax3 = rural_group.plot(kind='scatter',x='total_rides', y='avg_fare',
                      color='gold', s=final_dataframe['driver_count']*5, label ='Rural',
                      alpha = 0.3, edgecolor = "darkgoldenrod", linewidths = .5, ax=ax1)




plt.title("Pyber Rides")
plt.xlabel("Total Number of Rides(Per City)")
plt.ylabel("Average Fare ($)")
plt.legend(title = 'City Type:')
                                     

