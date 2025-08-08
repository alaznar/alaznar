---
jupyter:
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
  language_info:
    codemirror_mode:
      name: ipython
      version: 3
    file_extension: .py
    mimetype: text/x-python
    name: python
    nbconvert_exporter: python
    pygments_lexer: ipython3
    version: 3.13.5
  nbformat: 4
  nbformat_minor: 2
---

::: {.cell .markdown}
# Introduction

After doing some excercise, the first thing I do, as a Data enthusiast,
is to analyze the session\'s information my Garmin watch has captured.
Unfortunately, my life keeps going and it is quite uncomfortable to
playing around with this data in this diminute watch interface. Then I
thought I could pull all this data from the Garmin account and check my
own stats!

Through using this github repository
(<https://github.com/pe-st/garmin-connect-export>), I was able to pull
my data from my Garmin Connect account, in which all this information is
stored.

However, this method only pulls a fraction of all the information
gathered by the smartwatch. However, it is possible to access all your
data by requesting it to Garmin
(<https://www.garmin.com/es-ES/account/datamanagement/>). They will send
the information in less than 30 days. In my experience, I\'ve always
received all the information in just a few hours.

One key feature of Garmin is that dozens of sleep data are captured at
night. Using this GitHub repository\'s script, ExtractGarminData.py
(<https://github.com/tintin305/GarminSleepAnalytics/tree/master>), I was
able to extract all of my Sleep Data as well from the files exported.

The Data showcased in this project start in late November/December, when
I decided to take a bargain of a Garmin Forerunner 265 during Black
Friday :)

Let\'s check it out!
:::

::: {.cell .markdown}
# Exploratory Data Analysis (EDA)

### Activities Data

Let\'s take a quick look at our activities data.
:::

::: {.cell .code execution_count="96"}
``` python
import pandas as pd
df_activities = pd.read_csv("C:/Users/alexa/OneDrive/Documents/Code/Python/Projects/Garmin Data/garmin-connect-export-master/2025-07-25_garmin_connect_export/activities.csv")
df_activities.shape
```

::: {.output .execute_result execution_count="96"}
    (159, 50)
:::
:::

::: {.cell .code execution_count="97"}
``` python
df_activities.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 159 entries, 0 to 158
    Data columns (total 50 columns):
     #   Column                                 Non-Null Count  Dtype  
    ---  ------                                 --------------  -----  
     0   Start Time                             159 non-null    object 
     1   End Time                               159 non-null    object 
     2   Activity ID                            159 non-null    int64  
     3   Activity Name                          159 non-null    object 
     4   Description                            1 non-null      object 
     5   Location Name                          69 non-null     object 
     6   Time Zone                              159 non-null    object 
     7   Offset                                 159 non-null    object 
     8   Duration (h:m:s)                       159 non-null    object 
     9   Elapsed Duration (h:m:s)               159 non-null    object 
     10  Moving Duration (h:m:s)                154 non-null    object 
     11  Activity Parent                        159 non-null    object 
     12  Activity Type                          159 non-null    object 
     13  Event Type                             159 non-null    object 
     14  Device                                 158 non-null    object 
     15  Gear                                   0 non-null      float64
     16  Privacy                                159 non-null    object 
     17  File Format                            159 non-null    object 
     18  Distance (km)                          75 non-null     float64
     19  Average Speed (km/h)                   75 non-null     float64
     20  Average Speed (km/h or min/km)         75 non-null     object 
     21  Average Moving Speed (km/h)            73 non-null     float64
     22  Average Moving Speed (km/h or min/km)  73 non-null     object 
     23  Max. Speed (km/h)                      73 non-null     float64
     24  Max. Speed (km/h or min/km)            73 non-null     object 
     25  Elevation Gain (m)                     67 non-null     float64
     26  Elevation Loss (m)                     68 non-null     float64
     27  Elevation Min. (m)                     68 non-null     float64
     28  Elevation Max. (m)                     68 non-null     float64
     29  Elevation Corrected                    159 non-null    bool   
     30  Begin Latitude (°DD)                   69 non-null     float64
     31  Begin Longitude (°DD)                  69 non-null     float64
     32  End Latitude (°DD)                     69 non-null     float64
     33  End Longitude (°DD)                    69 non-null     float64
     34  Max. Heart Rate (bpm)                  158 non-null    float64
     35  Average Heart Rate (bpm)               158 non-null    float64
     36  Calories                               158 non-null    float64
     37  VO2max                                 66 non-null     float64
     38  Aerobic Training Effect                157 non-null    float64
     39  Anaerobic Training Effect              46 non-null     float64
     40  Avg. Run Cadence                       68 non-null     float64
     41  Max. Run Cadence                       68 non-null     float64
     42  Stride Length                          64 non-null     float64
     43  Steps                                  154 non-null    float64
     44  Avg. Cadence (rpm)                     0 non-null      float64
     45  Max. Cadence (rpm)                     0 non-null      float64
     46  Strokes                                0 non-null      float64
     47  Avg. Temp (°C)                         0 non-null      float64
     48  Min. Temp (°C)                         0 non-null      float64
     49  Max. Temp (°C)                         0 non-null      float64
    dtypes: bool(1), float64(29), int64(1), object(19)
    memory usage: 61.2+ KB
:::
:::

::: {.cell .markdown}
It seems like my Garmin is not sync with a weather app (I typically
don\'t carry my phone during my workouts). Also, it seems like cadence
is not captured either. Fortunately, I didn\'t suffered from any strokes
during my workouts. I can confirm this is true :)

Let\'s drop this column:
:::

::: {.cell .code execution_count="98"}
``` python
df_activities.dropna(axis=1, how='all', inplace = True)
```
:::

::: {.cell .code execution_count="99"}
``` python
df_activities.head()
```

::: {.output .execute_result execution_count="99"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Start Time</th>
      <th>End Time</th>
      <th>Activity ID</th>
      <th>Activity Name</th>
      <th>Description</th>
      <th>Location Name</th>
      <th>Time Zone</th>
      <th>Offset</th>
      <th>Duration (h:m:s)</th>
      <th>Elapsed Duration (h:m:s)</th>
      <th>...</th>
      <th>Max. Heart Rate (bpm)</th>
      <th>Average Heart Rate (bpm)</th>
      <th>Calories</th>
      <th>VO2max</th>
      <th>Aerobic Training Effect</th>
      <th>Anaerobic Training Effect</th>
      <th>Avg. Run Cadence</th>
      <th>Max. Run Cadence</th>
      <th>Stride Length</th>
      <th>Steps</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2025-07-23T18:27:35+02:00</td>
      <td>2025-07-23T19:58:26+02:00</td>
      <td>19826750062</td>
      <td>Fuerza</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Europe/Paris</td>
      <td>+02:00</td>
      <td>01:30:51</td>
      <td>01:30:51</td>
      <td>...</td>
      <td>140.0</td>
      <td>103.0</td>
      <td>396.0</td>
      <td>NaN</td>
      <td>0.7</td>
      <td>0.5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>396.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2025-07-21T18:25:11+02:00</td>
      <td>2025-07-21T19:55:55+02:00</td>
      <td>19804847077</td>
      <td>Fuerza</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Europe/Paris</td>
      <td>+02:00</td>
      <td>01:30:44</td>
      <td>01:30:44</td>
      <td>...</td>
      <td>137.0</td>
      <td>97.0</td>
      <td>424.0</td>
      <td>NaN</td>
      <td>0.4</td>
      <td>0.1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>398.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2025-07-20T09:53:48+02:00</td>
      <td>2025-07-20T11:27:52+02:00</td>
      <td>19790028215</td>
      <td>Fuerza</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Europe/Paris</td>
      <td>+02:00</td>
      <td>01:34:04</td>
      <td>01:34:04</td>
      <td>...</td>
      <td>142.0</td>
      <td>103.0</td>
      <td>497.0</td>
      <td>NaN</td>
      <td>0.8</td>
      <td>0.2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>538.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2025-07-19T22:00:54+02:00</td>
      <td>2025-07-19T22:06:28+02:00</td>
      <td>19785993661</td>
      <td>Relajación y concentración (c</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Europe/Paris</td>
      <td>+02:00</td>
      <td>00:05:34</td>
      <td>00:05:34</td>
      <td>...</td>
      <td>84.0</td>
      <td>72.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2025-07-19T09:48:34+02:00</td>
      <td>2025-07-19T10:28:34+02:00</td>
      <td>19779226448</td>
      <td>Bicicleta estática</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Europe/Paris</td>
      <td>+02:00</td>
      <td>00:40:00</td>
      <td>00:40:00</td>
      <td>...</td>
      <td>129.0</td>
      <td>112.0</td>
      <td>298.0</td>
      <td>NaN</td>
      <td>1.2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 43 columns</p>
</div>
```
:::
:::

::: {.cell .code execution_count="100"}
``` python
df_activities.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 159 entries, 0 to 158
    Data columns (total 43 columns):
     #   Column                                 Non-Null Count  Dtype  
    ---  ------                                 --------------  -----  
     0   Start Time                             159 non-null    object 
     1   End Time                               159 non-null    object 
     2   Activity ID                            159 non-null    int64  
     3   Activity Name                          159 non-null    object 
     4   Description                            1 non-null      object 
     5   Location Name                          69 non-null     object 
     6   Time Zone                              159 non-null    object 
     7   Offset                                 159 non-null    object 
     8   Duration (h:m:s)                       159 non-null    object 
     9   Elapsed Duration (h:m:s)               159 non-null    object 
     10  Moving Duration (h:m:s)                154 non-null    object 
     11  Activity Parent                        159 non-null    object 
     12  Activity Type                          159 non-null    object 
     13  Event Type                             159 non-null    object 
     14  Device                                 158 non-null    object 
     15  Privacy                                159 non-null    object 
     16  File Format                            159 non-null    object 
     17  Distance (km)                          75 non-null     float64
     18  Average Speed (km/h)                   75 non-null     float64
     19  Average Speed (km/h or min/km)         75 non-null     object 
     20  Average Moving Speed (km/h)            73 non-null     float64
     21  Average Moving Speed (km/h or min/km)  73 non-null     object 
     22  Max. Speed (km/h)                      73 non-null     float64
     23  Max. Speed (km/h or min/km)            73 non-null     object 
     24  Elevation Gain (m)                     67 non-null     float64
     25  Elevation Loss (m)                     68 non-null     float64
     26  Elevation Min. (m)                     68 non-null     float64
     27  Elevation Max. (m)                     68 non-null     float64
     28  Elevation Corrected                    159 non-null    bool   
     29  Begin Latitude (°DD)                   69 non-null     float64
     30  Begin Longitude (°DD)                  69 non-null     float64
     31  End Latitude (°DD)                     69 non-null     float64
     32  End Longitude (°DD)                    69 non-null     float64
     33  Max. Heart Rate (bpm)                  158 non-null    float64
     34  Average Heart Rate (bpm)               158 non-null    float64
     35  Calories                               158 non-null    float64
     36  VO2max                                 66 non-null     float64
     37  Aerobic Training Effect                157 non-null    float64
     38  Anaerobic Training Effect              46 non-null     float64
     39  Avg. Run Cadence                       68 non-null     float64
     40  Max. Run Cadence                       68 non-null     float64
     41  Stride Length                          64 non-null     float64
     42  Steps                                  154 non-null    float64
    dtypes: bool(1), float64(22), int64(1), object(19)
    memory usage: 52.5+ KB
:::
:::

::: {.cell .code execution_count="101"}
``` python
sum_cols = ['Steps','Calories','Elevation Gain (m)',
            'Distance (km)']
df_activities[sum_cols].sum()
```

::: {.output .execute_result execution_count="101"}
    Steps                 533452.00000
    Calories               79428.00000
    Elevation Gain (m)      1904.00000
    Distance (km)            560.73863
    dtype: float64
:::
:::

::: {.cell .code execution_count="102"}
``` python
import matplotlib.pyplot as plt

counts = df_activities['Activity Type'].value_counts()

plt.figure()
plt.bar(counts.index, counts.values)
plt.xlabel('Activity Name')
plt.ylabel('Count')
plt.title('Number of Activities done per Activity Type')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show
```

::: {.output .execute_result execution_count="102"}
    <function matplotlib.pyplot.show(close=None, block=None)>
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/f86a4ce5750a96673636cd3177c71af0c484d237.png)
:::
:::

::: {.cell .markdown}
As I already expected, most of my Fitness activities are lonely :(

I typically just work-out in the gym or simply go for a run. Let\'s see
how these two activities compare in terms of Average Heart Beat and
Maximum Heart Beat.
:::

::: {.cell .code execution_count="103"}
``` python
bins = 20

strength_averageHR = df_activities[df_activities['Activity Type'] == 'Strength Training']['Average Heart Rate (bpm)'].dropna()
running_averageHR  = df_activities[df_activities['Activity Type'] == 'Running']['Average Heart Rate (bpm)'].dropna()

plt.figure(figsize=(8, 5))

# Running Average HR Hist
plt.hist(running_averageHR, bins=bins, 
         color='blue',        
         alpha=0.6,           
         edgecolor='black',   
         label='Running')

# Gym Average HR Hist
plt.hist(strength_averageHR, bins=bins, 
         color='red', 
         alpha=0.6, 
         edgecolor='black',
         label='Strength Training')

# Lables and title
plt.xlabel('Average Heart Rate (bpm)')
plt.ylabel('Frequency')
plt.title('Average Heart rate per Activity Type')

# Legend, grif and layout
plt.legend()
plt.grid(axis='y', linestyle='--', alpha=0.4)
plt.tight_layout()
plt.show()
```

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/47c9d44b14e6ca3c365bd27fe4d444ec23a80154.png)
:::
:::

::: {.cell .code execution_count="104"}
``` python
bins = 20

strength_maxHR = df_activities[df_activities['Activity Type'] == 'Strength Training']['Max. Heart Rate (bpm)'].dropna()
running_maxHR  = df_activities[df_activities['Activity Type'] == 'Running']['Max. Heart Rate (bpm)'].dropna()

plt.figure(figsize=(8, 5))

# Running Average HR Hist
plt.hist(running_maxHR, bins=bins, 
         color='blue',        
         alpha=0.6,           
         edgecolor='black',   
         label='Running')

# Gym Average HR Hist
plt.hist(strength_maxHR, bins=bins, 
         color='red', 
         alpha=0.6, 
         edgecolor='black',
         label='Strength Training')

# Lables and title
plt.xlabel('Maximum Heart Rate (bpm)')
plt.ylabel('Frequency')
plt.title('Maximum Heart rate per Activity Type')

# Legend, grif and layout
plt.legend()
plt.grid(axis='y', linestyle='--', alpha=0.4)
plt.tight_layout()
plt.show()
```

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/d066da51a3eca4f745b7d3d493e018dbaf1aa1e3.png)
:::
:::

::: {.cell .markdown}
It seems like, on average, my heart rate is higher during cardio
activities. However, the difference is shorten when taking into account
Maximum Heart Rate recorded during a single session. This is probably
due to heavy sets in Leg Day. Squats and Deadlifts are exhausting! It
could be great if Garmin\'s autodetection system for recording which
type of excercise is being performed (e.g squat, deadlift, bench press,
etc.) could work better, so I could extract more insights regarding my
WOs.

Now, let\'s check when I mostly train!
:::

::: {.cell .code execution_count="105"}
``` python
df_activities['Start Time'] = pd.to_datetime(df_activities['Start Time'], utc = True)
```
:::

::: {.cell .code execution_count="106"}
``` python
# Determine day of the week --> Monday, Tuesday, Wednesday, etc.
df_activities['Weekday'] = df_activities['Start Time'].dt.day_name() 

# Filter data we need
mask = df_activities['Activity Type'].isin(['Running', 'Strength Training'])
sub = df_activities.loc[mask, ['Weekday', 'Activity Type']]

# Count sessions per weekday and group by activit type
counts = sub.groupby(['Weekday', 'Activity Type']) \
            .size() \
            .unstack(fill_value=0)

# Reorder the Weekdays
ordered = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
counts = counts.reindex(ordered)

# Plot
plt.figure(figsize=(10, 6))
counts.plot(kind='bar', 
            stacked=True, 
            color=['blue', 'red'],   # running=blue, strength=red
            edgecolor='black')

plt.xlabel('Day of Week')
plt.ylabel('Number of Sessions')
plt.title('Entrenamientos por Día de la Semana y Tipo de Actividad')
plt.legend(title='Activity Type')
plt.xticks(rotation=45, ha='right')
plt.grid(axis='y', linestyle='--', alpha=0.4)
plt.tight_layout()
plt.show()
```

::: {.output .display_data}
    <Figure size 1000x600 with 0 Axes>
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/a8915b2fb9345d6e9cad4102164931b42522ffa9.png)
:::
:::

::: {.cell .markdown}
We can repeat this excercise for the Months since I bought my Garmin
wtach!
:::

::: {.cell .code execution_count="107"}
``` python
# Create a new column for Month - Year
df_activities['YearMonth'] = df_activities['Start Time'].dt.to_period('M')

mask = df_activities['Activity Type'].isin(['Running', 'Strength Training'])
sub = df_activities.loc[mask, ['YearMonth', 'Activity Type']]

counts = (
    sub
    .groupby(['YearMonth', 'Activity Type'])
    .size()
    .unstack(fill_value=0)
    .sort_index()
)

all_months = pd.period_range(
    start=df_activities['YearMonth'].min(),
    end=  df_activities['YearMonth'].max(),
    freq='M'
)

counts = counts.reindex(all_months, fill_value=0)

labels = [p.strftime('%B %Y') for p in counts.index]

plt.figure(figsize=(12, 6))
counts.plot(
    kind='bar',
    stacked=True,
    edgecolor='black',
    color=['blue','red'],
    alpha=0.8,
    legend=True,
    ax=plt.gca()
)
plt.xlabel('Month – Year')
plt.ylabel('Number of sessions')
plt.title('Number of Workouts per month and activity Type')
plt.xticks(ticks=range(len(labels)), labels=labels, rotation=45, ha='right')
plt.grid(axis='y', linestyle='--', alpha=0.4)
plt.tight_layout()
plt.show()
```

::: {.output .stream .stderr}
    C:\Users\alexa\AppData\Local\Temp\ipykernel_23264\4045094107.py:2: UserWarning: Converting to PeriodArray/Index representation will drop timezone information.
      df_activities['YearMonth'] = df_activities['Start Time'].dt.to_period('M')
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/7ba457c2773c2922ba7581791c80d7b3181c3764.png)
:::
:::

::: {.cell .markdown}
This result was a little bit unexpected for me, since I have the feeling
I\'ve been working out more time lately. However, the number of sessions
I\'ve had since April has been dropping. Let\'s check how long have my
sessions been during this months.
:::

::: {.cell .code execution_count="108"}
``` python
# Convert duration to timedelta datatype
df_activities['Duration_td'] = pd.to_timedelta(df_activities['Duration (h:m:s)'])


# Keep the data that we need (only Running and Activity type, see mask df in previous chunk)
sub = df_activities.loc[mask, ['YearMonth', 'Activity Type', 'Duration_td']]

# Group and sum
duration_sum = (
    sub
    .groupby(['YearMonth', 'Activity Type'])['Duration_td']
    .sum()
    .unstack(fill_value=pd.Timedelta(0))
    .sort_index()
)

# Covert to hours
duration_hours = duration_sum / pd.Timedelta(hours=1)

# Plot
plt.figure(figsize=(12, 6))
duration_hours.plot(
    kind='bar',
    stacked=True,
    edgecolor='black',
    color = ["blue","red"]
)

labels = [p.strftime('%B %Y') for p in counts.index]
plt.xlabel('Month – Year')
plt.ylabel('Total workout hours')
plt.title('Workout duration by activity type')
plt.xticks(
    ticks=range(len(duration_hours.index)),
    labels=labels,
    rotation=45,
    ha='right'
)
plt.legend(title='Activity Type')
plt.grid(axis='y', linestyle='--', alpha=0.4)
plt.tight_layout()
plt.show()
```

::: {.output .display_data}
    <Figure size 1200x600 with 0 Axes>
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/a77696fc76b8d314548daad57759ffb3a5b5ccd5.png)
:::
:::

::: {.cell .code execution_count="109"}
``` python
# Swap to %
percent = counts.div(counts.sum(axis=1), axis=0) * 100

# Plot
ax = percent.plot(
    kind='bar',
    stacked=True,
    edgecolor='black',
    color = ["blue", "red"],
    figsize=(12, 6)
)
ax.set_xlabel('Month – Year')
ax.set_ylabel('Sessions (%)')
ax.set_title('Porcentage of sessions per Activity type')
ax.set_xticks(range(len(percent.index)))
ax.set_xticklabels([p.strftime('%B %Y') for p in percent.index], rotation=45, ha='right')
ax.legend(title='Activity Type', loc='upper right')
ax.grid(axis='y', linestyle='--', alpha=0.4)

plt.tight_layout()
plt.show()
```

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/c96b3f258ebe9352731a46cb44ae2465442be932.png)
:::
:::

::: {.cell .markdown}
Lastly, I think it would be interesting to see how many kilometers I run
per month. I expect to see a decrease after my last Half Marathon in
April.
:::

::: {.cell .code execution_count="110"}
``` python
df_run = df_activities[df_activities['Activity Type'] == 'Running']
sub = df_run.loc[mask, ['YearMonth', 'Distance (km)']]

distance_sum = (
    sub
    .groupby('YearMonth')['Distance (km)']
    .sum()
    .sort_index()
)

labels = [p.strftime('%B %Y') for p in distance_sum.index]

x = list(range(len(distance_sum)))

plt.figure(figsize=(12, 6))
plt.plot(
    x, 
    distance_sum.values, 
    marker='o',      # each value is a point
    linestyle='-',   # line
    linewidth=2,     
    markersize=6     
)

plt.xlabel('Month – Year')
plt.ylabel('Distance run (km)')
plt.title('Kilometers run per month')
plt.xticks(
    ticks=x,
    labels=labels,
    rotation=45,
    ha='right'
)
plt.grid(axis='y', linestyle='--', alpha=0.4)
plt.tight_layout()
plt.show()
```

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/3779f8d505d606a297176de7bd112f14ba922c5b.png)
:::
:::

::: {.cell .markdown}
### Sleep Data

It has been widely demonstrated that, sleep quality is key for
performance. Let\'s check my sleep data!
:::

::: {.cell .code execution_count="111"}
``` python
df_sleep = pd.read_csv("C:/Users/alexa/OneDrive/Documents/Code/Python/Projects/Garmin Data/data/DI_CONNECT/DI-Connect-Wellness/sleepData/ConfirmedSleepDataEntries.csv")
df_sleep.shape
```

::: {.output .execute_result execution_count="111"}
    (232, 20)
:::
:::

::: {.cell .code execution_count="112"}
``` python
df_sleep.head()
```

::: {.output .execute_result execution_count="112"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sleepStartTimestampGMT</th>
      <th>sleepEndTimestampGMT</th>
      <th>calendarDate</th>
      <th>sleepWindowConfirmationType</th>
      <th>deepSleepSeconds</th>
      <th>lightSleepSeconds</th>
      <th>remSleepSeconds</th>
      <th>awakeSleepSeconds</th>
      <th>unmeasurableSeconds</th>
      <th>averageRespiration</th>
      <th>lowestRespiration</th>
      <th>highestRespiration</th>
      <th>retro</th>
      <th>awakeCount</th>
      <th>avgSleepStress</th>
      <th>sleepScores</th>
      <th>restlessMomentCount</th>
      <th>napList</th>
      <th>spo2SleepSummary</th>
      <th>breathingDisruptionSeverity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2024-12-04 23:25:40</td>
      <td>2024-12-05 06:54:40</td>
      <td>2024-12-05</td>
      <td>ENHANCED_CONFIRMED_FINAL</td>
      <td>4080.0</td>
      <td>15960.0</td>
      <td>5880.0</td>
      <td>1020.0</td>
      <td>0.0</td>
      <td>13.0</td>
      <td>10.0</td>
      <td>15.0</td>
      <td>False</td>
      <td>0.0</td>
      <td>10.730000</td>
      <td>{'overallScore': 85, 'qualityScore': 91, 'dura...</td>
      <td>33.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2024-12-06 05:54:15</td>
      <td>2024-12-06 10:07:15</td>
      <td>2024-12-06</td>
      <td>ENHANCED_CONFIRMED_FINAL</td>
      <td>4380.0</td>
      <td>7860.0</td>
      <td>NaN</td>
      <td>2940.0</td>
      <td>0.0</td>
      <td>14.0</td>
      <td>11.0</td>
      <td>15.0</td>
      <td>False</td>
      <td>3.0</td>
      <td>60.810001</td>
      <td>{'overallScore': 26, 'qualityScore': 38, 'dura...</td>
      <td>12.0</td>
      <td>[{'napTimeSec': 7320, 'napStartTimestampGMT': ...</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2024-12-07 00:33:50</td>
      <td>2024-12-07 08:22:50</td>
      <td>2024-12-07</td>
      <td>ENHANCED_CONFIRMED_FINAL</td>
      <td>3300.0</td>
      <td>17460.0</td>
      <td>7380.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>8.0</td>
      <td>15.0</td>
      <td>False</td>
      <td>0.0</td>
      <td>16.650000</td>
      <td>{'overallScore': 87, 'qualityScore': 88, 'dura...</td>
      <td>28.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2024-12-07 22:38:31</td>
      <td>2024-12-08 07:13:25</td>
      <td>2024-12-08</td>
      <td>ENHANCED_CONFIRMED_FINAL</td>
      <td>3540.0</td>
      <td>20040.0</td>
      <td>7260.0</td>
      <td>60.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>9.0</td>
      <td>16.0</td>
      <td>False</td>
      <td>0.0</td>
      <td>17.020000</td>
      <td>{'overallScore': 88, 'qualityScore': 87, 'dura...</td>
      <td>32.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2024-12-08 21:08:05</td>
      <td>2024-12-09 05:55:05</td>
      <td>2024-12-09</td>
      <td>ENHANCED_CONFIRMED_FINAL</td>
      <td>4140.0</td>
      <td>21900.0</td>
      <td>4860.0</td>
      <td>720.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>8.0</td>
      <td>15.0</td>
      <td>False</td>
      <td>1.0</td>
      <td>24.900000</td>
      <td>{'overallScore': 77, 'qualityScore': 73, 'dura...</td>
      <td>45.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
After taking a look at the dataframe, we see that some variables have
some other variables nested. In order to unstack them, we will modify
the original script. I\'ve seen that the napList variable has the same
issue, so it is needed to adjust accordingly as well. Instead of
obatining a CSV file, let\'s load the data directly in a Pandas
dataframe.
:::

::: {.cell .code execution_count="113"}
``` python
import glob
import pandas as pd

def loadData():
    folderLocation = (
        'C:/Users/alexa/OneDrive/Documents/Code/Python/'
        'Projects/Garmin Data/data/DI_CONNECT/DI-Connect-Wellness/'
        'sleepData/*.json'
    )
    fileNames = glob.glob(folderLocation)

    sleepData = pd.DataFrame()
    for sleepFile in fileNames:
        data = pd.read_json(sleepFile)
        sleepData = pd.concat([sleepData, data], ignore_index=True, sort=False)

    # Convert to datetime datatype
    sleepData['sleepStartTimestampGMT'] = pd.to_datetime(sleepData['sleepStartTimestampGMT'])
    sleepData['sleepEndTimestampGMT']   = pd.to_datetime(sleepData['sleepEndTimestampGMT'])
    sleepData['calendarDate']           = pd.to_datetime(sleepData['calendarDate'])

    # Unstack sleepScores
    scores_df = pd.json_normalize(sleepData['sleepScores'])
    sleepData = pd.concat([sleepData.drop(columns=['sleepScores']), scores_df], axis=1)

    # Unstack napList
    #    Asegurarnos de que siempre sea lista (para no romper explode) --> check!
    sleepData['napList'] = sleepData['napList'].apply(lambda x: x if isinstance(x, list) else [])
    #    Explode: cada elemento de la lista pasa a una fila nueva
    sleep_naps = sleepData.explode('napList').reset_index(drop=True)
    #    Normalizar el diccionario en columnas
    nap_df = pd.json_normalize(sleep_naps['napList'])
    #    Opcional: renombrar para que no choque con otras columnas
    nap_df = nap_df.add_prefix('nap_')
    #    Concatenar y eliminar la vieja columna de lista
    sleep_naps = pd.concat([sleep_naps.drop(columns=['napList']), nap_df], axis=1)

    return sleep_naps

def removeUnconfirmedSleepEntries(df):
    return df[df['sleepWindowConfirmationType'] != 'UNCONFIRMED']

# — Uso —
if __name__ == "__main__":
    df = loadData()
    df_sleepUnstacked = removeUnconfirmedSleepEntries(df)  
```
:::

::: {.cell .markdown}
Let\'s check our dataframe after unstacking sleepScores and napList!
:::

::: {.cell .code execution_count="114"}
``` python
df_sleepUnstacked.head()
```

::: {.output .execute_result execution_count="114"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sleepStartTimestampGMT</th>
      <th>sleepEndTimestampGMT</th>
      <th>calendarDate</th>
      <th>sleepWindowConfirmationType</th>
      <th>deepSleepSeconds</th>
      <th>lightSleepSeconds</th>
      <th>remSleepSeconds</th>
      <th>awakeSleepSeconds</th>
      <th>unmeasurableSeconds</th>
      <th>averageRespiration</th>
      <th>...</th>
      <th>awakeTimeScore</th>
      <th>combinedAwakeScore</th>
      <th>restfulnessScore</th>
      <th>interruptionsScore</th>
      <th>feedback</th>
      <th>insight</th>
      <th>nap_napTimeSec</th>
      <th>nap_napStartTimestampGMT</th>
      <th>nap_napEndTimestampGMT</th>
      <th>nap_calendarDate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2024-12-04 23:25:40</td>
      <td>2024-12-05 06:54:40</td>
      <td>2024-12-05</td>
      <td>ENHANCED_CONFIRMED_FINAL</td>
      <td>4080.0</td>
      <td>15960.0</td>
      <td>5880.0</td>
      <td>1020.0</td>
      <td>0.0</td>
      <td>13.0</td>
      <td>...</td>
      <td>84.0</td>
      <td>92.0</td>
      <td>89.0</td>
      <td>92.0</td>
      <td>POSITIVE_LONG_AND_RECOVERING</td>
      <td>POSITIVE_LATE_BED_TIME</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2024-12-06 05:54:15</td>
      <td>2024-12-06 10:07:15</td>
      <td>2024-12-06</td>
      <td>ENHANCED_CONFIRMED_FINAL</td>
      <td>4380.0</td>
      <td>7860.0</td>
      <td>NaN</td>
      <td>2940.0</td>
      <td>0.0</td>
      <td>14.0</td>
      <td>...</td>
      <td>51.0</td>
      <td>56.0</td>
      <td>100.0</td>
      <td>63.0</td>
      <td>NEGATIVE_SHORT_AND_POOR_QUALITY</td>
      <td>NONE</td>
      <td>7320.0</td>
      <td>2024-12-06T14:51:50.0</td>
      <td>2024-12-06T16:53:50.0</td>
      <td>2024-12-06</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2024-12-07 00:33:50</td>
      <td>2024-12-07 08:22:50</td>
      <td>2024-12-07</td>
      <td>ENHANCED_CONFIRMED_FINAL</td>
      <td>3300.0</td>
      <td>17460.0</td>
      <td>7380.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>...</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>99.0</td>
      <td>100.0</td>
      <td>POSITIVE_LONG_AND_REFRESHING</td>
      <td>NONE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2024-12-07 22:38:31</td>
      <td>2024-12-08 07:13:25</td>
      <td>2024-12-08</td>
      <td>ENHANCED_CONFIRMED_FINAL</td>
      <td>3540.0</td>
      <td>20040.0</td>
      <td>7260.0</td>
      <td>60.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>...</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>98.0</td>
      <td>100.0</td>
      <td>POSITIVE_LONG_AND_CONTINUOUS</td>
      <td>NONE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2024-12-08 21:08:05</td>
      <td>2024-12-09 05:55:05</td>
      <td>2024-12-09</td>
      <td>ENHANCED_CONFIRMED_FINAL</td>
      <td>4140.0</td>
      <td>21900.0</td>
      <td>4860.0</td>
      <td>720.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>...</td>
      <td>91.0</td>
      <td>89.0</td>
      <td>83.0</td>
      <td>87.0</td>
      <td>POSITIVE_LONG_AND_CONTINUOUS</td>
      <td>POSITIVE_EXERCISE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 36 columns</p>
</div>
```
:::
:::

::: {.cell .code execution_count="115"}
``` python
df_sleepUnstacked.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 233 entries, 0 to 232
    Data columns (total 36 columns):
     #   Column                       Non-Null Count  Dtype         
    ---  ------                       --------------  -----         
     0   sleepStartTimestampGMT       224 non-null    datetime64[ns]
     1   sleepEndTimestampGMT         224 non-null    datetime64[ns]
     2   calendarDate                 224 non-null    datetime64[ns]
     3   sleepWindowConfirmationType  224 non-null    object        
     4   deepSleepSeconds             224 non-null    float64       
     5   lightSleepSeconds            224 non-null    float64       
     6   remSleepSeconds              214 non-null    float64       
     7   awakeSleepSeconds            224 non-null    float64       
     8   unmeasurableSeconds          224 non-null    float64       
     9   averageRespiration           224 non-null    float64       
     10  lowestRespiration            224 non-null    float64       
     11  highestRespiration           224 non-null    float64       
     12  retro                        233 non-null    bool          
     13  awakeCount                   224 non-null    float64       
     14  avgSleepStress               224 non-null    float64       
     15  restlessMomentCount          224 non-null    float64       
     16  spo2SleepSummary             55 non-null     object        
     17  breathingDisruptionSeverity  34 non-null     object        
     18  overallScore                 224 non-null    float64       
     19  qualityScore                 224 non-null    float64       
     20  durationScore                224 non-null    float64       
     21  recoveryScore                224 non-null    float64       
     22  deepScore                    221 non-null    float64       
     23  remScore                     221 non-null    float64       
     24  lightScore                   221 non-null    float64       
     25  awakeningsCountScore         224 non-null    float64       
     26  awakeTimeScore               224 non-null    float64       
     27  combinedAwakeScore           224 non-null    float64       
     28  restfulnessScore             224 non-null    float64       
     29  interruptionsScore           224 non-null    float64       
     30  feedback                     224 non-null    object        
     31  insight                      224 non-null    object        
     32  nap_napTimeSec               22 non-null     float64       
     33  nap_napStartTimestampGMT     22 non-null     object        
     34  nap_napEndTimestampGMT       22 non-null     object        
     35  nap_calendarDate             22 non-null     object        
    dtypes: bool(1), datetime64[ns](3), float64(24), object(8)
    memory usage: 64.1+ KB
:::
:::

::: {.cell .markdown}
It seems like in the new dataframe, we are seeing one more row (232 on
the initial one vs. 233 on the Unstacked one). In the data extraction
code above, the explode() function was used. This means that if one has
two nap records, it would duplicate into two rows. Let\'s check on which
day I took two naps!
:::

::: {.cell .code execution_count="116"}
``` python
# Create a new variable which checks which dates have more than one nap
df_sleepUnstacked['num_naps'] = (
    df_sleepUnstacked
      .groupby('calendarDate')['nap_napTimeSec']
      .transform('count')
)

# Check which dates have two or more naps.
df_two_naps = df_sleepUnstacked[df_sleepUnstacked['num_naps'] > 1]
print(df_two_naps[['calendarDate','sleepStartTimestampGMT','sleepEndTimestampGMT' ,'nap_napStartTimestampGMT',
                   'nap_napEndTimestampGMT','num_naps']])


```

::: {.output .stream .stdout}
        calendarDate sleepStartTimestampGMT sleepEndTimestampGMT  \
    157   2025-05-11    2025-05-11 03:42:11  2025-05-11 05:39:10   
    158   2025-05-11    2025-05-11 03:42:11  2025-05-11 05:39:10   

        nap_napStartTimestampGMT nap_napEndTimestampGMT  num_naps  
    157    2025-05-11T07:11:02.0  2025-05-11T08:28:02.0       2.0  
    158    2025-05-11T09:45:02.0  2025-05-11T10:37:02.0       2.0  
:::
:::

::: {.cell .markdown}
Apparently, I did not take two naps, my Garmin watch did not detect this
data correctly\... even though it was detcetcted as Confirmed record.
That day I was in the \"Feria de Sevilla\", it makes sense my Garmin
watch was confused I did not go to sleep early! Let\'s drop this
observations, since they were not correctly gathered.
:::

::: {.cell .code execution_count="117"}
``` python
df_sleepUnstacked = df_sleepUnstacked.drop(df_sleepUnstacked.index[[157, 158]]).reset_index(drop=True)
```
:::

::: {.cell .markdown}
Now, let\'s take a look at the data! Let\'s see how much I sleep on
average, per month of the year!
:::

::: {.cell .code execution_count="118"}
``` python
df_sleepUnstacked['Sleep Duration'] = pd.to_timedelta(df_sleepUnstacked['sleepEndTimestampGMT'] - df_sleepUnstacked['sleepStartTimestampGMT'])

df_sleepUnstacked['Sleep Duration'] = df_sleepUnstacked['Sleep Duration'] / pd.Timedelta(hours=1)

print(f"On average, I sleep {round(float(df_sleepUnstacked['Sleep Duration'].mean()),2)} hours")
```

::: {.output .stream .stdout}
    On average, I sleep 7.54 hours
:::
:::

::: {.cell .code execution_count="119"}
``` python
# Create a new column for Month - Year
df_sleepUnstacked['YearMonth'] = df_sleepUnstacked['calendarDate'].dt.to_period('M')

# Create a dataframe with average per month
avg = (
    df_sleepUnstacked
    .groupby('YearMonth')['Sleep Duration']
    .mean()
    .sort_index()
)

# Create a range of all of the months
all_months = pd.period_range(
    start=df_sleepUnstacked['YearMonth'].min(),
    end=  df_sleepUnstacked['YearMonth'].max(),
    freq='M'
)

# Re-sort
avg = avg.reindex(all_months, fill_value=pd.Timedelta(0))

# Labels are printed in 'Month name - YYYY' Format
labels = [p.strftime('%B %Y') for p in avg.index]

# Plot
plt.figure(figsize=(12, 6))
avg.plot(
    kind='bar',
    edgecolor='black',
    alpha=0.8,
    ax=plt.gca()
)
plt.xlabel('Month – Year')
plt.ylabel('Average sleep (h)')
plt.title('Average sleep per month')
plt.xticks(ticks=range(len(labels)), labels=labels, rotation=45, ha='right')
plt.grid(axis='y', linestyle='--', alpha=0.4)
plt.tight_layout()
plt.axhline(
    float(df_sleepUnstacked['Sleep Duration'].mean()),
    color='red',
    linestyle='--',
    linewidth=1.5,
)
plt.show()
```

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/fb30d7a0d0638aa9f39abc752b9616e1db53df5f.png)
:::
:::

::: {.cell .markdown}
Another interesting metric Garmin captures is the sleep Quality Score.
This Quality score is based on different parametrs, such as REM sleep,
deep sleep, duration, awakness time, breathing, etc. Let\'s see the
quality of my sleep based on different months of the year, or even if
there is a difference between the weekends and the working days!
:::

::: {.cell .code execution_count="120"}
``` python
df_sleepCopy = df_sleepUnstacked.copy()

# Compute weekends and weekdays and months
df_sleepCopy['weekday'] = df_sleepCopy['calendarDate'].dt.day_name()
df_sleepCopy['is_weekend'] = df_sleepCopy['weekday'].isin(['Saturday', 'Sunday'])
df_sleepCopy['month'] = df_sleepCopy['calendarDate'].dt.month

# Average quality score per weekend/day
weekday_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
df_sleepCopy['weekday'] = pd.Categorical(df_sleepCopy['weekday'], categories=weekday_order, ordered=True)
weekday_avg = df_sleepCopy.groupby('weekday')['qualityScore'].mean().reindex(weekday_order)

# Plot n1: average per day of the week
plt.figure()
weekday_avg.plot(kind='bar')
plt.xlabel('Day of the wek')
plt.ylabel('Quality Score')
plt.title('Average Sleep Quality Score per day of the week')
plt.grid(True, linestyle='--', alpha=0.5)
plt.show()

# Plot n2: boxplot weekend/day
plt.figure()
df_sleepCopy.boxplot(column='qualityScore', by='is_weekend')
plt.title('Sleep Quality Socre: weekends vs weekdays')
plt.suptitle('')
plt.xlabel('is_weekend')
plt.ylabel('qualityScore')
plt.show()

# Plot n3: tendency
monthly_avg = df_sleepCopy.groupby('month')['qualityScore'].mean().sort_index()
plt.figure()
plt.plot(monthly_avg.index, monthly_avg.values, marker='o')
plt.xlabel('Month')
plt.ylabel('Average Sleep Quality Scofe')
plt.title('Mensual Tendency')
plt.xticks(monthly_avg.index)
plt.grid(True, linestyle='--', alpha=0.5)
plt.show()
```

::: {.output .stream .stderr}
    C:\Users\alexa\AppData\Local\Temp\ipykernel_23264\289716103.py:11: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
      weekday_avg = df_sleepCopy.groupby('weekday')['qualityScore'].mean().reindex(weekday_order)
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/28022c3e104307f96ef48092231eef211823ad94.png)
:::

::: {.output .display_data}
    <Figure size 640x480 with 0 Axes>
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/04211c8bc58b8d7ac5d4ff43c7a888050fa2cf4e.png)
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/8a4ecd58ba6984bee22b30771bf7823dc1287c11.png)
:::
:::

::: {.cell .markdown}
Although I sleep quite enough, on average. However, I want to examine
when I typically go to sleep and when I usually wake up. To do so, first
of all, I need to convert the GMT timezone to CET/CEST!
:::

::: {.cell .code execution_count="121"}
``` python
import pandas as pd

# Copiamos el DataFrame original
df = df_sleepUnstacked.copy()

# 1) Marcamos que los timestamps están en UTC:
df['sleepStartUTC'] = df['sleepStartTimestampGMT'].dt.tz_localize('UTC')
df['sleepEndUTC']   = df['sleepEndTimestampGMT'].dt.tz_localize('UTC')

# 2) Convertimos a la zona de Europa/Madrid (CET/CEST automáticamente según fecha)
df['sleepStartLocal'] = (
    df['sleepStartUTC']
      .dt.tz_convert('Europe/Madrid')
      .dt.tz_localize(None)   # opcional: quita la info de tz, dejando naive datetime
)
df['sleepEndLocal'] = (
    df['sleepEndUTC']
      .dt.tz_convert('Europe/Madrid')
      .dt.tz_localize(None)
)

# Resultado: sleepStartLocal y sleepEndLocal reflejan la hora local,
# y si al convertir cruzan la medianoche, la fecha en el propio timestamp se ajusta.
# calendarDate permanece tal cual.

# Plot start and end time!
```
:::

::: {.cell .markdown}
### GPS Tracking Map
:::

::: {.cell .markdown}
With the data that has been pulled, we can plot all the runs I\'ve made
using the GPS system Garmin has integrated in their Watches. Due to
privacy reasons (I would not like to see published on the Internet my
Address!), I will show only my Half Marathon in Seville :)
:::

::: {.cell .code execution_count="122"}
``` python
%pip install gpxpy
```

::: {.output .stream .stdout}
    Requirement already satisfied: gpxpy in c:\users\alexa\appdata\local\programs\python\python313\lib\site-packages (1.6.2)
    Note: you may need to restart the kernel to use updated packages.
:::

::: {.output .stream .stderr}

    [notice] A new release of pip is available: 25.1.1 -> 25.2
    [notice] To update, run: python.exe -m pip install --upgrade pip
:::
:::

::: {.cell .code execution_count="123"}
``` python
import glob
import gpxpy
import folium
from scipy.interpolate import interp1d
from matplotlib import colormaps
```
:::

::: {.cell .code execution_count="124"}
``` python
files = glob.glob('C:/Users/alexa/OneDrive/Documents/Code/Python/Projects/Garmin Data/garmin-connect-export-master/2025-07-25_garmin_connect_export/activity_18104317620.gpx')
cmap = colormaps['Spectral']
inter = interp1d([0, 365], [0, 1])

feature_group = folium.FeatureGroup()
all_coords = []

for file in files:
    with open(file, 'r') as gpx_file:
        gpx = gpxpy.parse(gpx_file)

    if gpx.tracks and gpx.tracks[0].type == 'running':
        try:
            coords = [(p.latitude, p.longitude)
                      for p in gpx.tracks[0].segments[0].points]
            all_coords.extend(coords)

            # color by day-of-year
            day = gpx.get_time_bounds()[0].timetuple().tm_yday
            rgb = cmap(inter(day))
            rgb = '#%02x%02x%02x' % tuple(round(255*x) for x in rgb[:-1])

            folium.PolyLine(coords, weight=0.5, color=rgb).add_to(feature_group)
        except Exception:
            pass

# Compute center of all points
if all_coords:
    lats, lons = zip(*all_coords)
    center = [sum(lats) / len(lats), sum(lons) / len(lons)]
else:
    center = [0, 0]  # fallback

route_map = folium.Map(
    location=center,
    zoom_start=12,
    tiles='CartodbDarkMatterNoLabels',
)

feature_group.add_to(route_map)
route_map
```

::: {.output .execute_result execution_count="124"}
```{=html}
<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe srcdoc="&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    
    &lt;meta http-equiv=&quot;content-type&quot; content=&quot;text/html; charset=UTF-8&quot; /&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://code.jquery.com/jquery-3.7.1.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js&quot;&gt;&lt;/script&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap-glyphicons.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.2.0/css/all.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/gh/python-visualization/folium/folium/templates/leaflet.awesome.rotate.min.css&quot;/&gt;
    
            &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width,
                initial-scale=1.0, maximum-scale=1.0, user-scalable=no&quot; /&gt;
            &lt;style&gt;
                #map_ebb9fb23349eacb25aa5b5a390e550be {
                    position: relative;
                    width: 100.0%;
                    height: 100.0%;
                    left: 0.0%;
                    top: 0.0%;
                }
                .leaflet-container { font-size: 1rem; }
            &lt;/style&gt;

            &lt;style&gt;html, body {
                width: 100%;
                height: 100%;
                margin: 0;
                padding: 0;
            }
            &lt;/style&gt;

            &lt;style&gt;#map {
                position:absolute;
                top:0;
                bottom:0;
                right:0;
                left:0;
                }
            &lt;/style&gt;

            &lt;script&gt;
                L_NO_TOUCH = false;
                L_DISABLE_3D = false;
            &lt;/script&gt;

        
&lt;/head&gt;
&lt;body&gt;
    
    
            &lt;div class=&quot;folium-map&quot; id=&quot;map_ebb9fb23349eacb25aa5b5a390e550be&quot; &gt;&lt;/div&gt;
        
&lt;/body&gt;
&lt;script&gt;
    
    
            var map_ebb9fb23349eacb25aa5b5a390e550be = L.map(
                &quot;map_ebb9fb23349eacb25aa5b5a390e550be&quot;,
                {
                    center: [37.39047642340871, -5.998092854310256],
                    crs: L.CRS.EPSG3857,
                    ...{
  &quot;zoom&quot;: 12,
  &quot;zoomControl&quot;: true,
  &quot;preferCanvas&quot;: false,
}

                }
            );

            

        
    
            var tile_layer_0182d81ee6f5ccfa88d5d0106a1199ce = L.tileLayer(
                &quot;https://{s}.basemaps.cartocdn.com/dark_nolabels/{z}/{x}/{y}{r}.png&quot;,
                {
  &quot;minZoom&quot;: 0,
  &quot;maxZoom&quot;: 20,
  &quot;maxNativeZoom&quot;: 20,
  &quot;noWrap&quot;: false,
  &quot;attribution&quot;: &quot;\u0026copy; \u003ca href=\&quot;https://www.openstreetmap.org/copyright\&quot;\u003eOpenStreetMap\u003c/a\u003e contributors \u0026copy; \u003ca href=\&quot;https://carto.com/attributions\&quot;\u003eCARTO\u003c/a\u003e&quot;,
  &quot;subdomains&quot;: &quot;abcd&quot;,
  &quot;detectRetina&quot;: false,
  &quot;tms&quot;: false,
  &quot;opacity&quot;: 1,
}

            );
        
    
            tile_layer_0182d81ee6f5ccfa88d5d0106a1199ce.addTo(map_ebb9fb23349eacb25aa5b5a390e550be);
        
    
            var feature_group_41640e6b40f07bf7b78b1ecd19592d11 = L.featureGroup(
                {
}
            );
        
    
            var poly_line_76d81616a9100b10ac87cb9c1b650099 = L.polyline(
                [[37.37273926846683, -5.98989618010819], [37.37275385297835, -5.9899144526571035], [37.37277061678469, -5.989929372444749], [37.37279358319938, -5.9899436216801405], [37.37281537614763, -5.9899577870965], [37.37283834256232, -5.989969940856099], [37.37286767922342, -5.989986956119537], [37.37289173528552, -5.990004641935229], [37.372912941500545, -5.990019729360938], [37.37293724901974, -5.990034900605679], [37.372960299253464, -5.9900452103465796], [37.37298452295363, -5.990055939182639], [37.37301059067249, -5.990070691332221], [37.37302986904979, -5.990084856748581], [37.37305300310254, -5.990101033821702], [37.3730777297169, -5.990119390189648], [37.37309751100838, -5.990130286663771], [37.373121147975326, -5.990144619718194], [37.37314788624644, -5.990159707143903], [37.37316791899502, -5.990168508142233], [37.37318971194327, -5.99018108099699], [37.37321603111923, -5.990197006613016], [37.37324008718133, -5.990207986906171], [37.37326296977699, -5.990221565589309], [37.37329079769552, -5.990235898643732], [37.37331225536764, -5.990249561145902], [37.37333429977298, -5.990263978019357], [37.37336279824376, -5.990274036303163], [37.37338425591588, -5.990286860615015], [37.373405545949936, -5.990303540602326], [37.3734334576875, -5.990317119285464], [37.37345801666379, -5.990328686311841], [37.37347905524075, -5.990345869213343], [37.37349866889417, -5.990360789000988], [37.37351970747113, -5.99037554115057], [37.37353915348649, -5.990397417917848], [37.373560946434736, -5.990417702123523], [37.37358500249684, -5.9904322028160095], [37.37360168248415, -5.990450223907828], [37.37362188287079, -5.990462377667427], [37.37364283762872, -5.990475118160248], [37.373661529272795, -5.990483583882451], [37.37368122674525, -5.9904946479946375], [37.37370494753122, -5.9905067179352045], [37.37372296862304, -5.990513004362583], [37.373742163181305, -5.990525828674436], [37.37376152537763, -5.990538317710161], [37.37378289923072, -5.99054410122335], [37.373801339417696, -5.990557847544551], [37.373824724927545, -5.990569330751896], [37.373844338580966, -5.990580394864082], [37.37386059947312, -5.990594895556569], [37.37388012930751, -5.990605289116502], [37.373900916427374, -5.990615766495466], [37.373917093500495, -5.990631440654397], [37.37393938936293, -5.990642756223679], [37.37396126613021, -5.990654742345214], [37.37398247234523, -5.990663459524512], [37.374005019664764, -5.990672176703811], [37.374029411002994, -5.99067946895957], [37.374053215608, -5.9906848333776], [37.37407090142369, -5.990697238594294], [37.37409462220967, -5.990712996572256], [37.374113984405994, -5.990720791742206], [37.37413477152586, -5.99073369987309], [37.37415874376893, -5.990750379860401], [37.37418036907911, -5.990762449800968], [37.37419939599931, -5.9907797165215015], [37.374224876984954, -5.990794552490115], [37.37424633465707, -5.990805784240365], [37.37426661886275, -5.990822380408645], [37.37429075874388, -5.990837132558227], [37.374313808977604, -5.9908489510416985], [37.37433501519263, -5.990864122286439], [37.374358316883445, -5.990873761475086], [37.37438203766942, -5.990881975740194], [37.37439720891416, -5.9908966440707445], [37.374420929700136, -5.990909552201629], [37.37444414757192, -5.990921622142196], [37.37446325831115, -5.990934781730175], [37.37448597326875, -5.990944001823664], [37.37451086752117, -5.9909517131745815], [37.37453676760197, -5.9909567423164845], [37.374557722359896, -5.990964788943529], [37.37458236515522, -5.990980127826333], [37.374601727351546, -5.990994041785598], [37.374623687937856, -5.991010805591941], [37.374646654352546, -5.991029664874077], [37.37466677092016, -5.991042656823993], [37.3746869713068, -5.991060258820653], [37.374710608273745, -5.991077357903123], [37.37472888082266, -5.991084817796946], [37.37474706955254, -5.991096301004291], [37.3747715447098, -5.991107448935509], [37.37479107454419, -5.991110634058714], [37.37481286749244, -5.991120105609298], [37.37483633682132, -5.991128571331501], [37.374859638512135, -5.991141311824322], [37.374881096184254, -5.991154722869396], [37.37490984611213, -5.991168972104788], [37.37493272870779, -5.9911805391311646], [37.374955862760544, -5.991195794194937], [37.37498352304101, -5.991211300715804], [37.37500757910311, -5.991226471960545], [37.37503205426037, -5.991241643205285], [37.375059965997934, -5.991252455860376], [37.37508326768875, -5.991266872733831], [37.37510464154184, -5.991283133625984], [37.3751291166991, -5.991294616833329], [37.37515015527606, -5.991308195516467], [37.37517278641462, -5.99132152274251], [37.37519558519125, -5.991335352882743], [37.37522148527205, -5.991345243528485], [37.375244703143835, -5.9913569781929255], [37.375268088653684, -5.991368545219302], [37.375289630144835, -5.991383884102106], [37.37530899234116, -5.991400815546513], [37.375331204384565, -5.991413639858365], [37.37535433843732, -5.991426967084408], [37.375373700633645, -5.99143979139626], [37.375395745038986, -5.991454208269715], [37.37542080692947, -5.991462590172887], [37.37544083967805, -5.991471391171217], [37.375461710616946, -5.991483461111784], [37.37548677250743, -5.991496369242668], [37.37550797872245, -5.991510953754187], [37.37553555518389, -5.991523694247007], [37.375564724206924, -5.99153945222497], [37.37558634951711, -5.991552192717791], [37.375610657036304, -5.991566861048341], [37.37563479691744, -5.991583289578557], [37.375653237104416, -5.991601059213281], [37.37567259930074, -5.99162058904767], [37.37569699063897, -5.991641124710441], [37.37571685574949, -5.991655709221959], [37.3757385648787, -5.9916718024760485], [37.375762704759836, -5.99168362095952], [37.37578148022294, -5.991702312603593], [37.375806625932455, -5.991718573495746], [37.37583311274648, -5.991732822731137], [37.37585758790374, -5.9917474910616875], [37.37588600255549, -5.991756040602922], [37.37591374665499, -5.991768948733807], [37.375936629250646, -5.991776157170534], [37.375964960083365, -5.99178378470242], [37.37599027343094, -5.991794178262353], [37.376014245674014, -5.991807673126459], [37.376040732488036, -5.991817228496075], [37.376066129654646, -5.991830890998244], [37.376090520992875, -5.991844637319446], [37.37611650489271, -5.991858132183552], [37.37614123150706, -5.991874141618609], [37.37616411410272, -5.9918873850256205], [37.37618297338486, -5.991902304813266], [37.37620510160923, -5.991918314248323], [37.376221530139446, -5.991941783577204], [37.37624240107834, -5.99195871502161], [37.376268804073334, -5.991977071389556], [37.37629109993577, -5.991989644244313], [37.376309959217906, -5.992004983127117], [37.376334350556135, -5.992019483819604], [37.376357316970825, -5.992029793560505], [37.37638019956648, -5.992043623700738], [37.376408195123076, -5.99205787293613], [37.376430658623576, -5.992074636742473], [37.376452619209886, -5.992089975625277], [37.376479022204876, -5.992103721946478], [37.37650257535279, -5.992117719724774], [37.37652369774878, -5.992131298407912], [37.376549346372485, -5.992139261215925], [37.37657457590103, -5.992148062214255], [37.376594273373485, -5.992158371955156], [37.37662042491138, -5.992162898182869], [37.376644145697355, -5.992178404703736], [37.376664178445935, -5.9921918995678425], [37.376684714108706, -5.992206484079361], [37.376709105446935, -5.992221487686038], [37.37672980874777, -5.992230121046305], [37.37675042822957, -5.992243951186538], [37.376776076853275, -5.992261301726103], [37.376793678849936, -5.9922773111611605], [37.376814633607864, -5.99229441024363], [37.376839527860284, -5.992313269525766], [37.37686458975077, -5.992325255647302], [37.3768868856132, -5.992338918149471], [37.37691597081721, -5.992351742461324], [37.37693952396512, -5.99236230365932], [37.37696307711303, -5.9923783130943775], [37.3769941739738, -5.992390299215913], [37.377019403502345, -5.992399351671338], [37.37704220227897, -5.992412595078349], [37.37706751562655, -5.992422234266996], [37.377090314403176, -5.992433801293373], [37.37711294554174, -5.992447631433606], [37.37713658250868, -5.992459449917078], [37.37715938128531, -5.992470430210233], [37.37717606127262, -5.99249180406332], [37.377200620248914, -5.992506304755807], [37.37722492776811, -5.99252306856215], [37.377245128154755, -5.992535976693034], [37.3772685136646, -5.992549303919077], [37.37729223445058, -5.992562547326088], [37.37731100991368, -5.992573527619243], [37.37733380869031, -5.9925859328359365], [37.37735912203789, -5.992600601166487], [37.37738074734807, -5.992615269497037], [37.377404468134046, -5.992629351094365], [37.37742994911969, -5.99264619871974], [37.37745123915374, -5.99266279488802], [37.377473283559084, -5.9926787205040455], [37.377497255802155, -5.992696071043611], [37.377516282722354, -5.9927149303257465], [37.37753656692803, -5.99273269996047], [37.377561712637544, -5.992750721052289], [37.37758317030966, -5.992765976116061], [37.37760663963854, -5.992780979722738], [37.377629270777106, -5.992797911167145], [37.37764913588762, -5.99280646070838], [37.37767143175006, -5.992819955572486], [37.37769389525056, -5.99283579736948], [37.37771241925657, -5.99285046570003], [37.37773169763386, -5.992867145687342], [37.377755334600806, -5.9928865917027], [37.37777570262551, -5.992897571995854], [37.377794813364744, -5.992911905050278], [37.37782037816942, -5.992925316095352], [37.377842674031854, -5.992937721312046], [37.377862790599465, -5.9929493721574545], [37.37788584083319, -5.992959598079324], [37.37790847197175, -5.992974434047937], [37.37792716361582, -5.992989353835583], [37.37794853746891, -5.993005195632577], [37.37797183915973, -5.993019277229905], [37.3779889382422, -5.993029503151774], [37.37800922244787, -5.993045177310705], [37.37803252413869, -5.993061354383826], [37.378054484725, -5.993072921410203], [37.3780769482255, -5.993090607225895], [37.37810553051531, -5.993106281384826], [37.37812732346356, -5.993121117353439], [37.37814651802182, -5.99313628859818], [37.37816856242716, -5.993152968585491], [37.3781908582896, -5.993167134001851], [37.37820946611464, -5.993181969970465], [37.37823142670095, -5.993197141215205], [37.37825305201113, -5.993211641907692], [37.378272749483585, -5.9932241309434175], [37.37829554826021, -5.993235949426889], [37.37831884995103, -5.993250282481313], [37.3783377930522, -5.993263190612197], [37.378358412534, -5.993276434019208], [37.37838137894869, -5.993291772902012], [37.37840099260211, -5.993305519223213], [37.378421779721975, -5.993318343535066], [37.37844591960311, -5.99333125166595], [37.378466203808784, -5.993341477587819], [37.37848858349025, -5.9933519549667835], [37.378514064475894, -5.99336544983089], [37.37853527069092, -5.9933755081146955], [37.37855748273432, -5.993386236950755], [37.37857944332063, -5.993398809805512], [37.37859855405986, -5.993413897231221], [37.37861632369459, -5.993425631895661], [37.37864004448056, -5.993441725149751], [37.378662675619125, -5.993453627452254], [37.378684636205435, -5.993467876687646], [37.37870986573398, -5.993482964113355], [37.37873383797705, -5.993495704606175], [37.378754541277885, -5.993509702384472], [37.37878010608256, -5.993524622172117], [37.37880407832563, -5.99354138597846], [37.37882318906486, -5.993557898327708], [37.378846826031804, -5.993576170876622], [37.378870379179716, -5.99359268322587], [37.37888915464282, -5.993609195575118], [37.378912791609764, -5.9936231933534145], [37.378933830186725, -5.993639035150409], [37.37895126454532, -5.993653368204832], [37.37897456623614, -5.993663594126701], [37.378998287022114, -5.993677005171776], [37.37902024760842, -5.993685889989138], [37.37904011271894, -5.993696199730039], [37.37906366586685, -5.993712125346065], [37.37908084876835, -5.993725368753076], [37.37910088151693, -5.99374121055007], [37.37912518903613, -5.9937539510428905], [37.37914664670825, -5.993764931336045], [37.379168942570686, -5.993776833638549], [37.37919534556568, -5.993789406493306], [37.37921764142811, -5.993800889700651], [37.3792406078428, -5.993814887478948], [37.37926944158971, -5.993828717619181], [37.37929173745215, -5.993835339322686], [37.3793148715049, -5.993848331272602], [37.37933976575732, -5.993865514174104], [37.37935854122043, -5.993874985724688], [37.37938075326383, -5.993891330435872], [37.37940514460206, -5.9939103573560715], [37.37942576408386, -5.993921086192131], [37.37944613210857, -5.993937347084284], [37.37947119399905, -5.993953859433532], [37.37949223257601, -5.993963750079274], [37.37951327115297, -5.993981771171093], [37.379538752138615, -5.993999624624848], [37.37956054508686, -5.994016220793128], [37.379579823464155, -5.994033571332693], [37.37960362806916, -5.994047652930021], [37.37962701357901, -5.994062405079603], [37.379646208137274, -5.994074810296297], [37.37966892309487, -5.9940870478749275], [37.37969063222408, -5.994101800024509], [37.379709240049124, -5.994115127250552], [37.379732206463814, -5.994127448648214], [37.37975735217333, -5.99413993768394], [37.37977797165513, -5.994151085615158], [37.379798758774996, -5.994164580479264], [37.379825161769986, -5.994179584085941], [37.37984485924244, -5.994190648198128], [37.379867574200034, -5.9942059032619], [37.37989196553826, -5.994222918525338], [37.37991493195295, -5.994231551885605], [37.37993680872023, -5.994246639311314], [37.379958517849445, -5.994268599897623], [37.379980059340596, -5.994282262399793], [37.38000260666013, -5.994297349825501], [37.38002825528383, -5.994314281269908], [37.380050299689174, -5.9943261835724115], [37.38006957806647, -5.9943409357219934], [37.380096232518554, -5.994352838024497], [37.380118276923895, -5.994365829974413], [37.38013990223408, -5.994379324838519], [37.38016546703875, -5.994391143321991], [37.38018633797765, -5.994404973462224], [37.380206203088164, -5.994417713955045], [37.380228666588664, -5.9944310411810875], [37.380247777327895, -5.994442021474242], [37.38026604987681, -5.994452582672238], [37.380283484235406, -5.994462305679917], [37.38030628301203, -5.994467753916979], [37.38032656721771, -5.9944728668779135], [37.38035003654659, -5.994478482753038], [37.38037560135126, -5.994489546865225], [37.38039663992822, -5.994496671482921], [37.38041792996228, -5.994511088356376], [37.38044005818665, -5.994528271257877], [37.380459336563945, -5.994540508836508], [37.38047869876027, -5.994554841890931], [37.380505269393325, -5.994567079469562], [37.38052278757095, -5.994580993428826], [37.38054466433823, -5.99459633231163], [37.38057165406644, -5.9946139343082905], [37.3805929441005, -5.994624914601445], [37.380615239962935, -5.994638241827488], [37.38063854165375, -5.994652826339006], [37.380659747868776, -5.994663303717971], [37.38068061880767, -5.994680151343346], [37.38070249557495, -5.994700100272894], [37.38072345033288, -5.994717450812459], [37.380745662376285, -5.99473362788558], [37.380770137533545, -5.994752403348684], [37.380791679024696, -5.9947694186121225], [37.38081187941134, -5.994790205731988], [37.38083333708346, -5.994807975366712], [37.38085445947945, -5.994820715859532], [37.38087700679898, -5.994832115247846], [37.38090131431818, -5.994846699759364], [37.3809243645519, -5.9948657266795635], [37.38094615750015, -5.99488215520978], [37.38097054883838, -5.994896152988076], [37.38099234178662, -5.994914844632149], [37.381012458354235, -5.994930937886238], [37.381033496931195, -5.994948707520962], [37.38105671480298, -5.994966728612781], [37.38107725046575, -5.994979804381728], [37.38110180944204, -5.9949929639697075], [37.38112360239029, -5.995008051395416], [37.381145898252726, -5.995019534602761], [37.38117037340999, -5.995034286752343], [37.38119250163436, -5.9950486198067665], [37.38121303729713, -5.995063371956348], [37.38123407587409, -5.9950795490294695], [37.381255868822336, -5.995096480473876], [37.3812769073993, -5.995111903175712], [37.38129811361432, -5.995128331705928], [37.38132401369512, -5.995144173502922], [37.38134387880564, -5.9951593447476625], [37.381363324820995, -5.995176192373037], [37.381387297064066, -5.99519438110292], [37.381407329812646, -5.995210558176041], [37.3814286198467, -5.99522908218205], [37.38145343028009, -5.995249953120947], [37.381475139409304, -5.995268393307924], [37.38149927929044, -5.995286414399743], [37.381521072238684, -5.9953072015196085], [37.38154043443501, -5.995328072458506], [37.3815583717078, -5.9953473508358], [37.38158351741731, -5.995367551222444], [37.3816032987088, -5.995384901762009], [37.38162299618125, -5.995407113805413], [37.381645711138844, -5.995429577305913], [37.38166549243033, -5.9954440779984], [37.381688794121146, -5.995460003614426], [37.381714107468724, -5.995476096868515], [37.381737660616636, -5.9954889211803675], [37.38176020793617, -5.995505852624774], [37.38178577274084, -5.9955216106027365], [37.38180387765169, -5.99553594365716], [37.38182483240962, -5.995549103245139], [37.381847966462374, -5.995561424642801], [37.38186841830611, -5.9955755062401295], [37.38188836723566, -5.995592521503568], [37.381911082193255, -5.995611380785704], [37.381932623684406, -5.995625210925937], [37.381956931203604, -5.995641639456153], [37.38198157399893, -5.9956632647663355], [37.38200319930911, -5.995680196210742], [37.38202314823866, -5.995697630569339], [37.382046366110444, -5.995717495679855], [37.38206572830677, -5.995734930038452], [37.38208710215986, -5.995748843997717], [37.38211392425001, -5.995765272527933], [37.38213554956019, -5.995778264477849], [37.382156336680055, -5.995789496228099], [37.38217854872346, -5.995807014405727], [37.382201766595244, -5.995817659422755], [37.382216937839985, -5.995833165943623], [37.382236467674375, -5.995851689949632], [37.3822574224323, -5.995870130136609], [37.382277036085725, -5.995884630829096], [37.38229782320559, -5.995901981368661], [37.38232037052512, -5.995919918641448], [37.38234325312078, -5.995934838429093], [37.382362028583884, -5.995951937511563], [37.38238516263664, -5.995968701317906], [37.38240486010909, -5.995984375476837], [37.38242673687637, -5.995999798178673], [37.38245062530041, -5.996015220880508], [37.38247208297253, -5.996030140668154], [37.382491612806916, -5.996045647189021], [37.38251214846969, -5.996064590290189], [37.38252891227603, -5.996084623038769], [37.3825494479388, -5.996101722121239], [37.38256956450641, -5.996120413765311], [37.38258708268404, -5.996137680485845], [37.382607869803905, -5.996155953034759], [37.382627902552485, -5.9961748123168945], [37.38264852203429, -5.99618780426681], [37.382670817896724, -5.996203226968646], [37.38269126974046, -5.996224181726575], [37.382709542289376, -5.996238933876157], [37.3827291559428, -5.996254691854119], [37.382752457633615, -5.996273634955287], [37.382772993296385, -5.996287213638425], [37.38279512152076, -5.996302803978324], [37.382820937782526, -5.996316969394684], [37.382844826206565, -5.996329374611378], [37.382867122069, -5.996343707665801], [37.382890759035945, -5.996360387653112], [37.3829154856503, -5.996376480907202], [37.382936691865325, -5.99639399908483], [37.38295781426132, -5.9964121878147125], [37.38297885283828, -5.996429119259119], [37.38299888558686, -5.996446302160621], [37.38302042707801, -5.996465999633074], [37.3830399569124, -5.996489971876144], [37.383057894185185, -5.996507154777646], [37.38307759165764, -5.996526097878814], [37.38309913314879, -5.996549231931567], [37.3831176571548, -5.996566247195005], [37.383135007694364, -5.996586363762617], [37.38315587863326, -5.996605642139912], [37.38317297771573, -5.996625674888492], [37.38319091498852, -5.996646713465452], [37.3832091037184, -5.996668841689825], [37.38322620280087, -5.996689377352595], [37.383241122588515, -5.996710415929556], [37.38326174207032, -5.996733382344246], [37.3832817748189, -5.996753918007016], [37.38329501822591, -5.99677705205977], [37.38331329077482, -5.996798174455762], [37.38333139568567, -5.996820135042071], [37.383346231654286, -5.996841927990317], [37.38336643204093, -5.99686112254858], [37.38338395021856, -5.996883250772953], [37.38339903764427, -5.996902696788311], [37.38341932184994, -5.996925747022033], [37.38344069570303, -5.99694661796093], [37.38345838151872, -5.996967991814017], [37.38347623497248, -5.996991628780961], [37.383494758978486, -5.997016187757254], [37.38351403735578, -5.99703554995358], [37.383536249399185, -5.997058600187302], [37.383556282147765, -5.9970819018781185], [37.38357069902122, -5.997099839150906], [37.383587127551436, -5.9971213806420565], [37.38360640592873, -5.997142670676112], [37.38362107425928, -5.997160607948899], [37.38363901153207, -5.997181814163923], [37.38366248086095, -5.997204026207328], [37.38368268124759, -5.997216934338212], [37.383702378720045, -5.997237050905824], [37.383720986545086, -5.997261526063085], [37.38373590633273, -5.997285665944219], [37.38374998793006, -5.997307458892465], [37.38376909866929, -5.997330341488123], [37.38378628157079, -5.997352302074432], [37.38380312919617, -5.997373508289456], [37.38382190465927, -5.997397983446717], [37.38384151831269, -5.99742011167109], [37.38386104814708, -5.9974403120577335], [37.383884601294994, -5.9974597580730915], [37.38390538841486, -5.99747970700264], [37.38392265513539, -5.997498901560903], [37.38394511863589, -5.99752077832818], [37.38396339118481, -5.997540391981602], [37.38397981971502, -5.997558748349547], [37.38399733789265, -5.997578110545874], [37.38401301205158, -5.997598813846707], [37.38402801565826, -5.997617673128843], [37.38404511474073, -5.9976391308009624], [37.384063890203834, -5.997664108872414], [37.38408048637211, -5.9976844768971205], [37.38409926183522, -5.9977074433118105], [37.38411736674607, -5.997731331735849], [37.38413354381919, -5.9977494366467], [37.384151397272944, -5.997770894318819], [37.38417369313538, -5.997795620933175], [37.384194396436214, -5.997815150767565], [37.384214429184794, -5.997835351154208], [37.38423462957144, -5.9978576470166445], [37.384253069758415, -5.997874829918146], [37.3842728510499, -5.997895952314138], [37.384295063093305, -5.997921014204621], [37.384313168004155, -5.997940460219979], [37.38433009944856, -5.997960241511464], [37.38435382023454, -5.997982788830996], [37.384373685345054, -5.998001396656036], [37.38439313136041, -5.99801966920495], [37.38441559486091, -5.9980423003435135], [37.38443177193403, -5.998065182939172], [37.38444635644555, -5.998089071363211], [37.384467143565416, -5.9981117863208055], [37.38448491320014, -5.998131986707449], [37.38450142554939, -5.998155036941171], [37.38452296704054, -5.998180853202939], [37.38453830592334, -5.998201053589582], [37.38455280661583, -5.9982246067374945], [37.38457275554538, -5.998249668627977], [37.38458641804755, -5.998271545395255], [37.38460125401616, -5.998297026380897], [37.38462128676474, -5.998321417719126], [37.384634194895625, -5.998339606449008], [37.384649785235524, -5.998363578692079], [37.38467199727893, -5.998381348326802], [37.384687922894955, -5.9984001237899065], [37.38470636308193, -5.998422252014279], [37.38472421653569, -5.99844865500927], [37.38474106416106, -5.998465083539486], [37.38475858233869, -5.998489810153842], [37.38478045910597, -5.9985126927495], [37.384796887636185, -5.9985297080129385], [37.38481432199478, -5.998553764075041], [37.38483862951398, -5.998576730489731], [37.38485447131097, -5.998594081029296], [37.38486930727959, -5.998617801815271], [37.38489294424653, -5.9986353199929], [37.384911719709635, -5.998655017465353], [37.38492663949728, -5.998673876747489], [37.384951282292604, -5.998689802363515], [37.384970895946026, -5.998702710494399], [37.38499168306589, -5.9987149480730295], [37.38501297309995, -5.998730454593897], [37.385031497105956, -5.998745709657669], [37.38504943437874, -5.998760377988219], [37.38506946712732, -5.998781165108085], [37.38509025424719, -5.998798180371523], [37.385111125186086, -5.998817123472691], [37.3851328343153, -5.998841430991888], [37.38515085540712, -5.998866157606244], [37.38516837358475, -5.9988881181925535], [37.385187819600105, -5.9989087376743555], [37.38520651124418, -5.998929021880031], [37.385222436860204, -5.998947126790881], [37.385242553427815, -5.9989632200449705], [37.38526174798608, -5.998983001336455], [37.38527691923082, -5.99900177679956], [37.38529443740845, -5.99902524612844], [37.38531354814768, -5.999049637466669], [37.38533081486821, -5.999070927500725], [37.38534917123616, -5.999092385172844], [37.38536979071796, -5.999118704348803], [37.38538538105786, -5.9991369768977165], [37.38540113903582, -5.9991581831127405], [37.38541781902313, -5.999178886413574], [37.38543642684817, -5.999194476753473], [37.38545151427388, -5.999212916940451], [37.385470289736986, -5.99922850728035], [37.38548764027655, -5.999247953295708], [37.385505409911275, -5.999268153682351], [37.3855247721076, -5.999293299391866], [37.38554472103715, -5.999316181987524], [37.38556483760476, -5.999337220564485], [37.38558621145785, -5.999363036826253], [37.38560683093965, -5.999385165050626], [37.38562527112663, -5.999408969655633], [37.385638765990734, -5.999441323801875], [37.38565678708255, -5.999463703483343], [37.385674472898245, -5.999487172812223], [37.385695008561015, -5.999513324350119], [37.38571370020509, -5.999536542221904], [37.385734068229795, -5.999559843912721], [37.38575435243547, -5.999584486708045], [37.38577044568956, -5.999604938551784], [37.38578871823847, -5.999628659337759], [37.38581000827253, -5.999649781733751], [37.38582660444081, -5.999668054282665], [37.38585158251226, -5.999683979898691], [37.385874297469854, -5.999704012647271], [37.38589315675199, -5.999725973233581], [37.38591964356601, -5.999745167791843], [37.385942190885544, -5.9997613448649645], [37.385959373787045, -5.9997854847460985], [37.38597898744047, -5.999808954074979], [37.38600010983646, -5.999829908832908], [37.386016538366675, -5.999853713437915], [37.386034643277526, -5.999881373718381], [37.386053670197725, -5.999908531084657], [37.38606900908053, -5.999932670965791], [37.386083509773016, -5.999956224113703], [37.38609750755131, -5.999980280175805], [37.38611008040607, -6.000003581866622], [37.38612860441208, -6.000022944062948], [37.386146038770676, -6.000045323744416], [37.38616003654897, -6.000069295987487], [37.38617931492627, -6.000092681497335], [37.38619951531291, -6.000113049522042], [37.386217284947634, -6.000133752822876], [37.38623631186783, -6.000157725065947], [37.38625416532159, -6.000183876603842], [37.38627143204212, -6.000203574076295], [37.386291548609734, -6.000227630138397], [37.38630990497768, -6.000243807211518], [37.3863295186311, -6.000262415036559], [37.38635424524546, -6.0002849623560905], [37.38637293688953, -6.000299798324704], [37.386392215266824, -6.000320250168443], [37.386415181681514, -6.000344306230545], [37.386435298249125, -6.000364674255252], [37.38645164296031, -6.0003886464983225], [37.3864730168134, -6.000409936532378], [37.38649128936231, -6.000421671196818], [37.38650981336832, -6.0004443023353815], [37.38652884028852, -6.000473303720355], [37.386548705399036, -6.00049358792603], [37.386571587994695, -6.000514542683959], [37.38658742979169, -6.000542119145393], [37.38660545088351, -6.000561146065593], [37.38662414252758, -6.000582268461585], [37.38664174452424, -6.000603474676609], [37.38665825687349, -6.000622250139713], [37.38667728379369, -6.000643875449896], [37.3866989929229, -6.000669272616506], [37.38671743310988, -6.000687880441546], [37.38673805259168, -6.0007110144943], [37.38676303066313, -6.000731969252229], [37.38678096793592, -6.00074915215373], [37.3867994081229, -6.000771867111325], [37.38681583665311, -6.000797180458903], [37.386833522468805, -6.000815788283944], [37.386851543560624, -6.0008371621370316], [37.38687224686146, -6.0008651576936245], [37.38688565790653, -6.000887369737029], [37.3868995718658, -6.000913688912988], [37.38692086189985, -6.000940594822168], [37.38693729043007, -6.000961298123002], [37.38695497624576, -6.000985689461231], [37.386976182460785, -6.001016451045871], [37.38699118606746, -6.001036651432514], [37.387006944045424, -6.001061461865902], [37.38702689297497, -6.001086942851543], [37.387045584619045, -6.0011116694658995], [37.38706477917731, -6.0011334624141455], [37.38708648830652, -6.001156512647867], [37.387102749198675, -6.001172941178083], [37.38711942918599, -6.0011902917176485], [37.38714071922004, -6.001209821552038], [37.387160332873464, -6.001221053302288], [37.38717910833657, -6.001237817108631], [37.387198470532894, -6.001262040808797], [37.38721473142505, -6.00128224119544], [37.387232165783644, -6.001305626705289], [37.387248342856765, -6.001332029700279], [37.387262592092156, -6.001353235915303], [37.38727902062237, -6.00137653760612], [37.38729762844741, -6.00140143185854], [37.38731263205409, -6.001423140987754], [37.387330485507846, -6.001448538154364], [37.38735135644674, -6.001473851501942], [37.38736912608147, -6.001491788774729], [37.387390080839396, -6.001515844836831], [37.387411622330546, -6.001542415469885], [37.38742888905108, -6.001562951132655], [37.387439366430044, -6.001603100448847], [37.387453531846404, -6.001635454595089], [37.387465266510844, -6.0016662161797285], [37.38748219795525, -6.001695804297924], [37.38750038668513, -6.0017218720167875], [37.38751396536827, -6.001746263355017], [37.3875333275646, -6.001770822331309], [37.38755252212286, -6.001789178699255], [37.38757247105241, -6.001807954162359], [37.38759585656226, -6.001824047416449], [37.3876107763499, -6.001852881163359], [37.38763064146042, -6.001872913911939], [37.38765520043671, -6.001892946660519], [37.38767808303237, -6.001908369362354], [37.387703731656075, -6.0019230376929045], [37.38773005083203, -6.00194038823247], [37.38775343634188, -6.001951871439815], [37.387777492403984, -6.0019654501229525], [37.38779987208545, -6.001981543377042], [37.38781973719597, -6.001994786784053], [37.387843038886786, -6.0020096227526665], [37.387864580377936, -6.002024877816439], [37.38788763061166, -6.002038372680545], [37.38791378214955, -6.00204742513597], [37.38793993368745, -6.0020590759813786], [37.3879614751786, -6.002077432349324], [37.387989638373256, -6.0020905919373035], [37.38801344297826, -6.002106769010425], [37.3880357388407, -6.002120515331626], [37.38806264474988, -6.002129903063178], [37.388085862621665, -6.002137698233128], [37.38810514099896, -6.002146080136299], [37.38812911324203, -6.002151947468519], [37.38815258257091, -6.002157647162676], [37.38817705772817, -6.002163263037801], [37.388202622532845, -6.002170303836465], [37.38822483457625, -6.002180697396398], [37.38824402913451, -6.002192264422774], [37.38826640881598, -6.002199724316597], [37.388284178450704, -6.002212632447481], [37.38830890506506, -6.002219840884209], [37.38833564333618, -6.002223696559668], [37.38835919648409, -6.002233419567347], [37.388381995260715, -6.002240544185042], [37.38840898498893, -6.002244902774692], [37.38843220286071, -6.002260744571686], [37.38845441490412, -6.002272395417094], [37.38847469910979, -6.002283710986376], [37.388495318591595, -6.002297876402736], [37.38851577043533, -6.002306425943971], [37.38854041323066, -6.002314807847142], [37.38856262527406, -6.002318495884538], [37.3885814845562, -6.002321932464838], [37.388598499819636, -6.002317741513252], [37.3886192869395, -6.002310365438461], [37.38863663747907, -6.0023182444274426], [37.38865876570344, -6.002326710149646], [37.38868701271713, -6.002335427328944], [37.38870562054217, -6.002347664907575], [37.388724479824305, -6.002361914142966], [37.38875021226704, -6.002368787303567], [37.38877443596721, -6.002378594130278], [37.388795306906104, -6.002385970205069], [37.388817351311445, -6.002388233318925], [37.38883696496487, -6.002396112307906], [37.38885683007538, -6.002407008782029], [37.38887962885201, -6.002417653799057], [37.38890267908573, -6.002427376806736], [37.388925813138485, -6.002436596900225], [37.388952719047666, -6.002443553879857], [37.38897769711912, -6.002455037087202], [37.38899982534349, -6.0024643409997225], [37.38902782090008, -6.002473812550306], [37.38904776982963, -6.002474566921592], [37.389068473130465, -6.00248035043478], [37.38909252919257, -6.002484625205398], [37.389114908874035, -6.002486636862159], [37.38913561217487, -6.002494180575013], [37.38916352391243, -6.002500634640455], [37.38918833434582, -6.002506082877517], [37.389207696542144, -6.002509435638785], [37.38923535682261, -6.002516141161323], [37.38925916142762, -6.002525445073843], [37.38928045146167, -6.0025314800441265], [37.38930366933346, -6.002537598833442], [37.3893291503191, -6.002543214708567], [37.389354882761836, -6.002547238022089], [37.389376759529114, -6.002554530277848], [37.38940358161926, -6.0025618225336075], [37.38942889496684, -6.002565762028098], [37.38945077173412, -6.002572551369667], [37.38947876729071, -6.0025762394070625], [37.38950206898153, -6.0025818552821875], [37.38952369429171, -6.002584705129266], [37.38954707980156, -6.002588979899883], [37.38957289606333, -6.002593254670501], [37.38959259353578, -6.002597697079182], [37.38961388356984, -6.002599960193038], [37.38964095711708, -6.00260348059237], [37.389661241322756, -6.0026122815907], [37.38968496210873, -6.0026204120367765], [37.389711951836944, -6.00262938067317], [37.389736426994205, -6.002635834738612], [37.38975687883794, -6.002641869708896], [37.389781940728426, -6.002646144479513], [37.38980742171407, -6.00265184417367], [37.389828376471996, -6.002658549696207], [37.38984966650605, -6.002661986276507], [37.38987707532942, -6.00266651250422], [37.38990347832441, -6.002670368179679], [37.38992686383426, -6.00267724134028], [37.38994974642992, -6.002682186663151], [37.38997187465429, -6.002689227461815], [37.38999458961189, -6.002690568566322], [37.39001663401723, -6.002689898014069], [37.39003951661289, -6.002694088965654], [37.39006407558918, -6.002698699012399], [37.390088718384504, -6.00270364433527], [37.39011378027499, -6.002707751467824], [37.39013641141355, -6.002709763124585], [37.39015485160053, -6.0027161333709955], [37.39017848856747, -6.002721330150962], [37.39020480774343, -6.002730466425419], [37.390231378376484, -6.0027361661195755], [37.39025895483792, -6.002741949632764], [37.390286112204194, -6.002751505002379], [37.39031285047531, -6.00275594741106], [37.39033782854676, -6.002756869420409], [37.390366746112704, -6.002757539972663], [37.39039256237447, -6.002764496952295], [37.39041586406529, -6.002764916047454], [37.390438579022884, -6.002763155847788], [37.390460120514035, -6.00276374258101], [37.3904790636152, -6.002764161676168], [37.390502113848925, -6.002764832228422], [37.3905258346349, -6.002768352627754], [37.390551064163446, -6.00277598015964], [37.39057662896812, -6.002785535529256], [37.390601774677634, -6.002791067585349], [37.390628177672625, -6.0027968510985374], [37.390653574839234, -6.0028021316975355], [37.39068475551903, -6.002820152789354], [37.39071308635175, -6.002818560227752], [37.39073873497546, -6.00281897932291], [37.39073873497546, -6.00281897932291], [37.39091316238046, -6.0028754733502865], [37.391019612550735, -6.002968931570649], [37.39106495864689, -6.0029941610991955], [37.39109161309898, -6.003007236868143], [37.39111751317978, -6.003016624599695], [37.391144167631865, -6.003023497760296], [37.39116746932268, -6.003032382577658], [37.39118607714772, -6.003042357042432], [37.39121004939079, -6.003051744773984], [37.39123150706291, -6.003061635419726], [37.39125388674438, -6.003073118627071], [37.39127643406391, -6.003078315407038], [37.391299568116665, -6.0030836798250675], [37.39132077433169, -6.003092648461461], [37.39134483039379, -6.003098096698523], [37.391370395198464, -6.0031037125736475], [37.39138858392835, -6.0031116753816605], [37.39140920341015, -6.0031175427138805], [37.39143216982484, -6.003123410046101], [37.391449520364404, -6.00313363596797], [37.39147097803652, -6.003143358975649], [37.391493525356054, -6.003153836354613], [37.39151246845722, -6.0031617153435946], [37.391533171758056, -6.0031702648848295], [37.391559574753046, -6.00317713804543], [37.39158061333001, -6.003182837739587], [37.39160223864019, -6.003194656223059], [37.391628976911306, -6.003208654001355], [37.39165060222149, -6.003219718113542], [37.39167407155037, -6.003226591274142], [37.39170139655471, -6.003231788054109], [37.39172352477908, -6.003236314281821], [37.39174691028893, -6.0032447800040245], [37.391773480921984, -6.0032515693455935], [37.39179468713701, -6.003260454162955], [37.39181932993233, -6.003269087523222], [37.391845900565386, -6.003277972340584], [37.391868280246854, -6.003285851329565], [37.39189292304218, -6.003297250717878], [37.391917230561376, -6.003303788602352], [37.39193977788091, -6.003311835229397], [37.3919661808759, -6.003319714218378], [37.391996355727315, -6.003328766673803], [37.39202166907489, -6.00333790294826], [37.39205217920244, -6.0033428482711315], [37.39208076149225, -6.003350894898176], [37.39210297353566, -6.003363635390997], [37.39213331602514, -6.003373274579644], [37.392157120630145, -6.003387272357941], [37.39218067377806, -6.003396660089493], [37.39221001043916, -6.00340286269784], [37.39223373122513, -6.003410909324884], [37.39225619472563, -6.003423230722547], [37.39228393882513, -6.003430690616369], [37.3923079110682, -6.003440581262112], [37.39232827909291, -6.003450974822044], [37.39235719665885, -6.0034627094864845], [37.392381923273206, -6.003467990085483], [37.39240455441177, -6.003476707264781], [37.392432717606425, -6.003482826054096], [37.39245543256402, -6.003488525748253], [37.39247336983681, -6.003499673679471], [37.39249885082245, -6.003510067239404], [37.39252248778939, -6.003519622609019], [37.39254201762378, -6.003533368930221], [37.39256791770458, -6.00354066118598], [37.39259507507086, -6.003545103594661], [37.39261728711426, -6.003553988412023], [37.392644779756665, -6.003561029210687], [37.39266665652394, -6.003568489104509], [37.3926861025393, -6.0035790503025055], [37.392708817496896, -6.003584582358599], [37.39273689687252, -6.003589611500502], [37.39275483414531, -6.003599250689149], [37.39278023131192, -6.003606291487813], [37.39280755631626, -6.0036140866577625], [37.392825577408075, -6.00362372584641], [37.39284829236567, -6.003637472167611], [37.39287327043712, -6.003646859899163], [37.392892967909575, -6.003660187125206], [37.39291241392493, -6.003670999780297], [37.392935464158654, -6.003681141883135], [37.39295616745949, -6.003689020872116], [37.392977457493544, -6.003700001165271], [37.39299975335598, -6.003712322562933], [37.39302322268486, -6.003720453009009], [37.39304602146149, -6.003729924559593], [37.39306873641908, -6.003736881539226], [37.39309354685247, -6.003738893195987], [37.39311567507684, -6.003745011985302], [37.393140234053135, -6.003746353089809], [37.39316487684846, -6.00375322625041], [37.39318918436766, -6.003757920116186], [37.393212486058474, -6.00376688875258], [37.39323972724378, -6.003770576789975], [37.393262442201376, -6.0037746001034975], [37.39328406751156, -6.003778204321861], [37.39330653101206, -6.003780299797654], [37.39332614466548, -6.003785748034716], [37.39334701560438, -6.003790106624365], [37.39337123930454, -6.003790106624365], [37.39339403808117, -6.003784574568272], [37.39341214299202, -6.003788514062762], [37.39343167282641, -6.003788346424699], [37.39345480687916, -6.003789352253079], [37.39347567781806, -6.003789184615016], [37.39350023679435, -6.00378742441535], [37.3935236223042, -6.003786334767938], [37.39354499615729, -6.003783233463764], [37.393567292019725, -6.0037808027118444], [37.393592270091176, -6.003780132159591], [37.39361406303942, -6.003775103017688], [37.393638119101524, -6.003772420808673], [37.39366443827748, -6.003769235685468], [37.39368673413992, -6.003763871267438], [37.393709532916546, -6.003760015591979], [37.393734427168965, -6.003757668659091], [37.39375722594559, -6.003751382231712], [37.39377901889384, -6.003748280927539], [37.393803745508194, -6.00374392233789], [37.393826292827725, -6.00373855791986], [37.393848756328225, -6.003735288977623], [37.39387096837163, -6.00372975692153], [37.39389200694859, -6.00372388958931], [37.39391413517296, -6.003721123561263], [37.39393492229283, -6.0037127416580915], [37.393961073830724, -6.003706455230713], [37.39398093894124, -6.003703773021698], [37.3940072581172, -6.003695139661431], [37.394031565636396, -6.0036844946444035], [37.39405428059399, -6.003676950931549], [37.394079342484474, -6.003667311742902], [37.39410390146077, -6.003658175468445], [37.39412544295192, -6.0036518052220345], [37.39415000192821, -6.003642836585641], [37.394176153466105, -6.003634873777628], [37.39419752731919, -6.003631604835391], [37.3942213319242, -6.003624061122537], [37.39424622617662, -6.003617439419031], [37.39426676183939, -6.003612745553255], [37.39429257810116, -6.003605872392654], [37.39431655034423, -6.0035973228514194], [37.394336834549904, -6.003587935119867], [37.394360806792974, -6.003574021160603], [37.394387712702155, -6.003561532124877], [37.39441093057394, -6.003551809117198], [37.394433645531535, -6.003540745005012], [37.39445745013654, -6.003527753055096], [37.394477650523186, -6.003519874066114], [37.394497683271766, -6.003506630659103], [37.394522577524185, -6.0034918785095215], [37.39454537630081, -6.003481149673462], [37.39456976763904, -6.003468744456768], [37.394595332443714, -6.003455417230725], [37.39461452700198, -6.003445526584983], [37.39463799633086, -6.0034356359392405], [37.39466356113553, -6.0034241527318954], [37.394685773178935, -6.003414178267121], [37.39470739848912, -6.003401940688491], [37.39473111927509, -6.0033864341676235], [37.39475240930915, -6.0033767111599445], [37.394773699343204, -6.003367155790329], [37.394792726263404, -6.003349469974637], [37.39481410011649, -6.003334214910865], [37.39483564160764, -6.003322061151266], [37.39485366269946, -6.003303620964289], [37.39487562328577, -6.00328853353858], [37.3948965780437, -6.0032804030925035], [37.39491627551615, -6.003265483304858], [37.39493848755956, -6.003251234069467], [37.39495818503201, -6.003240002319217], [37.3949795588851, -6.003226591274142], [37.39500403404236, -6.003209324553609], [37.39502658136189, -6.003197925165296], [37.39504812285304, -6.003183843567967], [37.39507452584803, -6.003168672323227], [37.39509757608175, -6.003156853839755], [37.39511903375387, -6.003141682595015], [37.395143173635006, -6.003125924617052], [37.3951640445739, -6.003113770857453], [37.395186172798276, -6.003095079213381], [37.395209139212966, -6.003072950989008], [37.395230596885085, -6.003058701753616], [37.39525834098458, -6.003041351214051], [37.39528524689376, -6.003030706197023], [37.395312152802944, -6.003018133342266], [37.39533578976989, -6.002998519688845], [37.395358085632324, -6.00298167206347], [37.395380130037665, -6.002967925742269], [37.39540628157556, -6.002952167764306], [37.395425559952855, -6.002936493605375], [37.39544810727239, -6.002922328189015], [37.39547174423933, -6.0029007866978645], [37.39549705758691, -6.002883519977331], [37.39551734179258, -6.002873377874494], [37.39554215222597, -6.002860302105546], [37.395564783364534, -6.002847142517567], [37.39558515138924, -6.002833899110556], [37.39560208283365, -6.002811184152961], [37.395621528849006, -6.002788804471493], [37.39564181305468, -6.002770364284515], [37.395662516355515, -6.002747984603047], [37.39568674005568, -6.00272711366415], [37.395707527175546, -6.002710936591029], [37.395727057009935, -6.00268897600472], [37.39574776031077, -6.002667602151632], [37.39576972089708, -6.002655113115907], [37.395791513845325, -6.002638768404722], [37.39581188187003, -6.002617394551635], [37.395831076428294, -6.002601720392704], [37.395850606262684, -6.002579927444458], [37.39587541669607, -6.002557463943958], [37.395899472758174, -6.002544639632106], [37.39592168480158, -6.002529300749302], [37.39594456739724, -6.00251404568553], [37.39596778526902, -6.002498287707567], [37.395992428064346, -6.002476746216416], [37.39601472392678, -6.002461072057486], [37.39603593014181, -6.002450510859489], [37.396056884899735, -6.002433579415083], [37.396077420562506, -6.002416061237454], [37.396095944568515, -6.002405919134617], [37.396114049479365, -6.002387311309576], [37.39613433368504, -6.00236602127552], [37.39615327678621, -6.002350682392716], [37.396173225715756, -6.002331739291549], [37.396195605397224, -6.002314304932952], [37.39621312357485, -6.0023049172014], [37.39623391069472, -6.002295361831784], [37.39625679329038, -6.0022796876728535], [37.39627732895315, -6.002269629389048], [37.39629912190139, -6.002260325476527], [37.396318735554814, -6.002240125089884], [37.396338852122426, -6.002223696559668], [37.39636089652777, -6.002212380990386], [37.39638797007501, -6.002191845327616], [37.39640884101391, -6.002172818407416], [37.39642878994346, -6.002157479524612], [37.39644572138786, -6.002135938033462], [37.39646751433611, -6.002113139256835], [37.39648796617985, -6.002097465097904], [37.39650431089103, -6.002074331045151], [37.39652375690639, -6.002054130658507], [37.39654286764562, -6.0020417254418135], [37.39656088873744, -6.002020016312599], [37.39658092148602, -6.002000570297241], [37.39660179242492, -6.001986404880881], [37.396619729697704, -6.001965198665857], [37.3966408520937, -6.00194432772696], [37.39666155539453, -6.0019301623106], [37.396682258695364, -6.001909291371703], [37.39670665003359, -6.001891354098916], [37.39672844298184, -6.00187542848289], [37.39674981683493, -6.001855814829469], [37.39677328616381, -6.0018382128328085], [37.396793235093355, -6.001823041588068], [37.39681402221322, -6.0018049366772175], [37.39683791063726, -6.001787334680557], [37.39685978740454, -6.001774845644832], [37.396883172914386, -6.001761434599757], [37.39690722897649, -6.001748442649841], [37.39692776463926, -6.001735702157021], [37.39694603718817, -6.001717345789075], [37.39696984179318, -6.0017004981637], [37.396989623084664, -6.001683734357357], [37.39701041020453, -6.001666886731982], [37.3970325384289, -6.001649620011449], [37.39704905077815, -6.001633442938328], [37.397072771564126, -6.001622127369046], [37.39710085093975, -6.001611649990082], [37.39712725393474, -6.001599915325642], [37.39715122617781, -6.001583989709616], [37.39717612043023, -6.001566220074892], [37.39719741046429, -6.001553228124976], [37.39722205325961, -6.001531267538667], [37.397247198969126, -6.001512911170721], [37.39727100357413, -6.001501344144344], [37.397293550893664, -6.001478210091591], [37.39731576293707, -6.001457758247852], [37.39733738824725, -6.001445855945349], [37.397357588633895, -6.001424482092261], [37.39737871102989, -6.001408472657204], [37.397398659959435, -6.001399587839842], [37.397420201450586, -6.001382824033499], [37.397440653294325, -6.001367820426822], [37.39745833911002, -6.00135650485754], [37.39747946150601, -6.0013434290885925], [37.39749949425459, -6.001328676939011], [37.39751751534641, -6.0013181157410145], [37.39753997884691, -6.001306548714638], [37.39756168797612, -6.001288611441851], [37.397582307457924, -6.001277128234506], [37.39760393276811, -6.001264555379748], [37.39762522280216, -6.0012430138885975], [37.39764575846493, -6.001222729682922], [37.39766512066126, -6.001212922856212], [37.397683560848236, -6.001195488497615], [37.39770468324423, -6.001182412728667], [37.397723626345396, -6.001170426607132], [37.397745586931705, -6.001158440485597], [37.39776377566159, -6.001141173765063], [37.39778179675341, -6.001123320311308], [37.39780199714005, -6.001107143238187], [37.397823706269264, -6.00108677521348], [37.397846756502986, -6.001073531806469], [37.39786880090833, -6.001060456037521], [37.39788833074272, -6.001040590927005], [37.39790936931968, -6.001024162396789], [37.39792596548796, -6.001011757180095], [37.39794373512268, -6.000990550965071], [37.397964019328356, -6.000972278416157], [37.39798321388662, -6.000956855714321], [37.398004587739706, -6.00093824788928], [37.39802579395473, -6.00092408247292], [37.398044653236866, -6.000913018360734], [37.39806602708995, -6.000899355858564], [37.3980878200382, -6.00088108330965], [37.39810550585389, -6.000864822417498], [37.39812729880214, -6.000850489363074], [37.398151606321335, -6.000833474099636], [37.39817189052701, -6.000816123560071], [37.398190665990114, -6.0008015390485525], [37.3982138838619, -6.000784859061241], [37.39823693409562, -6.0007711965590715], [37.39825889468193, -6.000759629532695], [37.3982810229063, -6.000745128840208], [37.39830516278744, -6.00073448382318], [37.39832729101181, -6.0007240902632475], [37.39834623411298, -6.000707158818841], [37.39836785942316, -6.000688299536705], [37.39838680252433, -6.000676564872265], [37.39840792492032, -6.000657118856907], [37.39842711947858, -6.000635828822851], [37.3984488286078, -6.0006192326545715], [37.39846936427057, -6.000594170764089], [37.3984935041517, -6.000573551282287], [37.398515129461884, -6.000557038933039], [37.39853851497173, -6.000535832718015], [37.39855636842549, -6.000520410016179], [37.398579586297274, -6.000506998971105], [37.398602133616805, -6.0004878882318735], [37.39862342365086, -6.0004752315580845], [37.39864689297974, -6.000458970665932], [37.39867061376572, -6.000442123040557], [37.39869081415236, -6.000426532700658], [37.39871419966221, -6.000409768894315], [37.398734567686915, -6.000383784994483], [37.39875736646354, -6.000373726710677], [37.39878301508725, -6.000356208533049], [37.39880807697773, -6.0003359243273735], [37.398826349526644, -6.000323016196489], [37.39884822629392, -6.000304743647575], [37.39887001924217, -6.000279011204839], [37.39888862706721, -6.000264845788479], [37.39891058765352, -6.0002458188682795], [37.39893464371562, -6.0002281330525875], [37.39895551465452, -6.000213464722037], [37.398977391421795, -6.000196868553758], [37.399000357836485, -6.000171387568116], [37.3990204744041, -6.000156803056598], [37.39904176443815, -6.000135010108352], [37.39906556904316, -6.000114222988486], [37.399087361991405, -6.0000997222959995], [37.39910999312997, -6.000080024823546], [37.39913446828723, -6.000058650970459], [37.39915550686419, -6.000044150277972], [37.39917755126953, -6.000026380643249], [37.39920060150325, -6.000006012618542], [37.399220298975706, -5.99999226629734], [37.39923940971494, -5.999969048425555], [37.399259358644485, -5.999945662915707], [37.39928240887821, -5.999930072575808], [37.39930252544582, -5.999905597418547], [37.39932557567954, -5.999889336526394], [37.399345273151994, -5.999875422567129], [37.39936522208154, -5.999853629618883], [37.39938751794398, -5.999832004308701], [37.39940662868321, -5.99981733597815], [37.399426912888885, -5.999793782830238], [37.39944811910391, -5.99977090023458], [37.39946278743446, -5.999751202762127], [37.399478293955326, -5.999725889414549], [37.39949899725616, -5.999705353751779], [37.39951710216701, -5.99968733265996], [37.399536883458495, -5.999666545540094], [37.39956001751125, -5.9996455907821655], [37.399578373879194, -5.999628910794854], [37.399600334465504, -5.99960669875145], [37.39962363615632, -5.999582810327411], [37.39964132197201, -5.999568896368146], [37.399663953110576, -5.999551462009549], [37.39968624897301, -5.999528914690018], [37.39970628172159, -5.999511480331421], [37.39973067305982, -5.999494297429919], [37.399750873446465, -5.999473426491022], [37.399770990014076, -5.999453226104379], [37.399790436029434, -5.999436881393194], [37.399809043854475, -5.999416848644614], [37.39982698112726, -5.999393379315734], [37.39984198473394, -5.999373011291027], [37.39986335858703, -5.999355912208557], [37.39988598972559, -5.999335460364819], [37.39990308880806, -5.999319953843951], [37.39992379210889, -5.999302184209228], [37.399942567572, -5.99927787669003], [37.39996084012091, -5.999257257208228], [37.39998162724078, -5.9992435947060585], [37.40000585094094, -5.999225154519081], [37.40002680569887, -5.9992103185504675], [37.400050358846784, -5.999194141477346], [37.40007173269987, -5.999173438176513], [37.400089502334595, -5.999155333265662], [37.4001048412174, -5.999134965240955], [37.40012462250888, -5.999111831188202], [37.40013870410621, -5.999088529497385], [37.40015622228384, -5.999066568911076], [37.4001788534224, -5.999037902802229], [37.40019486285746, -5.9990169480443], [37.40021481178701, -5.9989959094673395], [37.40023878403008, -5.998967746272683], [37.40025915205479, -5.998948719352484], [37.400281615555286, -5.998929860070348], [37.400305001065135, -5.9989058040082455], [37.400324027985334, -5.998884094879031], [37.400345066562295, -5.99886548705399], [37.400364093482494, -5.9988404251635075], [37.400383120402694, -5.998818716034293], [37.40040231496096, -5.998798683285713], [37.40042301826179, -5.998771442100406], [37.40044455975294, -5.998754007741809], [37.40046861581504, -5.998737495392561], [37.40049057640135, -5.99871838465333], [37.40051044151187, -5.998701034113765], [37.40053240209818, -5.998683264479041], [37.40055235102773, -5.9986577834934], [37.400569366291165, -5.998639008030295], [37.40058965049684, -5.998621070757508], [37.40060968324542, -5.998600032180548], [37.400629967451096, -5.998582011088729], [37.400652430951595, -5.998565666377544], [37.40067841485143, -5.998545130714774], [37.40070045925677, -5.998527277261019], [37.40072434768081, -5.998508669435978], [37.400744799524546, -5.998476482927799], [37.400764748454094, -5.998455947265029], [37.40078545175493, -5.99844329059124], [37.40080749616027, -5.998423174023628], [37.40082451142371, -5.998405488207936], [37.400843789801, -5.998390316963196], [37.40086524747312, -5.998369948938489], [37.40088427439332, -5.998353939503431], [37.400902546942234, -5.998337008059025], [37.40092140622437, -5.998317478224635], [37.4009439535439, -5.998300211504102], [37.40096289664507, -5.998290320858359], [37.40098477341235, -5.998275987803936], [37.40100539289415, -5.998255368322134], [37.40102651529014, -5.998237766325474], [37.40104830823839, -5.9982184041291475], [37.40106943063438, -5.998194515705109], [37.40108845755458, -5.998171465471387], [37.401107819750905, -5.998147241771221], [37.40112600848079, -5.998125867918134], [37.40114260464907, -5.9981088526546955], [37.401161547750235, -5.998091669753194], [37.401184011250734, -5.998069709166884], [37.40120396018028, -5.998054118826985], [37.401228016242385, -5.998031320050359], [37.40125140175223, -5.998009191825986], [37.40127713419497, -5.997986393049359], [37.4012996815145, -5.997966108843684], [37.4013187084347, -5.997943310067058], [37.40133346058428, -5.997927216812968], [37.40135299041867, -5.997907519340515], [37.4013739451766, -5.997887318953872], [37.40139389410615, -5.997871225699782], [37.4014154355973, -5.997853456065059], [37.40143429487944, -5.9978267177939415], [37.40145298652351, -5.997809702530503], [37.40147218108177, -5.997795034199953], [37.40148928016424, -5.997774163261056], [37.4015052895993, -5.997754298150539], [37.40152372978628, -5.9977413062006235], [37.401542756706476, -5.997721692547202], [37.40156379528344, -5.997702917084098], [37.40158290602267, -5.997691852971911], [37.4016036093235, -5.997679950669408], [37.40162439644337, -5.997664276510477], [37.40163856185973, -5.997647512704134], [37.40165624767542, -5.997628569602966], [37.40167326293886, -5.997604681178927], [37.401686422526836, -5.997584229335189], [37.401702515780926, -5.997563190758228], [37.40172028541565, -5.997533435001969], [37.401737216860056, -5.997509965673089], [37.40175657905638, -5.997489681467414], [37.401773342862725, -5.997459003701806], [37.40179262124002, -5.997433690354228], [37.40181131288409, -5.997417513281107], [37.401827573776245, -5.997394798323512], [37.40184919908643, -5.997375436127186], [37.40187048912048, -5.997360600158572], [37.40188901312649, -5.997338891029358], [37.40191088989377, -5.9973227977752686], [37.401932682842016, -5.997306704521179], [37.401956068351865, -5.997279966250062], [37.40197819657624, -5.997263118624687], [37.402002001181245, -5.9972497913986444], [37.40202337503433, -5.997227327898145], [37.40204474888742, -5.997203020378947], [37.40206528455019, -5.997185502201319], [37.402086993679404, -5.997158596292138], [37.40211088210344, -5.99713571369648], [37.40213502198458, -5.99712104536593], [37.40215798839927, -5.997097240760922], [37.40218112245202, -5.997074861079454], [37.40220341831446, -5.9970595221966505], [37.402223786339164, -5.997035885229707], [37.402244908735156, -5.997016271576285], [37.402266785502434, -5.997003111988306], [37.40228849463165, -5.9969808999449015], [37.40231146104634, -5.996959609910846], [37.40233442746103, -5.996942846104503], [37.4023565556854, -5.996917448937893], [37.402376839891076, -5.9968954883515835], [37.4023980461061, -5.996880568563938], [37.40242059342563, -5.996859362348914], [37.40244196727872, -5.9968410898], [37.40246191620827, -5.996824661269784], [37.402480356395245, -5.996801275759935], [37.40250416100025, -5.99678048864007], [37.402522852644324, -5.996764060109854], [37.4025449808687, -5.996744781732559], [37.40256861783564, -5.996721899136901], [37.40258697420359, -5.996705386787653], [37.4026086833328, -5.9966890420764685], [37.402630811557174, -5.996666746214032], [37.402647910639644, -5.996649898588657], [37.40266827866435, -5.996631793677807], [37.40268680267036, -5.996607067063451], [37.4027088470757, -5.996589213609695], [37.402729047462344, -5.996572198346257], [37.40274933166802, -5.996550740674138], [37.40276902914047, -5.996535737067461], [37.40279057063162, -5.996522828936577], [37.402812112122774, -5.9964999463409185], [37.402832647785544, -5.996480835601687], [37.4028539378196, -5.9964660834521055], [37.40287606604397, -5.996445044875145], [37.40289802663028, -5.9964224975556135], [37.40292007103562, -5.996404644101858], [37.402943037450314, -5.99638276733458], [37.40296139381826, -5.996359884738922], [37.40298050455749, -5.9963462222367525], [37.40299659781158, -5.996325770393014], [37.40301772020757, -5.9963049832731485], [37.403036411851645, -5.99629282951355], [37.403055522590876, -5.99627380259335], [37.40307756699622, -5.9962522611021996], [37.403096091002226, -5.99623910151422], [37.40311729721725, -5.996222672984004], [37.40314235910773, -5.9962016344070435], [37.40316180512309, -5.996183194220066], [37.403183011338115, -5.996168190613389], [37.40320572629571, -5.9961506724357605], [37.40322391502559, -5.996135333552957], [37.403244618326426, -5.996118988841772], [37.403266578912735, -5.996095100417733], [37.403285605832934, -5.996078252792358], [37.403308069333434, -5.9960605669766665], [37.403330532833934, -5.996037349104881], [37.40335039794445, -5.996015975251794], [37.40336950868368, -5.996001306921244], [37.4033893737942, -5.995979681611061], [37.40341284312308, -5.995956966653466], [37.40343279205263, -5.99594933912158], [37.4034599494189, -5.995938777923584], [37.403486436232924, -5.9959356766194105], [37.403507977724075, -5.995945315808058], [37.4035326205194, -5.995956044644117], [37.40355709567666, -5.995973730459809], [37.40357670933008, -5.995986387133598], [37.4035965744406, -5.996004240587354], [37.403619792312384, -5.996025865897536], [37.40363865159452, -5.996038522571325], [37.403663294389844, -5.996053442358971], [37.403689278289676, -5.996074061840773], [37.4037104845047, -5.996089652180672], [37.40373244509101, -5.996112618595362], [37.40375733934343, -5.996137177571654], [37.40377561189234, -5.996159976348281], [37.403794722631574, -5.996185792610049], [37.403813330456614, -5.996210686862469], [37.40382917225361, -5.996233988553286], [37.40384694188833, -5.996259469538927], [37.40386202931404, -5.996290734037757], [37.403872422873974, -5.996318478137255], [37.40388507954776, -5.9963491559028625], [37.403902262449265, -5.9963771514594555], [37.403914080932736, -5.996402380988002], [37.403929168358445, -5.996431298553944], [37.403944758698344, -5.996460299938917], [37.403956577181816, -5.996482763439417], [37.40397250279784, -5.996508914977312], [37.40398641675711, -5.996535485610366], [37.404003012925386, -5.99655183032155], [37.40402027964592, -5.9965721145272255], [37.4040403123945, -5.996597176417708], [37.404054310172796, -5.9966157004237175], [37.40406872704625, -5.9966404270380735], [37.40408515557647, -5.996663561090827], [37.40409814752638, -5.996686276048422], [37.40411206148565, -5.996712930500507], [37.4041267298162, -5.996742434799671], [37.40413762629032, -5.996769005432725], [37.404150450602174, -5.996796581894159], [37.40416847169399, -5.996822901070118], [37.40418104454875, -5.996849974617362], [37.40419336594641, -5.996878053992987], [37.404213482514024, -5.996910994872451], [37.40422798320651, -5.996937062591314], [37.40424298681319, -5.996961873024702], [37.40426016971469, -5.996989784762263], [37.40427006036043, -5.9970182832330465], [37.40428422577679, -5.9970492124557495], [37.40429914556444, -5.997080057859421], [37.40431264042854, -5.997103778645396], [37.40432764403522, -5.997131271287799], [37.40434524603188, -5.997161529958248], [37.40435907617211, -5.9971896931529045], [37.404376259073615, -5.997217101976275], [37.40439218468964, -5.997245600447059], [37.40440643392503, -5.997268902137876], [37.4044235330075, -5.997295221313834], [37.40443945862353, -5.9973210375756025], [37.404451277107, -5.997345512732863], [37.40446695126593, -5.997374514117837], [37.40448363125324, -5.997402677312493], [37.4044943600893, -5.997428325936198], [37.404510201886296, -5.997458668425679], [37.404530150815845, -5.9974918607622385], [37.40454247221351, -5.9975210297852755], [37.40456066094339, -5.997550366446376], [37.40457985550165, -5.997578529641032], [37.40459728986025, -5.997604262083769], [37.404616149142385, -5.997640807181597], [37.40463685244322, -5.997669557109475], [37.40465294569731, -5.997697049751878], [37.40466979332268, -5.997730158269405], [37.40468722768128, -5.997757399454713], [37.404705164954066, -5.997777096927166], [37.404720755293965, -5.997800817713141], [37.40473668090999, -5.997826466336846], [37.40475042723119, -5.997850019484758], [37.404767610132694, -5.99787013605237], [37.40478445775807, -5.997893773019314], [37.40479786880314, -5.99791556596756], [37.40481379441917, -5.997938448563218], [37.40483424626291, -5.997964181005955], [37.404849249869585, -5.997989242896438], [37.40486601367593, -5.998014472424984], [37.40488386712968, -5.998043054714799], [37.404896607622504, -5.998069457709789], [37.40490800701082, -5.998097872361541], [37.40492586046457, -5.99812813103199], [37.4049380980432, -5.998156042769551], [37.4049505032599, -5.998191414400935], [37.40496416576207, -5.99821706302464], [37.40498009137809, -5.99824346601963], [37.404994340613484, -5.998267270624638], [37.405013367533684, -5.998291997238994], [37.405032478272915, -5.998318903148174], [37.40504890680313, -5.998344384133816], [37.40506743080914, -5.998368272557855], [37.40508176386356, -5.9983971901237965], [37.40509836003184, -5.998417055234313], [37.4051178060472, -5.998441195115447], [37.40513767115772, -5.998464077711105], [37.40515602752566, -5.998485703021288], [37.405178574845195, -5.998503221198916], [37.40520279854536, -5.9985217452049255], [37.405220065265894, -5.998539179563522], [37.405241606757045, -5.998556781560183], [37.40526767447591, -5.998571449890733], [37.40529332309961, -5.998585531488061], [37.40531528368592, -5.998600199818611], [37.40534001030028, -5.99861528724432], [37.405361887067556, -5.998615538701415], [37.4053855240345, -5.998625261709094], [37.405413184314966, -5.9986351523548365], [37.40543824620545, -5.998640768229961], [37.40546473301947, -5.998648563399911], [37.40549013018608, -5.998652586713433], [37.405513767153025, -5.998653005808592], [37.40553732030094, -5.998651161789894], [37.40556271746755, -5.998647725209594], [37.40558685734868, -5.998642696067691], [37.40560982376337, -5.998639678582549], [37.405636394396424, -5.998634984716773], [37.40566296502948, -5.998631631955504], [37.40568517707288, -5.998628782108426], [37.40571158006787, -5.998619059100747], [37.40573789924383, -5.998607240617275], [37.40576019510627, -5.998596092686057], [37.405785927549005, -5.998581340536475], [37.40580713376403, -5.998566001653671], [37.405829429626465, -5.998556362465024], [37.40585180930793, -5.998539263382554], [37.405873853713274, -5.998518140986562], [37.40589648485184, -5.998505903407931], [37.40591651760042, -5.998488971963525], [37.405942333862185, -5.99846868775785], [37.405960438773036, -5.998453767970204], [37.4059812258929, -5.998433148488402], [37.40600444376469, -5.998410936444998], [37.4060245603323, -5.998390819877386], [37.40604467689991, -5.998376654461026], [37.406063955277205, -5.998358801007271], [37.40608574822545, -5.998342372477055], [37.40610519424081, -5.998323764652014], [37.40612346678972, -5.998306246474385], [37.406141152605414, -5.998285124078393], [37.406160766258836, -5.998269617557526], [37.4061799608171, -5.99824883043766], [37.4061989877373, -5.998229468241334], [37.40621860139072, -5.998212452977896], [37.40623662248254, -5.9981907438486814], [37.40625757724047, -5.998166017234325], [37.406278531998396, -5.998146403580904], [37.40629965439439, -5.998124778270721], [37.40631960332394, -5.998107930645347], [37.40634139627218, -5.9980888199061155], [37.40636377595365, -5.998064428567886], [37.40638657473028, -5.998043054714799], [37.40640912204981, -5.9980246145278215], [37.406432423740625, -5.998001480475068], [37.406452875584364, -5.997981363907456], [37.40647458471358, -5.997963091358542], [37.40649897605181, -5.997939873486757], [37.40652269683778, -5.997920595109463], [37.40654499270022, -5.997903663665056], [37.40656645037234, -5.997879775241017], [37.406586315482855, -5.997861083596945], [37.406606851145625, -5.997846499085426], [37.40662897937, -5.997825795784593], [37.40664876066148, -5.997812552377582], [37.40666946396232, -5.997798554599285], [37.40669150836766, -5.997778186574578], [37.40671455860138, -5.997762260958552], [37.40673517808318, -5.997749520465732], [37.406760239973664, -5.997733091935515], [37.406779853627086, -5.997717250138521], [37.40680399350822, -5.99769864231348], [37.40682838484645, -5.997679196298122], [37.40685059688985, -5.997659247368574], [37.4068723898381, -5.997639466077089], [37.40689594298601, -5.997613063082099], [37.40691974759102, -5.997593700885773], [37.40694330073893, -5.997574590146542], [37.40697012282908, -5.997551623731852], [37.40699007175863, -5.997536536306143], [37.40701018832624, -5.9975210297852755], [37.4070296343416, -5.9974992368370295], [37.407044218853116, -5.997485155239701], [37.40706249140203, -5.997468223795295], [37.40708445198834, -5.997445760294795], [37.40710213780403, -5.997428661212325], [37.4071226734668, -5.99741374142468], [37.40714589133859, -5.997390691190958], [37.407165840268135, -5.997375017032027], [37.40718863904476, -5.997357666492462], [37.40721286274493, -5.997335202991962], [37.40723440423608, -5.997317852452397], [37.407256281003356, -5.997301507741213], [37.40728134289384, -5.997279714792967], [37.40730497986078, -5.997261945158243], [37.40732626989484, -5.997246941551566], [37.40734923630953, -5.997224897146225], [37.407371615990996, -5.9972085524350405], [37.40739441476762, -5.997195225208998], [37.40741511806846, -5.99717334844172], [37.40744101814926, -5.997156668454409], [37.40746415220201, -5.997142670676112], [37.407489800825715, -5.997121464461088], [37.40751201286912, -5.997102605178952], [37.4075336381793, -5.997090199962258], [37.40755744278431, -5.997069161385298], [37.40757906809449, -5.997048709541559], [37.40760094486177, -5.9970359690487385], [37.40762307308614, -5.9970188699662685], [37.407646207138896, -5.997001267969608], [37.407669592648745, -5.996989868581295], [37.407691637054086, -5.996968410909176], [37.40771426819265, -5.996948294341564], [37.407737486064434, -5.99693245254457], [37.40775885991752, -5.996909569948912], [37.40777830593288, -5.996890375390649], [37.40779900923371, -5.996872605755925], [37.407823987305164, -5.99684989079833], [37.407844606786966, -5.996834635734558], [37.4078653100878, -5.996819883584976], [37.40788785740733, -5.99679809063673], [37.40790889598429, -5.9967814944684505], [37.40793094038963, -5.9967700112611055], [37.407953068614006, -5.996750229969621], [37.40797335281968, -5.996733717620373], [37.40799447521567, -5.996716367080808], [37.408016100525856, -5.996693400666118], [37.408037055283785, -5.996675211936235], [37.40805842913687, -5.99665786139667], [37.40807871334255, -5.9966363199055195], [37.40809782408178, -5.996621400117874], [37.40811978466809, -5.996602289378643], [37.40814325399697, -5.996578903868794], [37.40816353820264, -5.996564570814371], [37.40818583406508, -5.99654302932322], [37.4082054477185, -5.9965211525559425], [37.40822497755289, -5.9965067356824875], [37.40824752487242, -5.996487624943256], [37.40826990455389, -5.996464658528566], [37.40828792564571, -5.996444122865796], [37.408308293670416, -5.9964236710220575], [37.40832958370447, -5.996402380988002], [37.40834760479629, -5.996387964114547], [37.408367386087775, -5.9963691886514425], [37.40838708356023, -5.996351251378655], [37.40840569138527, -5.996339181438088], [37.40842681378126, -5.996322585269809], [37.40844768472016, -5.996301379054785], [37.40846729837358, -5.996288303285837], [37.408489091321826, -5.996272377669811], [37.40850920788944, -5.996250668540597], [37.40853276103735, -5.996235832571983], [37.40855815820396, -5.99621563218534], [37.40858213044703, -5.9961924981325865], [37.408603839576244, -5.9961765725165606], [37.40862454287708, -5.99615503102541], [37.40864315070212, -5.996128208935261], [37.40865983068943, -5.996110774576664], [37.40867625921965, -5.996083617210388], [37.40868472494185, -5.996050341054797], [37.408697213977575, -5.9960222616791725], [37.408707020804286, -5.9959901589900255], [37.40871557034552, -5.995957050472498], [37.408722108229995, -5.995932826772332], [37.40873166359961, -5.995902149006724], [37.4087443202734, -5.995867531746626], [37.40875689312816, -5.995842218399048], [37.40877860225737, -5.9958175756037235], [37.40880307741463, -5.995795698836446], [37.40882746875286, -5.995783628895879], [37.40885345265269, -5.99577683955431], [37.40888463333249, -5.9957716427743435], [37.408909276127815, -5.995774157345295], [37.40893358364701, -5.995784047991037], [37.408958058804274, -5.995799386873841], [37.408979684114456, -5.995817072689533], [37.40899468772113, -5.9958411287516356], [37.40901262499392, -5.995867867022753], [37.40902327001095, -5.995893431827426], [37.40903031080961, -5.995925115421414], [37.40903852507472, -5.9959568828344345], [37.40904740989208, -5.995987392961979], [37.40905302576721, -5.9960175678133965], [37.40906534716487, -5.996051514521241], [37.40907557308674, -5.996080096811056], [37.40908236242831, -5.99610960111022], [37.40909359417856, -5.996140195056796], [37.40910390391946, -5.996167939156294], [37.40911706350744, -5.996195264160633], [37.409134078770876, -5.996222924441099], [37.40915126167238, -5.996247651055455], [37.409165762364864, -5.996271455660462], [37.40918219089508, -5.9962952602654696], [37.409200463443995, -5.996321998536587], [37.409216137602925, -5.996344210579991], [37.409231225028634, -5.9963697753846645], [37.40925142541528, -5.9963909815996885], [37.409268356859684, -5.996406152844429], [37.40928470157087, -5.996423587203026], [37.409304566681385, -5.9964442905038595], [37.409320157021284, -5.996461054310203], [37.40933616645634, -5.996483014896512], [37.40935527719557, -5.996508663520217], [37.40937069989741, -5.9965279418975115], [37.40938947536051, -5.99655426107347], [37.40941168740392, -5.996577143669128], [37.409429708495736, -5.9965962544083595], [37.40944781340659, -5.996618717908859], [37.409467343240976, -5.9966460429131985], [37.40948360413313, -5.996666830033064], [37.409498523920774, -5.996689461171627], [37.409519059583545, -5.996712762862444], [37.409536158666015, -5.9967275988310575], [37.40955417975783, -5.996749559417367], [37.40957731381059, -5.996775710955262], [37.40959323942661, -5.996791888028383], [37.40960908122361, -5.9968151897192], [37.40962483920157, -5.996842682361603], [37.40963665768504, -5.996864475309849], [37.409652499482036, -5.996888531371951], [37.409669095650315, -5.996914766728878], [37.409681249409914, -5.9969384875148535], [37.40969968959689, -5.9969626273959875], [37.409714944660664, -5.996988108381629], [37.40973321720958, -5.997011661529541], [37.409751657396555, -5.997034627944231], [37.40977236069739, -5.997058851644397], [37.40978862158954, -5.997079806402326], [37.40980563685298, -5.997102772817016], [37.40982717834413, -5.99712691269815], [37.409842517226934, -5.997153986245394], [37.40986380726099, -5.997176365926862], [37.409884091466665, -5.997202098369598], [37.409897753968835, -5.997226908802986], [37.409916780889034, -5.997260436415672], [37.40993865765631, -5.997284660115838], [37.409954415634274, -5.9973072074353695], [37.40997520275414, -5.9973278269171715], [37.40999188274145, -5.997350038960576], [37.41000537760556, -5.997371915727854], [37.41002247668803, -5.997390942648053], [37.41004007868469, -5.997414076700807], [37.410052651539445, -5.997435199096799], [37.41006941534579, -5.997461182996631], [37.410092214122415, -5.997486412525177], [37.410110821947455, -5.997506780549884], [37.41013026796281, -5.99752563983202], [37.41014912724495, -5.997549695894122], [37.41016320884228, -5.997570902109146], [37.41017762571573, -5.997594203799963], [37.410196820273995, -5.997616164386272], [37.41021065041423, -5.997632592916489], [37.41022506728768, -5.997654218226671], [37.410245183855295, -5.997675592079759], [37.41025951690972, -5.997694032266736], [37.410275023430586, -5.997715322300792], [37.41029388271272, -5.997736528515816], [37.41031106561422, -5.99775773473084], [37.410325817763805, -5.997777516022325], [37.41034802980721, -5.997796542942524], [37.410364458337426, -5.997812217101455], [37.4103813059628, -5.997836105525494], [37.41040041670203, -5.997859323397279], [37.4104175157845, -5.997881032526493], [37.41043251939118, -5.997905004769564], [37.41045070812106, -5.99793316796422], [37.410466047003865, -5.997957224026322], [37.410480715334415, -5.997977927327156], [37.41049563512206, -5.998004833236337], [37.41050669923425, -5.998031571507454], [37.410517344251275, -5.998062584549189], [37.41053025238216, -5.998099884018302], [37.41054215468466, -5.998130813241005], [37.410556487739086, -5.998161239549518], [37.41057576611638, -5.9981913305819035], [37.41059286519885, -5.998215135186911], [37.410615077242255, -5.998242208734155], [37.41063485853374, -5.998265007510781], [37.410653131082654, -5.998284704983234], [37.41067752242088, -5.998306246474385], [37.41070040501654, -5.998326279222965], [37.41072119213641, -5.998342037200928], [37.41074566729367, -5.998360980302095], [37.410767963156104, -5.998376905918121], [37.410790929570794, -5.998394843190908], [37.41081708110869, -5.998415546491742], [37.41083359345794, -5.998434238135815], [37.410850608721375, -5.998458629474044], [37.410872569307685, -5.998477740213275], [37.41088933311403, -5.998495174571872], [37.41090651601553, -5.998518895357847], [37.4109270516783, -5.998540353029966], [37.41094029508531, -5.998563906177878], [37.41095789708197, -5.998586621135473], [37.410977175459266, -5.9986162930727005], [37.410991843789816, -5.998636158183217], [37.41100969724357, -5.998663818463683], [37.4110253714025, -5.998690724372864], [37.41104322485626, -5.998711595311761], [37.41106141358614, -5.9987373277544975], [37.411085553467274, -5.998759623616934], [37.41110340692103, -5.998777644708753], [37.41111991927028, -5.998798934742808], [37.41114221513271, -5.998820476233959], [37.4111583083868, -5.998843777924776], [37.41117272526026, -5.998869594186544], [37.41119334474206, -5.9988908842206], [37.41120742633939, -5.998915024101734], [37.41122125647962, -5.998936481773853], [37.41124514490366, -5.99895735271275], [37.411260986700654, -5.998982498422265], [37.411279091611505, -5.999007560312748], [37.4113033991307, -5.999031951650977], [37.411322677508, -5.999054834246635], [37.41133759729564, -5.99907754920423], [37.41135813295841, -5.999098923057318], [37.41137405857444, -5.9991237334907055], [37.41138939745724, -5.999150555580854], [37.41140934638679, -5.999171510338783], [37.411427702754736, -5.999192465096712], [37.411442790180445, -5.999216018244624], [37.41146701388061, -5.9992356318980455], [37.4114849511534, -5.999259101226926], [37.41150205023587, -5.999280391260982], [37.41152602247894, -5.999300675466657], [37.41154647432268, -5.999319031834602], [37.41156357340515, -5.999338394030929], [37.41158704273403, -5.999358426779509], [37.411606488749385, -5.999381896108389], [37.411623587831855, -5.999401090666652], [37.41164345294237, -5.999421793967485], [37.41166147403419, -5.99944056943059], [37.41167756728828, -5.999460685998201], [37.411696426570415, -5.999481305480003], [37.41171360947192, -5.999502092599869], [37.41172953508794, -5.999523717910051], [37.41174956783652, -5.999545929953456], [37.41176666691899, -5.999568561092019], [37.41177974268794, -5.999592114239931], [37.41179860197008, -5.9996123146265745], [37.41181729361415, -5.99963259883225], [37.41183347068727, -5.9996517933905125], [37.41185191087425, -5.999672077596188], [37.411868926137686, -5.9996953792870045], [37.411885438486934, -5.999717591330409], [37.41190371103585, -5.999742737039924], [37.411921145394444, -5.999767128378153], [37.41193547844887, -5.9997922740876675], [37.4119511526078, -5.999817838892341], [37.41196934133768, -5.999844493344426], [37.41198250092566, -5.999865280464292], [37.41199800744653, -5.999902579933405], [37.41200689226389, -5.999934934079647], [37.412012089043856, -5.999965947121382], [37.412020303308964, -5.999996121972799], [37.412028182297945, -6.000025123357773], [37.412033043801785, -6.00005186162889], [37.4120375700295, -6.000078348442912], [37.41204293444753, -6.0001065116375685], [37.41204637102783, -6.0001265443861485], [37.41204980760813, -6.000153534114361], [37.412055004388094, -6.00018379278481], [37.4120565969497, -6.000208938494325], [37.41206221282482, -6.000239364802837], [37.41207000799477, -6.000273060053587], [37.41207243874669, -6.000301642343402], [37.41207713261247, -6.000331146642566], [37.41208132356405, -6.000360483303666], [37.412088280543685, -6.000381354242563], [37.41209180094302, -6.000403398647904], [37.412100015208125, -6.000431813299656], [37.41210722364485, -6.000458635389805], [37.41211208514869, -6.000487804412842], [37.41211929358542, -6.00052242167294], [37.41212566383183, -6.000545891001821], [37.41212918423116, -6.0005727130919695], [37.412134632468224, -6.000598110258579], [37.41213882341981, -6.000622333958745], [37.41214041598141, -6.000649742782116], [37.41214762441814, -6.000681258738041], [37.41215064190328, -6.000705733895302], [37.412149887531996, -6.000734567642212], [37.412156760692596, -6.000773962587118], [37.4121601972729, -6.00080581381917], [37.41216564550996, -6.000839089974761], [37.412172267213464, -6.000877059996128], [37.41217612288892, -6.000904385000467], [37.412182576954365, -6.000936236232519], [37.41219305433333, -6.000971356406808], [37.41219833493233, -6.000994993373752], [37.41220839321613, -6.001023491844535], [37.412218283861876, -6.001052241772413], [37.41222666576505, -6.001074369996786], [37.412239741533995, -6.001100018620491], [37.41225767880678, -6.001126924529672], [37.412272430956364, -6.001149388030171], [37.4122862610966, -6.001174701377749], [37.41230394691229, -6.001200685277581], [37.41231970489025, -6.0012187063694], [37.41233428940177, -6.0012430138885975], [37.412352645769715, -6.001266734674573], [37.41236312314868, -6.001290623098612], [37.41237351670861, -6.001317612826824], [37.41238667629659, -6.001348122954369], [37.412396064028144, -6.001373436301947], [37.412402015179396, -6.001403192058206], [37.41240612231195, -6.001435127109289], [37.41240763105452, -6.001458344981074], [37.41240310482681, -6.001490196213126], [37.41239229217172, -6.001524226740003], [37.41238173097372, -6.001550294458866], [37.41236597299576, -6.0015778709203005], [37.41234359331429, -6.00159983150661], [37.412325823679566, -6.001619612798095], [37.412301767617464, -6.001630341634154], [37.41227486170828, -6.001643082126975], [37.41225474514067, -6.0016452614218], [37.41223026998341, -6.001644255593419], [37.41220235824585, -6.00164232775569], [37.41217955946922, -6.001639310270548], [37.41215726360679, -6.001632185652852], [37.412129854783416, -6.001627659425139], [37.41210772655904, -6.001621121540666], [37.41208123974502, -6.001610392704606], [37.41205098107457, -6.001602848991752], [37.412030193954706, -6.001589270308614], [37.412007646635175, -6.0015749372541904], [37.41198283620179, -6.00156269967556], [37.4119614623487, -6.001553228124976], [37.411935813724995, -6.001552138477564], [37.41190572269261, -6.001562029123306], [37.41188049316406, -6.001569991931319], [37.41185501217842, -6.001574350520968], [37.41182810626924, -6.001586755737662], [37.411799440160394, -6.001595556735992], [37.41177563555539, -6.001599496230483], [37.411748645827174, -6.001607961952686], [37.41172693669796, -6.001612236723304], [37.41170531138778, -6.001619361341], [37.411680752411485, -6.001627659425139], [37.411658791825175, -6.001634867861867], [37.411636244505644, -6.00163989700377], [37.41161369718611, -6.0016432497650385], [37.41158679127693, -6.001646686345339], [37.41156617179513, -6.0016541462391615], [37.411540020257235, -6.001662611961365], [37.41151194088161, -6.001668144017458], [37.4114881362766, -6.001673843711615], [37.41146055981517, -6.001683818176389], [37.41143734194338, -6.001693541184068], [37.41141370497644, -6.0017006658017635], [37.41138679906726, -6.001709885895252], [37.41136106662452, -6.001718519255519], [37.411336340010166, -6.001721620559692], [37.41130834445357, -6.001728996634483], [37.41128001362085, -6.001732684671879], [37.411255203187466, -6.001738049089909], [37.41122846491635, -6.0017413180321455], [37.4112034868449, -6.001741150394082], [37.41117984987795, -6.001744084060192], [37.41115193814039, -6.001750538125634], [37.41112310439348, -6.001754142343998], [37.411097874864936, -6.001755651086569], [37.41107105277479, -6.0017631109803915], [37.41104632616043, -6.001769732683897], [37.41102277301252, -6.001773588359356], [37.41099486127496, -6.001782640814781], [37.410973738878965, -6.00178699940443], [37.41095094010234, -6.001791441813111], [37.41092495620251, -6.001799991354346], [37.4109045881778, -6.001806445419788], [37.410882376134396, -6.001814156770706], [37.410859325900674, -6.001826226711273], [37.410837449133396, -6.001832261681557], [37.41081599146128, -6.001835614442825], [37.41079478524625, -6.001842236146331], [37.41077248938382, -6.001849360764027], [37.4107510317117, -6.001854054629803], [37.41072932258248, -6.001862687990069], [37.410709792748094, -6.001871405169368], [37.41068850271404, -6.001878110691905], [37.410664362832904, -6.001889342442155], [37.410641480237246, -6.00190032273531], [37.41061868146062, -6.001905603334308], [37.41059529595077, -6.001919768750668], [37.41057509556413, -6.001928737387061], [37.410554978996515, -6.001933263614774], [37.410533940419555, -6.0019465908408165], [37.41051357239485, -6.001966372132301], [37.410488007590175, -6.001975843682885], [37.41046009585261, -6.001985650509596], [37.4104349501431, -6.001993613317609], [37.410412821918726, -6.0020009893924], [37.410387843847275, -6.002012388780713], [37.41036185994744, -6.002022111788392], [37.410338474437594, -6.002030912786722], [37.41031517274678, -6.002043234184384], [37.41029002703726, -6.002052454277873], [37.4102623667568, -6.002059495076537], [37.41023261100054, -6.00206988863647], [37.410205621272326, -6.002075336873531], [37.4101741053164, -6.002082293853164], [37.410145439207554, -6.002077013254166], [37.41011316888034, -6.002072906121612], [37.41008693352342, -6.002073157578707], [37.41005818359554, -6.002071816474199], [37.41003136150539, -6.002071900293231], [37.410001857206225, -6.002074750140309], [37.409976962953806, -6.002076594159007], [37.409948632121086, -6.002079611644149], [37.40991719998419, -6.002087239176035], [37.40989414975047, -6.002093693241477], [37.409870428964496, -6.0021053440868855], [37.40984293632209, -6.002116492018104], [37.409820556640625, -6.002126298844814], [37.409795159474015, -6.002134429290891], [37.4097665771842, -6.002142895013094], [37.40974210202694, -6.002142895013094], [37.409711591899395, -6.002143146470189], [37.40968267433345, -6.00214708596468], [37.40966012701392, -6.002149684354663], [37.40963322110474, -6.002152618020773], [37.40960681810975, -6.002163514494896], [37.409582594409585, -6.00216468796134], [37.40955585613847, -6.002168711274862], [37.40952719002962, -6.002171980217099], [37.409504475072026, -6.002174075692892], [37.409478072077036, -6.002176674082875], [37.40944756194949, -6.002184804528952], [37.40942509844899, -6.002190001308918], [37.40939944982529, -6.002193773165345], [37.409372041001916, -6.002203244715929], [37.40934991277754, -6.002206597477198], [37.409324683249, -6.0022131353616714], [37.40929568186402, -6.002221014350653], [37.40927455946803, -6.002228641882539], [37.40925067104399, -6.002232413738966], [37.40922326222062, -6.002240376546979], [37.409201888367534, -6.002249512821436], [37.40917883813381, -6.002256218343973], [37.40915251895785, -6.002266947180033], [37.40912846289575, -6.002267533913255], [37.40910247899592, -6.002270635217428], [37.409073896706104, -6.002281364053488], [37.40905201993883, -6.002289745956659], [37.409026538953185, -6.00229368545115], [37.40900030359626, -6.002300810068846], [37.408978175371885, -6.002306593582034], [37.40895462222397, -6.002311538904905], [37.40892595611513, -6.002318076789379], [37.40890424698591, -6.002324363216758], [37.40888161584735, -6.002331487834454], [37.40885722450912, -6.002345150336623], [37.40883752703667, -6.00235378369689], [37.4088151473552, -6.002359064295888], [37.40878991782665, -6.002365853637457], [37.40876913070679, -6.002372726798058], [37.40874448791146, -6.002381108701229], [37.40871615707874, -6.002391334623098], [37.4086941126734, -6.002396615222096], [37.40866812877357, -6.002400554716587], [37.4086391273886, -6.002411702647805], [37.40861624479294, -6.002412037923932], [37.408591099083424, -6.002418827265501], [37.40856243297458, -6.002430729568005], [37.40853904746473, -6.002438105642796], [37.40851080045104, -6.0024431347846985], [37.40848205052316, -6.002452354878187], [37.40845866501331, -6.0024575516581535], [37.40843226201832, -6.002465933561325], [37.40840242244303, -6.002473644912243], [37.40837777964771, -6.002478925511241], [37.408350287005305, -6.002484709024429], [37.40832212381065, -6.002492923289537], [37.408296475186944, -6.002497700974345], [37.40827141329646, -6.00250574760139], [37.4082435015589, -6.0025132074952126], [37.408222295343876, -6.002521924674511], [37.40819899365306, -6.002528043463826], [37.4081692378968, -6.002538604661822], [37.4081451818347, -6.00254338234663], [37.40812146104872, -6.002545226365328], [37.40809631533921, -6.002550842240453], [37.40807460620999, -6.002551931887865], [37.408052226528525, -6.002552267163992], [37.4080275837332, -6.002558637410402], [37.408004868775606, -6.002567019313574], [37.40798156708479, -6.002573054283857], [37.407956505194306, -6.002584034577012], [37.40792850963771, -6.002593422308564], [37.40790638141334, -6.002599708735943], [37.40788031369448, -6.0026088450104], [37.40785634145141, -6.0026157181710005], [37.407832369208336, -6.002618735656142], [37.40780797787011, -6.002628123387694], [37.40778526291251, -6.002632901072502], [37.40776179358363, -6.002633236348629], [37.40773790515959, -6.002636505290866], [37.40771200507879, -6.002639941871166], [37.407689122483134, -6.002640780061483], [37.40766397677362, -6.002644384279847], [37.407636819407344, -6.002650838345289], [37.40761217661202, -6.0026575438678265], [37.407586611807346, -6.002667183056474], [37.407559119164944, -6.00267774425447], [37.407535733655095, -6.002682521939278], [37.40751234814525, -6.002689143642783], [37.407486364245415, -6.0026966873556376], [37.40746457129717, -6.002700375393033], [37.40744017995894, -6.002705069258809], [37.407412100583315, -6.002711523324251], [37.407387625426054, -6.0027161333709955], [37.40736164152622, -6.002724431455135], [37.40733507089317, -6.0027361661195755], [37.40731160156429, -6.002740105614066], [37.40728922188282, -6.002746392041445], [37.407261561602354, -6.0027538519352674], [37.407238595187664, -6.002759383991361], [37.40721018053591, -6.002766760066152], [37.40718453191221, -6.002777069807053], [37.40716248750687, -6.002784362062812], [37.40713918581605, -6.002797605469823], [37.40711336955428, -6.002811435610056], [37.40709073841572, -6.002818560227752], [37.407064754515886, -6.002823673188686], [37.40703843533993, -6.002832390367985], [37.40701572038233, -6.002838341519237], [37.406991412863135, -6.0028463043272495], [37.4069638364017, -6.002852423116565], [37.40694288164377, -6.002856781706214], [37.40691798739135, -6.002863068133593], [37.406890243291855, -6.002872874960303], [37.40686803124845, -6.002877065911889], [37.40684363991022, -6.002883268520236], [37.406816398724914, -6.00289223715663], [37.40679552778602, -6.002898775041103], [37.40677230991423, -6.002903133630753], [37.406745655462146, -6.002914952114224], [37.40672579035163, -6.002915790304542], [37.4067032430321, -6.0029202327132225], [37.40667684003711, -6.002930039539933], [37.4066548794508, -6.002937750890851], [37.40663099102676, -6.002942360937595], [37.40660509094596, -6.002951581031084], [37.40658128634095, -6.002957699820399], [37.40655731409788, -6.002962477505207], [37.40653233602643, -6.002971529960632], [37.4065079446882, -6.002979576587677], [37.406481709331274, -6.002986030653119], [37.40645413286984, -6.002997346222401], [37.40643150173128, -6.0030032973736525], [37.40640627220273, -6.003009332343936], [37.40637584589422, -6.003024252131581], [37.40635204128921, -6.0030309576541185], [37.406327901408076, -6.003035316243768], [37.40630174987018, -6.003043530508876], [37.40627878345549, -6.003052331507206], [37.40625514648855, -6.003060545772314], [37.40622874349356, -6.003071274608374], [37.40620259195566, -6.003079069778323], [37.40617669187486, -6.003083009272814], [37.40614668466151, -6.003086529672146], [37.40611885674298, -6.003086110576987], [37.40609488449991, -6.003086697310209], [37.40606873296201, -6.003093905746937], [37.40604911930859, -6.0031019523739815], [37.406025398522615, -6.00310480222106], [37.40599673241377, -6.003112513571978], [37.40597376599908, -6.003120057284832], [37.40594794973731, -6.003124583512545], [37.405915427953005, -6.003130450844765], [37.405891623348, -6.003139838576317], [37.40586597472429, -6.003146208822727], [37.40584091283381, -6.0031552612781525], [37.40581635385752, -6.003164816647768], [37.40579213015735, -6.003166660666466], [37.405768828466535, -6.003178562968969], [37.40574477240443, -6.003180658444762], [37.40572230890393, -6.003183675929904], [37.405693056061864, -6.003195829689503], [37.40567092783749, -6.00320203229785], [37.405647123232484, -6.003204043954611], [37.4056204687804, -6.003217371180654], [37.40559951402247, -6.0032229870557785], [37.40557478740811, -6.003223657608032], [37.40554578602314, -6.003234134986997], [37.405520556494594, -6.003238745033741], [37.40549398586154, -6.003244528546929], [37.405466325581074, -6.0032533295452595], [37.40544478408992, -6.003258777782321], [37.40542156621814, -6.00326213054359], [37.4053958337754, -6.003273529931903], [37.40537278354168, -6.00328023545444], [37.4053477216512, -6.003286354243755], [37.405319306999445, -6.003296496346593], [37.40529868751764, -6.0033010225743055], [37.40527622401714, -6.003308482468128], [37.405249485746026, -6.0033203009516], [37.4052319675684, -6.0033278446644545], [37.40521009080112, -6.003334801644087], [37.40518318489194, -6.003349302336574], [37.40516021847725, -6.003353912383318], [37.40513691678643, -6.0033605340868235], [37.40511193871498, -6.00337159819901], [37.40509081631899, -6.003378387540579], [37.40506877191365, -6.003385093063116], [37.40504647605121, -6.003396827727556], [37.40502535365522, -6.003402443602681], [37.405002219602466, -6.003406131640077], [37.404975816607475, -6.0034161899238825], [37.40495067089796, -6.003426080569625], [37.40492334589362, -6.003430355340242], [37.40489384159446, -6.003439240157604], [37.404868276789784, -6.003445778042078], [37.40484396927059, -6.003448627889156], [37.40482016466558, -6.003458350896835], [37.40479753352702, -6.003467570990324], [37.40477473475039, -6.003472683951259], [37.40474590100348, -6.003482239320874], [37.40471765398979, -6.003492381423712], [37.40469594486058, -6.003498248755932], [37.40467071533203, -6.003506546840072], [37.40464523434639, -6.003512246534228], [37.40462520159781, -6.003516521304846], [37.40460357628763, -6.00352113135159], [37.40458161570132, -6.003527166321874], [37.404560409486294, -6.003530435264111], [37.40453786216676, -6.003540493547916], [37.40451238118112, -6.003551641479135], [37.40449394099414, -6.003557592630386], [37.4044703040272, -6.003564298152924], [37.40444205701351, -6.003576619550586], [37.404420683160424, -6.003585672006011], [37.40439687855542, -6.003591371700168], [37.4043682962656, -6.003602100536227], [37.40434633567929, -6.003608135506511], [37.404323453083634, -6.003616098314524], [37.40429528988898, -6.003627078607678], [37.404271652922034, -6.003633700311184], [37.404248770326376, -6.003636969253421], [37.40422370843589, -6.003651469945908], [37.404199820011854, -6.003658594563603], [37.40417576394975, -6.003664042800665], [37.40414978004992, -6.003675777465105], [37.40412396378815, -6.003683572635055], [37.40409806370735, -6.0036874283105135], [37.404073253273964, -6.003693882375956], [37.404046850278974, -6.0037006717175245], [37.40402522496879, -6.003701006993651], [37.403998570516706, -6.003706203773618], [37.40396881476045, -6.00371559150517], [37.40394936874509, -6.003718106076121], [37.40392589941621, -6.003721039742231], [37.403898825868964, -6.003729086369276], [37.403878290206194, -6.003732606768608], [37.403855407610536, -6.003736965358257], [37.40382741205394, -6.003743419423699], [37.40380545146763, -6.003749454393983], [37.40378382615745, -6.003752639517188], [37.403759099543095, -6.00376152433455], [37.403732696548104, -6.003769235685468], [37.40370746701956, -6.003769570961595], [37.40368416532874, -6.003770744428039], [37.403657510876656, -6.003774264827371], [37.403634628281, -6.003775857388973], [37.40361115895212, -6.003782479092479], [37.40358693525195, -6.003793124109507], [37.40356329828501, -6.003797817975283], [37.403537733480334, -6.0038055293262005], [37.40351057611406, -6.003812067210674], [37.40348291583359, -6.003817850723863], [37.40345852449536, -6.003821287304163], [37.403430780395865, -6.003829250112176], [37.4034084007144, -6.003835201263428], [37.40338543429971, -6.0038388054817915], [37.403359450399876, -6.00384266115725], [37.403336400166154, -6.003845511004329], [37.403311086818576, -6.003847103565931], [37.40328686311841, -6.003856994211674], [37.40326448343694, -6.003863029181957], [37.40324285812676, -6.003864873200655], [37.403219221159816, -6.003873003646731], [37.40319491364062, -6.003881134092808], [37.40317412652075, -6.0038859117776155], [37.40315107628703, -6.0038915276527405], [37.403127774596214, -6.00389982573688], [37.40310883149505, -6.003906698897481], [37.40308586508036, -6.003912314772606], [37.40305778570473, -6.003924384713173], [37.40303632803261, -6.0039315931499], [37.40301218815148, -6.0039333533495665], [37.402986623346806, -6.003941064700484], [37.4029628187418, -6.003949865698814], [37.40293993614614, -6.003954978659749], [37.402916215360165, -6.003963695839047], [37.40289215929806, -6.003972664475441], [37.40287262946367, -6.003978196531534], [37.402849830687046, -6.003986746072769], [37.40282351151109, -6.003995295614004], [37.40280297584832, -6.003997894003987], [37.40278017707169, -6.004002671688795], [37.402754947543144, -6.004009125754237], [37.40273089148104, -6.00401071831584], [37.40270968526602, -6.00401365198195], [37.40268303081393, -6.004021614789963], [37.40266241133213, -6.004023291170597], [37.402638187631965, -6.004023794084787], [37.40260826423764, -6.004028739407659], [37.4025843758136, -6.00403661839664], [37.40255721844733, -6.004038797691464], [37.40253148600459, -6.004048017784953], [37.402509693056345, -6.004054052755237], [37.40248756483197, -6.004062267020345], [37.40245948545635, -6.004072241485119], [37.40243995562196, -6.004078360274434], [37.4024154804647, -6.004086574539542], [37.40238639526069, -6.00409772247076], [37.40236577577889, -6.00410308688879], [37.40234180353582, -6.00411138497293], [37.4023150652647, -6.004118677228689], [37.40229184739292, -6.004122951999307], [37.40226410329342, -6.0041300766170025], [37.40223426371813, -6.0041421465575695], [37.402212135493755, -6.004148349165916], [37.40218548104167, -6.004154048860073], [37.40215421654284, -6.0041656997054815], [37.402130998671055, -6.004169136285782], [37.40210493095219, -6.0041742492467165], [37.40207442082465, -6.004185900092125], [37.40205463953316, -6.004193024709821], [37.40202924236655, -6.004201825708151], [37.401999067515135, -6.0042104590684175], [37.40197191014886, -6.0042075254023075], [37.401943914592266, -6.004211129620671], [37.40191709250212, -6.004214817658067], [37.40189144387841, -6.004215907305479], [37.40186638198793, -6.0042201820760965], [37.40183955989778, -6.0042299050837755], [37.401816761121154, -6.004233341664076], [37.40179262124002, -6.004245998337865], [37.40176672115922, -6.00425748154521], [37.40174224600196, -6.004264019429684], [37.40171651355922, -6.004273826256394], [37.40169145166874, -6.004281956702471], [37.401670161634684, -6.0042838007211685], [37.40164409391582, -6.004294445738196], [37.40161961875856, -6.004300564527512], [37.40159371867776, -6.0043067671358585], [37.40156672894955, -6.004317412152886], [37.401542756706476, -6.004326045513153], [37.401515347883105, -6.004338702186942], [37.40149305202067, -6.00435228087008], [37.40146832540631, -6.0043618362396955], [37.40144569426775, -6.004364183172584], [37.401425493881106, -6.004375582560897], [37.401400012895465, -6.004379438236356], [37.40137713029981, -6.004377594217658], [37.40135190077126, -6.004376923665404], [37.401326671242714, -6.004375666379929], [37.40129666402936, -6.004380527883768], [37.40126724354923, -6.004386395215988], [37.40124234929681, -6.004388239234686], [37.40121410228312, -6.004399051889777], [37.401187028735876, -6.004406595602632], [37.40116020664573, -6.0044099483639], [37.40113086998463, -6.004421431571245], [37.40110656246543, -6.004429226741195], [37.401082338765264, -6.0044345911592245], [37.401054007932544, -6.004443056881428], [37.40102701820433, -6.004446493461728], [37.40100204013288, -6.004454707726836], [37.400973960757256, -6.004466358572245], [37.40095183253288, -6.004467783495784], [37.400926100090146, -6.00448178127408], [37.400903552770615, -6.004491169005632], [37.40087606012821, -6.004497203975916], [37.40084613673389, -6.004510195925832], [37.400821996852756, -6.004519248381257], [37.400795090943575, -6.004522684961557], [37.40076567046344, -6.004535593092442], [37.400740357115865, -6.004541125148535], [37.40071487613022, -6.004545735195279], [37.400687048211694, -6.004555877298117], [37.40066383033991, -6.004562582820654], [37.40063692443073, -6.004568953067064], [37.40060809068382, -6.004577837884426], [37.40058537572622, -6.0045809391886], [37.400554697960615, -6.004583453759551], [37.40052854642272, -6.004588734358549], [37.400503903627396, -6.0045921709388494], [37.40047833882272, -6.004596194252372], [37.40044992417097, -6.004602983593941], [37.400426706299186, -6.0046068392694], [37.40040231496096, -6.00461564026773], [37.40037448704243, -6.004623016342521], [37.40034967660904, -6.004626285284758], [37.4003239441663, -6.004636259749532], [37.40029393695295, -6.004650928080082], [37.40027147345245, -6.004653023555875], [37.40024331025779, -6.004661489278078], [37.40021824836731, -6.004672888666391], [37.40019318647683, -6.00467867217958], [37.40016737021506, -6.004692669957876], [37.4001410510391, -6.004701387137175], [37.400116324424744, -6.004707841202617], [37.40008866414428, -6.004717145115137], [37.400060249492526, -6.004724772647023], [37.40003711543977, -6.004730220884085], [37.4000097066164, -6.004738267511129], [37.39998414181173, -6.004745559766889], [37.39996109157801, -6.004748661071062], [37.39993443712592, -6.004757545888424], [37.39990786649287, -6.004765257239342], [37.399882888421416, -6.004768190905452], [37.39985321648419, -6.00477883592248], [37.39982580766082, -6.004788223654032], [37.3997990693897, -6.00479643791914], [37.39977115765214, -6.004808843135834], [37.399744503200054, -6.004817979410291], [37.39971851930022, -6.004818733781576], [37.39969152957201, -6.004825187847018], [37.39966747350991, -6.004831390455365], [37.39964249543846, -6.004838598892093], [37.399614499881864, -6.004849495366216], [37.39959077909589, -6.00486047565937], [37.39956806413829, -6.004863493144512], [37.39954291842878, -6.004871875047684], [37.399519281461835, -6.004878832027316], [37.399494890123606, -6.00488050840795], [37.3994658049196, -6.0048898961395025], [37.39944049157202, -6.004897523671389], [37.39941316656768, -6.004901882261038], [37.399385841563344, -6.0049107670784], [37.399359941482544, -6.004918226972222], [37.399336472153664, -6.0049202386289835], [37.399308644235134, -6.00492887198925], [37.39928157068789, -6.004938175901771], [37.39925273694098, -6.00494597107172], [37.39922935143113, -6.00495183840394], [37.39920303225517, -6.004961058497429], [37.39918325096369, -6.004962902516127], [37.39915877580643, -6.004968266934156], [37.39913170225918, -6.0049753077328205], [37.39910865202546, -6.004976565018296], [37.39908166229725, -6.004982013255358], [37.39905090071261, -6.004988299682736], [37.39902617409825, -6.004990227520466], [37.39899918437004, -6.0049994476139545], [37.39897160790861, -6.005013361573219], [37.398948557674885, -6.005017384886742], [37.39892642945051, -6.005025263875723], [37.398896757513285, -6.005035657435656], [37.39887052215636, -6.005043955519795], [37.398844957351685, -6.005051247775555], [37.39881838671863, -6.005063820630312], [37.398792654275894, -6.005073459818959], [37.39876641891897, -6.005087457597256], [37.39874261431396, -6.005107657983899], [37.39872115664184, -6.00512282922864], [37.398696932941675, -6.005136240273714], [37.398671451956034, -6.005155434831977], [37.39865024574101, -6.0051714442670345], [37.39862862043083, -6.005186196416616], [37.398601211607456, -6.005205139517784], [37.398578664287925, -6.005220226943493], [37.398556703701615, -6.005239337682724], [37.398532731458545, -6.005261046811938], [37.39851353690028, -6.005275966599584], [37.398491241037846, -6.005290383473039], [37.39846852608025, -6.005309829488397], [37.398446314036846, -6.005328856408596], [37.398423263803124, -6.005346206948161], [37.39839845336974, -6.005373615771532], [37.398379761725664, -6.005393313243985], [37.398356795310974, -6.005407813936472], [37.39833374507725, -6.0054276790469885], [37.39831421524286, -6.005443437024951], [37.3982931766659, -6.00545609369874], [37.39826945587993, -6.0054724384099245], [37.398249842226505, -6.005492219701409], [37.39823039621115, -6.0055057145655155], [37.39820793271065, -6.0055251605808735], [37.39818563684821, -6.005544858053327], [37.39816409535706, -6.005559861660004], [37.398140877485275, -6.0055778827518225], [37.39811606705189, -6.0055979155004025], [37.39809486083686, -6.0056111589074135], [37.39807415753603, -6.005629096180201], [37.398049514740705, -6.005646446719766], [37.39803006872535, -6.00566228851676], [37.3980085272342, -6.0056788846850395], [37.39798832684755, -6.005701012909412], [37.39796762354672, -6.005716687068343], [37.39794817753136, -6.005728002637625], [37.397925378754735, -6.005748538300395], [37.397904423996806, -6.0057637095451355], [37.397879445925355, -6.005780305713415], [37.397858491167426, -6.005803439766169], [37.39783946424723, -6.005822299048305], [37.39781817421317, -6.005836045369506], [37.39779864437878, -6.0058497078716755], [37.39777903072536, -6.0058691538870335], [37.3977614287287, -6.00588021799922], [37.39773997105658, -6.005895556882024], [37.39771692082286, -6.005908632650971], [37.397697642445564, -6.005918774753809], [37.397675681859255, -6.005938136950135], [37.3976527992636, -6.0059574991464615], [37.39763452671468, -6.005972921848297], [37.397614158689976, -6.005993541330099], [37.39759177900851, -6.0060119815170765], [37.39757149480283, -6.00602732039988], [37.39754777401686, -6.006049197167158], [37.397527154535055, -6.006073001772165], [37.3975077085197, -6.0060911905020475], [37.39748474210501, -6.006114156916738], [37.39746286533773, -6.006133435294032], [37.39744174294174, -6.006144834682345], [37.39741852506995, -6.0061610117554665], [37.3973917029798, -6.006176183000207], [37.3973692394793, -6.006188504397869], [37.39734409376979, -6.006201999261975], [37.397320875898004, -6.006223624572158], [37.397301429882646, -6.006240472197533], [37.397278882563114, -6.006255224347115], [37.39725616760552, -6.006278358399868], [37.397235967218876, -6.006291266530752], [37.397214928641915, -6.006306437775493], [37.39719078876078, -6.006327476352453], [37.39717159420252, -6.0063480120152235], [37.39715097472072, -6.006366368383169], [37.397127002477646, -6.006386568769813], [37.39710822701454, -6.006400901824236], [37.39708920009434, -6.0064139775931835], [37.39706849679351, -6.0064345970749855], [37.39704988896847, -6.006455887109041], [37.39702759310603, -6.006467957049608], [37.397007727995515, -6.006485056132078], [37.39698794670403, -6.006506262347102], [37.39696833305061, -6.006521685048938], [37.396948551759124, -6.006540544331074], [37.396929524838924, -6.0065599065274], [37.396910497918725, -6.006575245410204], [37.396889040246606, -6.006595948711038], [37.396870851516724, -6.006615059450269], [37.39685056731105, -6.006631068885326], [37.3968322109431, -6.006653364747763], [37.39681268110871, -6.006674654781818], [37.39679298363626, -6.006687730550766], [37.39677102304995, -6.006699884310365], [37.39674755372107, -6.00671773776412], [37.396724838763475, -6.006731986999512], [37.39670195616782, -6.00675143301487], [37.39668058231473, -6.006770459935069], [37.3966596275568, -6.006787307560444], [37.39663959480822, -6.006808178499341], [37.39661897532642, -6.006832402199507], [37.39660003222525, -6.0068525187671185], [37.396580670028925, -6.006869953125715], [37.39656038582325, -6.0068887285888195], [37.396542616188526, -6.006903564557433], [37.396523002535105, -6.006922926753759], [37.396502047777176, -6.006940780207515], [37.396483439952135, -6.006954191252589], [37.396463826298714, -6.006973804906011], [37.39644077606499, -6.00699232891202], [37.39642216823995, -6.00700699724257], [37.396401381120086, -6.0070218332111835], [37.39637514576316, -6.007039351388812], [37.39635427482426, -6.007052008062601], [37.39633122459054, -6.007065754383802], [37.39630825817585, -6.007083859294653], [37.396290153265, -6.007101880386472], [37.39626978524029, -6.007117386907339], [37.396249920129776, -6.007137754932046], [37.39622628316283, -6.007156278938055], [37.3962053284049, -6.007169187068939], [37.39618479274213, -6.007183939218521], [37.396162413060665, -6.0072058998048306], [37.39614179357886, -6.00722442381084], [37.39612000063062, -6.007240517064929], [37.39609778858721, -6.007261471822858], [37.39607616327703, -6.007279409095645], [37.39605428650975, -6.007293658331037], [37.39603165537119, -6.007315116003156], [37.396012376993895, -6.007335064932704], [37.395992344245315, -6.007353505119681], [37.39597147330642, -6.007372951135039], [37.395949345082045, -6.007391894236207], [37.39593241363764, -6.007406143471599], [37.39591137506068, -6.007422152906656], [37.39588807336986, -6.007439503446221], [37.395865274593234, -6.0074488911777735], [37.39583862014115, -6.0074683371931314], [37.39581816829741, -6.007486945018172], [37.39579914137721, -6.007500691339374], [37.395777348428965, -6.007521646097302], [37.395758740603924, -6.00754301995039], [37.39573761820793, -6.007556933909655], [37.39571590907872, -6.007571937516332], [37.39569587633014, -6.007589288055897], [37.39567643031478, -6.007600016891956], [37.39565589465201, -6.007619295269251], [37.39563846029341, -6.007643518969417], [37.39561926573515, -6.007660115137696], [37.39559680223465, -6.007679309695959], [37.395575093105435, -6.007702359929681], [37.39555422216654, -6.0077177826315165], [37.39553142338991, -6.007736558094621], [37.395510636270046, -6.00775851868093], [37.3954869993031, -6.007776204496622], [37.39546788856387, -6.007798919454217], [37.395447017624974, -6.007824568077922], [37.39542966708541, -6.007847283035517], [37.39540720358491, -6.007878379896283], [37.39538918249309, -6.0079053696244955], [37.39537828601897, -6.0079339519143105], [37.395367389544845, -6.007969742640853], [37.39535825327039, -6.0080108139663935], [37.395352805033326, -6.008044425398111], [37.395352218300104, -6.008083652704954], [37.395353224128485, -6.008122628554702], [37.395351715385914, -6.008157581090927], [37.395351799204946, -6.008191276341677], [37.39535632543266, -6.008223965764046], [37.39536018110812, -6.008253721520305], [37.3953662160784, -6.008286075666547], [37.39537141285837, -6.008317591622472], [37.395375184714794, -6.008347598835826], [37.39537778310478, -6.008379617705941], [37.39538088440895, -6.0084116365760565], [37.39538373425603, -6.008444577455521], [37.39538767375052, -6.008476763963699], [37.39539320580661, -6.008511884137988], [37.39539354108274, -6.008545663207769], [37.395396726205945, -6.008578101173043], [37.395401587709785, -6.008613053709269], [37.395404102280736, -6.008646916598082], [37.39540862850845, -6.008683294057846], [37.39540829323232, -6.008717073127627], [37.39541499875486, -6.008748672902584], [37.395420614629984, -6.008784128353], [37.395423129200935, -6.0088159795850515], [37.395429499447346, -6.008844478055835], [37.395440647378564, -6.008872138336301], [37.3954507894814, -6.008900720626116], [37.395455399528146, -6.008927961811423], [37.39546403288841, -6.008960232138634], [37.395472414791584, -6.008991161361337], [37.395479287952185, -6.009016139432788], [37.39548448473215, -6.009053438901901], [37.39549286663532, -6.009081099182367], [37.395497811958194, -6.009110184386373], [37.395501751452684, -6.009140443056822], [37.395508624613285, -6.0091685224324465], [37.39551516249776, -6.009198613464832], [37.395518431439996, -6.009236918762326], [37.395522790029645, -6.009271955117583], [37.39552899263799, -6.00930355489254], [37.39553234539926, -6.009337585419416], [37.395537877455354, -6.0093720350414515], [37.39554357714951, -6.009402964264154], [37.395548271015286, -6.00943548604846], [37.39555564709008, -6.009468259289861], [37.395563861355186, -6.009499942883849], [37.395570231601596, -6.009530955925584], [37.39557744003832, -6.009566746652126], [37.395581463351846, -6.009599855169654], [37.39558364264667, -6.0096388310194016], [37.39558464847505, -6.009677220135927], [37.3955821339041, -6.009712843224406], [37.395576098933816, -6.009746789932251], [37.39556604065001, -6.009782245382667], [37.39555774256587, -6.009811665862799], [37.39554952830076, -6.009836727753282], [37.395544247701764, -6.009868998080492], [37.39553620107472, -6.009894646704197], [37.39552446641028, -6.009916355833411], [37.39550510421395, -6.009941920638084], [37.395484652370214, -6.00995734333992], [37.39546394906938, -6.009969748556614], [37.395439222455025, -6.009982656687498], [37.39541441202164, -6.009991206228733], [37.39539354108274, -6.010000091046095], [37.39537099376321, -6.010009981691837], [37.39534500986338, -6.010021045804024], [37.39532086998224, -6.010033618658781], [37.39529589191079, -6.010047784075141], [37.39527191966772, -6.0100608598440886], [37.39525096490979, -6.010068319737911], [37.39522699266672, -6.010078461840749], [37.39519899711013, -6.010087262839079], [37.39517888054252, -6.010094890370965], [37.39514929242432, -6.010101260617375], [37.39512029103935, -6.010106960311532], [37.39510319195688, -6.01010930724442], [37.3950760345906, -6.010111318901181], [37.39505155943334, -6.010118694975972], [37.395024821162224, -6.0101203713566065], [37.39499908871949, -6.010124897584319], [37.39496748894453, -6.010135626420379], [37.3949499707669, -6.010143086314201], [37.39492499269545, -6.010147612541914], [37.394896913319826, -6.010156832635403], [37.39486766047776, -6.010158089920878], [37.39484620280564, -6.010159011930227], [37.394823068752885, -6.010166807100177], [37.394798174500465, -6.010170495137572], [37.39477931521833, -6.010174518451095], [37.39475182257593, -6.010185666382313], [37.3947290237993, -6.010191282257438], [37.39470463246107, -6.010194048285484], [37.39467764273286, -6.010201089084148], [37.394650569185615, -6.010205363854766], [37.394627602770925, -6.010210225358605], [37.394596422091126, -6.010219529271126], [37.39457312040031, -6.010220618918538], [37.394548980519176, -6.010223552584648], [37.39451897330582, -6.010234281420708], [37.39449198357761, -6.010238388553262], [37.394467843696475, -6.0102390591055155], [37.39443984813988, -6.010247943922877], [37.3944147862494, -6.010255319997668], [37.39439223892987, -6.01025833748281], [37.3943648301065, -6.010264540091157], [37.39433960057795, -6.010272419080138], [37.39431579597294, -6.010273592546582], [37.39429014734924, -6.010279878973961], [37.394263073801994, -6.010283650830388], [37.39424027502537, -6.010281136259437], [37.39421303384006, -6.01028423756361], [37.394182020798326, -6.01029203273356], [37.39416140131652, -6.010290272533894], [37.39413592033088, -6.010292200371623], [37.39411043934524, -6.010299744084477], [37.394088646396995, -6.010301755741239], [37.39406366832554, -6.010312819853425], [37.3940350022167, -6.010324973613024], [37.394013963639736, -6.010329248383641], [37.39399015903473, -6.010336708277464], [37.3939623311162, -6.010347520932555], [37.39394053816795, -6.01035607047379], [37.39391287788749, -6.010361770167947], [37.39388521760702, -6.010371074080467], [37.39386317320168, -6.0103715769946575], [37.39383643493056, -6.010375935584307], [37.39380936138332, -6.010388759896159], [37.39378639496863, -6.010392531752586], [37.393759321421385, -6.010397644713521], [37.39373132586479, -6.01040686480701], [37.39370911382139, -6.010408792644739], [37.393683549016714, -6.010418767109513], [37.39365270361304, -6.010428322479129], [37.393625965341926, -6.010431759059429], [37.39359914325178, -6.010440140962601], [37.39357575774193, -6.010446343570948], [37.39355304278433, -6.010451121255755], [37.39352856762707, -6.010455312207341], [37.393502751365304, -6.010465789586306], [37.39347827620804, -6.010471321642399], [37.39345162175596, -6.010480374097824], [37.39342647604644, -6.010491605848074], [37.39340250380337, -6.010494790971279], [37.3933769389987, -6.010503927245736], [37.393350284546614, -6.010513314977288], [37.393325893208385, -6.010520271956921], [37.39330024458468, -6.010533515363932], [37.393271746113896, -6.010544244199991], [37.393247187137604, -6.010548183694482], [37.39322438836098, -6.010556062683463], [37.393199829384685, -6.010564025491476], [37.39317703060806, -6.010571401566267], [37.39315129816532, -6.010576011613011], [37.393126571550965, -6.010583387687802], [37.393102096393704, -6.010587075725198], [37.3930798843503, -6.0105951223522425], [37.39305163733661, -6.010603504255414], [37.39302858710289, -6.010606270283461], [37.3930056206882, -6.0106101259589195], [37.39297620020807, -6.010619346052408], [37.39295398816466, -6.010624459013343], [37.39292875863612, -6.010629907250404], [37.39290000870824, -6.010642647743225], [37.39287997595966, -6.010647341609001], [37.3928552493453, -6.010654047131538], [37.3928243201226, -6.010668044909835], [37.39280051551759, -6.010674415156245], [37.392775705084205, -6.010678270831704], [37.39274628460407, -6.010688580572605], [37.39272356964648, -6.010695789009333], [37.39269792102277, -6.010700315237045], [37.39266942255199, -6.010707188397646], [37.39264452829957, -6.010715151205659], [37.39261904731393, -6.010716073215008], [37.392592057585716, -6.010722778737545], [37.39256917499006, -6.010732585564256], [37.392544113099575, -6.010737027972937], [37.39252072758973, -6.010744320228696], [37.39249759353697, -6.010755887255073], [37.39247345365584, -6.010761586949229], [37.392446380108595, -6.010774411261082], [37.39242123439908, -6.010781200602651], [37.392396004870534, -6.010784050449729], [37.392371613532305, -6.01079486310482], [37.392345713451505, -6.010798048228025], [37.39232308231294, -6.010803412646055], [37.392299445346, -6.010815147310495], [37.39227228797972, -6.010827720165253], [37.392247058451176, -6.010836437344551], [37.39222032018006, -6.010847333818674], [37.39219643175602, -6.010851608589292], [37.39217581227422, -6.010853368788958], [37.39215301349759, -6.010860577225685], [37.392125856131315, -6.010863594710827], [37.3921007104218, -6.010865438729525], [37.392074055969715, -6.010871138423681], [37.3920505028218, -6.010871222242713], [37.39202753640711, -6.01087449118495], [37.3919987026602, -6.010879436507821], [37.391969449818134, -6.010883962735534], [37.391939694061875, -6.010888991877437], [37.39191052503884, -6.010896535590291], [37.391882948577404, -6.010901732370257], [37.39185595884919, -6.010904079303145], [37.391827795654535, -6.010913634672761], [37.39180130884051, -6.010917574167252], [37.39177331328392, -6.010918831452727], [37.391745653003454, -6.010918999090791], [37.39172352477908, -6.010921597480774], [37.391698295250535, -6.010925956070423], [37.391667449846864, -6.010937606915832], [37.39164071157575, -6.010943055152893], [37.391616236418486, -6.010948000475764], [37.391589833423495, -6.0109564661979675], [37.39156904630363, -6.010961076244712], [37.39155010320246, -6.010968284681439], [37.39153057336807, -6.010986641049385], [37.39150760695338, -6.010997369885445], [37.391485227271914, -6.011002650484443], [37.39146117120981, -6.0110097751021385], [37.39143988117576, -6.011015558615327], [37.39141792058945, -6.0110189113765955], [37.39139260724187, -6.011024946346879], [37.39136955700815, -6.011027293279767], [37.39134667441249, -6.011021006852388], [37.39132412709296, -6.011010445654392], [37.391301076859236, -6.011011451482773], [37.391278529539704, -6.011011619120836], [37.391254138201475, -6.011011032387614], [37.39123008213937, -6.011011367663741], [37.39120845682919, -6.011011702939868], [37.3911802098155, -6.01101572625339], [37.39115581847727, -6.011014301329851], [37.391129583120346, -6.011009020730853], [37.391096306964755, -6.011004997417331], [37.391074346378446, -6.0109965316951275], [37.391048949211836, -6.010984126478434], [37.39102036692202, -6.010969374328852], [37.39100025035441, -6.01095462217927], [37.39097845740616, -6.010941043496132], [37.3909523896873, -6.010926961898804], [37.390936044976115, -6.010913718491793], [37.39092028699815, -6.0109002236276865], [37.39089908078313, -6.010886812582612], [37.39088298752904, -6.010871976613998], [37.390865636989474, -6.010861080139875], [37.39084837026894, -6.010838532820344], [37.39082875661552, -6.010823026299477], [37.390810484066606, -6.010806513950229], [37.39078056067228, -6.010796288028359], [37.3907520622015, -6.010782793164253], [37.39072926342487, -6.0107671190053225], [37.390697579830885, -6.010754546150565], [37.390670757740736, -6.010741386562586], [37.39064603112638, -6.010727807879448], [37.39061267115176, -6.010712971910834], [37.390587609261274, -6.010698219761252], [37.39056346938014, -6.010680366307497], [37.39053069613874, -6.010671565309167], [37.39050596952438, -6.010657651349902], [37.390478728339076, -6.01064583286643], [37.390450816601515, -6.010635606944561], [37.390425670892, -6.010623704642057], [37.39040069282055, -6.010608617216349], [37.390373619273305, -6.0105962958186865], [37.39035098813474, -6.010581040754914], [37.390323327854276, -6.010564528405666], [37.39029466174543, -6.010553129017353], [37.39026792347431, -6.010546255856752], [37.390240179374814, -6.010536113753915], [37.390210842713714, -6.010531336069107], [37.390185948461294, -6.010518427938223], [37.39015552215278, -6.010504681617022], [37.39013481885195, -6.010489594191313], [37.39011193625629, -6.01047383621335], [37.3900889698416, -6.010457072407007], [37.390065835788846, -6.010442152619362], [37.39004387520254, -6.010425891727209], [37.39001361653209, -6.010412396863103], [37.389990063384175, -6.010397896170616], [37.389962654560804, -6.010385239496827], [37.389934826642275, -6.010379288345575], [37.38991152495146, -6.010369062423706], [37.38988964818418, -6.010359674692154], [37.38986290991306, -6.010343665257096], [37.38983097486198, -6.010338803753257], [37.38980691879988, -6.010326566174626], [37.38977565430105, -6.010322039946914], [37.38974690437317, -6.010319022461772], [37.389720836654305, -6.010316926985979], [37.38968848250806, -6.010318603366613], [37.38966149277985, -6.010316340252757], [37.38963592797518, -6.010314496234059], [37.389603573828936, -6.010319190099835], [37.38957365043461, -6.010322459042072], [37.38954775035381, -6.010324051603675], [37.38951740786433, -6.010330421850085], [37.38948849029839, -6.010336373001337], [37.38946409896016, -6.010336456820369], [37.389433505013585, -6.010341485962272], [37.38940752111375, -6.010344922542572], [37.38938229158521, -6.010354897007346], [37.38935597240925, -6.010363781824708], [37.389326468110085, -6.010370571166277], [37.389300651848316, -6.010376270860434], [37.389271734282374, -6.010384485125542], [37.38924323581159, -6.010394208133221], [37.38921959884465, -6.010404601693153], [37.38919327966869, -6.010421616956592], [37.38917173817754, -6.010435279458761], [37.389147598296404, -6.010446008294821], [37.38912186585367, -6.010460089892149], [37.38909998908639, -6.0104690585285425], [37.38907509483397, -6.0104770213365555], [37.38904969766736, -6.01048456504941], [37.389024049043655, -6.010496551170945], [37.38900284282863, -6.010502502322197], [37.38897996023297, -6.010506944730878], [37.38895447924733, -6.010510632768273], [37.38893344067037, -6.0105144046247005], [37.38890754058957, -6.010519936680794], [37.38888096995652, -6.010527983307838], [37.3888592608273, -6.010529827326536], [37.388832522556186, -6.010537538677454], [37.38880737684667, -6.010545501485467], [37.38878633826971, -6.010547848418355], [37.3887627851218, -6.010557906702161], [37.38873730413616, -6.010569138452411], [37.388711823150516, -6.010571988299489], [37.38868458196521, -6.010578358545899], [37.38865985535085, -6.0105871595442295], [37.38863714039326, -6.010590009391308], [37.388612078502774, -6.010597385466099], [37.38858877681196, -6.010608449578285], [37.388567151501775, -6.010612808167934], [37.388542257249355, -6.010627308860421], [37.38851870410144, -6.010645162314177], [37.38849272020161, -6.010656310245395], [37.38846816122532, -6.010671481490135], [37.388441590592265, -6.010678187012672], [37.38841543905437, -6.010678522288799], [37.38838367164135, -6.010687407106161], [37.388357603922486, -6.010698806494474], [37.38833539187908, -6.0107035003602505], [37.38830437883735, -6.010716576129198], [37.38827764056623, -6.010721102356911], [37.38825358450413, -6.010723365470767], [37.38822382874787, -6.0107350163161755], [37.38819834776223, -6.010742476209998], [37.38817311823368, -6.010743649676442], [37.38814285956323, -6.01075085811317], [37.388117630034685, -6.010753624141216], [37.38809491507709, -6.010751696303487], [37.38806574605405, -6.010761586949229], [37.3880460485816, -6.010762508958578], [37.3880217410624, -6.010767621919513], [37.3879924044013, -6.010778853669763], [37.387970024719834, -6.010788241401315], [37.387945130467415, -6.010797377675772], [37.38791302777827, -6.010808693245053], [37.387888971716166, -6.010818080976605], [37.38786424510181, -6.01081732660532], [37.387835662811995, -6.010826211422682], [37.38781353458762, -6.010832581669092], [37.38779081963003, -6.010838784277439], [37.38776106387377, -6.010850518941879], [37.38773658871651, -6.010854793712497], [37.387709598988295, -6.010859236121178], [37.387683195993304, -6.010865354910493], [37.38765746355057, -6.010874155908823], [37.38763106055558, -6.010880693793297], [37.38761018961668, -6.010888572782278], [37.38758537918329, -6.010898128151894], [37.38756090402603, -6.010902235284448], [37.38753500394523, -6.0109087731689215], [37.3875074274838, -6.010914389044046], [37.387479515746236, -6.010922687128186], [37.38745076581836, -6.010932996869087], [37.38742637448013, -6.01093995384872], [37.387397876009345, -6.010949844494462], [37.38736962899566, -6.010958394035697], [37.387346075847745, -6.010958896949887], [37.3873174097389, -6.010971134528518], [37.38729209639132, -6.010976079851389], [37.387267369776964, -6.010982198640704], [37.38724222406745, -6.010983539745212], [37.387214144691825, -6.010985551401973], [37.38718690350652, -6.010991251096129], [37.38716242834926, -6.010992927476764], [37.38713753409684, -6.010997286066413], [37.387115405872464, -6.011005500331521], [37.38708791323006, -6.011010278016329], [37.38706377334893, -6.0110109485685825], [37.38703954964876, -6.01101228967309], [37.38701976835728, -6.0110121220350266], [37.386998645961285, -6.011013966053724], [37.386972242966294, -6.01102109067142], [37.386942487210035, -6.011025365442038], [37.38691776059568, -6.011026035994291], [37.38688498735428, -6.011029137298465], [37.38685539923608, -6.011029304936528], [37.38683142699301, -6.011030478402972], [37.3868062812835, -6.011039363220334], [37.38678339868784, -6.0110504273325205], [37.38676303066313, -6.011057468131185], [37.3867317661643, -6.011067694053054], [37.3867042735219, -6.011072052642703], [37.38668038509786, -6.011077836155891], [37.38664610311389, -6.01108169183135], [37.38662171177566, -6.011083871126175], [37.386596482247114, -6.0110866371542215], [37.38656563684344, -6.011097282171249], [37.38653998821974, -6.011097365990281], [37.386515429243445, -6.0111090168356895], [37.38648374564946, -6.011115973815322], [37.38646178506315, -6.0111231822520494], [37.386436304077506, -6.011126451194286], [37.3864088114351, -6.011132737621665], [37.38638433627784, -6.011132067069411], [37.3863606993109, -6.011135587468743], [37.38633572123945, -6.011143047362566], [37.38631334155798, -6.011146903038025], [37.38629054278135, -6.011155787855387], [37.38626363687217, -6.011166600510478], [37.38623924553394, -6.011169198900461], [37.3862174525857, -6.011170456185937], [37.38618736155331, -6.0111732222139835], [37.38615986891091, -6.01117791607976], [37.386137656867504, -6.011180430650711], [37.386109828948975, -6.011193171143532], [37.386086108163, -6.0111988708376884], [37.38606037572026, -6.01120138540864], [37.38603129051626, -6.011205241084099], [37.3860065639019, -6.011207504197955], [37.385983848944306, -6.0112138744443655], [37.38595560193062, -6.011222004890442], [37.38593062385917, -6.011224938556552], [37.38590371794999, -6.011225776746869], [37.38587454892695, -6.011229045689106], [37.38584822975099, -6.011226279661059], [37.385823084041476, -6.011226279661059], [37.38579550758004, -6.011231392621994], [37.385769272223115, -6.011227956041694], [37.38574203103781, -6.0112241841852665], [37.385713616386056, -6.011223178356886], [37.385690649971366, -6.011224100366235], [37.38566927611828, -6.011224435642362], [37.3856370896101, -6.011232817545533], [37.38561127334833, -6.011233823373914], [37.38558202050626, -6.011237343773246], [37.38555125892162, -6.011244300752878], [37.385527957230806, -6.01124731823802], [37.385503398254514, -6.011248407885432], [37.38547448068857, -6.0112500842660666], [37.385448245331645, -6.01125075481832], [37.38542259670794, -6.011253772303462], [37.38539468497038, -6.011259891092777], [37.385371970012784, -6.011263746768236], [37.38534581847489, -6.011265087872744], [37.38531623035669, -6.01126441732049], [37.38529653288424, -6.01126492023468], [37.38527272827923, -6.011265339329839], [37.38524070940912, -6.01126978173852], [37.3852174077183, -6.011266512796283], [37.38519050180912, -6.011258801445365], [37.3851568903774, -6.011260310187936], [37.38512881100178, -6.011262824758887], [37.38510492257774, -6.011262154206634], [37.3850741609931, -6.011265674605966], [37.38505127839744, -6.011263495311141], [37.385024623945355, -6.011263830587268], [37.38500132225454, -6.011267015710473], [37.38498196005821, -6.011270619928837], [37.384958574548364, -6.01127028465271], [37.38492756150663, -6.0112701170146465], [37.384902499616146, -6.011263579130173], [37.384873162955046, -6.011257376521826], [37.38484030589461, -6.0112494975328445], [37.384815495461226, -6.011238265782595], [37.384786577895284, -6.0112333204597235], [37.38475556485355, -6.011227536946535], [37.38473913632333, -6.011220412328839], [37.384714744985104, -6.011217562481761], [37.384683983400464, -6.0112168081104755], [37.38465397618711, -6.011208845302463], [37.38463050685823, -6.011202810332179], [37.384597482159734, -6.0112018045037985], [37.38457376137376, -6.011196942999959], [37.38454509526491, -6.0111914947628975], [37.38451709970832, -6.011183280497789], [37.38449069671333, -6.011178754270077], [37.3844678979367, -6.011179760098457], [37.38443948328495, -6.011179760098457], [37.38442154601216, -6.011182526126504], [37.38439321517944, -6.011182861402631], [37.38436287268996, -6.011192332953215], [37.38433487713337, -6.011202223598957], [37.3843090608716, -6.011203648522496], [37.38427855074406, -6.011197026818991], [37.384252566844225, -6.0111961886286736], [37.38422054797411, -6.011198703199625], [37.38419406116009, -6.011197445914149], [37.38416883163154, -6.011190991848707], [37.38413597457111, -6.011195434257388], [37.3841089848429, -6.0111914947628975], [37.38408434204757, -6.011193171143532], [37.384050227701664, -6.011188896372914], [37.38402625545859, -6.011180682107806], [37.3840016964823, -6.011176239699125], [37.383971605449915, -6.011176994070411], [37.38394947722554, -6.011169366538525], [37.38392382860184, -6.011164505034685], [37.383894408121705, -6.011154614388943], [37.3838737886399, -6.011142041534185], [37.38384956493974, -6.011133240535855], [37.38381846807897, -6.011129468679428], [37.383799189701676, -6.011123517528176], [37.383776642382145, -6.011120583862066], [37.38374847918749, -6.011117482557893], [37.383725345134735, -6.0111096035689116], [37.38370112143457, -6.011100215837359], [37.383673964068294, -6.011086804792285], [37.38365024328232, -6.011073477566242], [37.383626187220216, -6.011060569435358], [37.38358830101788, -6.011053528636694], [37.383563155308366, -6.011044476181269], [37.38353557884693, -6.011032909154892], [37.38350666128099, -6.011019833385944], [37.38348654471338, -6.011003153398633], [37.38346324302256, -6.0109873954206705], [37.38343734294176, -6.010969206690788], [37.38341856747866, -6.010948671028018], [37.38339627161622, -6.010933080688119], [37.38336651585996, -6.010925369337201], [37.38334245979786, -6.010905923321843], [37.383315805345774, -6.010890835896134], [37.383287977427244, -6.010879436507821], [37.38326174207032, -6.010864432901144], [37.38323441706598, -6.01084909401834], [37.38320365548134, -6.010829312726855], [37.38317683339119, -6.010808609426022], [37.383156046271324, -6.010789750143886], [37.383129224181175, -6.010765610262752], [37.38311463966966, -6.010736022144556], [37.3830915056169, -6.010715654119849], [37.38306635990739, -6.010701153427362], [37.3830484226346, -6.010681875050068], [37.38302696496248, -6.010664105415344], [37.38299896940589, -6.010649688541889], [37.38298430107534, -6.010630493983626], [37.38296234048903, -6.01061406545341], [37.382935769855976, -6.010591769590974], [37.38291883841157, -6.010568803176284], [37.3828967101872, -6.01055103354156], [37.382869720458984, -6.010534940287471], [37.382851615548134, -6.010515242815018], [37.38282856531441, -6.01049680262804], [37.38280241377652, -6.010486325249076], [37.38278355449438, -6.010471573099494], [37.38275757059455, -6.010455060750246], [37.3827307485044, -6.010437877848744], [37.38271222449839, -6.010419018566608], [37.38269059918821, -6.010399321094155], [37.382666459307075, -6.010381383821368], [37.38265153951943, -6.0103565733879805], [37.38263008184731, -6.010338384658098], [37.38260527141392, -6.01031843572855], [37.38259102217853, -6.0102917812764645], [37.382575599476695, -6.010271832346916], [37.38254894502461, -6.010249871760607], [37.38253318704665, -6.010224809870124], [37.382515920326114, -6.010202011093497], [37.382491109892726, -6.01018013432622], [37.382474932819605, -6.010160353034735], [37.38245263695717, -6.010139649733901], [37.38242304883897, -6.010124981403351], [37.38240033388138, -6.010111067444086], [37.38237787038088, -6.010093130171299], [37.38235205411911, -6.010074689984322], [37.38233596086502, -6.0100516397506], [37.382310731336474, -6.010031187906861], [37.3822851665318, -6.010005371645093], [37.382272593677044, -6.00997687317431], [37.38225440494716, -6.009955666959286], [37.382229594513774, -6.009937226772308], [37.38220939412713, -6.009918283671141], [37.38218542188406, -6.00990068167448], [37.38215667195618, -6.009883666411042], [37.382135298103094, -6.009867237880826], [37.38211065530777, -6.009852485731244], [37.38208618015051, -6.009833291172981], [37.38206413574517, -6.0098152700811625], [37.38204242661595, -6.009800182655454], [37.38201887346804, -6.009786520153284], [37.381996996700764, -6.009770007804036], [37.381975622847676, -6.009755088016391], [37.381949219852686, -6.009744945913553], [37.38192507997155, -6.009730696678162], [37.38190236501396, -6.009719045832753], [37.381875459104776, -6.009705802425742], [37.381850983947515, -6.009692223742604], [37.38182919099927, -6.0096771363168955], [37.381802117452025, -6.009664982557297], [37.38178174942732, -6.009650900959969], [37.38176171667874, -6.009632628411055], [37.381737576797605, -6.00960697978735], [37.38171871751547, -6.009589629247785], [37.38169834949076, -6.0095705185085535], [37.38166943192482, -6.00955568253994], [37.38164487294853, -6.009540343657136], [37.38161897286773, -6.009529028087854], [37.381591480225325, -6.009524753317237], [37.38156432285905, -6.0095245856791735], [37.381538758054376, -6.009520646184683], [37.38150774501264, -6.0095177963376045], [37.38148033618927, -6.00951611995697], [37.381453262642026, -6.009509498253465], [37.38142501562834, -6.009500781074166], [37.38139710389078, -6.009491812437773], [37.381374053657055, -6.009485945105553], [37.38134597428143, -6.00948728621006], [37.381322756409645, -6.009481335058808], [37.38129853270948, -6.009479323402047], [37.381266597658396, -6.009480580687523], [37.381242122501135, -6.009482508525252], [37.38121756352484, -6.009483179077506], [37.38118814304471, -6.009484687820077], [37.3811643384397, -6.009481167420745], [37.38114061765373, -6.009480580687523], [37.38111136481166, -6.009479742497206], [37.381092086434364, -6.009473372250795], [37.38107146695256, -6.009468510746956], [37.381045734509826, -6.009462811052799], [37.381020337343216, -6.0094553511589766], [37.38099510781467, -6.009446633979678], [37.380967950448394, -6.009438168257475], [37.380940122529864, -6.009432887658477], [37.38091799430549, -6.009426936507225], [37.38089209422469, -6.009423499926925], [37.38086694851518, -6.009418638423085], [37.380840545520186, -6.0094156209379435], [37.380814645439386, -6.009410172700882], [37.380788661539555, -6.0094064846634865], [37.3807639349252, -6.009401204064488], [37.38073610700667, -6.009398438036442], [37.380711548030376, -6.009394079446793], [37.38068883307278, -6.009388715028763], [37.380662178620696, -6.009384607896209], [37.380636278539896, -6.009377650916576], [37.38061431795359, -6.009367676451802], [37.38059001043439, -6.009359462186694], [37.38056377507746, -6.009349236264825], [37.38054072484374, -6.0093354899436235], [37.38051633350551, -6.009316295385361], [37.380492025986314, -6.009295927360654], [37.38047392107546, -6.009277319535613], [37.38045606762171, -6.009255275130272], [37.380433436483145, -6.00923172198236], [37.38041767850518, -6.009209007024765], [37.3804029263556, -6.009187633171678], [37.380383061245084, -6.009165840223432], [37.38036881200969, -6.00914228707552], [37.38035414367914, -6.009120577946305], [37.38033444620669, -6.009095599874854], [37.38031533546746, -6.0090757347643375], [37.38029488362372, -6.009055953472853], [37.380275689065456, -6.009040530771017], [37.380252219736576, -6.00902502425015], [37.38023143261671, -6.009013121947646], [37.38020896911621, -6.008998956531286], [37.380182230845094, -6.008980600163341], [37.38016001880169, -6.008963584899902], [37.38014015369117, -6.008947659283876], [37.380116349086165, -6.008929051458836], [37.38009640015662, -6.0089111141860485], [37.380076283589005, -6.0088910814374685], [37.380046946927905, -6.008873311802745], [37.3800247348845, -6.008854117244482], [37.38000134937465, -6.008835760876536], [37.37997335381806, -6.008816733956337], [37.3799557518214, -6.008794521912932], [37.37993580289185, -6.008772728964686], [37.3799142614007, -6.0087517742067575], [37.37989515066147, -6.00873526185751], [37.37987612374127, -6.008708858862519], [37.379850978031754, -6.008691508322954], [37.37982591614127, -6.008673487231135], [37.37979901023209, -6.008656304329634], [37.37977252341807, -6.008645156398416], [37.37974880263209, -6.008640378713608], [37.37972600385547, -6.008636523038149], [37.379706306383014, -6.008619926869869], [37.379685854539275, -6.008602073416114], [37.37966498360038, -6.00858673453331], [37.37963874824345, -6.008571395650506], [37.379615530371666, -6.008565025404096], [37.37959214486182, -6.008555553853512], [37.37956926226616, -6.008540969341993], [37.379550486803055, -6.008519260212779], [37.379531878978014, -6.0084926057606936], [37.379511343315244, -6.008459161967039], [37.3794929869473, -6.008430914953351], [37.37947798334062, -6.00840836763382], [37.379461051896214, -6.008391687646508], [37.37944453954697, -6.008377270773053], [37.37942995503545, -6.008364865556359], [37.37940967082977, -6.008349610492587], [37.37938955426216, -6.0083353612571955], [37.37937237136066, -6.008316418156028], [37.37935418263078, -6.008297139778733], [37.379332054406404, -6.008273838087916], [37.37931311130524, -6.008252631872892], [37.379293497651815, -6.008233604952693], [37.37926634028554, -6.008224971592426], [37.37924421206117, -6.00820854306221], [37.37922325730324, -6.008188929408789], [37.37919827923179, -6.008168309926987], [37.3791821859777, -6.008145594969392], [37.37916374579072, -6.008124137297273], [37.379143461585045, -6.008104523643851], [37.37913097254932, -6.008085831999779], [37.37911052070558, -6.008067894726992], [37.379082441329956, -6.0080526396632195], [37.379058469086885, -6.0080417431890965], [37.37903734669089, -6.008032942190766], [37.37901387736201, -6.008012238889933], [37.378994934260845, -6.007992122322321], [37.37897942773998, -6.007972927764058], [37.37895746715367, -6.007952643558383], [37.3789334949106, -6.0079368855804205], [37.37891438417137, -6.00791878066957], [37.37888705916703, -6.007902855053544], [37.37886996008456, -6.00788114592433], [37.3788539506495, -6.007862873375416], [37.37882821820676, -6.007845271378756], [37.378806844353676, -6.007826663553715], [37.37878706306219, -6.0078102350234985], [37.378762336447835, -6.007789866998792], [37.378741381689906, -6.0077684093266726], [37.37871807999909, -6.007748879492283], [37.37869058735669, -6.007727589458227], [37.37866963259876, -6.00770621560514], [37.37864775583148, -6.007687104865909], [37.37862294539809, -6.007667575031519], [37.37860391847789, -6.007647458463907], [37.37858212552965, -6.0076311975717545], [37.37855731509626, -6.007613930851221], [37.37853585742414, -6.007597334682941], [37.378513645380735, -6.007580067962408], [37.37848506309092, -6.007564561441541], [37.37846310250461, -6.007545283064246], [37.37844147719443, -6.007524747401476], [37.37841566093266, -6.00750882178545], [37.37839512526989, -6.007489375770092], [37.378374421969056, -6.007467163726687], [37.37834952771664, -6.007447047159076], [37.37832932732999, -6.0074282716959715], [37.37831021659076, -6.0074099991470575], [37.37828649580479, -6.007388373836875], [37.37826612778008, -6.007367502897978], [37.37824458628893, -6.007349062711], [37.37821466289461, -6.007329951971769], [37.37819286994636, -6.007308159023523], [37.37817074172199, -6.007286114618182], [37.37814601510763, -6.0072648245841265], [37.37812547944486, -6.007243366912007], [37.37810326740146, -6.007226100191474], [37.37807795405388, -6.007204307243228], [37.3780595138669, -6.007186621427536], [37.378042079508305, -6.007169270887971], [37.378018107265234, -6.007149154320359], [37.37799706868827, -6.007132055237889], [37.377973264083266, -6.007110513746738], [37.377947280183434, -6.007087966427207], [37.37792741507292, -6.00707346573472], [37.37790511921048, -6.007055528461933], [37.3778834939003, -6.007034573704004], [37.37786170095205, -6.007008757442236], [37.3778420034796, -6.0069879703223705], [37.37781509757042, -6.0069661773741245], [37.37779506482184, -6.006947904825211], [37.37777293659747, -6.00692686624825], [37.377749467268586, -6.006904235109687], [37.37772876396775, -6.006884034723043], [37.37770764157176, -6.006861571222544], [37.37768744118512, -6.006835838779807], [37.37766179256141, -6.006815638393164], [37.37763572484255, -6.006791666150093], [37.377603286877275, -6.00677490234375], [37.377581745386124, -6.006757551804185], [37.37755659967661, -6.006743470206857], [37.37753162160516, -6.006723353639245], [37.377509493380785, -6.006703991442919], [37.37748401239514, -6.006683288142085], [37.377457190304995, -6.006666021421552], [37.377435648813844, -6.006645821034908], [37.37741150893271, -6.00662587210536], [37.377387369051576, -6.006606174632907], [37.377363899722695, -6.006589075550437], [37.3773393407464, -6.006571892648935], [37.377311177551746, -6.00655697286129], [37.3772877920419, -6.00654280744493], [37.37726625055075, -6.006525456905365], [37.377239763736725, -6.006502825766802], [37.37721805460751, -6.0064805299043655], [37.37719274125993, -6.006461922079325], [37.3771683499217, -6.006442056968808], [37.377145886421204, -6.00642504170537], [37.37712560221553, -6.006406601518393], [37.37710179761052, -6.0063880775123835], [37.37707966938615, -6.006371984258294], [37.37705779261887, -6.006352789700031], [37.377033568918705, -6.006331667304039], [37.37701068632305, -6.0063153225928545], [37.37699023447931, -6.006296211853623], [37.37696743570268, -6.006274586543441], [37.37694648094475, -6.00625648163259], [37.3769251909107, -6.006237789988518], [37.37689987756312, -6.006216919049621], [37.37687406130135, -6.0062045976519585], [37.37685143016279, -6.006189174950123], [37.37682762555778, -6.006170818582177], [37.376805916428566, -6.006156653165817], [37.37678688950837, -6.006143661215901], [37.376762414351106, -6.006128070876002], [37.37673969939351, -6.006110804155469], [37.37671832554042, -6.006092699244618], [37.376691503450274, -6.0060732532292604], [37.37666384316981, -6.006059003993869], [37.37664062529802, -6.006042072549462], [37.37661296501756, -6.006023129448295], [37.376587400212884, -6.006003683432937], [37.37656409852207, -6.005988344550133], [37.37653543241322, -6.0059730894863605], [37.37651137635112, -6.005956409499049], [37.37648572772741, -6.005939142778516], [37.376460414379835, -6.0059162601828575], [37.37643920816481, -6.005901005119085], [37.37641699612141, -6.005884408950806], [37.376388581469655, -6.005869908258319], [37.376367626711726, -6.005853563547134], [37.376344157382846, -6.005839230492711], [37.37631800584495, -6.005825735628605], [37.376294704154134, -6.005814839154482], [37.37626721151173, -6.005801344290376], [37.37623770721257, -6.005792375653982], [37.376209711655974, -6.0057878494262695], [37.376179452985525, -6.005788184702396], [37.37615053541958, -6.005793632939458], [37.37612949684262, -6.005800841376185], [37.37610451877117, -6.005806373432279], [37.37608037889004, -6.00580595433712], [37.37605590373278, -6.0058072954416275], [37.37602765671909, -6.005802601575851], [37.375996643677354, -6.005790615454316], [37.37597040832043, -6.005778210237622], [37.375944927334785, -6.005759434774518], [37.37592036835849, -6.0057383961975574], [37.37590310163796, -6.005717609077692], [37.37588156014681, -6.005693050101399], [37.3758614435792, -6.00566471926868], [37.37584652379155, -6.005638735368848], [37.375830095261335, -6.005612667649984], [37.37581416964531, -6.005585510283709], [37.37580201588571, -6.005559442564845], [37.375796819105744, -6.005535218864679], [37.375787012279034, -6.005503619089723], [37.3757782112807, -6.005482999607921], [37.37576320767403, -6.005460871383548], [37.375747449696064, -6.005439329892397], [37.3757335357368, -6.005419045686722], [37.37571828067303, -6.005400940775871], [37.375699169933796, -6.005376214161515], [37.37568508833647, -6.005352074280381], [37.37566597759724, -6.005335729569197], [37.37564845941961, -6.005319133400917], [37.37563052214682, -6.005300609394908], [37.37560948356986, -6.005288539454341], [37.3755902890116, -6.005273284390569], [37.375573022291064, -6.0052568558603525], [37.37555122934282, -6.005239924415946], [37.37553379498422, -6.005222825333476], [37.37551527097821, -6.005207318812609], [37.375493897125125, -6.00519273430109], [37.37547788769007, -6.005166247487068], [37.3754605371505, -6.005142694339156], [37.3754348885268, -6.00512869656086], [37.37541259266436, -6.005113944411278], [37.37539348192513, -6.005093744024634], [37.37536665983498, -6.005079243332148], [37.375344363972545, -6.0050630662590265], [37.375327348709106, -6.005045380443335], [37.375299856066704, -6.005025850608945], [37.375277895480394, -6.005002046003938], [37.37525585107505, -6.004988215863705], [37.37523171119392, -6.004961393773556], [37.37520891241729, -6.00494303740561], [37.37519172951579, -6.004921076819301], [37.375168930739164, -6.004897356033325], [37.37515560351312, -6.0048795863986015], [37.37513657659292, -6.004863660782576], [37.37511436454952, -6.004848908632994], [37.375094499439, -6.004837090149522], [37.37507253885269, -6.00482284091413], [37.37504848279059, -6.004811692982912], [37.37502836622298, -6.004791744053364], [37.37500439397991, -6.004778249189258], [37.374981259927154, -6.004761820659041], [37.37495946697891, -6.004744302481413], [37.37493859604001, -6.004727957770228], [37.37491604872048, -6.004710607230663], [37.37489936873317, -6.004692921414971], [37.374876234680414, -6.004678588360548], [37.37485645338893, -6.004663920029998], [37.37484019249678, -6.004648329690099], [37.37482778728008, -6.004627458751202], [37.37481404095888, -6.004610192030668], [37.37480607815087, -6.004597116261721], [37.374791745096445, -6.004573395475745], [37.37478034570813, -6.004557050764561], [37.374772131443024, -6.004533329978585], [37.37475771456957, -6.00451304577291], [37.374748243018985, -6.004500053822994], [37.37473575398326, -6.004484128206968], [37.374714044854045, -6.004472561180592], [37.37469091080129, -6.004461832344532], [37.374667190015316, -6.004447666928172], [37.374640703201294, -6.004437105730176], [37.37461463548243, -6.004419419914484], [37.374587981030345, -6.0044019017368555], [37.37456023693085, -6.0043818689882755], [37.37453333102167, -6.004360914230347], [37.37451547756791, -6.004343647509813], [37.37449293024838, -6.004326296970248], [37.374472226947546, -6.004312634468079], [37.37444750033319, -6.004301570355892], [37.37442696467042, -6.004283130168915], [37.37440190277994, -6.004266198724508], [37.374379774555564, -6.004249099642038], [37.374361250549555, -6.004227306693792], [37.3743418045342, -6.004211213439703], [37.37432185560465, -6.004192940890789], [37.37430249340832, -6.0041736625134945], [37.37428346648812, -6.004155222326517], [37.374258153140545, -6.004136363044381], [37.37423510290682, -6.004118761047721], [37.37421523779631, -6.004098057746887], [37.37419277429581, -6.004085568711162], [37.374171735718846, -6.004074504598975], [37.374153127893806, -6.00406059063971], [37.374127646908164, -6.004044581204653], [37.37410702742636, -6.004027398303151], [37.374083222821355, -6.004013484343886], [37.37405916675925, -6.003999235108495], [37.37403804436326, -6.003986746072769], [37.37401273101568, -6.003970904275775], [37.37399118952453, -6.003952380269766], [37.373973252251744, -6.003928240388632], [37.37395020201802, -6.0039065312594175], [37.37392916344106, -6.003889013081789], [37.37390569411218, -6.003871243447065], [37.37388281151652, -6.003854395821691], [37.37386571243405, -6.003832519054413], [37.37384559586644, -6.00381089374423], [37.37382640130818, -6.003786837682128], [37.373803770169616, -6.003766469657421], [37.37378105521202, -6.003746856004], [37.373755406588316, -6.0037316009402275], [37.37373302690685, -6.003714418038726], [37.37370327115059, -6.003699079155922], [37.37367485649884, -6.003688182681799], [37.37365012988448, -6.003672927618027], [37.37362272106111, -6.0036570858210325], [37.373594138771296, -6.00364769808948], [37.37357184290886, -6.003640657290816], [37.373545691370964, -6.003630515187979], [37.37352456897497, -6.003616936504841], [37.37349648959935, -6.003606878221035], [37.37346958369017, -6.003601597622037], [37.37344862893224, -6.003587935119867], [37.373422058299184, -6.003572512418032], [37.37339800223708, -6.003555161878467], [37.37337704747915, -6.00353579968214], [37.3733520694077, -6.00352356210351], [37.37332893535495, -6.003510402515531], [37.37330102361739, -6.003497494384646], [37.37326900474727, -6.003487017005682], [37.37324100919068, -6.003476455807686], [37.37321133725345, -6.0034722648561], [37.37317998893559, -6.003468660637736], [37.373145539313555, -6.0034677386283875], [37.37311796285212, -6.003466732800007], [37.37308259122074, -6.003467990085483], [37.3730550147593, -6.003467235714197], [37.3730256780982, -6.003462374210358], [37.37299860455096, -6.003459692001343], [37.3729757219553, -6.00345223210752], [37.37294931896031, -6.0034512262791395], [37.37292216159403, -6.003453405573964], [37.372898273169994, -6.003449046984315], [37.37286818213761, -6.003454579040408], [37.37283884547651, -6.003456674516201], [37.372813783586025, -6.00345516577363], [37.372786458581686, -6.003468492999673], [37.372763408347964, -6.003473857417703], [37.3727389331907, -6.0034688282758], [37.37271026708186, -6.003473522141576], [37.37268168479204, -6.003481401130557], [37.37265536561608, -6.003489280119538], [37.37262728624046, -6.003502691164613], [37.37260373309255, -6.003518784418702], [37.37258043140173, -6.003526831045747], [37.372551346197724, -6.003531524911523], [37.372525949031115, -6.003541247919202], [37.37249996513128, -6.003551641479135], [37.3724714666605, -6.00356119684875], [37.37244523130357, -6.003567231819034], [37.37241866067052, -6.003576787188649], [37.372389659285545, -6.00358902476728], [37.3723678663373, -6.003598999232054], [37.37234213389456, -6.003603525459766], [37.372309947386384, -6.003609811887145], [37.3722830414772, -6.003614757210016], [37.372255800291896, -6.003623055294156], [37.37222520634532, -6.0036341194063425], [37.37220123410225, -6.003639986738563], [37.37217583693564, -6.003646021708846], [37.37214750610292, -6.003649542108178], [37.37212177366018, -6.0036523919552565], [37.37209537066519, -6.003660941496491], [37.37206351943314, -6.003678711131215], [37.37203870899975, -6.003681812435389], [37.37201465293765, -6.003688937053084], [37.371985986828804, -6.003700755536556], [37.37195782363415, -6.003716345876455], [37.371941143646836, -6.003728918731213], [37.371923457831144, -6.003746856004], [37.371908873319626, -6.0037631168961525], [37.371888337656856, -6.003779796883464], [37.371865874156356, -6.003799410536885], [37.37184399738908, -6.003823969513178], [37.37182538956404, -6.0038417391479015], [37.37180317752063, -6.00386562757194], [37.37178239040077, -6.00389102473855], [37.371762692928314, -6.003913236781955], [37.3717428278178, -6.003936119377613], [37.371725142002106, -6.003964114934206], [37.37170619890094, -6.003983728587627], [37.371688429266214, -6.004006192088127], [37.37166437320411, -6.0040263924747705], [37.37164685502648, -6.004036283120513], [37.37162799574435, -6.004047514870763], [37.37160603515804, -6.004059500992298], [37.371586756780744, -6.004061261191964], [37.37156613729894, -6.004061345010996], [37.37154291942716, -6.004058411344886], [37.37152213230729, -6.004050113260746], [37.37150318920612, -6.004036786034703], [37.371484162285924, -6.004016334190965], [37.37147276289761, -6.003993032500148], [37.371463626623154, -6.003966461867094], [37.371453987434506, -6.003935448825359], [37.371444683521986, -6.00390468724072], [37.37144342623651, -6.003874177113175], [37.37143504433334, -6.0038393922150135], [37.37142825499177, -6.003811480477452], [37.37142398022115, -6.003782646730542], [37.37141861580312, -6.00374735891819], [37.371419202536345, -6.003716345876455], [37.37141844816506, -6.003683488816023], [37.37141367048025, -6.003648368641734], [37.371406964957714, -6.003620456904173], [37.3714058753103, -6.00358902476728], [37.37140143290162, -6.003556922078133], [37.37139950506389, -6.003523310646415], [37.37139908596873, -6.0034907050430775], [37.37139749340713, -6.003453237935901], [37.37139464356005, -6.0034202970564365], [37.37139422446489, -6.003390541300178], [37.371389362961054, -6.003355337306857], [37.37138483673334, -6.0033237375319], [37.37138131633401, -6.003295239061117], [37.37137402407825, -6.003261208534241], [37.37136606127024, -6.003228016197681], [37.371362121775746, -6.003199266269803], [37.37135415896773, -6.003165319561958], [37.37134703435004, -6.003132546320558], [37.371342089027166, -6.003099270164967], [37.37133705988526, -6.003064066171646], [37.37133312039077, -6.0030309576541185], [37.37133261747658, -6.003000698983669], [37.371333204209805, -6.002966081723571], [37.37133387476206, -6.002933057025075], [37.37133446149528, -6.002905480563641], [37.37133353948593, -6.0028737131506205], [37.371335132047534, -6.0028439573943615], [37.37133454531431, -6.002814369276166], [37.37133387476206, -6.0027791652828455], [37.3713352996856, -6.002746811136603], [37.37133731134236, -6.002718899399042], [37.37133446149528, -6.002686712890863], [37.37133362330496, -6.002654358744621], [37.37133244983852, -6.00262594409287], [37.371330354362726, -6.00259124301374], [37.37133194692433, -6.002557128667831], [37.37133060581982, -6.00252628326416], [37.37132775597274, -6.002491666004062], [37.37133110873401, -6.002456629648805], [37.37133429385722, -6.002424694597721], [37.37133638933301, -6.002386389300227], [37.371334629133344, -6.002349341288209], [37.37133454531431, -6.002317741513252], [37.371332701295614, -6.002282705157995], [37.37133395858109, -6.002246243879199], [37.371334210038185, -6.0022137220948935], [37.371336137875915, -6.002177344635129], [37.3713387362659, -6.0021421406418085], [37.37134133465588, -6.002111881971359], [37.37134066410363, -6.002075839787722], [37.37134275957942, -6.002042563632131], [37.371345860883594, -6.002012388780713], [37.37134896218777, -6.001973748207092], [37.37135726027191, -6.001941645517945], [37.37136363051832, -6.001906609162688], [37.37136555835605, -6.001872746273875], [37.3713643848896, -6.001838883385062], [37.37136128358543, -6.001806445419788], [37.37135801464319, -6.001771409064531], [37.37135969102383, -6.001737043261528], [37.371360529214144, -6.001704353839159], [37.37136044539511, -6.001667724922299], [37.371361535042524, -6.001634951680899], [37.371362792328, -6.001606285572052], [37.37136287614703, -6.001567477360368], [37.37137016840279, -6.001534704118967], [37.37137712538242, -6.001500505954027], [37.371378634124994, -6.001465134322643], [37.3713799752295, -6.001432361081243], [37.37138089723885, -6.001398665830493], [37.37137729302049, -6.00135700777173], [37.37137754447758, -6.001321719959378], [37.371380645781755, -6.001291712746024], [37.371381567791104, -6.001253826543689], [37.3713836632669, -6.00121920928359], [37.37138290889561, -6.001191884279251], [37.371379220858216, -6.001155423000455], [37.371380142867565, -6.001122146844864], [37.37138391472399, -6.001094151288271], [37.37138726748526, -6.0010612942278385], [37.37138810567558, -6.00102717988193], [37.371389362961054, -6.000999603420496], [37.37139011733234, -6.000963393598795], [37.37138953059912, -6.000923914834857], [37.371390871703625, -6.000891476869583], [37.37139388918877, -6.000853842124343], [37.37139648757875, -6.00081511773169], [37.37139732576907, -6.000781673938036], [37.37139749340713, -6.000751499086618], [37.37139833159745, -6.000713696703315], [37.371397241950035, -6.000681174919009], [37.37139749340713, -6.000647898763418], [37.37139388918877, -6.0006095096468925], [37.371394308283925, -6.000575730577111], [37.37139103934169, -6.000540861859918], [37.371392799541354, -6.000505071133375], [37.37139548175037, -6.000471794977784], [37.371397661045194, -6.000444050878286], [37.37140143290162, -6.000409601256251], [37.37140327692032, -6.0003777500241995], [37.37140730023384, -6.000345479696989], [37.37141400575638, -6.000309940427542], [37.371417693793774, -6.000283872708678], [37.371421633288264, -6.000250680372119], [37.37142255529761, -6.000213548541069], [37.37142255529761, -6.000188319012523], [37.371427584439516, -6.000154707580805], [37.37143110483885, -6.000118665397167], [37.37143227830529, -6.000086730346084], [37.371432865038514, -6.0000532027333975], [37.371437810361385, -6.00001422688365], [37.371438732370734, -5.999981369823217], [37.37144158221781, -5.999950859695673], [37.37143998965621, -5.999915571883321], [37.37144007347524, -5.999887157231569], [37.37143739126623, -5.999855138361454], [37.37142984755337, -5.999819850549102], [37.371421130374074, -5.999784646555781], [37.37141794525087, -5.999751789495349], [37.371410988271236, -5.999712646007538], [37.371409395709634, -5.999679286032915], [37.37140730023384, -5.999646596610546], [37.37140855751932, -5.999611476436257], [37.37141107209027, -5.999583480879664], [37.37141367048025, -5.99954660050571], [37.37141794525087, -5.999511731788516], [37.37142079509795, -5.9994844906032085], [37.371421214193106, -5.999448951333761], [37.37141895107925, -5.999413998797536], [37.37141844816506, -5.999388266354799], [37.37141861580312, -5.999353313818574], [37.37141853198409, -5.999322133138776], [37.371421633288264, -5.999295059591532], [37.37142381258309, -5.999261783435941], [37.371424566954374, -5.99922657944262], [37.371423561125994, -5.999193973839283], [37.371420627459884, -5.999161787331104], [37.37141744233668, -5.999121470376849], [37.371415765956044, -5.999090289697051], [37.371411910280585, -5.999059695750475], [37.37140570767224, -5.999025581404567], [37.371403612196445, -5.998995993286371], [37.37140294164419, -5.998963303864002], [37.3714042827487, -5.998927177861333], [37.37140629440546, -5.998895242810249], [37.37140738405287, -5.9988610446453094], [37.371410485357046, -5.998824164271355], [37.371414760127664, -5.998792899772525], [37.37142054364085, -5.998765155673027], [37.371423561125994, -5.998730370774865], [37.37142683006823, -5.998699273914099], [37.371428506448865, -5.998668763786554], [37.37143160775304, -5.998633140698075], [37.37143152393401, -5.998603720217943], [37.37143554724753, -5.998572204262018], [37.37143755890429, -5.998536664992571], [37.37143789418042, -5.998505149036646], [37.37144015729427, -5.9984781593084335], [37.37144300714135, -5.9984413627535105], [37.371444096788764, -5.998408002778888], [37.371447533369064, -5.998378833755851], [37.37145038321614, -5.9983427077531815], [37.37145029939711, -5.998311275616288], [37.37144543789327, -5.998278837651014], [37.37144543789327, -5.998246902599931], [37.37144719809294, -5.998216224834323], [37.37145038321614, -5.998187055811286], [37.3714542388916, -5.998154869303107], [37.371457759290934, -5.99812469445169], [37.37146195024252, -5.998095944523811], [37.37146278843284, -5.998062081634998], [37.3714611120522, -5.998029476031661], [37.37146278843284, -5.997998882085085], [37.37146211788058, -5.997967617586255], [37.371458765119314, -5.997936520725489], [37.37145893275738, -5.9979053400456905], [37.37145759165287, -5.9978673700243235], [37.37145666964352, -5.997834261506796], [37.37146044149995, -5.99780417047441], [37.37146002240479, -5.997770139947534], [37.37146186642349, -5.9977395460009575], [37.37146790139377, -5.997713226824999], [37.37147024832666, -5.9976786095649], [37.3714716732502, -5.997643992304802], [37.37147712148726, -5.997613063082099], [37.37148005515337, -5.997576350346208], [37.37148055806756, -5.997541481629014], [37.37148164771497, -5.997509462758899], [37.371481731534004, -5.9974724147468805], [37.37148148007691, -5.997440395876765], [37.371478797867894, -5.997412316501141], [37.3714803904295, -5.997383985668421], [37.371480725705624, -5.99735121242702], [37.37148189917207, -5.997321708127856], [37.37148676067591, -5.997290024533868], [37.37148793414235, -5.997256748378277], [37.37148902378976, -5.997226154431701], [37.37149137072265, -5.997189609333873], [37.37149849534035, -5.997151639312506], [37.37150008790195, -5.997118446975946], [37.37150168046355, -5.997082404792309], [37.37150159664452, -5.997042423114181], [37.37150302156806, -5.997008308768272], [37.37150453031063, -5.996974278241396], [37.37150612287223, -5.996938152238727], [37.37150310538709, -5.996909067034721], [37.37150209955871, -5.996876964345574], [37.37150092609227, -5.9968439396470785], [37.37150000408292, -5.996811082586646], [37.371500758454204, -5.996784176677465], [37.37150000408292, -5.996750313788652], [37.37149824388325, -5.9967199712991714], [37.37149874679744, -5.996684934943914], [37.37150008790195, -5.996650569140911], [37.37150059081614, -5.996623327955604], [37.37150109373033, -5.996590722352266], [37.371501261368394, -5.996553339064121], [37.371504195034504, -5.996522409841418], [37.37151031382382, -5.996484188362956], [37.37151458859444, -5.996450912207365], [37.37151852808893, -5.9964195638895035], [37.37152121029794, -5.996386623010039], [37.37152439542115, -5.996352341026068], [37.3715269099921, -5.996320238336921], [37.37153026275337, -5.996283106505871], [37.37153244204819, -5.996245974674821], [37.371535962447524, -5.996215883642435], [37.37153914757073, -5.996180092915893], [37.37154417671263, -5.996145140379667], [37.37154996022582, -5.996113624423742], [37.37155725248158, -5.996075151488185], [37.371561946347356, -5.996038606390357], [37.37156940624118, -5.996005916967988], [37.37157963216305, -5.995969204232097], [37.371589774265885, -5.995938358828425], [37.371600000187755, -5.995909022167325], [37.3716080468148, -5.9958776738494635], [37.371613746508956, -5.995847750455141], [37.37162556499243, -5.995818413794041], [37.37163872458041, -5.99578564055264], [37.37165247090161, -5.995760578662157], [37.37166755832732, -5.995734678581357], [37.37168147228658, -5.995702827349305], [37.37170041538775, -5.995675669983029], [37.37171407788992, -5.995648009702563], [37.37172774039209, -5.995614565908909], [37.37173905596137, -5.995587157085538], [37.37175070680678, -5.995560334995389], [37.37176302820444, -5.995525466278195], [37.371774427592754, -5.99549344740808], [37.371786665171385, -5.995468217879534], [37.37180074676871, -5.995431840419769], [37.37180694937706, -5.99540401250124], [37.371820947155356, -5.9953749272972345], [37.37183251418173, -5.995340896770358], [37.37184600904584, -5.995314912870526], [37.3718636110425, -5.995287587866187], [37.37187970429659, -5.995248109102249], [37.37189286388457, -5.99522371776402], [37.37190962769091, -5.9951994102448225], [37.37193049862981, -5.9951752703636885], [37.371943490579724, -5.9951558243483305], [37.37196293659508, -5.995131768286228], [37.37198079004884, -5.995104191824794], [37.371991937980056, -5.99507812410593], [37.37200819887221, -5.995051888749003], [37.37202353775501, -5.995024899020791], [37.37203904427588, -5.995000591501594], [37.372055388987064, -5.994981816038489], [37.37207659520209, -5.994956670328975], [37.372095957398415, -5.994933201000094], [37.372115990146995, -5.994917191565037], [37.37213836982846, -5.994894141331315], [37.372160498052835, -5.99487628787756], [37.37217918969691, -5.994862541556358], [37.372200479730964, -5.994841838255525], [37.37221992574632, -5.994822308421135], [37.37223727628589, -5.994805376976728], [37.37225756049156, -5.994781320914626], [37.372278179973364, -5.994758019223809], [37.37229670397937, -5.994740920141339], [37.37231506034732, -5.99472239613533], [37.37233500927687, -5.994704877957702], [37.372356886044145, -5.994686856865883], [37.37237784080207, -5.994674200192094], [37.372398879379034, -5.994654167443514], [37.37241673283279, -5.994640756398439], [37.372437017038465, -5.994623405858874], [37.37245780415833, -5.994608653709292], [37.37247733399272, -5.9945974219590425], [37.37249887548387, -5.994580993428826], [37.372527373954654, -5.994566157460213], [37.37255025655031, -5.994557440280914], [37.372572887688875, -5.994542520493269], [37.372596776112914, -5.994525337591767], [37.37261228263378, -5.994506226852536], [37.37263181246817, -5.994499186053872], [37.37265679053962, -5.994481248781085], [37.37267715856433, -5.9944649040699005], [37.372696939855814, -5.994457108899951], [37.37272116355598, -5.994442105293274], [37.372742872685194, -5.994427856057882], [37.37276743166149, -5.9944177977740765], [37.372792828828096, -5.99440379999578], [37.37281604669988, -5.994389383122325], [37.37283834256232, -5.99438133649528], [37.372864075005054, -5.994372284039855], [37.372888466343284, -5.994365997612476], [37.372912438586354, -5.994359711185098], [37.37294060178101, -5.994350491091609], [37.37296264618635, -5.9943468030542135], [37.372986786067486, -5.994341103360057], [37.37301176413894, -5.994334481656551], [37.373033221811056, -5.994327692314982], [37.37305627204478, -5.994320148602128], [37.3730811662972, -5.994309587404132], [37.37310145050287, -5.994301959872246], [37.37312416546047, -5.994290476664901], [37.37314956262708, -5.9942825976759195], [37.37317353487015, -5.994278993457556], [37.37319901585579, -5.994275473058224], [37.373226005584, -5.994273712858558], [37.37325148656964, -5.994273545220494], [37.37328099086881, -5.994269102811813], [37.37330814823508, -5.994266085326672], [37.37333228811622, -5.994264576584101], [37.37335701473057, -5.994261642917991], [37.373380567878485, -5.9942516684532166], [37.373404456302524, -5.994248650968075], [37.37343261949718, -5.994241442531347], [37.37345902249217, -5.994234401732683], [37.373487520962954, -5.994233395904303], [37.37351518124342, -5.994231887161732], [37.373541835695505, -5.99422643892467], [37.373567232862115, -5.994227696210146], [37.37359765917063, -5.994233395904303], [37.37362037412822, -5.994238508865237], [37.3736440949142, -5.994243705645204], [37.37366965971887, -5.994249824434519], [37.37369463779032, -5.994253680109978], [37.37372087314725, -5.994255691766739], [37.37375054508448, -5.994259212166071], [37.37377619370818, -5.994259715080261], [37.37380142323673, -5.994261559098959], [37.373826736584306, -5.99426357075572], [37.37384811043739, -5.994270779192448], [37.373869735747576, -5.994274886325002], [37.37389697693288, -5.994278742000461], [37.373921955004334, -5.994280418381095], [37.3739477712661, -5.994279915466905], [37.37397886812687, -5.994284022599459], [37.37400820478797, -5.994290392845869], [37.37403460778296, -5.994300451129675], [37.374059753492475, -5.994309252128005], [37.37408054061234, -5.9943147003650665], [37.37410602159798, -5.99432023242116], [37.37412898801267, -5.994329620152712], [37.3741449136287, -5.994335571303964], [37.3741639405489, -5.994342193007469], [37.374189756810665, -5.994346970692277], [37.3742123041302, -5.99434956908226], [37.37423895858228, -5.9943524189293385], [37.37426619976759, -5.994350910186768], [37.37429251894355, -5.99434956908226], [37.37432001158595, -5.994349233806133], [37.374348090961576, -5.994345126673579], [37.37437583506107, -5.994334984570742], [37.374399807304144, -5.994325261563063], [37.37442553974688, -5.994312772527337], [37.37445470876992, -5.994294919073582], [37.374482369050384, -5.994279161095619], [37.374512795358896, -5.994252255186439], [37.374539617449045, -5.9942281153053045], [37.37456384114921, -5.9942028019577265], [37.37458571791649, -5.994173213839531], [37.37460868433118, -5.994150247424841], [37.37462888471782, -5.994130214676261], [37.374653527513146, -5.994105152785778], [37.374676913022995, -5.994089227169752], [37.37470021471381, -5.9940764028579], [37.37472586333752, -5.994060477241874], [37.374747321009636, -5.994048994034529], [37.37477548420429, -5.994035080075264], [37.374799624085426, -5.994017058983445], [37.374823512509465, -5.994012700393796], [37.37484773620963, -5.9940090123564005], [37.37487003207207, -5.99399677477777], [37.374893920496106, -5.99399333819747], [37.374917808920145, -5.993987554684281], [37.37493574619293, -5.9939624927937984], [37.37494982779026, -5.993938352912664], [37.374962735921144, -5.993908178061247], [37.37497614696622, -5.993871549144387], [37.37498637288809, -5.993849337100983], [37.37499970011413, -5.993822179734707], [37.375011099502444, -5.99378764629364], [37.37501394934952, -5.993757890537381], [37.37501956522465, -5.993730062618852], [37.37502811476588, -5.993691757321358], [37.37503037787974, -5.993664348497987], [37.37504010088742, -5.993631072342396], [37.375048734247684, -5.993597628548741], [37.375057032331824, -5.993572985753417], [37.37506566569209, -5.993536524474621], [37.37507144920528, -5.9935003984719515], [37.37507865764201, -5.993467960506678], [37.37508368678391, -5.993430158123374], [37.375091314315796, -5.99339478649199], [37.37509651109576, -5.993363941088319], [37.37510455772281, -5.993323791772127], [37.375113693997264, -5.9932906832546], [37.37511947751045, -5.993261178955436], [37.37512995488942, -5.9932229574769735], [37.37513221800327, -5.993189932778478], [37.375135738402605, -5.99315732717514], [37.37514194101095, -5.993120865896344], [37.37514805980027, -5.993086583912373], [37.37515409477055, -5.99305909126997], [37.375162644311786, -5.993019277229905], [37.3751703556627, -5.992985665798187], [37.37517856992781, -5.992955071851611], [37.37518913112581, -5.992913581430912], [37.37520111724734, -5.992881981655955], [37.37521075643599, -5.992848621681333], [37.37521578557789, -5.992813417688012], [37.3752242513001, -5.992783242836595], [37.37523238174617, -5.992750469595194], [37.37523598596454, -5.992713253945112], [37.37524411641061, -5.992686012759805], [37.37525140866637, -5.992653239518404], [37.37525895237923, -5.992616945877671], [37.37526951357722, -5.992587441578507], [37.375279488042, -5.992552489042282], [37.37528686411679, -5.992519548162818], [37.37529532983899, -5.992491217330098], [37.37530295737088, -5.992455091327429], [37.375312093645334, -5.992423323914409], [37.37531871534884, -5.992395663633943], [37.37532407976687, -5.992360459640622], [37.37533229403198, -5.992326680570841], [37.37534411251545, -5.992297176271677], [37.375352242961526, -5.99226163700223], [37.37536179833114, -5.992229115217924], [37.375374203547835, -5.992198772728443], [37.37538434565067, -5.992165748029947], [37.37539163790643, -5.992134902626276], [37.37540270201862, -5.992101961746812], [37.37540957517922, -5.992063153535128], [37.37541829235852, -5.992030128836632], [37.375427931547165, -5.991990230977535], [37.375439163297415, -5.9919483214616776], [37.375449473038316, -5.9919212479144335], [37.37547311000526, -5.991890151053667], [37.375503117218614, -5.991857964545488], [37.37552650272846, -5.991834998130798], [37.37555483356118, -5.99181555211544], [37.37558752298355, -5.99179987795651], [37.375613674521446, -5.991791244596243], [37.37563672475517, -5.99178621545434], [37.3756576795131, -5.991779174655676], [37.37567871809006, -5.99176911637187], [37.375702103599906, -5.991760902106762], [37.375725572928786, -5.991744892671704], [37.375748539343476, -5.991728296503425], [37.3757681529969, -5.991716394200921], [37.37578944303095, -5.991710107773542], [37.37581458874047, -5.991708934307098], [37.37583856098354, -5.991709101945162], [37.375866305083036, -5.991716142743826], [37.37589329481125, -5.991734750568867], [37.375917602330446, -5.991745982319117], [37.37594693899155, -5.991758722811937], [37.37597099505365, -5.9917710442095995], [37.37599312327802, -5.99178085103631], [37.37601701170206, -5.991793172433972], [37.3760374635458, -5.9918060805648565], [37.376053892076015, -5.991822509095073], [37.37607803195715, -5.991841787472367], [37.376102255657315, -5.991854863241315], [37.3761220369488, -5.991867771372199], [37.37614760175347, -5.991880763322115], [37.376170149073005, -5.991892330348492], [37.376188756898046, -5.991905238479376], [37.37621038220823, -5.991918230429292], [37.37623343244195, -5.991925438866019], [37.37625254318118, -5.991939101368189], [37.376277185976505, -5.9919521771371365], [37.376300655305386, -5.991967096924782], [37.37632337026298, -5.991980172693729], [37.37634960561991, -5.991995260119438], [37.37637466751039, -5.9920119401067495], [37.376392018049955, -5.9920296259224415], [37.37641288898885, -5.992048317566514], [37.37643023952842, -5.992063907906413], [37.376448679715395, -5.992075894027948], [37.37647290341556, -5.99209064617753], [37.37648941576481, -5.992109337821603], [37.376507772132754, -5.992128364741802], [37.37652897834778, -5.992147056385875], [37.376545406877995, -5.992164071649313], [37.376564936712384, -5.992178320884705], [37.37658882513642, -5.992193492129445], [37.376609947532415, -5.992208831012249], [37.37662930972874, -5.992224421352148], [37.37665269523859, -5.9922401793301105], [37.37667499110103, -5.99225576967001], [37.3766955267638, -5.9922699350863695], [37.37672318704426, -5.992284184321761], [37.37674715928733, -5.992295416072011], [37.37677188590169, -5.9923049714416265], [37.37679879181087, -5.992313269525766], [37.37682184204459, -5.9923232439905405], [37.376844892278314, -5.992332547903061], [37.37687213346362, -5.99233883433044], [37.37689686007798, -5.992350149899721], [37.376917731016874, -5.99236473441124], [37.37694145180285, -5.992379318922758], [37.37696408294141, -5.992391724139452], [37.376988558098674, -5.992402033880353], [37.377012027427554, -5.992416366934776], [37.377029126510024, -5.992427011951804], [37.37705544568598, -5.992441512644291], [37.37708159722388, -5.992457522079349], [37.377103893086314, -5.99247470498085], [37.377132307738066, -5.992492977529764], [37.37715502269566, -5.992511082440615], [37.377177653834224, -5.992529941722751], [37.377206571400166, -5.992548801004887], [37.37723263911903, -5.992567241191864], [37.377258790656924, -5.992583250626922], [37.3772877920419, -5.9925974160432816], [37.377307238057256, -5.992615856230259], [37.37732986919582, -5.992631362751126], [37.377356104552746, -5.992643851786852], [37.3773758020252, -5.992657765746117], [37.37740262411535, -5.9926713444292545], [37.37743070349097, -5.992684420198202], [37.37745442427695, -5.992707218974829], [37.37748594023287, -5.992726078256965], [37.377513349056244, -5.992742506787181], [37.37753799185157, -5.992761785164475], [37.377562718465924, -5.992780392989516], [37.37759138457477, -5.992803443223238], [37.37761862576008, -5.992817860096693], [37.37764460965991, -5.992835462093353], [37.37767394632101, -5.992856081575155], [37.37770026549697, -5.992869241163135], [37.37772147171199, -5.992886507883668], [37.37774971872568, -5.992907462641597], [37.377770422026515, -5.992922214791179], [37.3777952324599, -5.992935625836253], [37.377825574949384, -5.992950797080994], [37.37784896045923, -5.992965884506702], [37.377873016521335, -5.992982145398855], [37.37790344282985, -5.99300142377615], [37.37792448140681, -5.993021121248603], [37.37794702872634, -5.993037214502692], [37.37797041423619, -5.9930503740906715], [37.37799354828894, -5.993064036592841], [37.37801902927458, -5.993077279999852], [37.37804342061281, -5.993092618882656], [37.378068482503295, -5.993107035756111], [37.378094131127, -5.9931196924299], [37.37812061794102, -5.9931328520178795], [37.37814484164119, -5.993147687986493], [37.37817493267357, -5.993162356317043], [37.37819731235504, -5.99317392334342], [37.37822153605521, -5.993187166750431], [37.37824718467891, -5.993201918900013], [37.37826772034168, -5.993215497583151], [37.37829579971731, -5.993232261389494], [37.37831993959844, -5.993247264996171], [37.378344582393765, -5.993259167298675], [37.378368973731995, -5.993269728496671], [37.37839135341346, -5.9932859893888235], [37.37841130234301, -5.993307027965784], [37.378434939309955, -5.9933272283524275], [37.37845757044852, -5.993342734873295], [37.37848397344351, -5.993355978280306], [37.378508783876896, -5.993373496457934], [37.3785325884819, -5.993391601368785], [37.378562008962035, -5.993410209193826], [37.37858673557639, -5.9934185072779655], [37.3786139767617, -5.993433175608516], [37.37864339724183, -5.993453292176127], [37.37866644747555, -5.993466200307012], [37.37869662232697, -5.993485394865274], [37.378724282607436, -5.993503583595157], [37.37874641083181, -5.993521437048912], [37.37877616658807, -5.993543565273285], [37.37880013883114, -5.993557898327708], [37.3788246139884, -5.993573237210512], [37.3788523580879, -5.993590587750077], [37.37887775525451, -5.993602238595486], [37.378901559859514, -5.993617242202163], [37.37892871722579, -5.993634341284633], [37.37895201891661, -5.993647249415517], [37.378975320607424, -5.993662755936384], [37.37900348380208, -5.993678430095315], [37.37902536056936, -5.993694439530373], [37.37904882989824, -5.993709024041891], [37.37907665781677, -5.993723943829536], [37.379101384431124, -5.993739785626531], [37.37912518903613, -5.993758477270603], [37.37914622761309, -5.9937745705246925], [37.37916709855199, -5.993785802274942], [37.37919291481376, -5.993799716234207], [37.37921755760908, -5.993812205269933], [37.37923859618604, -5.993824526667595], [37.3792665079236, -5.99383257329464], [37.37929190509021, -5.99384238012135], [37.379314452409744, -5.993852522224188], [37.379344794899225, -5.9938623290508986], [37.37937027588487, -5.99387314170599], [37.37939240410924, -5.993889318779111], [37.379418052732944, -5.993903819471598], [37.379442946985364, -5.993917649611831], [37.37946515902877, -5.993934161961079], [37.37949198111892, -5.993949081748724], [37.37951595336199, -5.99396157078445], [37.37954160198569, -5.993977999314666], [37.37957093864679, -5.993990572169423], [37.37959457561374, -5.994002977386117], [37.37962215207517, -5.994014795869589], [37.37964612431824, -5.994026362895966], [37.37966355867684, -5.994037175551057], [37.3796845972538, -5.994054023176432], [37.379706809297204, -5.994068942964077], [37.37972600385547, -5.994080593809485], [37.379747880622745, -5.994095429778099], [37.379766404628754, -5.994109343737364], [37.3797831684351, -5.994126191362739], [37.37980470992625, -5.994139099493623], [37.37982641905546, -5.994155360385776], [37.379847625270486, -5.994167514145374], [37.37987377680838, -5.994176818057895], [37.37989749759436, -5.994190480560064], [37.37992214038968, -5.9942004550248384], [37.37995147705078, -5.994211686775088], [37.37997771240771, -5.994220739230514], [37.38000319339335, -5.994235323742032], [37.38003194332123, -5.994250997900963], [37.380056669935584, -5.994267258793116], [37.38008223474026, -5.994284860789776], [37.38011023029685, -5.99429827183485], [37.38012917339802, -5.9943147003650665], [37.38015314564109, -5.994332218542695], [37.38018214702606, -5.994344288483262], [37.38020192831755, -5.994357615709305], [37.380226906389, -5.994371445849538], [37.380251633003354, -5.994385024532676], [37.380272671580315, -5.99439843557775], [37.38029840402305, -5.994409499689937], [37.38032187335193, -5.994423748925328], [37.38034190610051, -5.994441602379084], [37.38037023693323, -5.99445316940546], [37.38039622083306, -5.994467167183757], [37.38041801378131, -5.994481332600117], [37.38044005818665, -5.994500108063221], [37.38045925274491, -5.9945170395076275], [37.38048305734992, -5.994533970952034], [37.38050946034491, -5.99454989656806], [37.380530750378966, -5.994561044499278], [37.38055581226945, -5.99457236006856], [37.38058137707412, -5.994583759456873], [37.38060534931719, -5.994594823569059], [37.38063435070217, -5.994607647880912], [37.38065832294524, -5.994622567668557], [37.380681708455086, -5.994634637609124], [37.38070894964039, -5.994652071967721], [37.380733424797654, -5.994666824117303], [37.38075655885041, -5.994683168828487], [37.38078438676894, -5.994701022282243], [37.38081179559231, -5.994713930413127], [37.38083593547344, -5.994730526581407], [37.380860494449735, -5.994749553501606], [37.38088186830282, -5.994767742231488], [37.3809037450701, -5.994784086942673], [37.38092763349414, -5.994798168540001], [37.38094573840499, -5.994815435260534], [37.38096979446709, -5.994832534343004], [37.38099401816726, -5.994846867397428], [37.38101329654455, -5.994861116632819], [37.38103995099664, -5.994879389181733], [37.38106249831617, -5.994896907359362], [37.38108068704605, -5.994913000613451], [37.38110398873687, -5.9949299320578575], [37.381128715351224, -5.994944851845503], [37.38114950247109, -5.994961950927973], [37.381175234913826, -5.994977625086904], [37.3811992071569, -5.994993047788739], [37.38122267648578, -5.995004698634148], [37.38124690018594, -5.99501509219408], [37.38127204589546, -5.995030263438821], [37.38128931261599, -5.995048200711608], [37.381312949582934, -5.995071502402425], [37.3813353292644, -5.995090948417783], [37.38135527819395, -5.995105113834143], [37.38137581385672, -5.9951219614595175], [37.38139534369111, -5.995140904560685], [37.38141135312617, -5.995156746357679], [37.3814270272851, -5.995174180716276], [37.38144647330046, -5.995192704722285], [37.38146466203034, -5.995207540690899], [37.38148234784603, -5.995224555954337], [37.38150321878493, -5.99524081684649], [37.38152392208576, -5.995257161557674], [37.381545547395945, -5.995269818231463], [37.38156968727708, -5.995281636714935], [37.381593408063054, -5.995297059416771], [37.38161654211581, -5.995313404127955], [37.38164202310145, -5.995327904820442], [37.38166306167841, -5.995346009731293], [37.38168686628342, -5.995361600071192], [37.38171570003033, -5.995375011116266], [37.38173849880695, -5.9953926131129265], [37.381761549040675, -5.99540795199573], [37.381787868216634, -5.995421530678868], [37.381812343373895, -5.995431505143642], [37.38183606415987, -5.995445670560002], [37.38185576163232, -5.995458997786045], [37.38188149407506, -5.995474588125944], [37.38190068863332, -5.995491435751319], [37.38192340359092, -5.995510965585709], [37.38194486126304, -5.995526136830449], [37.381965313106775, -5.995539799332619], [37.381986854597926, -5.995557736605406], [37.38201015628874, -5.9955784399062395], [37.38202750682831, -5.995597215369344], [37.38204871304333, -5.995618589222431], [37.38206857815385, -5.9956397116184235], [37.38208567723632, -5.9956626780331135], [37.38210336305201, -5.9956827107816935], [37.38212616182864, -5.995703581720591], [37.38214644603431, -5.995723949745297], [37.38216849043965, -5.995743395760655], [37.38219313323498, -5.99575974047184], [37.38221677020192, -5.995772648602724], [37.38223722204566, -5.995789580047131], [37.38225909881294, -5.995805086567998], [37.382273180410266, -5.995826795697212], [37.38229103386402, -5.995845152065158], [37.38231207244098, -5.995862083509564], [37.38233336247504, -5.995877422392368], [37.38235113210976, -5.995893431827426], [37.3823734279722, -5.995907010510564], [37.38239413127303, -5.995917320251465], [37.38241181708872, -5.995934335514903], [37.38243612460792, -5.995950093492866], [37.382453978061676, -5.99596886895597], [37.38247384317219, -5.995992757380009], [37.38250016234815, -5.996012287214398], [37.38251868635416, -5.996028631925583], [37.382542323321104, -5.9960434678941965], [37.382563864812255, -5.996061069890857], [37.382580544799566, -5.996076408773661], [37.382605941966176, -5.996100716292858], [37.38262505270541, -5.996122341603041], [37.38264240324497, -5.996141619980335], [37.382667968049645, -5.996168525889516], [37.382690850645304, -5.996183194220066], [37.38271348178387, -5.996198952198029], [37.382739298045635, -5.996217895299196], [37.382764276117086, -5.996228540316224], [37.38278699107468, -5.996246561408043], [37.38281272351742, -5.996264833956957], [37.382833594456315, -5.996281849220395], [37.38285815343261, -5.99630213342607], [37.382883969694376, -5.996321579441428], [37.38290710374713, -5.996340354904532], [37.38293400965631, -5.996368853375316], [37.38295345567167, -5.996400201693177], [37.38297449424863, -5.996423503383994], [37.383000226691365, -5.9964510798454285], [37.38302151672542, -5.996468598023057], [37.38304750062525, -5.996489385142922], [37.3830727301538, -5.996506651863456], [37.38309083506465, -5.996524421498179], [37.383115477859974, -5.996548309922218], [37.38313282839954, -5.996567839756608], [37.383154621347785, -5.996589045971632], [37.38317591138184, -5.996610587462783], [37.38319544121623, -5.996632715687156], [37.38321639597416, -5.996653251349926], [37.383233243599534, -5.996669428423047], [37.38325369544327, -5.996689712628722], [37.383285127580166, -5.9966931492090225], [37.38330541178584, -5.99670379422605], [37.38333265297115, -5.99672282114625], [37.383346147835255, -5.996745536103845], [37.3833629116416, -5.996773112565279], [37.3833835311234, -5.996798425912857], [37.38340079784393, -5.996826337650418], [37.383422339335084, -5.99685181863606], [37.3834437970072, -5.996876964345574], [37.3834662605077, -5.996902864426374], [37.38348805345595, -5.996926836669445], [37.38350532017648, -5.996953407302499], [37.38352468237281, -5.99698374979198], [37.38354630768299, -5.997009398415685], [37.38356340676546, -5.9970342088490725], [37.383587127551436, -5.997058432549238], [37.38360732793808, -5.997082320973277], [37.383630126714706, -5.997107466682792], [37.383650997653604, -5.997133199125528], [37.38366893492639, -5.9971570037305355], [37.38369097933173, -5.997181897982955], [37.383712604641914, -5.99720511585474], [37.383732721209526, -5.997227746993303], [37.383754178881645, -5.997251803055406], [37.38377186469734, -5.9972756914794445], [37.38379122689366, -5.997301507741213], [37.383812852203846, -5.997326318174601], [37.383832884952426, -5.997351128607988], [37.383857695385814, -5.9973712451756], [37.383875800296664, -5.997395887970924], [37.383895413950086, -5.997417513281107], [37.38392106257379, -5.997435785830021], [37.383940760046244, -5.99745674058795], [37.38396121188998, -5.997478282079101], [37.38398543559015, -5.997495632618666], [37.384005300700665, -5.997511809691787], [37.384029692038894, -5.997530166059732], [37.38405140116811, -5.997547935694456], [37.3840709310025, -5.997564950957894], [37.38409046083689, -5.997587749734521], [37.38410655409098, -5.997604429721832], [37.38412415608764, -5.997628318145871], [37.38414402119815, -5.997652122750878], [37.384161623194814, -5.997671233490109], [37.3841829970479, -5.9976879972964525], [37.384204622358084, -5.997710209339857], [37.384225744754076, -5.99773233756423], [37.38424845971167, -5.9977527894079685], [37.384270168840885, -5.997771816328168], [37.38429321907461, -5.9977946151047945], [37.38431325182319, -5.997820012271404], [37.38433135673404, -5.997849851846695], [37.3843555804342, -5.997871896252036], [37.38437578082085, -5.997896790504456], [37.38439849577844, -5.997920343652368], [37.38442204892635, -5.997942052781582], [37.38444007001817, -5.997964683920145], [37.38446295261383, -5.997986141592264], [37.384487092494965, -5.998008102178574], [37.384503688663244, -5.998024195432663], [37.38452757708728, -5.998046156018972], [37.384543754160404, -5.998072810471058], [37.384559009224176, -5.998098626732826], [37.384581975638866, -5.998118994757533], [37.38460016436875, -5.9981421288102865], [37.384620448574424, -5.998165933415294], [37.38464115187526, -5.998188061639667], [37.38466026261449, -5.9982087649405], [37.38468297757208, -5.998227205127478], [37.38470477052033, -5.9982499200850725], [37.38472388125956, -5.99827422760427], [37.384744584560394, -5.9982985351234674], [37.38476042635739, -5.9983215015381575], [37.38478012382984, -5.998345138505101], [37.38480602391064, -5.998369362205267], [37.38482647575438, -5.99839149042964], [37.38484801724553, -5.998411774635315], [37.384868301451206, -5.998433651402593], [37.38488221541047, -5.9984516724944115], [37.38490643911064, -5.998477656394243], [37.38492161035538, -5.9985049813985825], [37.38493745215237, -5.998534485697746], [37.384959077462554, -5.998561391606927], [37.384979194030166, -5.998585447669029], [37.3850017413497, -5.99861042574048], [37.385023115202785, -5.998632973060012], [37.38503711298108, -5.998654095456004], [37.38505270332098, -5.998683180660009], [37.38507122732699, -5.998709080740809], [37.38508966751397, -5.9987332206219435], [37.38510835915804, -5.998756689950824], [37.385128643363714, -5.998777225613594], [37.38514708355069, -5.99880195222795], [37.38516711629927, -5.998826678842306], [37.3851864784956, -5.998849058523774], [37.385210702195764, -5.998872863128781], [37.3852314054966, -5.998895075172186], [37.385249426588416, -5.998917622491717], [37.38527398556471, -5.998935140669346], [37.38529410213232, -5.998957185074687], [37.38531371578574, -5.998979145660996], [37.385336346924305, -5.99899934604764], [37.38535763695836, -5.999022731557488], [37.38538160920143, -5.999040752649307], [37.3854021448642, -5.999063299968839], [37.38542268052697, -5.999084338545799], [37.38544464111328, -5.999104455113411], [37.3854660987854, -5.999127505347133], [37.38548847846687, -5.999152483418584], [37.38551069051027, -5.9991782158613205], [37.38552829250693, -5.999199589714408], [37.385549414902925, -5.9992247354239225], [37.38557212986052, -5.999247953295708], [37.385589983314276, -5.999268153682351], [37.38561253063381, -5.999294053763151], [37.385634910315275, -5.999317606911063], [37.38565502688289, -5.999341914430261], [37.38567916676402, -5.999368820339441], [37.385699367150664, -5.999388517811894], [37.3857200704515, -5.999405365437269], [37.38574521616101, -5.999418776482344], [37.385764326900244, -5.999433360993862], [37.38578385673463, -5.999457500874996], [37.385803470388055, -5.999481473118067], [37.38581880927086, -5.99949823692441], [37.385836662724614, -5.99952070042491], [37.38585837185383, -5.999540314078331], [37.385879158973694, -5.999558251351118], [37.385904137045145, -5.999577110633254], [37.385926600545645, -5.999595383182168], [37.38594554364681, -5.999620612710714], [37.38596926443279, -5.999646512791514], [37.385986698791385, -5.99967023357749], [37.386010671034455, -5.999695463106036], [37.386028775945306, -5.999722285196185], [37.38604545593262, -5.999746592715383], [37.3860652372241, -5.999777354300022], [37.386088874191046, -5.999796129763126], [37.38610966131091, -5.999818844720721], [37.38613162189722, -5.99984323605895], [37.386149391531944, -5.9998679626733065], [37.38616825081408, -5.99989109672606], [37.38618744537234, -5.99991749972105], [37.38619993440807, -5.999938203021884], [37.38621963188052, -5.999962761998177], [37.38624209538102, -5.999984722584486], [37.386260787025094, -6.000004084780812], [37.38628593273461, -6.000028643757105], [37.38630764186382, -6.000049263238907], [37.38632574677467, -6.000074241310358], [37.38634619861841, -6.000101650133729], [37.38636623136699, -6.000120425596833], [37.38638827577233, -6.000145152211189], [37.38640998490155, -6.000173734501004], [37.3864280898124, -6.0001948568969965], [37.38644854165614, -6.000222936272621], [37.38647025078535, -6.000246908515692], [37.38648768514395, -6.000268952921033], [37.38650981336832, -6.000294266268611], [37.38653210923076, -6.000317400321364], [37.38655323162675, -6.000339528545737], [37.38657460547984, -6.000361992046237], [37.386596985161304, -6.000384287908673], [37.386613078415394, -6.0004127863794565], [37.386630177497864, -6.000446984544396], [37.3866471927613, -6.000467436388135], [37.38666613586247, -6.000497946515679], [37.386685917153955, -6.000527869910002], [37.386704441159964, -6.000549579039216], [37.38672640174627, -6.000574138015509], [37.38674542866647, -6.000600289553404], [37.38676009699702, -6.000623842701316], [37.3867769446224, -6.000649658963084], [37.38678281195462, -6.000677403062582], [37.38678817637265, -6.000703303143382], [37.38680929876864, -6.0007367469370365], [37.386823799461126, -6.00076231174171], [37.3868440836668, -6.000788547098637], [37.38686738535762, -6.0008147824555635], [37.38688691519201, -6.000831462442875], [37.386915078386664, -6.000850321725011], [37.38693586550653, -6.000871527940035], [37.38695430569351, -6.000890890136361], [37.38697844557464, -6.000916538760066], [37.38700023852289, -6.000933637842536], [37.38701976835728, -6.000955766066909], [37.38704089075327, -6.00098418071866], [37.38705991767347, -6.001001531258225], [37.38708196207881, -6.001026928424835], [37.387101240456104, -6.001055259257555], [37.387118423357606, -6.001080069690943], [37.387137869372964, -6.001110328361392], [37.38715572282672, -6.00114150904119], [37.38716988824308, -6.001170510426164], [37.387190675362945, -6.001203367486596], [37.38720794208348, -6.001231027767062], [37.38722185604274, -6.001257598400116], [37.387242978438735, -6.001290958374739], [37.38726049661636, -6.001313589513302], [37.38727826625109, -6.001338819041848], [37.387299137189984, -6.001363210380077], [37.387314811348915, -6.001384165138006], [37.387336185202, -6.001410400494933], [37.38736107945442, -6.001432025805116], [37.38738094456494, -6.00145373493433], [37.38740407861769, -6.001474857330322], [37.38742738030851, -6.001491621136665], [37.38744799979031, -6.00151333026588], [37.38747180439532, -6.001541409641504], [37.38749208860099, -6.001562783494592], [37.38751522265375, -6.001588683575392], [37.38753869198263, -6.001613913103938], [37.38755595870316, -6.001635538414121], [37.38757951185107, -6.001654816791415], [37.38760029897094, -6.001676106825471], [37.38761555403471, -6.001698570325971], [37.38763751462102, -6.001721452921629], [37.387662241235375, -6.001741150394082], [37.38768386654556, -6.001762859523296], [37.38771362230182, -6.0017878375947475], [37.38773667253554, -6.001804014667869], [37.3877680208534, -6.001815078780055], [37.387799033895135, -6.001825220882893], [37.38782241940498, -6.001840056851506], [37.38785167224705, -6.001854473724961], [37.38788184709847, -6.001870483160019], [37.38790422677994, -6.001885570585728], [37.387932976707816, -6.001905687153339], [37.387957284227014, -6.0019247978925705], [37.38798066973686, -6.0019442439079285], [37.38800891675055, -6.001963438466191], [37.38803381100297, -6.001979699358344], [37.388057950884104, -6.001996044069529], [37.388086281716824, -6.002012640237808], [37.388109248131514, -6.002028733491898], [37.38813942298293, -6.002041641622782], [37.388166496530175, -6.002052119001746], [37.388195330277085, -6.002064608037472], [37.38822718150914, -6.002070140093565], [37.388254925608635, -6.002080617472529], [37.38828107714653, -6.00209578871727], [37.3883103299886, -6.002104673534632], [37.38833270967007, -6.002113642171025], [37.38835676573217, -6.002121269702911], [37.388388030231, -6.002130154520273], [37.38841342739761, -6.002140128985047], [37.388440081849694, -6.002144739031792], [37.38846816122532, -6.002151947468519], [37.38849397748709, -6.002156473696232], [37.388521218672395, -6.002160664647818], [37.38855089060962, -6.0021668672561646], [37.38857704214752, -6.002168040722609], [37.38860487006605, -6.0021724831312895], [37.38863630220294, -6.00217642262578], [37.38865935243666, -6.002182038500905], [37.38868810236454, -6.002185558900237], [37.38871844485402, -6.002190504223108], [37.388740656897426, -6.002195617184043], [37.388767562806606, -6.002202406525612], [37.388795306906104, -6.0022051725536585], [37.38881550729275, -6.00221112370491], [37.388844760134816, -6.002217996865511], [37.388867726549506, -6.002224199473858], [37.388895973563194, -6.002230821177363], [37.3889200296253, -6.002238700166345], [37.38893930800259, -6.002244232222438], [37.38896948285401, -6.002250267192721], [37.38899848423898, -6.002258313819766], [37.3890268150717, -6.002264600247145], [37.38905908539891, -6.00227122195065], [37.389084063470364, -6.002278598025441], [37.3891063593328, -6.002287901937962], [37.389135947450995, -6.002298966050148], [37.38916075788438, -6.002311119809747], [37.389188753440976, -6.002319417893887], [37.38921817392111, -6.002326710149646], [37.38923527300358, -6.002332242205739], [37.38926419056952, -6.002339283004403], [37.38929704762995, -6.002349592745304], [37.38932135514915, -6.002358393743634], [37.389351446181536, -6.002364093437791], [37.38938162103295, -6.002373564988375], [37.38940701819956, -6.002375492826104], [37.3894361872226, -6.002379432320595], [37.38946443423629, -6.002384293824434], [37.38949293270707, -6.0023909993469715], [37.38952252082527, -6.00239728577435], [37.389546409249306, -6.0024098586291075], [37.38956920802593, -6.0024184081703424], [37.38959904760122, -6.002427376806736], [37.38961782306433, -6.00243391469121], [37.38964808173478, -6.002440117299557], [37.389674903824925, -6.002447661012411], [37.38969761878252, -6.002451349049807], [37.38972586579621, -6.0024563781917095], [37.38975168205798, -6.002458557486534], [37.38977188244462, -6.002461323514581], [37.38980121910572, -6.002470292150974], [37.38982753828168, -6.00247741676867], [37.38985528238118, -6.002487726509571], [37.38988570868969, -6.002499796450138], [37.38990733399987, -6.002512704581022], [37.38992971368134, -6.0025229305028915], [37.389963660389185, -6.0025345813483], [37.38999358378351, -6.002540783956647], [37.390024261549115, -6.002553943544626], [37.39005376584828, -6.002565259113908], [37.39008033648133, -6.002576742321253], [37.390111684799194, -6.0025898180902], [37.39014169201255, -6.002600714564323], [37.390163987874985, -6.002609683200717], [37.390192318707705, -6.002620747312903], [37.390218721702695, -6.002628793939948], [37.39024395123124, -6.002636672928929], [37.39027404226363, -6.002648491412401], [37.39030052907765, -6.002661986276507], [37.390321819111705, -6.002675900235772], [37.39034520462155, -6.002688640728593], [37.39036934450269, -6.002703979611397], [37.39038996398449, -6.00272074341774], [37.39041393622756, -6.002729963511229], [37.390435142442584, -6.0027402732521296], [37.39045266062021, -6.002736501395702], [37.3904655687511, -6.002732142806053], [37.39048828370869, -6.002731053158641], [37.390505550429225, -6.0027282033115625], [37.39053161814809, -6.002726275473833], [37.39055626094341, -6.0027258563786745], [37.390581322833896, -6.002722671255469], [37.39070034585893, -6.002718647941947], [37.390715181827545, -6.002724347636104], [37.39074753597379, -6.002719569951296], [37.3907935526222, -6.002713534981012], [37.39082003943622, -6.002688389271498], [37.39083646796644, -6.002682689577341], [37.39083940163255, -6.002685623243451], [37.390840742737055, -6.002688389271498], [37.390840742737055, -6.002688389271498], [37.391120195388794, -6.002705739811063], [37.39105682820082, -6.002885615453124], [37.391079207882285, -6.002912940457463], [37.3911059461534, -6.002932051196694], [37.39113084040582, -6.0029552690684795], [37.39115665666759, -6.002976978197694], [37.39118456840515, -6.0029924008995295], [37.39120543934405, -6.003007153049111], [37.39122999832034, -6.003020312637091], [37.39125723950565, -6.003034645691514], [37.39127819426358, -6.003045290708542], [37.39130945876241, -6.003056354820728], [37.39133368246257, -6.003070520237088], [37.39135497249663, -6.003083428367972], [37.39138288423419, -6.003087367862463], [37.39140149205923, -6.0030904691666365], [37.391423201188445, -6.0031001921743155], [37.3914497718215, -6.0031072329729795], [37.39147022366524, -6.0031146090477705], [37.39149319007993, -6.003124164417386], [37.39151967689395, -6.00312827154994], [37.391541972756386, -6.003137659281492], [37.391557060182095, -6.003149729222059], [37.39158321171999, -6.00316196680069], [37.39160324446857, -6.003174455836415], [37.391625959426165, -6.003185268491507], [37.39165596663952, -6.003189543262124], [37.39167960360646, -6.00319666787982], [37.39170575514436, -6.003204043954611], [37.391734002158046, -6.003210414201021], [37.391753029078245, -6.003224579617381], [37.39177624695003, -6.003234721720219], [37.39180005155504, -6.003242181614041], [37.39182067103684, -6.003251736983657], [37.3918467387557, -6.0032550897449255], [37.39187330938876, -6.00325676612556], [37.391897616907954, -6.00326070562005], [37.39192686975002, -6.003258861601353], [37.3919540271163, -6.003264142200351], [37.391975820064545, -6.003272943198681], [37.392003647983074, -6.003278475254774], [37.392024602741, -6.0032866057008505], [37.39205142483115, -6.003297753632069], [37.39208151586354, -6.00330806337297], [37.39210314117372, -6.0033169481903315], [37.39213180728257, -6.003322480246425], [37.39216072484851, -6.003329353407025], [37.39218360744417, -6.003336980938911], [37.3922130279243, -6.003337986767292], [37.39223691634834, -6.003344440832734], [37.392258206382394, -6.003354415297508], [37.3922801669687, -6.003360701724887], [37.392302714288235, -6.003371179103851], [37.392322327941656, -6.003388613462448], [37.39235376007855, -6.003393391147256], [37.39237882196903, -6.003401102498174], [37.39240053109825, -6.0034161899238825], [37.3924339748919, -6.003425158560276], [37.39245283417404, -6.00343463011086], [37.39247823134065, -6.003452483564615], [37.39250874146819, -6.003466146066785], [37.392532378435135, -6.003481233492494], [37.392564145848155, -6.003491710871458], [37.392591470852494, -6.00350059568882], [37.39261359907687, -6.003509564325213], [37.3926448635757, -6.003507887944579], [37.3926705121994, -6.003516186028719], [37.39269683137536, -6.003522304818034], [37.392723988741636, -6.003527082502842], [37.392745362594724, -6.003532446920872], [37.3927759565413, -6.003539320081472], [37.39280730485916, -6.003544433042407], [37.39283228293061, -6.00355239585042], [37.39286731928587, -6.00356069393456], [37.39289774559438, -6.0035658068954945], [37.39292389713228, -6.003572763875127], [37.392954491078854, -6.003582067787647], [37.3929778765887, -6.003596568480134], [37.39299573004246, -6.003620037809014], [37.393023977056146, -6.00363596342504], [37.39304753020406, -6.003646943718195], [37.3930746037513, -6.003652811050415], [37.3931041918695, -6.003656582906842], [37.393130511045456, -6.003661025315523], [37.39315993152559, -6.003663623705506], [37.39319044165313, -6.003660690039396], [37.393215503543615, -6.003667814657092], [37.39324442110956, -6.0036668088287115], [37.39327350631356, -6.0036632884293795], [37.393300998955965, -6.003663120791316], [37.39333285018802, -6.003654319792986], [37.39336025901139, -6.003650212660432], [37.393390350043774, -6.003644345328212], [37.39342052489519, -6.003640154376626], [37.3934475146234, -6.003643088042736], [37.393476432189345, -6.003638142719865], [37.39350493066013, -6.0036329459398985], [37.3935304954648, -6.003631353378296], [37.39355966448784, -6.00362716242671], [37.393585396930575, -6.003625066950917], [37.39361305721104, -6.003621630370617], [37.39364029839635, -6.003618696704507], [37.393663516268134, -6.003618612885475], [37.39369251765311, -6.003612075001001], [37.39372093230486, -6.00360662676394], [37.39374683238566, -6.003604615107179], [37.3937757499516, -6.003597490489483], [37.39380106329918, -6.0035948920995], [37.393826795741916, -6.003595059737563], [37.393857054412365, -6.003587264567614], [37.393881948664784, -6.003585839644074], [37.39391111768782, -6.003579134121537], [37.39394045434892, -6.003570165485144], [37.393967024981976, -6.003565387800336], [37.39399476908147, -6.003552228212357], [37.394021255895495, -6.003542169928551], [37.39404656924307, -6.003536386415362], [37.39407733082771, -6.003523645922542], [37.394105242565274, -6.003516940400004], [37.394134663045406, -6.003508307039738], [37.39416550844908, -6.003493890166283], [37.394188391044736, -6.003490872681141], [37.394213704392314, -6.003476958721876], [37.394237173721194, -6.00346103310585], [37.39425988867879, -6.003451477736235], [37.39428821951151, -6.003431444987655], [37.394311437383294, -6.003418369218707], [37.39433574490249, -6.003409568220377], [37.39435812458396, -6.003395151346922], [37.394383773207664, -6.0033778846263885], [37.394410679116845, -6.003363886848092], [37.39443666301668, -6.003350727260113], [37.39446029998362, -6.003341004252434], [37.39448653534055, -6.003330275416374], [37.394515704363585, -6.003315942361951], [37.39454043097794, -6.0033101588487625], [37.39456591196358, -6.003300016745925], [37.39458963274956, -6.003289874643087], [37.39461209625006, -6.003284510225058], [37.39463799633086, -6.003274451941252], [37.39466456696391, -6.003266237676144], [37.394695077091455, -6.003251653164625], [37.39472458139062, -6.003229189664125], [37.394747380167246, -6.00321477279067], [37.394774202257395, -6.003200523555279], [37.39479943178594, -6.003180323168635], [37.39482147619128, -6.00316246971488], [37.394847543910146, -6.003137659281492], [37.39487361162901, -6.003119302913547], [37.394898757338524, -6.003103209659457], [37.39492197521031, -6.0030819196254015], [37.39494351670146, -6.0030704364180565], [37.394967656582594, -6.003052499145269], [37.39499380812049, -6.003034394234419], [37.3950192052871, -6.003021653741598], [37.39504384808242, -6.00299927406013], [37.395064467564225, -6.002989215776324], [37.39509020000696, -6.002978067845106], [37.395119201391935, -6.002961555495858], [37.39514241926372, -6.002952586859465], [37.39516731351614, -6.002935320138931], [37.395190028473735, -6.00291864015162], [37.39521073177457, -6.002903636544943], [37.395235542207956, -6.002884525805712], [37.395259933546185, -6.002868432551622], [37.3952834866941, -6.002851584926248], [37.39530812948942, -6.002827025949955], [37.39533067680895, -6.0028100945055485], [37.39535498432815, -6.0027865413576365], [37.39537702873349, -6.002761395648122], [37.39539857022464, -6.002748319879174], [37.39542287774384, -6.002724934369326], [37.395446095615625, -6.002704901620746], [37.39546730183065, -6.002687634900212], [37.395491944625974, -6.002661399543285], [37.39551122300327, -6.002633739262819], [37.395529663190246, -6.002616640180349], [37.39554894156754, -6.002595936879516], [37.39556981250644, -6.002574898302555], [37.39559235982597, -6.00256291218102], [37.3956149071455, -6.002548495307565], [37.39563728682697, -6.002536676824093], [37.395659079775214, -6.002524355426431], [37.39568087272346, -6.002504825592041], [37.395701156929135, -6.002485463395715], [37.39572144113481, -6.0024734772741795], [37.39574423991144, -6.002454534173012], [37.39576544612646, -6.002438440918922], [37.39578673616052, -6.00242561660707], [37.39580743946135, -6.002407930791378], [37.39582805894315, -6.002395525574684], [37.39584683440626, -6.0023818630725145], [37.39586954936385, -6.002366188913584], [37.39589000120759, -6.002351688221097], [37.39591103978455, -6.0023347567766905], [37.395934760570526, -6.002317238599062], [37.395953787490726, -6.002301815897226], [37.395974826067686, -6.002286225557327], [37.39599737338722, -6.0022693779319525], [37.39601941779256, -6.002260744571686], [37.396045653149486, -6.002243813127279], [37.39607121795416, -6.00222528912127], [37.39609267562628, -6.0022148955613375], [37.39611539058387, -6.002201484516263], [37.39614028483629, -6.002188995480537], [37.39616475999355, -6.002177931368351], [37.39618856459856, -6.0021604131907225], [37.39621262066066, -6.002144403755665], [37.3962349165231, -6.002129819244146], [37.39625779911876, -6.002111630514264], [37.39627858623862, -6.0020944476127625], [37.39630104973912, -6.0020802821964025], [37.39632745273411, -6.00206159055233], [37.39635025151074, -6.002046503126621], [37.396373553201556, -6.0020325891673565], [37.396399369463325, -6.002010460942984], [37.39642024040222, -6.0020010732114315], [37.396440859884024, -6.001986237242818], [37.396460892632604, -6.0019666235893965], [37.3964833561331, -6.001949440687895], [37.39650573581457, -6.001925719901919], [37.3965285345912, -6.001909878104925], [37.396554434672, -6.001891186460853], [37.39657815545797, -6.001879619434476], [37.396604139357805, -6.001865621656179], [37.39662894979119, -6.001847852021456], [37.39665325731039, -6.001827651634812], [37.39667488262057, -6.00181239657104], [37.396698854863644, -6.00179236382246], [37.39672115072608, -6.001778030768037], [37.39674353040755, -6.001761518418789], [37.39676808938384, -6.001745508983731], [37.396792732179165, -6.001732349395752], [37.396817710250616, -6.001707706600428], [37.39684293977916, -6.001691613346338], [37.39686766639352, -6.001679124310613], [37.39689423702657, -6.001659845933318], [37.39692005328834, -6.001641992479563], [37.39694461226463, -6.001619026064873], [37.39697051234543, -6.001593209803104], [37.396993059664965, -6.001577535644174], [37.397019965574145, -6.001550378277898], [37.397042931988835, -6.001535123214126], [37.39706866443157, -6.001520538702607], [37.39709414541721, -6.00150553509593], [37.39711602218449, -6.001487011089921], [37.39713907241821, -6.001465553417802], [37.39716053009033, -6.001446945592761], [37.397181149572134, -6.00143127143383], [37.39720554091036, -6.001407550647855], [37.397227082401514, -6.001388523727655], [37.397248120978475, -6.001373017206788], [37.39727494306862, -6.001352062448859], [37.39729723893106, -6.001337645575404], [37.39732129499316, -6.001326078549027], [37.39734912291169, -6.001312332227826], [37.39737041294575, -6.00129833444953], [37.397397235035896, -6.001285929232836], [37.397421291098, -6.0012670699507], [37.397445514798164, -6.001253491267562], [37.397472923621535, -6.0012350510805845], [37.397495387122035, -6.001217281445861], [37.39751600660384, -6.00119867362082], [37.39754014648497, -6.001173444092274], [37.39756101742387, -6.0011534951627254], [37.3975819721818, -6.00112734362483], [37.39760317839682, -6.001105131581426], [37.39762547425926, -6.001090379431844], [37.39764818921685, -6.001062383875251], [37.39767090417445, -6.0010442789644], [37.397695211693645, -6.001028772443533], [37.39772094413638, -6.001010751351714], [37.39774432964623, -6.000998597592115], [37.397768972441554, -6.000980408862233], [37.39779428578913, -6.000961046665907], [37.39781624637544, -6.000945959240198], [37.397840889170766, -6.000924753025174], [37.3978668730706, -6.0009052231907845], [37.39788958802819, -6.000892398878932], [37.39791573956609, -6.000870270654559], [37.39794063381851, -6.000856105238199], [37.39796242676675, -6.000841688364744], [37.39798732101917, -6.000819057226181], [37.398008946329355, -6.000806149095297], [37.39803325384855, -6.000793240964413], [37.398059237748384, -6.000776644796133], [37.39808136597276, -6.0007628146559], [37.39810776896775, -6.0007435362786055], [37.39813316613436, -6.000727275386453], [37.39815931767225, -6.000713529065251], [37.39818471483886, -6.000695927068591], [37.39820952527225, -6.000676900148392], [37.39823601208627, -6.000661142170429], [37.39826417528093, -6.000634906813502], [37.39828705787659, -6.000614957883954], [37.39831111393869, -6.000596014782786], [37.39833592437208, -6.000572629272938], [37.3983552865684, -6.000556871294975], [37.398379258811474, -6.000538347288966], [37.398400297388434, -6.000508759170771], [37.39842049777508, -6.000490738078952], [37.39844438619912, -6.000467101112008], [37.398466262966394, -6.0004454758018255], [37.39848570898175, -6.000430807471275], [37.39850867539644, -6.000408930703998], [37.39852795377374, -6.000388311222196], [37.398550333455205, -6.000374229624867], [37.39857221022248, -6.000352688133717], [37.39859685301781, -6.000336678698659], [37.39862191490829, -6.000317987054586], [37.39864588715136, -6.000294853001833], [37.39866751246154, -6.000280687585473], [37.39869114942849, -6.000265264883637], [37.398716295138, -6.000245902687311], [37.39873833954334, -6.000233246013522], [37.39875979721546, -6.0002131294459105], [37.39878075197339, -6.000191839411855], [37.39880363456905, -6.000174572691321], [37.398826349526644, -6.000144565477967], [37.3988476395607, -6.000128388404846], [37.39886817522347, -6.0001076851040125], [37.398891393095255, -6.000085975974798], [37.39891259931028, -6.000070469453931], [37.39893598482013, -6.000043395906687], [37.39896004088223, -6.000025123357773], [37.39898459985852, -6.000011460855603], [37.399007147178054, -5.999985812231898], [37.39902701228857, -5.999970221891999], [37.399046793580055, -5.999947255477309], [37.39906615577638, -5.999920768663287], [37.39908551797271, -5.9999029990285635], [37.39910747855902, -5.999876847490668], [37.399130361154675, -5.999856144189835], [37.399151818826795, -5.9998417273163795], [37.39917604252696, -5.999822868034244], [37.39920093677938, -5.999806188046932], [37.399224573746324, -5.999791184440255], [37.399250557646155, -5.999771151691675], [37.39927276968956, -5.9997527953237295], [37.39929724484682, -5.999734858050942], [37.39932448603213, -5.999714322388172], [37.399345776066184, -5.9996994864195585], [37.39936916157603, -5.999674005433917], [37.39939162507653, -5.9996535535901785], [37.39941366948187, -5.9996346943080425], [37.39943680353463, -5.999606866389513], [37.39946186542511, -5.99958130158484], [37.399484161287546, -5.9995601791888475], [37.399506624788046, -5.999535955488682], [37.39952347241342, -5.999519862234592], [37.39954400807619, -5.9994998294860125], [37.39956638775766, -5.999479377642274], [37.399585749953985, -5.999467642977834], [37.39960838109255, -5.999445598572493], [37.39963428117335, -5.999424643814564], [37.399655571207404, -5.99940737709403], [37.39968138746917, -5.9993864223361015], [37.39970485679805, -5.999367982149124], [37.399729331955314, -5.9993537329137325], [37.399753304198384, -5.999331437051296], [37.39977409131825, -5.999315679073334], [37.39979445934296, -5.9992949757725], [37.399816419929266, -5.999270165339112], [37.39983796142042, -5.999253066256642], [37.399858832359314, -5.999234709888697], [37.399883307516575, -5.999213336035609], [37.39990392699838, -5.999197158962488], [37.39992320537567, -5.999179808422923], [37.399940975010395, -5.999152734875679], [37.39995832554996, -5.999135551974177], [37.39997986704111, -5.999108646064997], [37.39999981597066, -5.9990842547267675], [37.40002051927149, -5.999066988006234], [37.40004004910588, -5.999043434858322], [37.40005999803543, -5.9990250784903765], [37.40008044987917, -5.999007141217589], [37.400099309161305, -5.9989803191274405], [37.40011992864311, -5.998959615826607], [37.40013962611556, -5.998941175639629], [37.40016368217766, -5.998918041586876], [37.40018354728818, -5.998893147334456], [37.40020324476063, -5.9988748747855425], [37.4002240318805, -5.998850567266345], [37.40023978985846, -5.998832546174526], [37.40026275627315, -5.998815279453993], [37.40028480067849, -5.998792229220271], [37.4003051687032, -5.99877261556685], [37.4003241956234, -5.998751744627953], [37.40034238435328, -5.998731544241309], [37.40036090835929, -5.998714109882712], [37.40038144402206, -5.998690975829959], [37.400400806218386, -5.998668177053332], [37.40042100660503, -5.998652754351497], [37.40044347010553, -5.998627105727792], [37.40046367049217, -5.998606653884053], [37.40048596635461, -5.998585699126124], [37.400509770959616, -5.998559044674039], [37.400532234460115, -5.998541694134474], [37.40055251866579, -5.998519230633974], [37.40057732909918, -5.998494252562523], [37.40059744566679, -5.998479500412941], [37.40061798132956, -5.998459132388234], [37.400636337697506, -5.99843792617321], [37.40065444260836, -5.998420407995582], [37.400671960785985, -5.998387802392244], [37.40068621002138, -5.998367182910442], [37.400708086788654, -5.99834555760026], [37.400730550289154, -5.998325441032648], [37.4007557798177, -5.998301971703768], [37.400783356279135, -5.998276993632317], [37.400803389027715, -5.99826006218791], [37.40082719363272, -5.998238185420632], [37.40084622055292, -5.998215638101101], [37.40086658857763, -5.998194431886077], [37.40088611841202, -5.998168364167213], [37.400906570255756, -5.9981530252844095], [37.40092878229916, -5.9981265384703875], [37.40095158107579, -5.998100638389587], [37.40097622387111, -5.998081946745515], [37.40099768154323, -5.998049173504114], [37.40102048031986, -5.99802796728909], [37.40104168653488, -5.998001480475068], [37.401068257167935, -5.997974406927824], [37.4010872002691, -5.997963091358542], [37.40111100487411, -5.997941046953201], [37.40113330073655, -5.997924031689763], [37.40114964544773, -5.997912883758545], [37.4011683370918, -5.997893186286092], [37.40118862129748, -5.997876925393939], [37.40120731294155, -5.997860413044691], [37.40123103372753, -5.997837362810969], [37.40125123411417, -5.997815318405628], [37.40127612836659, -5.997797213494778], [37.401298424229026, -5.997770223766565], [37.40131921134889, -5.997750693932176], [37.4013395793736, -5.9977298229932785], [37.40136849693954, -5.997710041701794], [37.401389284059405, -5.997694116085768], [37.40141258575022, -5.997670646756887], [37.40143496543169, -5.997644830495119], [37.401455752551556, -5.997626222670078], [37.401477713137865, -5.997599484398961], [37.4015002604574, -5.997576769441366], [37.40152037702501, -5.997560927644372], [37.401545187458396, -5.997534524649382], [37.40156790241599, -5.997518012300134], [37.40158902481198, -5.9975007455796], [37.401617439463735, -5.997478952631354], [37.401642333716154, -5.9974609315395355], [37.40166622214019, -5.997436037287116], [37.401689356192946, -5.997415333986282], [37.401710227131844, -5.9973973128944635], [37.401729337871075, -5.997370490804315], [37.401747442781925, -5.997344925999641], [37.40176755934954, -5.997320115566254], [37.40179002285004, -5.997297232970595], [37.40180821157992, -5.997281558811665], [37.40183025598526, -5.99725984968245], [37.401850707829, -5.997236464172602], [37.40187032148242, -5.997215090319514], [37.40189337171614, -5.997181897982955], [37.40191658958793, -5.9971552435308695], [37.401940394192934, -5.997140323743224], [37.4019662104547, -5.997116267681122], [37.40199068561196, -5.99709078669548], [37.40201189182699, -5.997074190527201], [37.40203452296555, -5.997049463912845], [37.4020544718951, -5.997028090059757], [37.40207601338625, -5.997004117816687], [37.40209956653416, -5.996979475021362], [37.40212261676788, -5.996963130310178], [37.40214826539159, -5.996936392039061], [37.40217165090144, -5.996914515271783], [37.4021908454597, -5.996897080913186], [37.402215069159865, -5.9968736954033375], [37.4022376164794, -5.9968536626547575], [37.40225924178958, -5.9968336299061775], [37.402281956747174, -5.996807897463441], [37.40230274386704, -5.996791301295161], [37.40232780575752, -5.996766071766615], [37.40234959870577, -5.996748302131891], [37.40236862562597, -5.996735477820039], [37.40239033475518, -5.996709829196334], [37.40240978077054, -5.99669155664742], [37.402434507384896, -5.9966701827943325], [37.40246216766536, -5.996644366532564], [37.40248295478523, -5.996630368754268], [37.402508687227964, -5.996607067063451], [37.402534168213606, -5.996583765372634], [37.402557553723454, -5.996565660461783], [37.40258110687137, -5.996543196961284], [37.4026036541909, -5.996519308537245], [37.402626033872366, -5.996499778702855], [37.40265059284866, -5.996473040431738], [37.402674900367856, -5.996455522254109], [37.40269660949707, -5.996437082067132], [37.40272041410208, -5.996413780376315], [37.40273776464164, -5.996393915265799], [37.40275628864765, -5.996371954679489], [37.402775986120105, -5.996346306055784], [37.40279492922127, -5.996325854212046], [37.40281353704631, -5.996299535036087], [37.402837509289384, -5.996273048222065], [37.402857122942805, -5.99625376984477], [37.402881095185876, -5.996227785944939], [37.40290146321058, -5.996206160634756], [37.402923591434956, -5.9961868822574615], [37.40294429473579, -5.996161485090852], [37.40296348929405, -5.9961421228945255], [37.40298595279455, -5.996123598888516], [37.40300850011408, -5.996101386845112], [37.40302886813879, -5.996082192286849], [37.40305518731475, -5.996061572805047], [37.40308117121458, -5.996035085991025], [37.40310254506767, -5.996010946109891], [37.403122410178185, -5.99597473628819], [37.40313917398453, -5.995945148169994], [37.40315694361925, -5.995911117643118], [37.40317437797785, -5.995875326916575], [37.40318929776549, -5.995847079902887], [37.4032046366483, -5.995811456814408], [37.403216706588864, -5.995787987485528], [37.403231458738446, -5.995756220072508], [37.40324637852609, -5.995728224515915], [37.40326381288469, -5.995709700509906], [37.40328066051006, -5.995677765458822], [37.4032960832119, -5.995652033016086], [37.40331502631307, -5.995626300573349], [37.40333463996649, -5.995598388835788], [37.40335182286799, -5.995574668049812], [37.40337043069303, -5.995544325560331], [37.40338895469904, -5.995517838746309], [37.403407311066985, -5.995494872331619], [37.403426421806216, -5.995462015271187], [37.40344377234578, -5.995438881218433], [37.40346330218017, -5.9954126458615065], [37.403484005481005, -5.9953840635716915], [37.403505127877, -5.9953599236905575], [37.40352432243526, -5.995325306430459], [37.403544774279, -5.995300998911262], [37.40356572903693, -5.9952762722969055], [37.403590958565474, -5.995248947292566], [37.40360780619085, -5.995229417458177], [37.40362884476781, -5.995201421901584], [37.40364795550704, -5.995170744135976], [37.40366538986564, -5.995148783549666], [37.40368475206196, -5.995121793821454], [37.403699085116386, -5.995099246501923], [37.40371132269502, -5.9950807224959135], [37.4037226382643, -5.9950480330735445], [37.40373261272907, -5.995019702240825], [37.40373990498483, -5.994988521561027], [37.40374602377415, -5.99495206028223], [37.40374904125929, -5.994920879602432], [37.403750298544765, -5.994883580133319], [37.40374895744026, -5.99484795704484], [37.40374795161188, -5.994814429432154], [37.403743509203196, -5.994775202125311], [37.40373973734677, -5.9947429317981005], [37.403737138956785, -5.994707476347685], [37.40373378619552, -5.994672439992428], [37.40372649393976, -5.994641929864883], [37.403721464797854, -5.99460756406188], [37.40371283143759, -5.994574539363384], [37.40370805375278, -5.994542688131332], [37.4036948941648, -5.994504718109965], [37.403682824224234, -5.9944746270775795], [37.40366849116981, -5.9944436978548765], [37.403649631887674, -5.9944110084325075], [37.40363362245262, -5.994383599609137], [37.40361920557916, -5.994350239634514], [37.40361048839986, -5.994313694536686], [37.40360235795379, -5.994293158873916], [37.40360051393509, -5.994258122518659], [37.40360478870571, -5.994224930182099], [37.40361417643726, -5.994181931018829], [37.40361819975078, -5.994153516367078], [37.40362599492073, -5.994116552174091], [37.40363370627165, -5.9940823540091515], [37.40363538265228, -5.9940490778535604], [37.403638903051615, -5.99401144310832], [37.403638400137424, -5.993975903838873], [37.40363471210003, -5.993942208588123], [37.403622809797525, -5.993903316557407], [37.40361819975078, -5.99387364462018], [37.403615936636925, -5.9938443917781115], [37.40361124277115, -5.993813211098313], [37.40360646508634, -5.993780521675944], [37.40360478870571, -5.993749340996146], [37.40360051393509, -5.9937195014208555], [37.40359858609736, -5.993691589683294], [37.40359481424093, -5.993665941059589], [37.40358777344227, -5.993641801178455], [37.40358299575746, -5.993622187525034], [37.403581738471985, -5.993592012673616], [37.40358492359519, -5.993559993803501], [37.403586097061634, -5.993529818952084], [37.4035825766623, -5.993497045710683], [37.40357888862491, -5.993463937193155], [37.40357260219753, -5.993425380438566], [37.40356522612274, -5.993384225293994], [37.403564136475325, -5.9933493565768], [37.403560196980834, -5.993311638012528], [37.403549971058965, -5.993276098743081], [37.40354930050671, -5.993246929720044], [37.40354670211673, -5.993213653564453], [37.40354385226965, -5.993180042132735], [37.40354242734611, -5.99314852617681], [37.403543097898364, -5.993123631924391], [37.40354594774544, -5.993091445416212], [37.403544606640935, -5.993061438202858], [37.40354544483125, -5.993026737123728], [37.403540248051286, -5.992994969710708], [37.403538320213556, -5.992960687726736], [37.40353329107165, -5.992928920313716], [37.40352583117783, -5.992899667471647], [37.40351594053209, -5.992862032726407], [37.40350982174277, -5.992823811247945], [37.403504122048616, -5.992788355797529], [37.403494818136096, -5.992754073813558], [37.40349146537483, -5.992724569514394], [37.40348719060421, -5.992691293358803], [37.40347797051072, -5.992659609764814], [37.40347042679787, -5.992629854008555], [37.403460117056966, -5.992600852623582], [37.40345383062959, -5.9925738628953695], [37.40344234742224, -5.992529354989529], [37.40343304350972, -5.992492139339447], [37.403428265824914, -5.992457689717412], [37.4034150224179, -5.992417372763157], [37.403406808152795, -5.992382001131773], [37.4034049641341, -5.992351407185197], [37.403401443734765, -5.992311928421259], [37.4034015275538, -5.99227144382894], [37.40340278483927, -5.992233976721764], [37.40340278483927, -5.992192402482033], [37.40340203046799, -5.9921597968786955], [37.40339733660221, -5.992125514894724], [37.40339339710772, -5.9920864552259445], [37.40339390002191, -5.992052173241973], [37.4033893737942, -5.9920113533735275], [37.403384763747454, -5.9919695276767015], [37.40338115952909, -5.991936335340142], [37.403375124558806, -5.991900963708758], [37.403370346874, -5.991871878504753], [37.40336464717984, -5.991833908483386], [37.40336372517049, -5.991797614842653], [37.40336389280856, -5.991764673963189], [37.4033566005528, -5.991725279018283], [37.40335366688669, -5.991689907386899], [37.40335232578218, -5.991662414744496], [37.40334930829704, -5.991623438894749], [37.40335081703961, -5.991593515500426], [37.403347631916404, -5.991550013422966], [37.40334268659353, -5.991512881591916], [37.40333857946098, -5.991473821923137], [37.40333321504295, -5.9914361871778965], [37.403332544490695, -5.991401486098766], [37.403327682986856, -5.991368796676397], [37.403322737663984, -5.991332503035665], [37.403314523398876, -5.991305261850357], [37.40330840460956, -5.991273997351527], [37.40330186672509, -5.9912406373769045], [37.40329868160188, -5.991213312372565], [37.403297424316406, -5.991177186369896], [37.40329382009804, -5.991142736747861], [37.403291556984186, -5.991110131144524], [37.403284600004554, -5.991073669865727], [37.403278732672334, -5.991043159738183], [37.403273871168494, -5.991009045392275], [37.40326666273177, -5.990973673760891], [37.40326188504696, -5.990940313786268], [37.40325786173344, -5.990914078429341], [37.403252832591534, -5.990883819758892], [37.40325056947768, -5.990853980183601], [37.40324495360255, -5.990817686542869], [37.40324336104095, -5.990786254405975], [37.40324201993644, -5.990753984078765], [37.403236823156476, -5.990713918581605], [37.403231877833605, -5.990680307149887], [37.4032252561301, -5.990646360442042], [37.403219724074006, -5.990611910820007], [37.403214275836945, -5.9905782993882895], [37.4032115098089, -5.990539491176605], [37.403212347999215, -5.99051333963871], [37.403207486495376, -5.990484254434705], [37.403201116248965, -5.990449637174606], [37.403201535344124, -5.990412505343556], [37.40320379845798, -5.990364057943225], [37.403201116248965, -5.990329021587968], [37.40319826640189, -5.99029297940433], [37.40319290198386, -5.990253081545234], [37.40318762138486, -5.990218799561262], [37.40318234078586, -5.99018594250083], [37.40317773073912, -5.990145290270448], [37.4031750485301, -5.990110170096159], [37.403170354664326, -5.990074630826712], [37.403166918084025, -5.990035319700837], [37.4031613022089, -5.989996427670121], [37.40315744653344, -5.98996214568615], [37.403154680505395, -5.989930797368288], [37.40315392613411, -5.9899035561829805], [37.40314872935414, -5.989872124046087], [37.40314420312643, -5.989840775728226], [37.403137581422925, -5.989809846505523], [37.40313179790974, -5.989774893969297], [37.40312919951975, -5.9897383488714695], [37.40312810987234, -5.989704988896847], [37.4031232483685, -5.989676406607032], [37.40312215872109, -5.989646399393678], [37.40312073379755, -5.989608848467469], [37.403117045760155, -5.989573895931244], [37.4031129386276, -5.989542463794351], [37.40311536937952, -5.98950625397265], [37.40311813540757, -5.989471469074488], [37.4031145311892, -5.989435845986009], [37.40311210043728, -5.989396367222071], [37.4031163752079, -5.989364432170987], [37.403119979426265, -5.989324199035764], [37.4031198117882, -5.9892910066992044], [37.40312266163528, -5.989251863211393], [37.40312500856817, -5.989213809370995], [37.40312567912042, -5.989181874319911], [37.40312358364463, -5.989145999774337], [37.403123918920755, -5.9891139809042215], [37.40312702022493, -5.989084141328931], [37.403125343844295, -5.989040723070502], [37.40312710404396, -5.989009458571672], [37.40312660112977, -5.9889729134738445], [37.40312257781625, -5.988935194909573], [37.403118973597884, -5.9888978116214275], [37.40311469882727, -5.988860009238124], [37.403113609179854, -5.988825727254152], [37.40311830304563, -5.988794378936291], [37.40311729721725, -5.98875624127686], [37.40311671048403, -5.988721372559667], [37.40311620756984, -5.988688515499234], [37.40311763249338, -5.988652557134628], [37.403120985254645, -5.988621208816767], [37.40312207490206, -5.98858424462378], [37.4031216558069, -5.988549375906587], [37.403124421834946, -5.988518698140979], [37.40312702022493, -5.988480895757675], [37.4031266849488, -5.988447703421116], [37.40312660112977, -5.988414008170366], [37.40312945097685, -5.988372517749667], [37.403130289167166, -5.988340163603425], [37.40312987007201, -5.9882996790111065], [37.40312710404396, -5.988261373713613], [37.40312702022493, -5.988227007910609], [37.40312316454947, -5.98818845115602], [37.40312207490206, -5.988150732591748], [37.40312207490206, -5.9881156124174595], [37.403116039931774, -5.988077893853188], [37.40310933440924, -5.988043611869216], [37.40310229361057, -5.988007066771388], [37.40309173241258, -5.987969012930989], [37.40308444015682, -5.987935904413462], [37.403074130415916, -5.987900784239173], [37.40306558087468, -5.9878649935126305], [37.40305971354246, -5.9878317173570395], [37.40305091254413, -5.98779265768826], [37.403043285012245, -5.987759381532669], [37.40303532220423, -5.987727614119649], [37.403021240606904, -5.987695511430502], [37.403007075190544, -5.9876644145697355], [37.40299550816417, -5.987634407356381], [37.40298377349973, -5.987605741247535], [37.402969524264336, -5.987576739862561], [37.40295368246734, -5.987545223906636], [37.402942618355155, -5.98751462996006], [37.40292929112911, -5.98748529329896], [37.402913281694055, -5.987456878647208], [37.40290867164731, -5.98743692971766], [37.40289710462093, -5.987405916675925], [37.40289308130741, -5.987377669662237], [37.40288704633713, -5.987346824258566], [37.402868857607245, -5.987310195341706], [37.40285477600992, -5.987280020490289], [37.40284404717386, -5.98725026473403], [37.402829211205244, -5.987212881445885], [37.40281253121793, -5.987184550613165], [37.40279383957386, -5.987157057970762], [37.40277414210141, -5.9871248714625835], [37.40276265889406, -5.987096959725022], [37.402746230363846, -5.987060833722353], [37.40272452123463, -5.987029820680618], [37.40270893089473, -5.986999897286296], [37.40268839523196, -5.986972572281957], [37.402677247300744, -5.9869470074772835], [37.402665512636304, -5.986922532320023], [37.40264640189707, -5.986894704401493], [37.4026311468333, -5.986870396882296], [37.40261111408472, -5.986837958917022], [37.40259485319257, -5.986806359142065], [37.402580101042986, -5.986771993339062], [37.40256501361728, -5.9867406450212], [37.40254858508706, -5.986713571473956], [37.40253542549908, -5.986686246469617], [37.40251857787371, -5.986653054133058], [37.40250307135284, -5.986621538177133], [37.402488151565194, -5.986590776592493], [37.40246920846403, -5.986557751893997], [37.402454456314445, -5.986531348899007], [37.40243182517588, -5.986504862084985], [37.402410535141826, -5.986481057479978], [37.402394441887736, -5.986457923427224], [37.40237751044333, -5.986426826566458], [37.40235907025635, -5.9863989148288965], [37.402345994487405, -5.986367482692003], [37.40232361480594, -5.986338313668966], [37.402310874313116, -5.986311323940754], [37.402287824079394, -5.986284669488668], [37.40226938389242, -5.986255165189505], [37.40225387737155, -5.986228929832578], [37.40222982130945, -5.986202945932746], [37.40220903418958, -5.9861786384135485], [37.402193108573556, -5.986153408885002], [37.402174500748515, -5.986122647300363], [37.402160754427314, -5.986093059182167], [37.40213871002197, -5.986066740006208], [37.402116833254695, -5.9860387444496155], [37.40210032090545, -5.986012509092689], [37.402078779414296, -5.985987698659301], [37.40205363370478, -5.985966492444277], [37.40203745663166, -5.985949309542775], [37.40201549604535, -5.9859321266412735], [37.401994625106454, -5.985912261530757], [37.401977609843016, -5.985890217125416], [37.401954643428326, -5.98586943000555], [37.40193176083267, -5.985853085294366], [37.40191055461764, -5.985830118879676], [37.40188649855554, -5.985805811360478], [37.401867136359215, -5.985782761126757], [37.40184492431581, -5.985758453607559], [37.40181986242533, -5.985737834125757], [37.40179806947708, -5.985720902681351], [37.40177451632917, -5.985699947923422], [37.40175196900964, -5.985683435574174], [37.401729840785265, -5.985667174682021], [37.4017037730664, -5.985648734495044], [37.40168105810881, -5.985634066164494], [37.40165641531348, -5.985616212710738], [37.40162850357592, -5.9855966828763485], [37.40160436369479, -5.985583355650306], [37.401576368138194, -5.98556843586266], [37.40154929459095, -5.985558042302728], [37.40152255631983, -5.985549911856651], [37.40149606950581, -5.9855348244309425], [37.40147494710982, -5.985523676499724], [37.401450388133526, -5.9855118580162525], [37.40142515860498, -5.985495932400227], [37.40140160545707, -5.98547950387001], [37.40137252025306, -5.985457627102733], [37.40134930238128, -5.985442623496056], [37.40132491104305, -5.98542770370841], [37.40129456855357, -5.985409850254655], [37.40126933902502, -5.985394846647978], [37.401246624067426, -5.985380262136459], [37.401223657652736, -5.985366851091385], [37.40120186470449, -5.985358469188213], [37.40117638371885, -5.985358636826277], [37.401148891076446, -5.985349249094725], [37.40112843923271, -5.9853369276970625], [37.401102455332875, -5.985321924090385], [37.40107814781368, -5.985323768109083], [37.401056522503495, -5.985320415347815], [37.40102986805141, -5.9853127878159285], [37.401008158922195, -5.985303316265345], [37.40098301321268, -5.9852915816009045], [37.400957280769944, -5.9852768294513226], [37.40093817003071, -5.98526157438755], [37.40091470070183, -5.98524447530508], [37.400889387354255, -5.985230728983879], [37.40086717531085, -5.985216312110424], [37.400837587192655, -5.985201811417937], [37.400814117863774, -5.985179431736469], [37.40079450421035, -5.985164763405919], [37.40076910704374, -5.985143054276705], [37.40074664354324, -5.985122853890061], [37.40072443149984, -5.985102066770196], [37.40070020779967, -5.985085889697075], [37.400680761784315, -5.985068455338478], [37.40065603516996, -5.985053116455674], [37.400630470365286, -5.985035179182887], [37.40060959942639, -5.985016236081719], [37.40058235824108, -5.984992096200585], [37.400558637455106, -5.984969548881054], [37.40054044872522, -5.984953120350838], [37.40051723085344, -5.984930070117116], [37.40049971267581, -5.984906265512109], [37.400482362136245, -5.984886232763529], [37.400465849787, -5.984858823940158], [37.40045143291354, -5.984834097325802], [37.40042980760336, -5.984817165881395], [37.40040751174092, -5.984798138961196], [37.400390077382326, -5.984779028221965], [37.400367613881826, -5.98475681617856], [37.40034271962941, -5.984734101220965], [37.40032318979502, -5.984711553901434], [37.40029754117131, -5.984683642163873], [37.40027801133692, -5.984660591930151], [37.40025454200804, -5.9846345242112875], [37.40023199468851, -5.98460678011179], [37.40021539852023, -5.984579790383577], [37.40019075572491, -5.984558332711458], [37.40016946569085, -5.984539641067386], [37.40014876239002, -5.984516590833664], [37.4001360218972, -5.984490774571896], [37.40011716261506, -5.984464790672064], [37.40009838715196, -5.984440818428993], [37.40008112043142, -5.9844068717211485], [37.40007005631924, -5.984377870336175], [37.40004641935229, -5.984351970255375], [37.40002915263176, -5.9843232203274965], [37.40001976490021, -5.984292542561889], [37.40000333636999, -5.984258763492107], [37.39998900331557, -5.984231270849705], [37.39997592754662, -5.98420369438827], [37.39995840936899, -5.984174776822329], [37.39994323812425, -5.984148792922497], [37.399928821250796, -5.984121719375253], [37.39991616457701, -5.984088275581598], [37.399904765188694, -5.984062040224671], [37.39988557063043, -5.984029266983271], [37.399870064109564, -5.98399892449379], [37.39986310712993, -5.983969587832689], [37.39984625950456, -5.983936311677098], [37.39983460865915, -5.983909321948886], [37.399821700528264, -5.9838788118213415], [37.39980686455965, -5.983842182904482], [37.39979152567685, -5.983807146549225], [37.39977249875665, -5.983781414106488], [37.39975548349321, -5.983752747997642], [37.39974852651358, -5.983722656965256], [37.399734780192375, -5.9836861956864595], [37.399726901203394, -5.98365786485374], [37.3997163400054, -5.98362903110683], [37.3997057788074, -5.98359483294189], [37.39969630725682, -5.983571615070105], [37.39967929199338, -5.983544625341892], [37.39966755732894, -5.983521239832044], [37.39965624175966, -5.983492070809007], [37.39963696338236, -5.983462315052748], [37.399619948118925, -5.983429877087474], [37.399609135463834, -5.983400121331215], [37.399593126028776, -5.983367767184973], [37.39957518875599, -5.9833363350480795], [37.399564208462834, -5.983310267329216], [37.3995469417423, -5.983275398612022], [37.39953453652561, -5.983244637027383], [37.39952255040407, -5.983213288709521], [37.39950737915933, -5.983180180191994], [37.39949547685683, -5.983152939006686], [37.399481646716595, -5.983120920136571], [37.399466559290886, -5.983089739456773], [37.39945566281676, -5.983061492443085], [37.39943940192461, -5.9830324072390795], [37.39942573942244, -5.9830032382160425], [37.39941836334765, -5.982973398640752], [37.399406461045146, -5.98294205032289], [37.399393720552325, -5.9829154796898365], [37.39938139915466, -5.982885891571641], [37.39936463534832, -5.982848256826401], [37.39935424178839, -5.982814058661461], [37.399339992552996, -5.982778687030077], [37.39932540804148, -5.9827446565032005], [37.39931191317737, -5.982716577127576], [37.39929288625717, -5.982682127505541], [37.39928148686886, -5.9826539643108845], [37.39927327260375, -5.982629153877497], [37.39925508387387, -5.982597218826413], [37.39924284629524, -5.982566624879837], [37.3992223944515, -5.982539132237434], [37.399204540997744, -5.982512980699539], [37.39918970502913, -5.982485068961978], [37.399177299812436, -5.982453301548958], [37.39916858263314, -5.982422037050128], [37.3991565965116, -5.982392197474837], [37.39914176054299, -5.982361771166325], [37.39913161844015, -5.982334362342954], [37.399118542671204, -5.982307959347963], [37.39910454489291, -5.982280131429434], [37.39909339696169, -5.982255069538951], [37.399071687832475, -5.982229420915246], [37.39905718713999, -5.982199748978019], [37.39903824403882, -5.98217393271625], [37.39902072586119, -5.9821488708257675], [37.39900597371161, -5.9821229707449675], [37.39898610860109, -5.982097238302231], [37.39896741695702, -5.982074020430446], [37.39894788712263, -5.98204929381609], [37.39892282523215, -5.982030518352985], [37.39890530705452, -5.982014508917928], [37.398880664259195, -5.981993051245809], [37.39885602146387, -5.981971509754658], [37.39883590489626, -5.981952650472522], [37.39881352521479, -5.9819338750094175], [37.39878997206688, -5.981916021555662], [37.398772621527314, -5.981894480064511], [37.398747727274895, -5.981875117868185], [37.39872417412698, -5.98185483366251], [37.398704309016466, -5.981834214180708], [37.39867773838341, -5.981809571385384], [37.398655945435166, -5.981796914711595], [37.398631470277905, -5.981786018237472], [37.398608922958374, -5.9817710146307945], [37.39858964458108, -5.981751652434468], [37.39856332540512, -5.9817297756671906], [37.39853826351464, -5.981705132871866], [37.398514710366726, -5.981681998819113], [37.39849484525621, -5.981666827574372], [37.398473555222154, -5.981652243062854], [37.398448660969734, -5.981638832017779], [37.39842309616506, -5.981625337153673], [37.398401722311974, -5.981613351032138], [37.398376828059554, -5.981606813147664], [37.39835076034069, -5.9816040471196175], [37.39832838065922, -5.9815996047109365], [37.398300217464566, -5.9815963357687], [37.39827314391732, -5.981590049341321], [37.39824607037008, -5.981581835076213], [37.39821790717542, -5.981574123725295], [37.398194186389446, -5.9815669152885675], [37.39816795103252, -5.981561467051506], [37.39814322441816, -5.981548726558685], [37.39812000654638, -5.981538332998753], [37.39809268154204, -5.981529951095581], [37.398067032918334, -5.981520898640156], [37.39804188720882, -5.981512684375048], [37.39801154471934, -5.9815030451864], [37.39798430353403, -5.98149424418807], [37.39795722998679, -5.9814871195703745], [37.397928312420845, -5.98148200660944], [37.397907776758075, -5.981472618877888], [37.39787785336375, -5.981467505916953], [37.3978528752923, -5.981462728232145], [37.397830076515675, -5.981457615271211], [37.39780392497778, -5.98145417869091], [37.39777752198279, -5.981452167034149], [37.39774877205491, -5.9814514964818954], [37.39772203378379, -5.981452334672213], [37.397699151188135, -5.981451747938991], [37.39767191000283, -5.981454597786069], [37.397645339369774, -5.9814526699483395], [37.39762170240283, -5.981453089043498], [37.39759102463722, -5.981456022709608], [37.39756202325225, -5.981453759595752], [37.39753411151469, -5.981452334672213], [37.39750678651035, -5.981455519795418], [37.39748474210501, -5.981452334672213], [37.39746144041419, -5.981451999396086], [37.39743637852371, -5.981452334672213], [37.39740896970034, -5.981452250853181], [37.39738239906728, -5.981457866728306], [37.39735775627196, -5.981460548937321], [37.397334622219205, -5.981465494260192], [37.39730930887163, -5.981477648019791], [37.39728600718081, -5.981482090428472], [37.39726044237614, -5.98149080760777], [37.39723806269467, -5.981507068499923], [37.39721073769033, -5.981519557535648], [37.39717746153474, -5.981534728780389], [37.39715759642422, -5.981542943045497], [37.39712960086763, -5.981555934995413], [37.39710185676813, -5.981580829247832], [37.39708031527698, -5.981586864218116], [37.39705257117748, -5.981600945815444], [37.39703002385795, -5.981618883088231], [37.397012589499354, -5.981626342982054], [37.39698643796146, -5.98164620809257], [37.396963806822896, -5.981664480641484], [37.396941762417555, -5.98167940042913], [37.396914856508374, -5.9817050490528345], [37.39689205773175, -5.981715526431799], [37.396868001669645, -5.98173463717103], [37.39684612490237, -5.981762716546655], [37.396827852353454, -5.981779647991061], [37.39680186845362, -5.981796244159341], [37.39677932113409, -5.981816528365016], [37.39675928838551, -5.98182650282979], [37.396731628105044, -5.981847457587719], [37.39670941606164, -5.981869166716933], [37.396687623113394, -5.981886936351657], [37.3966596275568, -5.981910740956664], [37.396636912599206, -5.981929348781705], [37.396611515432596, -5.981945609673858], [37.396585531532764, -5.98196379840374], [37.396563990041614, -5.981975952163339], [37.396540185436606, -5.981995230540633], [37.39651554264128, -5.9820140060037374], [37.396493246778846, -5.982025824487209], [37.396468268707395, -5.982051640748978], [37.396444380283356, -5.9820652194321156], [37.39642024040222, -5.982070500031114], [37.39639475941658, -5.982089275494218], [37.3963696975261, -5.982102183625102], [37.396344719454646, -5.982117773965001], [37.39632334560156, -5.982140153646469], [37.3963020555675, -5.982153648510575], [37.39627540111542, -5.982174435630441], [37.39625059068203, -5.982194971293211], [37.39622242748737, -5.98220519721508], [37.39619350992143, -5.982222380116582], [37.39616886712611, -5.982234450057149], [37.396144811064005, -5.982249621301889], [37.39612025208771, -5.982270073145628], [37.39609720185399, -5.982292452827096], [37.39607633091509, -5.982307372614741], [37.39604875445366, -5.98233126103878], [37.39602880552411, -5.982348192483187], [37.39600516855717, -5.982363782823086], [37.39597784355283, -5.9823850728571415], [37.395953787490726, -5.982401082292199], [37.39592713303864, -5.98242212086916], [37.395900981500745, -5.9824413154274225], [37.39587843418121, -5.9824597556144], [37.39585429430008, -5.982486493885517], [37.395832082256675, -5.982501162216067], [37.395808612927794, -5.982521194964647], [37.39578438922763, -5.982546340674162], [37.395762177184224, -5.982562769204378], [37.39573703147471, -5.982585903257132], [37.3957097902894, -5.982611300423741], [37.39568338729441, -5.982634099200368], [37.39565723575652, -5.98265228793025], [37.39563016220927, -5.982668465003371], [37.39560208283365, -5.982691263779998], [37.39557769149542, -5.982711296528578], [37.39555631764233, -5.9827278926968575], [37.395534524694085, -5.982746919617057], [37.39550929516554, -5.982763348147273], [37.3954854067415, -5.982780531048775], [37.39546277560294, -5.982796289026737], [37.39543997682631, -5.98280911333859], [37.39541784860194, -5.982825709506869], [37.395392870530486, -5.982839874923229], [37.39537090994418, -5.982849849388003], [37.39535070955753, -5.982868205755949], [37.39532916806638, -5.982886394485831], [37.395310224965215, -5.982896704226732], [37.395289773121476, -5.982913468033075], [37.395269740372896, -5.9829287230968475], [37.395246773958206, -5.982941966503859], [37.39522447809577, -5.9829578921198845], [37.39520008675754, -5.982969040051103], [37.395176365971565, -5.982979433611035], [37.395150884985924, -5.9829929284751415], [37.395129511132836, -5.9830003045499325], [37.395109394565225, -5.983012290671468], [37.39508341066539, -5.983029725030065], [37.39506094716489, -5.983039531856775], [37.39503521472216, -5.983049003407359], [37.39500747062266, -5.983061995357275], [37.39498173817992, -5.983072388917208], [37.39495617337525, -5.983090493828058], [37.394930021837354, -5.983110694214702], [37.39490839652717, -5.98312821239233], [37.394884172827005, -5.983146820217371], [37.39485885947943, -5.983165092766285], [37.394836228340864, -5.983180934563279], [37.394813848659396, -5.983202811330557], [37.394793815910816, -5.983220916241407], [37.39477705210447, -5.983238434419036], [37.39475551061332, -5.98326207138598], [37.39473883062601, -5.983279086649418], [37.394719552248716, -5.983296437188983], [37.394697507843375, -5.983320996165276], [37.394674960523844, -5.983341867104173], [37.39465174265206, -5.983362905681133], [37.394628105685115, -5.9833834413439035], [37.39460740238428, -5.983401630073786], [37.39458443596959, -5.983421076089144], [37.39456373266876, -5.983433984220028], [37.39453950896859, -5.983448317274451], [37.39451696164906, -5.983468936756253], [37.3944959230721, -5.98348980769515], [37.39447496831417, -5.983510427176952], [37.394449738785625, -5.983530543744564], [37.39442517980933, -5.983546553179622], [37.394400453194976, -5.9835634008049965], [37.39437480457127, -5.983577566221356], [37.394350580871105, -5.983587875962257], [37.39432392641902, -5.983598604798317], [37.394295847043395, -5.983606735244393], [37.39426977932453, -5.983615117147565], [37.39424077793956, -5.983625091612339], [37.39421336911619, -5.9836317133158445], [37.3941885586828, -5.98363833501935], [37.39416332915425, -5.983649818226695], [37.394140949472785, -5.983659205958247], [37.394120413810015, -5.983667336404324], [37.39409493282437, -5.9836803283542395], [37.394072553142905, -5.983683345839381], [37.394048077985644, -5.98368770442903], [37.39401957951486, -5.983699439093471], [37.39399258978665, -5.9837050549685955], [37.39396568387747, -5.9837118443101645], [37.39393651485443, -5.9837239142507315], [37.39391388371587, -5.983726345002651], [37.39388773217797, -5.9837336372584105], [37.39386225119233, -5.983745791018009], [37.39383987151086, -5.983751658350229], [37.39381137304008, -5.983754592016339], [37.39378455094993, -5.9837566036731005], [37.393756890669465, -5.983762135729194], [37.39373065531254, -5.983771439641714], [37.393709952011704, -5.983776887878776], [37.3936861474067, -5.983783174306154], [37.39366091787815, -5.983790382742882], [37.39363552071154, -5.983793567866087], [37.393613224849105, -5.983799435198307], [37.39358673803508, -5.983800692483783], [37.39356209523976, -5.983801279217005], [37.393534099683166, -5.983801111578941], [37.393503757193685, -5.983802787959576], [37.393475929275155, -5.983803290873766], [37.39344634115696, -5.98380689509213], [37.393423626199365, -5.98380739800632], [37.39339747466147, -5.983821228146553], [37.39337341859937, -5.983829358592629], [37.393347853794694, -5.983835309743881], [37.39331943914294, -5.98384746350348], [37.393288258463144, -5.983854336664081], [37.39326160401106, -5.983863137662411], [37.39322757348418, -5.9838799852877855], [37.39319848828018, -5.983894402161241], [37.39317485131323, -5.983908651396632], [37.393149537965655, -5.983947878703475], [37.393131433054805, -5.983987357467413], [37.393118776381016, -5.984027925878763], [37.39311559125781, -5.984070422127843], [37.393116261810064, -5.984106380492449], [37.39312003366649, -5.984143177047372], [37.39313067868352, -5.984184332191944], [37.39313646219671, -5.984219368547201], [37.393152387812734, -5.984257170930505], [37.39316018298268, -5.98429405130446], [37.39316898398101, -5.984327914193273], [37.393183233216405, -5.9843632858246565], [37.393193962052464, -5.984396645799279], [37.393202846869826, -5.984425563365221], [37.393215922638774, -5.984458504244685], [37.3932249750942, -5.98449002020061], [37.39323151297867, -5.984520949423313], [37.3932437505573, -5.9845496993511915], [37.39325498230755, -5.9845714922994375], [37.39326369948685, -5.984606109559536], [37.393273590132594, -5.984640391543508], [37.393278451636434, -5.984672326594591], [37.39328557625413, -5.9847131464630365], [37.39328800700605, -5.984752960503101], [37.393285827711225, -5.98478564992547], [37.39328515715897, -5.984823703765869], [37.39328448660672, -5.984848011285067], [37.39328909665346, -5.984875503927469], [37.393297059461474, -5.98490878008306], [37.393300076946616, -5.984940715134144], [37.393300496041775, -5.984968962147832], [37.39330468699336, -5.98500651307404], [37.39330166950822, -5.985041297972202], [37.39329722709954, -5.985074490308762], [37.39329303614795, -5.985112963244319], [37.393281385302544, -5.985141796991229], [37.39326763898134, -5.985176498070359], [37.3932559043169, -5.985210696235299], [37.393245259299874, -5.985237518325448], [37.393233021721244, -5.985278002917767], [37.39322413690388, -5.985311279073358], [37.39321139641106, -5.985336089506745], [37.393200332298875, -5.98537489771843], [37.393188178539276, -5.985397193580866], [37.393179880455136, -5.985425021499395], [37.393171079456806, -5.985462488606572], [37.39316252991557, -5.985493250191212], [37.393156914040446, -5.985524766147137], [37.39315154962242, -5.9855651669204235], [37.39314668811858, -5.985595006495714], [37.39313847385347, -5.9856250975281], [37.393127074465156, -5.985657786950469], [37.393115339800715, -5.985683184117079], [37.39310561679304, -5.985711347311735], [37.39309220574796, -5.985743114724755], [37.39308558404446, -5.9857710264623165], [37.39307904615998, -5.985811511054635], [37.393080554902554, -5.985850403085351], [37.39308415912092, -5.985890803858638], [37.393078124150634, -5.985929863527417], [37.39307066425681, -5.985955512151122], [37.3930642940104, -5.985991219058633], [37.39305255934596, -5.986024662852287], [37.393041076138616, -5.986045701429248], [37.39303009584546, -5.986076965928078], [37.39301836118102, -5.98610638640821], [37.39300671033561, -5.986128598451614], [37.39300260320306, -5.986164892092347], [37.39299355074763, -5.986185763031244], [37.39298525266349, -5.986212082207203], [37.39298567175865, -5.986240832135081], [37.39298617467284, -5.98626765422523], [37.39299036562443, -5.98630528897047], [37.392997993156314, -5.986339068040252], [37.39300620742142, -5.98637267947197], [37.39301827736199, -5.986408302560449], [37.39302800036967, -5.986439231783152], [37.393036130815744, -5.986468987539411], [37.39304618909955, -5.986508131027222], [37.39304535090923, -5.9865403175354], [37.393056666478515, -5.986559931188822], [37.39307468757033, -5.986576694995165], [37.39309145137668, -5.986589686945081], [37.39310628734529, -5.986608881503344], [37.393120704218745, -5.986628998070955], [37.393132438883185, -5.986649449914694], [37.393146520480514, -5.98667224869132], [37.393159847706556, -5.986699992790818], [37.39317233674228, -5.986726228147745], [37.39319513551891, -5.986750954762101], [37.393211647868156, -5.986780459061265], [37.393219862133265, -5.986807867884636], [37.393225729465485, -5.98684910684824], [37.39322405308485, -5.986882885918021], [37.39321609027684, -5.986917000263929], [37.393208127468824, -5.986951952800155], [37.39320142194629, -5.986978439614177], [37.3931919503957, -5.987010207027197], [37.39318449050188, -5.987048847600818], [37.39318013191223, -5.987078687176108], [37.39316923543811, -5.987114394083619], [37.393161021173, -5.987148676067591], [37.39315305836499, -5.987181533128023], [37.39314258098602, -5.9872164856642485], [37.39313126541674, -5.98724733106792], [37.39312003366649, -5.987277254462242], [37.393110897392035, -5.987319415435195], [37.3931007552892, -5.987348249182105], [37.39309153519571, -5.987380435690284], [37.39307879470289, -5.987414717674255], [37.393067982047796, -5.987443970516324], [37.39305775612593, -5.98747406154871], [37.393048871308565, -5.987507672980428], [37.39303361624479, -5.9875404462218285], [37.39302531816065, -5.9875795897096395], [37.39301634952426, -5.987610183656216], [37.3930074647069, -5.987644633278251], [37.392996568232775, -5.98767782561481], [37.39298642612994, -5.98770615644753], [37.392977541312575, -5.987747814506292], [37.39297645166516, -5.987777654081583], [37.39296991378069, -5.987815707921982], [37.39296304062009, -5.987849989905953], [37.39295482635498, -5.987880500033498], [37.392947785556316, -5.987913189455867], [37.392937391996384, -5.987945711240172], [37.392927249893546, -5.987976640462875], [37.39292297512293, -5.988013939931989], [37.39292196929455, -5.988047802820802], [37.3929139226675, -5.988079654052854], [37.39290403202176, -5.988112427294254], [37.392897410318255, -5.988143943250179], [37.392894392833114, -5.988180739805102], [37.39288793876767, -5.98821728490293], [37.392887603491545, -5.988246453925967], [37.392884166911244, -5.9882802329957485], [37.392880311235785, -5.988313090056181], [37.39287402480841, -5.988347539678216], [37.39287067204714, -5.988380648195744], [37.39286455325782, -5.988410068675876], [37.392858853563666, -5.9884430933743715], [37.39286782220006, -5.988489277660847], [37.39287385717034, -5.988527834415436], [37.39286966621876, -5.988564798608422], [37.39286145195365, -5.988600002601743], [37.39284795708954, -5.988629087805748], [37.39283060654998, -5.988663705065846], [37.392817446962, -5.988694382831454], [37.39280470646918, -5.988727323710918], [37.39279456436634, -5.9887679759413], [37.39278333261609, -5.988798318430781], [37.39276765845716, -5.988832768052816], [37.392754163593054, -5.988870905712247], [37.39274033345282, -5.988900493830442], [37.39273144863546, -5.9889361169189215], [37.39272155798972, -5.988970985636115], [37.39271158352494, -5.989001747220755], [37.39270118996501, -5.989040723070502], [37.39269347861409, -5.989070730283856], [37.39268836565316, -5.9890978038311005], [37.39267629571259, -5.989133007824421], [37.39266850054264, -5.989160584285855], [37.39266104064882, -5.989189920946956], [37.392650144174695, -5.989227304235101], [37.39263673312962, -5.989252617582679], [37.39262449555099, -5.989284887909889], [37.39261016249657, -5.989320008084178], [37.39260144531727, -5.98935286514461], [37.392591554671526, -5.989384213462472], [37.392582166939974, -5.989412376657128], [37.39257847890258, -5.989441964775324], [37.392570935189724, -5.989477001130581], [37.392557272687554, -5.989502565935254], [37.39254646003246, -5.989528214558959], [37.3925444483757, -5.9895640052855015], [37.39253707230091, -5.989594012498856], [37.39253003150225, -5.989620583131909], [37.39252148196101, -5.989653775468469], [37.39251444116235, -5.989679340273142], [37.39250807091594, -5.98971001803875], [37.39250371232629, -5.989748239517212], [37.39249398931861, -5.989777073264122], [37.39248275756836, -5.98981655202806], [37.392480075359344, -5.9898564498871565], [37.39247940480709, -5.989895341917872], [37.39248686470091, -5.98994311876595], [37.39248778671026, -5.989987039938569], [37.3924839310348, -5.990021573379636], [37.39248585887253, -5.990063063800335], [37.392488960176706, -5.990098938345909], [37.39249206148088, -5.990133639425039], [37.39250035956502, -5.990172531455755], [37.392508406192064, -5.990201197564602], [37.39252106286585, -5.990237491205335], [37.392534893006086, -5.990273281931877], [37.39255006425083, -5.990302870050073], [37.392561212182045, -5.990340420976281], [37.3925687558949, -5.990376714617014], [37.39257873035967, -5.9904102422297], [37.39258836954832, -5.990445194765925], [37.392599852755666, -5.990474196150899], [37.39261083304882, -5.990504790097475], [37.39261904731393, -5.990541921928525], [37.39262315444648, -5.990577712655067], [37.39262843504548, -5.99061693996191], [37.392640337347984, -5.990652982145548], [37.39264704287052, -5.99067896604538], [37.39265240728855, -5.9907114040106535], [37.39265902899206, -5.990739231929183], [37.392663890495896, -5.99076253362], [37.39267227239907, -5.99080310203135], [37.392679397016764, -5.990837551653385], [37.39268308505416, -5.9908713307231665], [37.39269046112895, -5.990907624363899], [37.3926982562989, -5.990942409262061], [37.392699932679534, -5.990980127826333], [37.39270118996501, -5.9910244680941105], [37.39269783720374, -5.991057576611638], [37.39270018413663, -5.991094205528498], [37.39270035177469, -5.99112706258893], [37.392699932679534, -5.9911552257835865], [37.39270722493529, -5.991188921034336], [37.39270848222077, -5.991219179704785], [37.39270806312561, -5.991256143897772], [37.392709320411086, -5.9913036692887545], [37.392708230763674, -5.991339543834329], [37.392713595181704, -5.991382375359535], [37.39271502010524, -5.9914173278957605], [37.39271434955299, -5.9914550464600325], [37.39271694794297, -5.991493184119463], [37.392724407836795, -5.9915268793702126], [37.39272843115032, -5.991549594327807], [37.39274268038571, -5.991580355912447], [37.39274184219539, -5.9916105307638645], [37.39273924380541, -5.9916484169662], [37.392741087824106, -5.991688147187233], [37.39273647777736, -5.99171438254416], [37.39274066872895, -5.991753861308098], [37.39274628460407, -5.991791244596243], [37.39274796098471, -5.991817060858011], [37.392749888822436, -5.99184975028038], [37.392753241583705, -5.991878919303417], [37.39275290630758, -5.99190448410809], [37.39275055937469, -5.99193767644465], [37.39275064319372, -5.991971623152494], [37.39274863153696, -5.992007497698069], [37.392751313745975, -5.99204421043396], [37.392754666507244, -5.992074217647314], [37.392755253240466, -5.992106823250651], [37.39276237785816, -5.992141608148813], [37.392765982076526, -5.992178823798895], [37.39276899956167, -5.992212351411581], [37.392778722569346, -5.9922510758042336], [37.392782997339964, -5.9922820050269365], [37.39278232678771, -5.992309665307403], [37.3927846737206, -5.992338499054313], [37.39278953522444, -5.992364231497049], [37.392791798338294, -5.992393819615245], [37.39279858767986, -5.992429442703724], [37.39279909059405, -5.992456264793873], [37.39280286245048, -5.992484092712402], [37.392809903249145, -5.992521978914738], [37.39281074143946, -5.9925532434135675], [37.39281367510557, -5.992584256455302], [37.392815351486206, -5.9926218912005424], [37.392817279323936, -5.992656173184514], [37.39282138645649, -5.992696406319737], [37.3928243201226, -5.992731610313058], [37.39282582886517, -5.992768490687013], [37.39282716996968, -5.992804784327745], [37.3928277567029, -5.992835378274322], [37.39282742142677, -5.99286999553442], [37.392825493589044, -5.992902517318726], [37.392823146656156, -5.99293009378016], [37.39282180555165, -5.992961777374148], [37.39282230846584, -5.992992287501693], [37.39282289519906, -5.993025479838252], [37.39282524213195, -5.993061773478985], [37.392826080322266, -5.993091780692339], [37.392825493589044, -5.993123380467296], [37.39282733760774, -5.993156069889665], [37.39282733760774, -5.993188424035907], [37.39283010363579, -5.993221867829561], [37.39283018745482, -5.993255982175469], [37.39282884635031, -5.993293700739741], [37.39283077418804, -5.993337873369455], [37.39283102564514, -5.993371065706015], [37.39282926544547, -5.993398809805512], [37.39282566122711, -5.993429319933057], [37.39282574504614, -5.993452537804842], [37.39282767288387, -5.993487406522036], [37.392828511074185, -5.9935221914201975], [37.392827002331614, -5.993553623557091], [37.39283077418804, -5.993593102321029], [37.39283060654998, -5.993622438982129], [37.392827002331614, -5.99365227855742], [37.39282876253128, -5.993682201951742], [37.39283085800707, -5.9937097784131765], [37.39283027127385, -5.993741545826197], [37.39282633177936, -5.993778929114342], [37.392827002331614, -5.993810361251235], [37.39282331429422, -5.993842212483287], [37.392821134999394, -5.993880853056908], [37.39282188937068, -5.993916811421514], [37.39281937479973, -5.993954027071595], [37.39282046444714, -5.993992751464248], [37.39281962625682, -5.9940246026962996], [37.3928190395236, -5.994066093116999], [37.3928156029433, -5.994103644043207], [37.392809400334954, -5.994139602407813], [37.3928071372211, -5.994179667904973], [37.39280319772661, -5.994219146668911], [37.39280101843178, -5.994256446138024], [37.39279883913696, -5.994291063398123], [37.39280244335532, -5.994322411715984], [37.39280479028821, -5.994356106966734], [37.39280621521175, -5.994393825531006], [37.39281149581075, -5.994431376457214], [37.39281099289656, -5.994474124163389], [37.39280965179205, -5.994519721716642], [37.39280537702143, -5.994565486907959], [37.39279322326183, -5.994613850489259], [37.39279028959572, -5.994652071967721], [37.39278710447252, -5.994692975655198], [37.39278048276901, -5.994729353114963], [37.392777632921934, -5.994765982031822], [37.392774783074856, -5.994808226823807], [37.3927706759423, -5.99484502337873], [37.39276573061943, -5.994880814105272], [37.39276807755232, -5.994922304525971], [37.392767407000065, -5.994960023090243], [37.392766904085875, -5.99499749019742], [37.39276237785816, -5.995035879313946], [37.39275290630758, -5.995066473260522], [37.39274469204247, -5.995106203481555], [37.392737064510584, -5.995140904560685], [37.39272432401776, -5.9951729234308], [37.39270940423012, -5.9952108934521675], [37.39269289188087, -5.99523738026619], [37.39267445169389, -5.995275937020779], [37.39265576004982, -5.99530678242445], [37.392636900767684, -5.9953248873353004], [37.392619801685214, -5.9953478537499905], [37.392600774765015, -5.995364114642143], [37.39258099347353, -5.995376016944647], [37.392554422840476, -5.995382638648152], [37.3925291094929, -5.995391607284546], [37.39250471815467, -5.995396468788385], [37.39247898571193, -5.995397642254829], [37.39245040342212, -5.995402839034796], [37.39242173731327, -5.995402587577701], [37.392394579946995, -5.995400995016098], [37.392367674037814, -5.995399318635464], [37.392342863604426, -5.995401246473193], [37.392313443124294, -5.995400492101908], [37.39229089580476, -5.9954120591282845], [37.39226834848523, -5.995418094098568], [37.392246471717954, -5.995430750772357], [37.392226438969374, -5.99543628282845], [37.39220422692597, -5.99544215016067], [37.3921749740839, -5.995451286435127], [37.39215049892664, -5.995453046634793], [37.392119988799095, -5.995460087433457], [37.392088724300265, -5.995466457679868], [37.39206098020077, -5.99546704441309], [37.39203038625419, -5.995465870946646], [37.39199903793633, -5.995460841804743], [37.391971880570054, -5.995451956987381], [37.39193785004318, -5.995446760207415], [37.391908345744014, -5.995447179302573], [37.3918832000345, -5.995444329455495], [37.39185528829694, -5.995438797399402], [37.391823437064886, -5.995436450466514], [37.39179376512766, -5.9954361990094185], [37.391762249171734, -5.995434187352657], [37.39173551090062, -5.995430080220103], [37.39170583896339, -5.995423877611756], [37.391675831750035, -5.99541331641376], [37.391650937497616, -5.995400073006749], [37.39161992445588, -5.99538934417069], [37.39159536547959, -5.995376603677869], [37.39156418479979, -5.995375011116266], [37.39153543487191, -5.995374675840139], [37.391508696600795, -5.995368305593729], [37.39147801883519, -5.995362773537636], [37.39145069383085, -5.995363360270858], [37.391419764608145, -5.995359839871526], [37.39138732664287, -5.995355481281877], [37.391363522037864, -5.995349362492561], [37.391333347186446, -5.995346596464515], [37.3913097102195, -5.995350033044815], [37.391279535368085, -5.995351038873196], [37.391249695792794, -5.995360091328621], [37.39122203551233, -5.995367299765348], [37.39118783734739, -5.995375011116266], [37.39116252399981, -5.995374256744981], [37.391130086034536, -5.99536688067019], [37.39110452122986, -5.9953581634908915], [37.391076777130365, -5.995357660576701], [37.391042076051235, -5.995360929518938], [37.39100435748696, -5.995360175147653], [37.39097720012069, -5.995357995852828], [37.39094853401184, -5.995349697768688], [37.39091978408396, -5.995340058580041], [37.3908900283277, -5.99532974883914], [37.39085851237178, -5.9953186847269535], [37.39083261229098, -5.995311476290226], [37.390806544572115, -5.995307872071862], [37.39078097976744, -5.995302507653832], [37.39075952209532, -5.995294461026788], [37.390736220404506, -5.9952756855636835], [37.390711745247245, -5.99526378326118], [37.390686767175794, -5.995254144072533], [37.39065793342888, -5.995242828503251], [37.39063178189099, -5.99523251876235], [37.390605378896, -5.995222879573703], [37.39057327620685, -5.995210977271199], [37.39054477773607, -5.995201421901584], [37.39051753655076, -5.99518951959908], [37.39048719406128, -5.995188262313604], [37.39045794121921, -5.995184574276209], [37.39042508415878, -5.995174180716276], [37.390397507697344, -5.99516361951828], [37.39036783576012, -5.995153309777379], [37.390338499099016, -5.995144676417112], [37.39031477831304, -5.995137467980385], [37.39028460346162, -5.9951340314000845], [37.39025895483792, -5.995133277028799], [37.39023271948099, -5.995129086077213], [37.39020396955311, -5.995123470202088], [37.39017798565328, -5.995117854326963], [37.390144960954785, -5.995118105784059], [37.39011495374143, -5.995117267593741], [37.39008645527065, -5.995116429403424], [37.39005535840988, -5.995103353634477], [37.39003038033843, -5.995091954246163], [37.390001295134425, -5.995076699182391], [37.389968018978834, -5.995070831850171], [37.38994010724127, -5.995062617585063], [37.38990414887667, -5.995057839900255], [37.389873806387186, -5.995053146034479], [37.38984505645931, -5.9950434230268], [37.389815635979176, -5.995031436905265], [37.38979082554579, -5.995026575401425], [37.3897625785321, -5.995021881535649], [37.389735504984856, -5.9950141701847315], [37.38970994018018, -5.995008051395416], [37.38967649638653, -5.995004950091243], [37.38964548334479, -5.994997825473547], [37.38961732015014, -5.994994975626469], [37.389584043994546, -5.994993215426803], [37.38955940119922, -5.994981313124299], [37.389529813081026, -5.9949711710214615], [37.38949729129672, -5.994960693642497], [37.389467200264335, -5.994957089424133], [37.38943878561258, -5.994955496862531], [37.3894068505615, -5.994954239577055], [37.38938455469906, -5.994955413043499], [37.389356307685375, -5.99495105445385], [37.38932814449072, -5.994948288425803], [37.38930190913379, -5.994941834360361], [37.389274248853326, -5.994934039190412], [37.38924943841994, -5.994937559589744], [37.389226388186216, -5.994936805218458], [37.389197554439306, -5.9949363861233], [37.38916905596852, -5.994937811046839], [37.3891444131732, -5.994936218485236], [37.38911792635918, -5.994926495477557], [37.389094457030296, -5.994916437193751], [37.3890719935298, -5.994910066947341], [37.38904248923063, -5.994900092482567], [37.3890183493495, -5.994894979521632], [37.388995634391904, -5.994886513799429], [37.388965459540486, -5.994886681437492], [37.388940984383225, -5.994879053905606], [37.38891382701695, -5.994875701144338], [37.38888289779425, -5.99487335421145], [37.388857919722795, -5.994866648688912], [37.38883218728006, -5.994856841862202], [37.38879891112447, -5.994851142168045], [37.3887749388814, -5.994842844083905], [37.38874970935285, -5.994836641475558], [37.388722971081734, -5.994831779971719], [37.38870126195252, -5.9948283433914185], [37.38867871463299, -5.994825074449182], [37.38865449093282, -5.994821721687913], [37.38863194361329, -5.994823398068547], [37.38860855810344, -5.994824403896928], [37.38858265802264, -5.994822392240167], [37.38856153562665, -5.994818201288581], [37.38853714428842, -5.994821302592754], [37.38851300440729, -5.994820632040501], [37.38849397748709, -5.99481912329793], [37.388467993587255, -5.994818201288581], [37.388443602249026, -5.994814010336995], [37.38842465914786, -5.994802862405777], [37.38840068690479, -5.994785679504275], [37.388377552852035, -5.9947688318789005], [37.38835785537958, -5.994754582643509], [37.38833488896489, -5.994741339236498], [37.38830890506506, -5.994724994525313], [37.38828652538359, -5.9947016928344965], [37.388263810426, -5.994684929028153], [37.3882429394871, -5.994666237384081], [37.38822282291949, -5.994643438607454], [37.38820538856089, -5.994617789983749], [37.388189882040024, -5.994594320654869], [37.38817068748176, -5.994572611525655], [37.38815115764737, -5.994547214359045], [37.38813481293619, -5.994523158296943], [37.3881134390831, -5.994503125548363], [37.388096591457725, -5.994476806372404], [37.38808276131749, -5.994450319558382], [37.3880616389215, -5.9944201447069645], [37.38803976215422, -5.994395837187767], [37.3880185559392, -5.99437446333468], [37.38799382932484, -5.994353927671909], [37.387972958385944, -5.994336241856217], [37.38794974051416, -5.994320819154382], [37.387926019728184, -5.9943043906241655], [37.387907076627016, -5.994290979579091], [37.38788126036525, -5.994280166924], [37.38785586319864, -5.994265330955386], [37.38783105276525, -5.994259212166071], [37.387799033895135, -5.994257451966405], [37.3877730499953, -5.9942475613206625], [37.38774513825774, -5.994237922132015], [37.387708593159914, -5.994228031486273], [37.38768118433654, -5.994220990687609], [37.38764916546643, -5.99421520717442], [37.38762049935758, -5.994201125577092], [37.38759510219097, -5.994187379255891], [37.3875626642257, -5.994184194132686], [37.38753642886877, -5.99417413584888], [37.387508265674114, -5.994168687611818], [37.38748060539365, -5.994165251031518], [37.38745671696961, -5.994158126413822], [37.38742746412754, -5.994153432548046], [37.38739863038063, -5.994149157777429], [37.387370551005006, -5.994145888835192], [37.38733786158264, -5.994137926027179], [37.38731078803539, -5.994126191362739], [37.38728329539299, -5.994122922420502], [37.38725722767413, -5.9941191505640745], [37.387231243774295, -5.994115127250552], [37.38719914108515, -5.99411160685122], [37.38717081025243, -5.994112025946379], [37.38714465871453, -5.9941059071570635], [37.387113980948925, -5.99410037510097], [37.387090008705854, -5.99409400485456], [37.38705849274993, -5.994092831388116], [37.38703016191721, -5.994088975712657], [37.38700174726546, -5.9940852876752615], [37.38696310669184, -5.994082521647215], [37.386935111135244, -5.994091993197799], [37.386909211054444, -5.994093753397465], [37.38687953911722, -5.994089981541038], [37.38685422576964, -5.994085371494293], [37.3868218716234, -5.994081180542707], [37.38679219968617, -5.9940764028579], [37.386763198301196, -5.994080509990454], [37.38672874867916, -5.994078665971756], [37.386701088398695, -5.994079504162073], [37.386671332642436, -5.994077743962407], [37.38663864322007, -5.994080677628517], [37.38661056384444, -5.994077660143375], [37.38657586276531, -5.994079755619168], [37.38654895685613, -5.994077660143375], [37.38652146421373, -5.994076821953058], [37.38649279810488, -5.994075313210487], [37.386467484757304, -5.9940732177346945], [37.38643898628652, -5.994069781154394], [37.386411326006055, -5.994061650708318], [37.38638433627784, -5.994054274633527], [37.38635106012225, -5.994050167500973], [37.38632130436599, -5.994052849709988], [37.38629196770489, -5.994048826396465], [37.386260870844126, -5.994041953235865], [37.386234886944294, -5.994039857760072], [37.386204628273845, -5.9940381813794374], [37.38617160357535, -5.994032984599471], [37.38614268600941, -5.99402560852468], [37.3861104156822, -5.994022339582443], [37.38608024083078, -5.994020914658904], [37.38605232909322, -5.994017813354731], [37.38602123223245, -5.994013790041208], [37.38599248230457, -5.994017142802477], [37.38596482202411, -5.994017226621509], [37.38593431189656, -5.994013454765081], [37.385907489806414, -5.9940107725560665], [37.385880667716265, -5.994010269641876], [37.38585074432194, -5.994006749242544], [37.38582392223179, -5.994004905223846], [37.38579357974231, -5.993998534977436], [37.38576692529023, -5.993990991264582], [37.38573876209557, -5.993985794484615], [37.38570925779641, -5.993979508057237], [37.38568344153464, -5.993976993486285], [37.38565234467387, -5.9939733892679214], [37.385623175650835, -5.993973053991795], [37.38560096360743, -5.993971880525351], [37.38557162694633, -5.993970790877938], [37.38554589450359, -5.993970958516002], [37.38552334718406, -5.993969449773431], [37.3854954354465, -5.993961235508323], [37.385467691347, -5.993957296013832], [37.38543927669525, -5.9939533565193415], [37.38540851511061, -5.993948243558407], [37.38538395613432, -5.993948662653565], [37.385354870930314, -5.993952350690961], [37.38532746210694, -5.993950758129358], [37.38530390895903, -5.993945226073265], [37.38527147099376, -5.993940196931362], [37.385237608104944, -5.993940364569426], [37.385210283100605, -5.993929887190461], [37.38517533056438, -5.993921756744385], [37.385145323351026, -5.993915302678943], [37.38511623814702, -5.993910022079945], [37.38508405163884, -5.993914296850562], [37.3850567266345, -5.9939150512218475], [37.385030491277575, -5.993916979059577], [37.38499998115003, -5.993915805593133], [37.384972739964724, -5.993908178061247], [37.38494340330362, -5.993905076757073], [37.38491540774703, -5.9939012210816145], [37.384893111884594, -5.99389367736876], [37.38486084155738, -5.993895269930363], [37.38483184017241, -5.993892839178443], [37.38480594009161, -5.993890492245555], [37.38477450795472, -5.993888480588794], [37.3847496137023, -5.993881523609161], [37.38471952266991, -5.993881020694971], [37.38469060510397, -5.993879763409495], [37.384662106633186, -5.9938760753721], [37.38462891429663, -5.993873728439212], [37.38459832035005, -5.993863921612501], [37.384572671726346, -5.993851600214839], [37.38453973084688, -5.99384137429297], [37.38451240584254, -5.993832154199481], [37.38448524847627, -5.993821509182453], [37.38445306196809, -5.993821760639548], [37.38442380912602, -5.993816060945392], [37.38439782522619, -5.993809020146728], [37.38436874002218, -5.9938002191483974], [37.384342420846224, -5.993788735941052], [37.384320041164756, -5.993771385401487], [37.384289698675275, -5.993759483098984], [37.38427150994539, -5.993745736777782], [37.38424745388329, -5.993729894980788], [37.38422431983054, -5.993724949657917], [37.384196324273944, -5.993718663230538], [37.38416631706059, -5.993717070668936], [37.3841415066272, -5.993709610775113], [37.384110828861594, -5.993710868060589], [37.384084006771445, -5.993701815605164], [37.3840606212616, -5.993692595511675], [37.384032625705004, -5.99369234405458], [37.38400848582387, -5.9936879854649305], [37.383983759209514, -5.993679603561759], [37.38395467400551, -5.993664935231209], [37.38392776809633, -5.993651356548071], [37.383896335959435, -5.9936402924358845], [37.383868005126715, -5.993627971038222], [37.3838412668556, -5.9936184994876385], [37.38380673341453, -5.993618248030543], [37.383778654038906, -5.993616236373782], [37.383745880797505, -5.993613554164767], [37.38371042534709, -5.993609866127372], [37.38368469290435, -5.993600562214851], [37.38365535624325, -5.993597796186805], [37.38362543284893, -5.993595281615853], [37.38360095769167, -5.993584971874952], [37.38356734625995, -5.993578769266605], [37.383540188893676, -5.993570890277624], [37.383515210822225, -5.993563011288643], [37.3834836948663, -5.9935531206429005], [37.3834559507668, -5.993548342958093], [37.38342418335378, -5.993545660749078], [37.38339073956013, -5.993534680455923], [37.38336592912674, -5.993522442877293], [37.38332980312407, -5.993521856144071], [37.38329996354878, -5.993518503382802], [37.38326970487833, -5.9935148153454065], [37.38323735073209, -5.993500566110015], [37.38321010954678, -5.993489837273955], [37.383185885846615, -5.993481790646911], [37.38315646536648, -5.993471648544073], [37.38313098438084, -5.9934653621166945], [37.383099468424916, -5.99346125498414], [37.38307021558285, -5.993455890566111], [37.38303953781724, -5.993454633280635], [37.383008524775505, -5.9934495203197], [37.38298187032342, -5.993444658815861], [37.38294918090105, -5.9934385400265455], [37.38291800022125, -5.9934344328939915], [37.38289210014045, -5.993424793705344], [37.38286159001291, -5.993416830897331], [37.38283535465598, -5.993409706279635], [37.38281331025064, -5.993401492014527], [37.3827809561044, -5.993384476751089], [37.38275773823261, -5.993375089019537], [37.382731921970844, -5.993366288021207], [37.382706105709076, -5.993352206423879], [37.382688503712416, -5.993338208645582], [37.38266268745065, -5.993320858106017], [37.382639134302735, -5.99330049008131], [37.38261734135449, -5.9932812955230474], [37.38259479403496, -5.993259083479643], [37.382580460980535, -5.993226142600179], [37.3825563210994, -5.993204936385155], [37.38253511488438, -5.993185909464955], [37.38251583650708, -5.993169480934739], [37.382492534816265, -5.993142323568463], [37.3824770282954, -5.993115250021219], [37.38245707936585, -5.993087757378817], [37.38243319094181, -5.993062108755112], [37.38241701386869, -5.9930371306836605], [37.382395723834634, -5.993004105985165], [37.38238172605634, -5.992973009124398], [37.382371835410595, -5.992931267246604], [37.38235850818455, -5.992895141243935], [37.3823475278914, -5.992861865088344], [37.38233478739858, -5.992822470143437], [37.38232313655317, -5.992788942530751], [37.382317604497075, -5.9927549958229065], [37.38230771385133, -5.992714008316398], [37.38229447044432, -5.992682576179504], [37.38228667527437, -5.992649719119072], [37.382275108247995, -5.9926109947264194], [37.38226940855384, -5.992579227313399], [37.382261361926794, -5.992544107139111], [37.38225759007037, -5.992512926459312], [37.3822473641485, -5.992478476837277], [37.38223998807371, -5.992440925911069], [37.38222758285701, -5.992406727746129], [37.3822161834687, -5.99237160757184], [37.382205706089735, -5.992330871522427], [37.38220042549074, -5.992303462699056], [37.382193971425295, -5.992270186543465], [37.38218039274216, -5.992238251492381], [37.38217159174383, -5.99220547825098], [37.38216195255518, -5.992165915668011], [37.382153403013945, -5.992128448560834], [37.38213839940727, -5.99208720959723], [37.38212247379124, -5.992049742490053], [37.38211459480226, -5.992016214877367], [37.38210001029074, -5.991982184350491], [37.3820866830647, -5.991948153823614], [37.38207746297121, -5.9919085912406445], [37.382066398859024, -5.991872800514102], [37.38206170499325, -5.991840194910765], [37.3820502217859, -5.991804571822286], [37.38204435445368, -5.991764171048999], [37.38204083405435, -5.991731313988566], [37.38202951848507, -5.991694098338485], [37.38202239386737, -5.991657888516784], [37.382012670859694, -5.991624528542161], [37.38200521096587, -5.991582283750176], [37.38199741579592, -5.991550264880061], [37.38199473358691, -5.99151749163866], [37.381988698616624, -5.99147885106504], [37.381981909275055, -5.991452699527144], [37.38196891732514, -5.991419926285744], [37.38196355290711, -5.991389332339168], [37.38195382989943, -5.991362761706114], [37.381951650604606, -5.991328563541174], [37.38194217905402, -5.9912957064807415], [37.381937485188246, -5.991259245201945], [37.38192927092314, -5.991223873570561], [37.38192055374384, -5.991192441433668], [37.381910998374224, -5.991160087287426], [37.38189800642431, -5.991129158064723], [37.381891049444675, -5.991100994870067], [37.38187847658992, -5.99107182584703], [37.38187453709543, -5.991040896624327], [37.38186640664935, -5.991008961573243], [37.38185500726104, -5.9909791219979525], [37.38184620626271, -5.99094576202333], [37.38183564506471, -5.990917347371578], [37.38182189874351, -5.9908845741301775], [37.38181645050645, -5.99084559828043], [37.381801614537835, -5.990806706249714], [37.38178761675954, -5.99076496437192], [37.3817721940577, -5.990731185302138], [37.3817602917552, -5.990689862519503], [37.381750820204616, -5.99065599963069], [37.3817396722734, -5.990620963275433], [37.38172709941864, -5.990580311045051], [37.38171972334385, -5.990551058202982], [37.381708743050694, -5.990514932200313], [37.38169826567173, -5.990475537255406], [37.38169197924435, -5.9904454462230206], [37.38168125040829, -5.990412002429366], [37.38167278468609, -5.990380570292473], [37.38166373223066, -5.990349892526865], [37.3816548474133, -5.990313766524196], [37.381642861291766, -5.990280406549573], [37.38163372501731, -5.990241598337889], [37.38162484019995, -5.990205053240061], [37.381615871563554, -5.990171525627375], [37.38160589709878, -5.990135399624705], [37.38159533590078, -5.990100782364607], [37.38158678635955, -5.990067254751921], [37.38157454878092, -5.990032386034727], [37.38156432285905, -5.990001205354929], [37.38155409693718, -5.989973209798336], [37.38154102116823, -5.989936329424381], [37.381532303988934, -5.989904226735234], [37.38152123987675, -5.989868603646755], [37.38150808028877, -5.989828621968627], [37.38149701617658, -5.989796938374639], [37.38148427568376, -5.989758046343923], [37.38147187046707, -5.9897189028561115], [37.38146189600229, -5.989685459062457], [37.38145242445171, -5.989648075774312], [37.38144286908209, -5.989618990570307], [37.38143113441765, -5.989583702757955], [37.381421998143196, -5.989545816555619], [37.381411185488105, -5.9895124565809965], [37.38140070810914, -5.989475157111883], [37.38139341585338, -5.989435678347945], [37.381389141082764, -5.989398211240768], [37.38138469867408, -5.9893599059432745], [37.38138310611248, -5.989323360845447], [37.381383357569575, -5.989289917051792], [37.381383273750544, -5.989260412752628], [37.38138159736991, -5.989234261214733], [37.38138101063669, -5.989207774400711], [37.381380340084434, -5.9891758393496275], [37.38137589767575, -5.9891437366604805], [37.38137187436223, -5.989116160199046], [37.38136013969779, -5.9890886675566435], [37.38135133869946, -5.989061174914241], [37.38134396262467, -5.989037286490202], [37.38133323378861, -5.989007782191038], [37.38132443279028, -5.988978445529938], [37.38131915219128, -5.988946091383696], [37.38131085410714, -5.98891013301909], [37.38130490295589, -5.988876856863499], [37.38129618577659, -5.988837210461497], [37.38127699121833, -5.988803179934621], [37.381252851337194, -5.988783985376358], [37.38122083246708, -5.988774178549647], [37.38118948414922, -5.988766048103571], [37.38115880638361, -5.988764204084873], [37.38112511113286, -5.988771580159664], [37.381095606833696, -5.988777447491884], [37.381064761430025, -5.988788176327944], [37.38103894516826, -5.988787841051817], [37.3810107819736, -5.988790858536959], [37.38098085857928, -5.988794965669513], [37.38095462322235, -5.988797312602401], [37.38092444837093, -5.988800497725606], [37.380894776433706, -5.988807873800397], [37.380867786705494, -5.988810388371348], [37.38083643838763, -5.988821620121598], [37.38080844283104, -5.988827319815755], [37.38078304566443, -5.988833857700229], [37.38075429573655, -5.988848693668842], [37.38072822801769, -5.988856740295887], [37.38070090301335, -5.988866211846471], [37.380673326551914, -5.988883478567004], [37.38064927048981, -5.988895548507571], [37.380620604380965, -5.988905020058155], [37.380590764805675, -5.988917341455817], [37.38056469708681, -5.988921448588371], [37.38053435459733, -5.988930249586701], [37.380503257736564, -5.988940307870507], [37.380475010722876, -5.988946175202727], [37.38044165074825, -5.988959250971675], [37.380412397906184, -5.988966794684529], [37.38038197159767, -5.98897903226316], [37.38034903071821, -5.988992862403393], [37.38032195717096, -5.989003758877516], [37.380294632166624, -5.989020019769669], [37.38026604987681, -5.989033346995711], [37.380240904167295, -5.98904307000339], [37.38021047785878, -5.989062264561653], [37.38018432632089, -5.9890719037503], [37.380153983831406, -5.989085817709565], [37.38012498244643, -5.989102413877845], [37.38009757362306, -5.989118088036776], [37.380067482590675, -5.989130912348628], [37.3800384812057, -5.989138204604387], [37.38001115620136, -5.98915102891624], [37.37997938878834, -5.9891667030751705], [37.37995223142207, -5.989175084978342], [37.37992339767516, -5.989186903461814], [37.37989481538534, -5.989202577620745], [37.379871513694525, -5.989214479923248], [37.37984142266214, -5.989229399710894], [37.37981133162975, -5.989237865433097], [37.37978417426348, -5.9892429783940315], [37.37975156866014, -5.989251947030425], [37.379720555618405, -5.989255886524916], [37.37969105131924, -5.989265441894531], [37.37966121174395, -5.989286480471492], [37.37963640131056, -5.989298466593027], [37.37960413098335, -5.98931422457099], [37.37957353703678, -5.989332748576999], [37.37954772077501, -5.989350434392691], [37.37952299416065, -5.989373903721571], [37.37950103357434, -5.989397121593356], [37.379481587558985, -5.989417573437095], [37.37945920787752, -5.989443389698863], [37.379437163472176, -5.989460991695523], [37.37941679544747, -5.989475240930915], [37.37939567305148, -5.989492842927575], [37.37937664613128, -5.989508684724569], [37.379357032477856, -5.989520167931914], [37.379332557320595, -5.989534500986338], [37.379308165982366, -5.989538440480828], [37.379283271729946, -5.989544056355953], [37.37925745546818, -5.989558976143599], [37.37923272885382, -5.989569872617722], [37.37920984625816, -5.989580936729908], [37.37918528728187, -5.989601472392678], [37.37916139885783, -5.9896155539900064], [37.37913533113897, -5.9896323177963495], [37.379107335582376, -5.9896529372781515], [37.3790817707777, -5.989667689427733], [37.37905419431627, -5.989686045795679], [37.37902779132128, -5.989701887592673], [37.37900113686919, -5.989721668884158], [37.37897548824549, -5.989743797108531], [37.37895134836435, -5.98976475186646], [37.37892435863614, -5.989785706624389], [37.37889485433698, -5.989809678867459], [37.37887004390359, -5.989827951416373], [37.37884263508022, -5.989847397431731], [37.37881279550493, -5.9898714534938335], [37.37878295592964, -5.989889558404684], [37.378754457458854, -5.989906825125217], [37.37872654572129, -5.989920822903514], [37.37870106473565, -5.989935407415032], [37.3786726500839, -5.989952590316534], [37.37864758819342, -5.989965163171291], [37.37862378358841, -5.9899798315018415], [37.378598134964705, -5.989996679127216], [37.37857047468424, -5.990007659420371], [37.37854180857539, -5.990027356892824], [37.378513561561704, -5.990043617784977], [37.37848749384284, -5.990052167326212], [37.378457905724645, -5.99007211625576], [37.37843125127256, -5.990084353834391], [37.37840208224952, -5.990100530907512], [37.37837115302682, -5.990114947780967], [37.37834516912699, -5.99013171158731], [37.37831884995103, -5.990149648860097], [37.37828901037574, -5.990164987742901], [37.37826185300946, -5.990173956379294], [37.37823100760579, -5.990189714357257], [37.37820393405855, -5.9902027901262045], [37.37817736342549, -5.9902113396674395], [37.37815045751631, -5.990230366587639], [37.37812288105488, -5.990240843966603], [37.37810050137341, -5.990251572802663], [37.37807074561715, -5.990262720733881], [37.3780441749841, -5.990273030474782], [37.37801852636039, -5.990284346044064], [37.37799505703151, -5.990296918898821], [37.377972258254886, -5.990303624421358], [37.37794619053602, -5.990318292751908], [37.37791861407459, -5.990334218367934], [37.37788936123252, -5.990343606099486], [37.37786002457142, -5.9903582744300365], [37.37783278338611, -5.990369589999318], [37.37780570983887, -5.99037897773087], [37.37777754664421, -5.990393310785294], [37.37774913199246, -5.9904051292687654], [37.3777193762362, -5.990413008257747], [37.37768702208996, -5.990419713780284], [37.37766179256141, -5.990427425131202], [37.37763547338545, -5.990442261099815], [37.37761292606592, -5.990463467314839], [37.377585181966424, -5.990479476749897], [37.37755777314305, -5.99049967713654], [37.37753237597644, -5.990518284961581], [37.37751108594239, -5.990534629672766], [37.377484599128366, -5.990559021010995], [37.37746071070433, -5.990571761503816], [37.377433804795146, -5.990584418177605], [37.37740614451468, -5.990603948011994], [37.3773810826242, -5.990614928305149], [37.3773517459631, -5.990631356835365], [37.3773240018636, -5.990645857527852], [37.37729575484991, -5.990654658526182], [37.37726499326527, -5.990667147561908], [37.3772375844419, -5.9906757809221745], [37.3772098403424, -5.990690030157566], [37.37718435935676, -5.990702519193292], [37.37715594470501, -5.990710565820336], [37.37712560221553, -5.990726659074426], [37.37709576264024, -5.99073956720531], [37.37706759944558, -5.990748032927513], [37.377040442079306, -5.990763455629349], [37.37701269797981, -5.9907726757228374], [37.376984283328056, -5.990790277719498], [37.37695310264826, -5.990807460620999], [37.376928627491, -5.9908186085522175], [37.37690130248666, -5.990829085931182], [37.376875234767795, -5.990834534168243], [37.376851765438914, -5.990841658785939], [37.37682695500553, -5.990846436470747], [37.37680013291538, -5.990849956870079], [37.37677683122456, -5.990858841687441], [37.37675269134343, -5.990875270217657], [37.376727629452944, -5.990890860557556], [37.3767023999244, -5.990915754809976], [37.37667406909168, -5.990929082036018], [37.37664288841188, -5.990950120612979], [37.37661522813141, -5.990967806428671], [37.37658731639385, -5.990980127826333], [37.376556219533086, -5.990998232737184], [37.37652864307165, -5.991010470315814], [37.376498468220234, -5.991020947694778], [37.376469718292356, -5.991034107282758], [37.37644541077316, -5.991040393710136], [37.3764172475785, -5.991064701229334], [37.37638983875513, -5.99107576534152], [37.37636301666498, -5.991091942414641], [37.37633225508034, -5.991107448935509], [37.37630585208535, -5.991120021790266], [37.37627550959587, -5.991142150014639], [37.37624726258218, -5.991158997640014], [37.37621884793043, -5.991176683455706], [37.37618590705097, -5.991191351786256], [37.37615439109504, -5.991200404241681], [37.37612220458686, -5.991224041208625], [37.3760945443064, -5.991237452253699], [37.37606378272176, -5.991256646811962], [37.376029919832945, -5.991276176646352], [37.3760012537241, -5.991278439760208], [37.37595741637051, -5.991275925189257], [37.375922380015254, -5.991271482780576], [37.37589186988771, -5.991258323192596], [37.37586127594113, -5.991233009845018], [37.37583822570741, -5.991205517202616], [37.37580947577953, -5.991174587979913], [37.375780306756496, -5.9911449160426855], [37.37575298175216, -5.991117423400283], [37.375722555443645, -5.991090768948197], [37.37569246441126, -5.991065287962556], [37.375660026445985, -5.991045003756881], [37.375624906271696, -5.991029245778918], [37.37559431232512, -5.991025809198618], [37.37555919215083, -5.991039806976914], [37.37552558071911, -5.991050032898784], [37.37548811361194, -5.991063276305795], [37.37544922158122, -5.991070568561554], [37.37541485577822, -5.991086075082421], [37.37538317218423, -5.991110885515809], [37.37535526044667, -5.991126475855708], [37.3753278516233, -5.991148520261049], [37.37530027516186, -5.991168217733502], [37.3752707708627, -5.991188418120146], [37.375239254906774, -5.99120962433517], [37.37521419301629, -5.991224544122815], [37.37518385052681, -5.99124382250011], [37.37515551969409, -5.991257149726152], [37.37512375228107, -5.991262681782246], [37.375086368992925, -5.991258407011628], [37.37505728378892, -5.991248181089759], [37.375022917985916, -5.99123552441597], [37.374989641830325, -5.991218341514468], [37.374958377331495, -5.99120425991714], [37.37492619082332, -5.99118473008275], [37.374899201095104, -5.991165200248361], [37.37486768513918, -5.991147179156542], [37.374839186668396, -5.991130918264389], [37.37480565905571, -5.991117591038346], [37.37477590329945, -5.991096803918481], [37.37474706955254, -5.991081800311804], [37.37471345812082, -5.99106696434319], [37.37468546256423, -5.991053469479084], [37.37465520389378, -5.9910388849675655], [37.3746282979846, -5.991019941866398], [37.37460139207542, -5.991002256050706], [37.374567529186606, -5.990982977673411], [37.374543556943536, -5.990964621305466], [37.37451002933085, -5.990947522222996], [37.37448270432651, -5.990930255502462], [37.374455546960235, -5.9909131564199924], [37.37442143261433, -5.990896224975586], [37.374395029619336, -5.99088029935956], [37.37436284311116, -5.990863870829344], [37.37432981841266, -5.990849453955889], [37.37430106848478, -5.990831684321165], [37.3742691334337, -5.9908118192106485], [37.37424021586776, -5.990789020434022], [37.37421087920666, -5.990772088989615], [37.374178944155574, -5.9907532297074795], [37.37415203824639, -5.990739902481437], [37.374128149822354, -5.99072540178895], [37.374107111245394, -5.990712745115161], [37.374083725735545, -5.9906919579952955], [37.37405899912119, -5.990667063742876], [37.3740360327065, -5.990648120641708], [37.37400686368346, -5.990631524473429], [37.37397928722203, -5.99061300046742], [37.373947352170944, -5.9905944764614105], [37.373915584757924, -5.990576120093465], [37.373881973326206, -5.990549884736538], [37.37384635023773, -5.990531947463751], [37.37381391227245, -5.99051577039063], [37.37377761863172, -5.99049967713654], [37.373744593933225, -5.990477716550231], [37.37370394170284, -5.99045374430716], [37.373672844842076, -5.990432957187295], [37.37363445572555, -5.990411834791303], [37.37360302358866, -5.9903843421489], [37.373567735776305, -5.9903633035719395], [37.37353269942105, -5.990340756252408], [37.373499339446425, -5.990320472046733], [37.37346497364342, -5.990300355479121], [37.373436726629734, -5.9902790654450655], [37.37340462394059, -5.99026121199131], [37.373376125469804, -5.99024017341435], [37.37334083765745, -5.990216787904501], [37.37331250682473, -5.990198850631714], [37.37327872775495, -5.990176722407341], [37.37324268557131, -5.990157276391983], [37.373213432729244, -5.9901356510818005], [37.37317722290754, -5.990120060741901], [37.3731501493603, -5.990101872012019], [37.37311888486147, -5.990082258358598], [37.37308586016297, -5.990064237266779], [37.37305937334895, -5.990047473460436], [37.3730259295553, -5.990028530359268], [37.37299642525613, -5.990009503439069], [37.37296826206148, -5.989986537024379], [37.37293431535363, -5.989961726590991], [37.37290506251156, -5.989942532032728], [37.37287363037467, -5.989927276968956], [37.37284521572292, -5.989909674972296], [37.37281755544245, -5.9899000357836485], [37.37279106862843, -5.989883439615369], [37.372769359499216, -5.9898661728948355], [37.372751673683524, -5.989857288077474], [37.37273423932493, -5.989855946972966], [37.372717056423426, -5.989850666373968]],
                {&quot;bubblingMouseEvents&quot;: true, &quot;color&quot;: &quot;#c52c4b&quot;, &quot;dashArray&quot;: null, &quot;dashOffset&quot;: null, &quot;fill&quot;: false, &quot;fillColor&quot;: &quot;#c52c4b&quot;, &quot;fillOpacity&quot;: 0.2, &quot;fillRule&quot;: &quot;evenodd&quot;, &quot;lineCap&quot;: &quot;round&quot;, &quot;lineJoin&quot;: &quot;round&quot;, &quot;noClip&quot;: false, &quot;opacity&quot;: 1.0, &quot;smoothFactor&quot;: 1.0, &quot;stroke&quot;: true, &quot;weight&quot;: 0.5}
            ).addTo(feature_group_41640e6b40f07bf7b78b1ecd19592d11);
        
    
            feature_group_41640e6b40f07bf7b78b1ecd19592d11.addTo(map_ebb9fb23349eacb25aa5b5a390e550be);
        
&lt;/script&gt;
&lt;/html&gt;" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>
```
:::
:::

::: {.cell .markdown}
# Run Data Analysis
:::

::: {.cell .markdown}
After being playing around with my own data, let\'s try to extract some
information from my runs! I started running on March 2024. When I bought
my Garmin watch, I was already focused on my Half-Marathon training
plan. Let\'s take a look at some stats.
:::

::: {.cell .code execution_count="125"}
``` python
df_run = df_activities[df_activities['Activity Type'] == 'Running']
df_run.shape
df_run.info()

df_run.head()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    Index: 64 entries, 9 to 158
    Data columns (total 46 columns):
     #   Column                                 Non-Null Count  Dtype              
    ---  ------                                 --------------  -----              
     0   Start Time                             64 non-null     datetime64[ns, UTC]
     1   End Time                               64 non-null     object             
     2   Activity ID                            64 non-null     int64              
     3   Activity Name                          64 non-null     object             
     4   Description                            1 non-null      object             
     5   Location Name                          63 non-null     object             
     6   Time Zone                              64 non-null     object             
     7   Offset                                 64 non-null     object             
     8   Duration (h:m:s)                       64 non-null     object             
     9   Elapsed Duration (h:m:s)               64 non-null     object             
     10  Moving Duration (h:m:s)                63 non-null     object             
     11  Activity Parent                        64 non-null     object             
     12  Activity Type                          64 non-null     object             
     13  Event Type                             64 non-null     object             
     14  Device                                 63 non-null     object             
     15  Privacy                                64 non-null     object             
     16  File Format                            64 non-null     object             
     17  Distance (km)                          64 non-null     float64            
     18  Average Speed (km/h)                   64 non-null     float64            
     19  Average Speed (km/h or min/km)         64 non-null     object             
     20  Average Moving Speed (km/h)            63 non-null     float64            
     21  Average Moving Speed (km/h or min/km)  63 non-null     object             
     22  Max. Speed (km/h)                      63 non-null     float64            
     23  Max. Speed (km/h or min/km)            63 non-null     object             
     24  Elevation Gain (m)                     62 non-null     float64            
     25  Elevation Loss (m)                     63 non-null     float64            
     26  Elevation Min. (m)                     63 non-null     float64            
     27  Elevation Max. (m)                     63 non-null     float64            
     28  Elevation Corrected                    64 non-null     bool               
     29  Begin Latitude (°DD)                   63 non-null     float64            
     30  Begin Longitude (°DD)                  63 non-null     float64            
     31  End Latitude (°DD)                     63 non-null     float64            
     32  End Longitude (°DD)                    63 non-null     float64            
     33  Max. Heart Rate (bpm)                  63 non-null     float64            
     34  Average Heart Rate (bpm)               63 non-null     float64            
     35  Calories                               64 non-null     float64            
     36  VO2max                                 63 non-null     float64            
     37  Aerobic Training Effect                63 non-null     float64            
     38  Anaerobic Training Effect              10 non-null     float64            
     39  Avg. Run Cadence                       63 non-null     float64            
     40  Max. Run Cadence                       63 non-null     float64            
     41  Stride Length                          63 non-null     float64            
     42  Steps                                  63 non-null     float64            
     43  Weekday                                64 non-null     object             
     44  YearMonth                              64 non-null     period[M]          
     45  Duration_td                            64 non-null     timedelta64[ns]    
    dtypes: bool(1), datetime64[ns, UTC](1), float64(22), int64(1), object(19), period[M](1), timedelta64[ns](1)
    memory usage: 23.1+ KB
:::

::: {.output .execute_result execution_count="125"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Start Time</th>
      <th>End Time</th>
      <th>Activity ID</th>
      <th>Activity Name</th>
      <th>Description</th>
      <th>Location Name</th>
      <th>Time Zone</th>
      <th>Offset</th>
      <th>Duration (h:m:s)</th>
      <th>Elapsed Duration (h:m:s)</th>
      <th>...</th>
      <th>VO2max</th>
      <th>Aerobic Training Effect</th>
      <th>Anaerobic Training Effect</th>
      <th>Avg. Run Cadence</th>
      <th>Max. Run Cadence</th>
      <th>Stride Length</th>
      <th>Steps</th>
      <th>Weekday</th>
      <th>YearMonth</th>
      <th>Duration_td</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>9</th>
      <td>2025-07-15 04:57:13+00:00</td>
      <td>2025-07-15T07:24:47+02:00</td>
      <td>19736714765</td>
      <td>Madrid Corrida</td>
      <td>NaN</td>
      <td>Madrid</td>
      <td>Europe/Paris</td>
      <td>+02:00</td>
      <td>00:27:34</td>
      <td>00:27:34</td>
      <td>...</td>
      <td>51.0</td>
      <td>3.2</td>
      <td>NaN</td>
      <td>164.88</td>
      <td>176.0</td>
      <td>110.13</td>
      <td>4556.0</td>
      <td>Tuesday</td>
      <td>2025-07</td>
      <td>0 days 00:27:34</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2025-07-12 06:25:53+00:00</td>
      <td>2025-07-12T09:30:56+02:00</td>
      <td>19706561166</td>
      <td>Madrid Corrida</td>
      <td>NaN</td>
      <td>Madrid</td>
      <td>Europe/Paris</td>
      <td>+02:00</td>
      <td>01:04:54</td>
      <td>01:05:03</td>
      <td>...</td>
      <td>50.0</td>
      <td>4.3</td>
      <td>0.3</td>
      <td>164.91</td>
      <td>176.0</td>
      <td>106.36</td>
      <td>10690.0</td>
      <td>Saturday</td>
      <td>2025-07</td>
      <td>0 days 01:04:54</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2025-07-10 04:50:06+00:00</td>
      <td>2025-07-10T07:20:07+02:00</td>
      <td>19695202111</td>
      <td>Madrid Corrida</td>
      <td>NaN</td>
      <td>Madrid</td>
      <td>Europe/Paris</td>
      <td>+02:00</td>
      <td>00:30:01</td>
      <td>00:30:01</td>
      <td>...</td>
      <td>50.0</td>
      <td>3.3</td>
      <td>NaN</td>
      <td>164.42</td>
      <td>174.0</td>
      <td>106.46</td>
      <td>4934.0</td>
      <td>Thursday</td>
      <td>2025-07</td>
      <td>0 days 00:30:01</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2025-06-30 04:51:51+00:00</td>
      <td>2025-06-30T07:24:07+02:00</td>
      <td>19586024281</td>
      <td>Madrid - Base</td>
      <td>NaN</td>
      <td>Madrid</td>
      <td>Europe/Paris</td>
      <td>+02:00</td>
      <td>00:32:16</td>
      <td>00:32:16</td>
      <td>...</td>
      <td>51.0</td>
      <td>3.1</td>
      <td>NaN</td>
      <td>163.55</td>
      <td>172.0</td>
      <td>105.52</td>
      <td>5282.0</td>
      <td>Monday</td>
      <td>2025-06</td>
      <td>0 days 00:32:16</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2025-06-19 19:25:12+00:00</td>
      <td>2025-06-19T22:06:17+02:00</td>
      <td>19485360382</td>
      <td>Madrid - Base</td>
      <td>NaN</td>
      <td>Madrid</td>
      <td>Europe/Paris</td>
      <td>+02:00</td>
      <td>00:41:05</td>
      <td>00:41:05</td>
      <td>...</td>
      <td>51.0</td>
      <td>3.3</td>
      <td>NaN</td>
      <td>159.72</td>
      <td>167.0</td>
      <td>107.09</td>
      <td>6556.0</td>
      <td>Thursday</td>
      <td>2025-06</td>
      <td>0 days 00:41:05</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 46 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
### Calories estimation

It might be interesting to see how Garmin estimates Energy expenditure
during the Running Activities. Runs are one of the most energy-consuming
exercises there are there (if not the most). However, energetic
requirements might vary from person to person based on Sex, Age and
Weight, among other factors. Being these parameters fixed, I would love
to see how Garmin estimates Energetic expenditure based on different
parameters for my runs.

Let\'s explore the features
:::

::: {.cell .code execution_count="147"}
``` python
import numpy as np
import seaborn as sns
# Selección de variables numéricas (o un subconjunto que te interese)
numeric_df = df_run.select_dtypes(include=[np.number])

# Dibujar
sns.pairplot(numeric_df, diag_kind="kde", corner=False)  
# corner=True dibuja sólo media matriz (evita duplicados)
plt.suptitle("Pairplot de Variables Numéricas", y=1.02)
plt.show()
```

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/7cd34c02f9f3ef526aa92f39b1b57f3b15611e25.png)
:::
:::

::: {.cell .code execution_count="148"}
``` python
# Correlation matrix
corr = df_run.select_dtypes(include=[np.number]) \
                            .corr()

# Plot heatmap
fig, ax = plt.subplots(figsize=(10, 8))
im = ax.imshow(corr.values)
ax.set_xticks(np.arange(len(corr.columns)))
ax.set_yticks(np.arange(len(corr.index)))
ax.set_xticklabels(corr.columns, rotation=45, ha='right')
ax.set_yticklabels(corr.index)
plt.title('Matriz de Correlación')
plt.colorbar(im)
plt.tight_layout()
plt.show()
```

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/889c6b18ac76c2b6ff5c4bc91d3f7e3f1fcae07e.png)
:::
:::

::: {.cell .markdown}
Interestingly, there is a very clear lineal relationship between
kilometers run and Calories burnt!
:::

::: {.cell .code execution_count="149"}
``` python
plt.figure(figsize=(14,6))
sns.jointplot(x=df_run['Distance (km)'],y=df_run['Calories'], data=df_run.dropna(),kind='scatter')
```

::: {.output .execute_result execution_count="149"}
    <seaborn.axisgrid.JointGrid at 0x1ab367cfd90>
:::

::: {.output .display_data}
    <Figure size 1400x600 with 0 Axes>
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/549ad45feafc026c7105190401a3caaf5e29c2bd.png)
:::
:::

::: {.cell .markdown}
Maybe we can draft a regression over this data, and check the magnitude
of this relationship.
:::

::: {.cell .code execution_count="150"}
``` python
from sklearn.linear_model import LinearRegression

# Create vectors for data
X = df_run[['Distance (km)',]].dropna()
y = df_run.loc[X.index, 'Calories']

# Fit the linear regression model
model = LinearRegression()
model.fit(X, y)

# Extract relevante information from the model
slope = model.coef_[0]          # Coefficient
intercept = model.intercept_    # Intercept
r2 = model.score(X, y)          # Goodnes-of-fit

# Display results
print(f"Regression coef (slope): {slope:.4f}")
print(f"Ordenada al origen (intercept): {intercept:.4f}")
print(f"Coeficiente de determinación R²: {r2:.4f}")

# Plot regression line
plt.figure()
plt.scatter(X, y)
plt.plot(X, model.predict(X), linewidth=2)
plt.xlabel('Distance')
plt.ylabel('Calories')
plt.title('Linear regression')
plt.tight_layout()
plt.show()
```

::: {.output .stream .stdout}
    Regression coef (slope): 84.5452
    Ordenada al origen (intercept): -17.6648
    Coeficiente de determinación R²: 0.9971
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/6abb36eb1b52975d2bd01024249b895b6d3b805a.png)
:::
:::

::: {.cell .markdown}
Unfortunately, LinearRegression does not provides the t statistic and
p-values for their regression coefficients. We can either calculate them
manually by applying Linear Algebra theory or use the statsmodel module
to obtain them automatically.
:::

::: {.cell .code execution_count="151"}
``` python
import statsmodels.api as sm

X_const = sm.add_constant(X)
est = sm.OLS(y, X_const)
est2 = est.fit()
print(est2.summary())
```

::: {.output .stream .stdout}
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:               Calories   R-squared:                       0.997
    Model:                            OLS   Adj. R-squared:                  0.997
    Method:                 Least Squares   F-statistic:                 2.125e+04
    Date:                Thu, 07 Aug 2025   Prob (F-statistic):           2.41e-80
    Time:                        21:26:09   Log-Likelihood:                -275.36
    No. Observations:                  64   AIC:                             554.7
    Df Residuals:                      62   BIC:                             559.0
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    =================================================================================
                        coef    std err          t      P>|t|      [0.025      0.975]
    ---------------------------------------------------------------------------------
    const           -17.6648      5.163     -3.422      0.001     -27.985      -7.345
    Distance (km)    84.5452      0.580    145.772      0.000      83.386      85.705
    ==============================================================================
    Omnibus:                       17.939   Durbin-Watson:                   1.441
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):               26.077
    Skew:                           1.048   Prob(JB):                     2.18e-06
    Kurtosis:                       5.321   Cond. No.                         20.4
    ==============================================================================

    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
:::
:::

::: {.cell .markdown}
Alright, it seems like this relationship is statistically significant!
Let\'s try to check other features as well and create a more complete
model. Since we have 45 variables, let\'s drop those that we know that
do not impact Calories burnt.
:::

::: {.cell .code execution_count="152"}
``` python
# We preselect a few columns
selected_cols = ['Distance (km)','Duration_td','Steps','Stride Length','Max. Run Cadence','Avg. Run Cadence',
            'Average Heart Rate (bpm)','Max. Heart Rate (bpm)','Elevation Gain (m)','Elevation Loss (m)',
            'Max. Speed (km/h)','Average Speed (km/h)'
            ]

X3 = df_run[selected_cols].dropna()

y3 = df_run.loc[X3.index, 'Calories']
# Convert time delta to minutes

X3['Duration_td'] = X3['Duration_td'].dt.total_seconds() / 60
```
:::

::: {.cell .code execution_count="153"}
``` python
X3.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    Index: 62 entries, 9 to 158
    Data columns (total 12 columns):
     #   Column                    Non-Null Count  Dtype  
    ---  ------                    --------------  -----  
     0   Distance (km)             62 non-null     float64
     1   Duration_td               62 non-null     float64
     2   Steps                     62 non-null     float64
     3   Stride Length             62 non-null     float64
     4   Max. Run Cadence          62 non-null     float64
     5   Avg. Run Cadence          62 non-null     float64
     6   Average Heart Rate (bpm)  62 non-null     float64
     7   Max. Heart Rate (bpm)     62 non-null     float64
     8   Elevation Gain (m)        62 non-null     float64
     9   Elevation Loss (m)        62 non-null     float64
     10  Max. Speed (km/h)         62 non-null     float64
     11  Average Speed (km/h)      62 non-null     float64
    dtypes: float64(12)
    memory usage: 6.3 KB
:::
:::

::: {.cell .markdown}
Let\'s fit the lineal regression model
:::

::: {.cell .code execution_count="154"}
``` python
X3_const = sm.add_constant(X3)
model3 = sm.OLS(y3, X3_const).fit()

print(model3.summary())
```

::: {.output .stream .stdout}
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:               Calories   R-squared:                       0.999
    Model:                            OLS   Adj. R-squared:                  0.999
    Method:                 Least Squares   F-statistic:                     4558.
    Date:                Thu, 07 Aug 2025   Prob (F-statistic):           2.71e-70
    Time:                        21:26:09   Log-Likelihood:                -230.71
    No. Observations:                  62   AIC:                             487.4
    Df Residuals:                      49   BIC:                             515.1
    Df Model:                          12                                         
    Covariance Type:            nonrobust                                         
    ============================================================================================
                                   coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------------
    const                     -586.1073    324.291     -1.807      0.077   -1237.794      65.579
    Distance (km)               78.1201      8.040      9.717      0.000      61.964      94.276
    Duration_td                  6.0852      4.765      1.277      0.208      -3.489      15.660
    Steps                       -0.0342      0.033     -1.023      0.312      -0.101       0.033
    Stride Length                3.3840      2.119      1.597      0.117      -0.873       7.641
    Max. Run Cadence            -0.0561      0.078     -0.720      0.475      -0.213       0.100
    Avg. Run Cadence             3.5058      2.154      1.628      0.110      -0.823       7.834
    Average Heart Rate (bpm)     2.3612      0.480      4.916      0.000       1.396       3.326
    Max. Heart Rate (bpm)       -0.2079      0.373     -0.558      0.580      -0.957       0.541
    Elevation Gain (m)           0.3781      0.201      1.884      0.065      -0.025       0.781
    Elevation Loss (m)          -0.2213      0.191     -1.161      0.251      -0.604       0.162
    Max. Speed (km/h)           -0.1569      0.277     -0.566      0.574      -0.714       0.400
    Average Speed (km/h)       -62.1005     23.480     -2.645      0.011    -109.285     -14.917
    ==============================================================================
    Omnibus:                       31.067   Durbin-Watson:                   1.005
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):               92.327
    Skew:                           1.408   Prob(JB):                     8.94e-21
    Kurtosis:                       8.273   Cond. No.                     1.89e+06
    ==============================================================================

    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 1.89e+06. This might indicate that there are
    strong multicollinearity or other numerical problems.
:::
:::

::: {.cell .markdown}
Before jumping into conclussions, it is good practice to check
assumptions from the model!
:::

::: {.cell .code execution_count="155"}
``` python
from statsmodels.stats.diagnostic import het_breuschpagan
from scipy.stats import shapiro
from statsmodels.graphics.gofplots import qqplot

# Normality test
resid = model3.resid
stat_shapiro, p_shapiro = shapiro(resid)
print(f"Shapiro-Wilk → estadístico = {stat_shapiro:.4f}, p-valor = {p_shapiro:.4f}")

# Breusch-Pagan
bp_test = het_breuschpagan(resid, X3_const)
labels = ['LM estadístico', 'LM p-valor', 'F estadístico', 'F p-valor']
bp_results = dict(zip(labels, bp_test))
print("Breusch-Pagan →", bp_results)

# Model diagnosis

# Histogram of residuals
plt.figure()
plt.hist(resid, bins=30)
plt.title('Residuals histogram')
plt.xlabel('Residuals')
plt.ylabel('Frequency')
plt.tight_layout()
plt.show()

# QQ-Plot
plt.figure()
qqplot(resid, line='s')
plt.title('QQ-Plot')
plt.tight_layout()
plt.show()

# Residuals vs fitted
fitted = model3.fittedvalues
plt.figure()
plt.scatter(fitted, resid, alpha=0.6)
plt.axhline(0, linestyle='--')
plt.xlabel('Fitted values')
plt.ylabel('Residuals')
plt.title('Residuals vs fitted')
plt.tight_layout()
plt.show()
```

::: {.output .stream .stdout}
    Shapiro-Wilk → estadístico = 0.8783, p-valor = 0.0000
    Breusch-Pagan → {'LM estadístico': np.float64(22.54725071216234), 'LM p-valor': np.float64(0.03182499405632668), 'F estadístico': np.float64(2.3336254651428963), 'F p-valor': np.float64(0.01865321708745624)}
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/2ed2484268d58eade38c38f367f3a165eac25043.png)
:::

::: {.output .display_data}
    <Figure size 640x480 with 0 Axes>
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/d16a32167470fb9e2ed12386965ab3bf9993a34a.png)
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/ff720ca8ca43bb6cb984164eef50e3726ac598f3.png)
:::
:::

::: {.cell .markdown}
It seems like the residues are not following a normal distribution,
however, I don\'t see it quite worrying, since the QQ-plot shows that
the distribution has some heavy tails. There might be some residuals
influencing this tails, however, it might not be have a big impact in
the model. FItted value vs residuals seems more worrying, displaying
some heterocedasticity.

Let\'s try to fix this by adjusting the data to a Weighted Least Squares
regression and a log-log transformation!.
:::

::: {.cell .code execution_count="156"}
``` python
y4 = np.log(y3)    
X4 = np.log(X3)   
X4_const = sm.add_constant(X4)

# Fit log-log OLS
init_ols = sm.OLS(y4, X4_const).fit()
print("Initial OLS summary:")
print(init_ols.summary())

# Compute weights as 1 / y**2 (Var(y) ~ y**2)
fitted_vals = init_ols.fittedvalues
weights = 1.0 / (fitted_vals ** 2)

# Fit WLS
wls_model = sm.WLS(y4, X4_const, weights=weights).fit()
print("\nWLS (1/fitted²) summary:")
print(wls_model.summary())

# Check normality
sw_stat, sw_p = shapiro(wls_model.resid)
print(f"\nShapiro–Wilk (WLS): W={sw_stat:.3f}, p-value={sw_p:.3f}")

# Check homoskedasticity
bp_stat, bp_pvalue, _, _ = het_breuschpagan(wls_model.resid, X4_const)
print(f"Breusch–Pagan p-value (WLS): {bp_pvalue:.3f}")

# QQplot
qqplot(wls_model.resid, line='s')
plt.title('QQ-Plot of Residuals (WLS log–log)')
plt.show()

# Residuals vs Fitted
plt.figure(figsize=(6,4))
plt.scatter(wls_model.fittedvalues, wls_model.resid, alpha=0.6)
plt.axhline(0, linestyle='--', color='gray')
plt.xlabel('Fitted Values (log)')
plt.ylabel('Residuals')
plt.title('Residuals vs Fitted (WLS log–log)')
plt.show()
```

::: {.output .stream .stdout}
    Initial OLS summary:
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:               Calories   R-squared:                       0.999
    Model:                            OLS   Adj. R-squared:                  0.999
    Method:                 Least Squares   F-statistic:                     5167.
    Date:                Thu, 07 Aug 2025   Prob (F-statistic):           1.26e-71
    Time:                        21:26:09   Log-Likelihood:                 183.46
    No. Observations:                  62   AIC:                            -340.9
    Df Residuals:                      49   BIC:                            -313.3
    Df Model:                          12                                         
    Covariance Type:            nonrobust                                         
    ============================================================================================
                                   coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------------
    const                       69.1954     45.745      1.513      0.137     -22.733     161.123
    Distance (km)               17.7206     11.086      1.598      0.116      -4.558      39.999
    Duration_td                -17.0909     11.095     -1.540      0.130     -39.386       5.204
    Steps                        0.3591      0.665      0.540      0.592      -0.978       1.696
    Stride Length                0.2490      0.303      0.822      0.415      -0.360       0.858
    Max. Run Cadence             0.0016      0.020      0.080      0.936      -0.038       0.041
    Avg. Run Cadence            -0.1360      0.615     -0.221      0.826      -1.371       1.100
    Average Heart Rate (bpm)     0.6767      0.095      7.132      0.000       0.486       0.867
    Max. Heart Rate (bpm)       -0.0673      0.080     -0.844      0.403      -0.228       0.093
    Elevation Gain (m)           0.0034      0.002      1.425      0.160      -0.001       0.008
    Elevation Loss (m)          -0.0023      0.004     -0.556      0.581      -0.010       0.006
    Max. Speed (km/h)           -0.0060      0.009     -0.642      0.524      -0.025       0.013
    Average Speed (km/h)       -17.4166     11.045     -1.577      0.121     -39.613       4.780
    ==============================================================================
    Omnibus:                       17.171   Durbin-Watson:                   0.645
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):               29.806
    Skew:                           0.908   Prob(JB):                     3.37e-07
    Kurtosis:                       5.870   Cond. No.                     4.37e+05
    ==============================================================================

    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 4.37e+05. This might indicate that there are
    strong multicollinearity or other numerical problems.

    WLS (1/fitted²) summary:
                                WLS Regression Results                            
    ==============================================================================
    Dep. Variable:               Calories   R-squared:                       0.999
    Model:                            WLS   Adj. R-squared:                  0.999
    Method:                 Least Squares   F-statistic:                     5159.
    Date:                Thu, 07 Aug 2025   Prob (F-statistic):           1.31e-71
    Time:                        21:26:09   Log-Likelihood:                 183.94
    No. Observations:                  62   AIC:                            -341.9
    Df Residuals:                      49   BIC:                            -314.2
    Df Model:                          12                                         
    Covariance Type:            nonrobust                                         
    ============================================================================================
                                   coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------------
    const                       64.9775     44.331      1.466      0.149     -24.108     154.063
    Distance (km)               16.7276     10.734      1.558      0.126      -4.844      38.299
    Duration_td                -16.1983     10.738     -1.509      0.138     -37.776       5.380
    Steps                        0.4614      0.647      0.713      0.479      -0.839       1.762
    Stride Length                0.2533      0.303      0.837      0.406      -0.355       0.861
    Max. Run Cadence             0.0018      0.019      0.094      0.926      -0.037       0.041
    Avg. Run Cadence            -0.2145      0.611     -0.351      0.727      -1.443       1.014
    Average Heart Rate (bpm)     0.6829      0.092      7.390      0.000       0.497       0.869
    Max. Heart Rate (bpm)       -0.0734      0.078     -0.947      0.348      -0.229       0.082
    Elevation Gain (m)           0.0035      0.002      1.515      0.136      -0.001       0.008
    Elevation Loss (m)          -0.0030      0.004     -0.749      0.458      -0.011       0.005
    Max. Speed (km/h)           -0.0046      0.009     -0.517      0.607      -0.023       0.013
    Average Speed (km/h)       -16.4232     10.688     -1.537      0.131     -37.902       5.056
    ==============================================================================
    Omnibus:                       16.806   Durbin-Watson:                   0.651
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):               27.837
    Skew:                           0.912   Prob(JB):                     9.02e-07
    Kurtosis:                       5.729   Cond. No.                     4.26e+05
    ==============================================================================

    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 4.26e+05. This might indicate that there are
    strong multicollinearity or other numerical problems.

    Shapiro–Wilk (WLS): W=0.926, p-value=0.001
    Breusch–Pagan p-value (WLS): 0.173
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/1dc57b326132b68d8268a5c33fd9f7e27c7a7e67.png)
:::

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/8cafdbe3fe4e7e8895c3b8ed81943a859bb5c7de.png)
:::
:::

::: {.cell .markdown}
It seems like we have more homocedasticity in our residuals!

It seems like these variables explain \~99% of the variance in calories
burnt per running session. Apparently, only average heart rate seems to
be a clear, robust estimator of Calories burnt. I\'m also seeing that
distance, duration, average speed and elevation gain appear to be
important. I expect that the former 3 are highly correlated, so we might
be having some multicollinearity issues here. Let\'s check it by
computing the Compute Variance-Inflation Factors (VIFs)
:::

::: {.cell .code execution_count="157"}
``` python
from statsmodels.stats.outliers_influence import variance_inflation_factor

# Build DataFrame of VIFs
vif_data = []
for i, col in enumerate(X4_const.columns):
    vif = variance_inflation_factor(X4_const.values, i)
    vif_data.append((col, vif))

vif_df = pd.DataFrame(vif_data, columns=['variable', 'VIF']).set_index('variable')
print(vif_df)
```

::: {.output .stream .stdout}
                                       VIF
    variable                              
    const                     6.508415e+08
    Distance (km)             7.183566e+06
    Duration_td               7.410635e+06
    Steps                     2.687946e+04
    Stride Length             4.983918e+01
    Max. Run Cadence          1.324249e+00
    Avg. Run Cadence          1.021789e+02
    Average Heart Rate (bpm)  4.510061e+00
    Max. Heart Rate (bpm)     3.668030e+00
    Elevation Gain (m)        3.781389e+00
    Elevation Loss (m)        4.711520e+00
    Max. Speed (km/h)         1.262094e+00
    Average Speed (km/h)      8.021429e+04
:::
:::

::: {.cell .markdown}
As expected, VIF values for duration, distance, steps, average speed and
average run cadence are quite high! These variables are measuring
similar stuff! Let\'s drop a few variables.
:::

::: {.cell .code execution_count="158"}
``` python
X_reduced = X4.drop(columns=['Average Speed (km/h)','Steps','Duration_td'])
Xr_const  = sm.add_constant(X_reduced)

init = sm.OLS(y4, Xr_const).fit()
weights = 1.0 / (init.fittedvalues ** 2)
wls_reduced = sm.WLS(y4, Xr_const, weights=weights).fit()
print(wls_reduced.summary())

vif_data = [(col, variance_inflation_factor(Xr_const.values, i))
            for i, col in enumerate(Xr_const.columns)]
print(pd.DataFrame(vif_data, columns=['variable','VIF']).set_index('variable'))
```

::: {.output .stream .stdout}
                                WLS Regression Results                            
    ==============================================================================
    Dep. Variable:               Calories   R-squared:                       0.999
    Model:                            WLS   Adj. R-squared:                  0.999
    Method:                 Least Squares   F-statistic:                     6186.
    Date:                Thu, 07 Aug 2025   Prob (F-statistic):           1.69e-75
    Time:                        21:26:10   Log-Likelihood:                 178.80
    No. Observations:                  62   AIC:                            -337.6
    Df Residuals:                      52   BIC:                            -316.3
    Df Model:                           9                                         
    Covariance Type:            nonrobust                                         
    ============================================================================================
                                   coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------------
    const                        5.5493      0.507     10.952      0.000       4.533       6.566
    Distance (km)                0.9916      0.007    146.403      0.000       0.978       1.005
    Stride Length               -0.4311      0.059     -7.306      0.000      -0.550      -0.313
    Max. Run Cadence             0.0012      0.020      0.058      0.954      -0.039       0.041
    Avg. Run Cadence            -0.4080      0.081     -5.025      0.000      -0.571      -0.245
    Average Heart Rate (bpm)     0.7185      0.091      7.893      0.000       0.536       0.901
    Max. Heart Rate (bpm)       -0.1224      0.077     -1.592      0.117      -0.277       0.032
    Elevation Gain (m)           0.0040      0.002      1.626      0.110      -0.001       0.009
    Elevation Loss (m)          -0.0029      0.004     -0.699      0.488      -0.011       0.005
    Max. Speed (km/h)           -0.0081      0.009     -0.887      0.379      -0.026       0.010
    ==============================================================================
    Omnibus:                        5.954   Durbin-Watson:                   0.925
    Prob(Omnibus):                  0.051   Jarque-Bera (JB):                6.053
    Skew:                           0.422   Prob(JB):                       0.0485
    Kurtosis:                       4.277   Cond. No.                     3.37e+03
    ==============================================================================

    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 3.37e+03. This might indicate that there are
    strong multicollinearity or other numerical problems.
                                       VIF
    variable                              
    const                     75594.533624
    Distance (km)                 2.578029
    Stride Length                 1.700301
    Max. Run Cadence              1.279319
    Avg. Run Cadence              1.692063
    Average Heart Rate (bpm)      3.906399
    Max. Heart Rate (bpm)         3.235268
    Elevation Gain (m)            3.742042
    Elevation Loss (m)            4.511107
    Max. Speed (km/h)             1.203239
:::
:::

::: {.cell .markdown}
It seems like dropping these variables has not decreased the
Goodness-of-fit of the regression and the multicollinearity issue has
been solved. We can see that Distance, stride length, average cadence
and average heart beat are robust estimators of calories burnts (which
is ulitmately estimated by the Garmin software).

This morning I went for a runm let\'s try and predict the calories
burnt!

I am attaching the sessions summary:
:::

::: {.cell .code execution_count="159"}
``` python
import numpy as np
import pandas as pd
import statsmodels.api as sm

# New data from this morning's run!
new_data = {
    'Distance (km)':            [7.68],
    'Stride Length':            [110],
    'Max. Run Cadence':         [169],
    'Avg. Run Cadence':         [163],
    'Average Heart Rate (bpm)': [140],
    'Max. Heart Rate (bpm)':    [161],
    'Elevation Gain (m)':       [67],
    'Elevation Loss (m)':       [95],
    'Max. Speed (km/h)':        [13.2],
}

# Convert to pd dataframe
df_new = pd.DataFrame(new_data)
X_new = df_new.copy()

# log transform
X_new_log = np.log(X_new)
X_new_const = sm.add_constant(X_new_log,has_constant="add")

# Predict and reconvert to normal scale
log_cal_pred = wls_reduced.predict(X_new_const)

cal_pred = np.exp(log_cal_pred)

print(f"In this run, you have burnt: {cal_pred.values[0]:.1f} kcal")
```

::: {.output .stream .stdout}
    In this run, you have burnt: 592.0 kcal
:::
:::

::: {.cell .markdown}
Actually, my Garmin predicts 575 kcal, which is close enough!
:::

::: {.cell .code execution_count="6"}
``` python
%matplotlib inline
from IPython.display import Image
Image('C:/Users/alexa/OneDrive/Documents/Code/Python/Projects/Garmin Data/img/Run_1.png')
```

::: {.output .execute_result execution_count="6"}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/c7afe89ad547a78e21d182b8592e092b62870273.png)
:::
:::

::: {.cell .code execution_count="7"}
``` python
Image('C:/Users/alexa/OneDrive/Documents/Code/Python/Projects/Garmin Data/img/Run_2.png')
```

::: {.output .execute_result execution_count="7"}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/06f0c642d2719905884f28133deaabbfdd6248b4.png)
:::
:::

::: {.cell .markdown}
Energetic expenditure might also depend on other factors not included in
the initial dataset, such as weight. When I started training with my
Garmin, I was weighting slightly more, so a couple of months back I
changed that information. This might alter Calories burn estimation by
Garmin\'s algorithm. Most of my runs were estimated with my old weight,
which explains this slight overestimation of my model.

Age might also be a factor, but I don\'t think it would make much of a
difference!
:::

::: {.cell .markdown}
### Unsupervised Clustering of run data
:::

::: {.cell .markdown}
Something interasting we can investigate is if a Clustering algorithm is
able to differenciate my runs into different categories! When I was
training for my Half Marathons, I typically had an easy run, a long run
and a tempo run/intervals. I would love to see if this algorithm finds a
similar pattern, or if new types of runs appear in my data!

#### Preprocessing, missing values and feature selection

Before running into the algorithm, I would like to take a look at my
missing data.
:::

::: {.cell .code execution_count="139"}
``` python
# Checking NAs
missing_counts = df_run.isnull().sum().sort_values(ascending=False)
missing_pct = (df_run.isnull().mean() * 100).sort_values(ascending=False)

# Juntamos en un DataFrame para verlo más claro
missing_summary = pd.concat([missing_counts, missing_pct], axis=1, keys=['n_missing','pct_missing'])
missing_summary
```

::: {.output .execute_result execution_count="139"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>n_missing</th>
      <th>pct_missing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Description</th>
      <td>63</td>
      <td>98.4375</td>
    </tr>
    <tr>
      <th>Anaerobic Training Effect</th>
      <td>54</td>
      <td>84.3750</td>
    </tr>
    <tr>
      <th>Elevation Gain (m)</th>
      <td>2</td>
      <td>3.1250</td>
    </tr>
    <tr>
      <th>VO2max</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Moving Duration (h:m:s)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Location Name</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Average Moving Speed (km/h or min/km)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Max. Speed (km/h)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Max. Speed (km/h or min/km)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Elevation Loss (m)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Device</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Average Moving Speed (km/h)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Elevation Max. (m)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Begin Latitude (°DD)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Steps</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Avg. Run Cadence</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Max. Run Cadence</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Stride Length</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Aerobic Training Effect</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Max. Heart Rate (bpm)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Begin Longitude (°DD)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Elevation Min. (m)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>End Latitude (°DD)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>End Longitude (°DD)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>Average Heart Rate (bpm)</th>
      <td>1</td>
      <td>1.5625</td>
    </tr>
    <tr>
      <th>End Time</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Start Time</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Activity Name</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Time Zone</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Activity ID</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Event Type</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Activity Type</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Activity Parent</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Elapsed Duration (h:m:s)</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Offset</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Duration (h:m:s)</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Distance (km)</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Average Speed (km/h)</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Elevation Corrected</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>File Format</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Average Speed (km/h or min/km)</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Privacy</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Calories</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Weekday</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>YearMonth</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>Duration_td</th>
      <td>0</td>
      <td>0.0000</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .code execution_count="140"}
``` python
df_run.describe().T
```

::: {.output .execute_result execution_count="140"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Activity ID</th>
      <td>64.0</td>
      <td>18502980267.6875</td>
      <td>584254245.159401</td>
      <td>17695650735.0</td>
      <td>18001978434.75</td>
      <td>18379566503.5</td>
      <td>18909040914.5</td>
      <td>19736714765.0</td>
    </tr>
    <tr>
      <th>Distance (km)</th>
      <td>64.0</td>
      <td>7.994535</td>
      <td>3.945679</td>
      <td>2.07868</td>
      <td>5.408405</td>
      <td>6.8316</td>
      <td>10.011958</td>
      <td>21.40773</td>
    </tr>
    <tr>
      <th>Average Speed (km/h)</th>
      <td>64.0</td>
      <td>10.53764</td>
      <td>0.484387</td>
      <td>9.4464</td>
      <td>10.2528</td>
      <td>10.575</td>
      <td>10.8558</td>
      <td>12.0384</td>
    </tr>
    <tr>
      <th>Average Moving Speed (km/h)</th>
      <td>63.0</td>
      <td>10.565372</td>
      <td>0.479912</td>
      <td>9.448487</td>
      <td>10.286161</td>
      <td>10.597899</td>
      <td>10.863663</td>
      <td>12.039606</td>
    </tr>
    <tr>
      <th>Max. Speed (km/h)</th>
      <td>63.0</td>
      <td>14.3552</td>
      <td>5.426437</td>
      <td>10.5156</td>
      <td>12.3282</td>
      <td>13.2696</td>
      <td>14.8482</td>
      <td>54.2808</td>
    </tr>
    <tr>
      <th>Elevation Gain (m)</th>
      <td>62.0</td>
      <td>26.0</td>
      <td>37.363961</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>8.5</td>
      <td>34.5</td>
      <td>180.0</td>
    </tr>
    <tr>
      <th>Elevation Loss (m)</th>
      <td>63.0</td>
      <td>31.52381</td>
      <td>37.283075</td>
      <td>4.0</td>
      <td>10.0</td>
      <td>15.0</td>
      <td>38.0</td>
      <td>180.0</td>
    </tr>
    <tr>
      <th>Elevation Min. (m)</th>
      <td>63.0</td>
      <td>591.803175</td>
      <td>198.8263</td>
      <td>7.8</td>
      <td>647.1</td>
      <td>686.4</td>
      <td>690.5</td>
      <td>696.4</td>
    </tr>
    <tr>
      <th>Elevation Max. (m)</th>
      <td>63.0</td>
      <td>610.936508</td>
      <td>201.405138</td>
      <td>12.8</td>
      <td>690.9</td>
      <td>696.0</td>
      <td>703.6</td>
      <td>730.2</td>
    </tr>
    <tr>
      <th>Begin Latitude (°DD)</th>
      <td>63.0</td>
      <td>40.609221</td>
      <td>1.114868</td>
      <td>37.372739</td>
      <td>40.442378</td>
      <td>40.443247</td>
      <td>40.443515</td>
      <td>47.386734</td>
    </tr>
    <tr>
      <th>Begin Longitude (°DD)</th>
      <td>63.0</td>
      <td>-3.223347</td>
      <td>1.838614</td>
      <td>-5.989897</td>
      <td>-3.705386</td>
      <td>-3.705119</td>
      <td>-3.704587</td>
      <td>8.533423</td>
    </tr>
    <tr>
      <th>End Latitude (°DD)</th>
      <td>63.0</td>
      <td>40.608254</td>
      <td>1.114543</td>
      <td>37.372711</td>
      <td>40.441702</td>
      <td>40.442391</td>
      <td>40.443353</td>
      <td>47.383529</td>
    </tr>
    <tr>
      <th>End Longitude (°DD)</th>
      <td>63.0</td>
      <td>-3.224317</td>
      <td>1.839202</td>
      <td>-5.989846</td>
      <td>-3.707282</td>
      <td>-3.705741</td>
      <td>-3.705053</td>
      <td>8.539263</td>
    </tr>
    <tr>
      <th>Max. Heart Rate (bpm)</th>
      <td>63.0</td>
      <td>166.809524</td>
      <td>7.244257</td>
      <td>148.0</td>
      <td>162.5</td>
      <td>168.0</td>
      <td>171.0</td>
      <td>181.0</td>
    </tr>
    <tr>
      <th>Average Heart Rate (bpm)</th>
      <td>63.0</td>
      <td>151.730159</td>
      <td>6.08331</td>
      <td>135.0</td>
      <td>148.0</td>
      <td>152.0</td>
      <td>155.5</td>
      <td>167.0</td>
    </tr>
    <tr>
      <th>Calories</th>
      <td>64.0</td>
      <td>658.234375</td>
      <td>334.074355</td>
      <td>165.0</td>
      <td>437.25</td>
      <td>553.0</td>
      <td>816.25</td>
      <td>1779.0</td>
    </tr>
    <tr>
      <th>VO2max</th>
      <td>63.0</td>
      <td>51.063492</td>
      <td>0.503953</td>
      <td>50.0</td>
      <td>51.0</td>
      <td>51.0</td>
      <td>51.0</td>
      <td>53.0</td>
    </tr>
    <tr>
      <th>Aerobic Training Effect</th>
      <td>63.0</td>
      <td>3.501587</td>
      <td>0.503373</td>
      <td>2.0</td>
      <td>3.2</td>
      <td>3.4</td>
      <td>3.85</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>Anaerobic Training Effect</th>
      <td>10.0</td>
      <td>0.64</td>
      <td>0.704273</td>
      <td>0.1</td>
      <td>0.125</td>
      <td>0.25</td>
      <td>1.05</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>Avg. Run Cadence</th>
      <td>63.0</td>
      <td>159.804444</td>
      <td>4.608838</td>
      <td>142.58</td>
      <td>158.515</td>
      <td>160.97</td>
      <td>162.355</td>
      <td>166.42</td>
    </tr>
    <tr>
      <th>Max. Run Cadence</th>
      <td>63.0</td>
      <td>177.571429</td>
      <td>20.803779</td>
      <td>163.0</td>
      <td>167.0</td>
      <td>169.0</td>
      <td>174.0</td>
      <td>247.0</td>
    </tr>
    <tr>
      <th>Stride Length</th>
      <td>63.0</td>
      <td>109.838095</td>
      <td>4.597206</td>
      <td>97.43</td>
      <td>106.625</td>
      <td>109.92</td>
      <td>112.32</td>
      <td>120.71</td>
    </tr>
    <tr>
      <th>Steps</th>
      <td>63.0</td>
      <td>7379.333333</td>
      <td>3740.980068</td>
      <td>2026.0</td>
      <td>4950.0</td>
      <td>6252.0</td>
      <td>8598.0</td>
      <td>19434.0</td>
    </tr>
    <tr>
      <th>Duration_td</th>
      <td>64</td>
      <td>0 days 00:45:44.562500</td>
      <td>0 days 00:23:07.526449054</td>
      <td>0 days 00:13:07</td>
      <td>0 days 00:30:05.500000</td>
      <td>0 days 00:39:00</td>
      <td>0 days 00:54:50.750000</td>
      <td>0 days 02:00:25</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
As you can see, we have a couple of variables where the missing value
porcentage is quite high, so we might drop them directly. Also, we have
a few variables that might skew the algorithm, such as latitud, longitud
or date (and timezone) when the workout was done. Also, we might want to
drop features that might add multicolinearity (such as Speed in min/km
or Moving speed and start and end time).

We will also convert the Duration timedelta variable to seconds and will
drop the Activity Type column, since all runs will be Runs.
:::

::: {.cell .code execution_count="141"}
``` python
# Drop unnecessay columns
drop = [
    'Description', 'Anaerobic Training Effect',
    'Begin Latitude (°DD)', 'Begin Longitude (°DD)',
    'End Latitude (°DD)',   'End Longitude (°DD)',
    'Weekday', 'YearMonth',
    'Activity ID', 'Activity Name', 'Location Name',
    'Device', 'Time Zone', 'Offset',
    'File Format', 'Privacy',
    'Duration (h:m:s)', 'Elapsed Duration (h:m:s)',
    'Moving Duration (h:m:s)',
    'Average Speed (km/h or min/km)',
    'Average Moving Speed (km/h or min/km)',
    'Max. Speed (km/h or min/km)',
    'Elevation Min. (m)', 'Elevation Max. (m)',
    'Average Moving Speed (km/h)',
    'Event Type','Activity Type',
    'Elevation Corrected',
    'Start Time', 'End Time', 'Activity Parent'
]

# Drop colums and convert duration to seconds
df_clean = df_run.drop(columns = drop)
df_clean['duration_sec'] = df_clean['Duration_td'].dt.total_seconds()
df_clean = df_clean.drop(columns=['Duration_td'])



# Check everything is OK
print(df_clean.info())
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    Index: 64 entries, 9 to 158
    Data columns (total 15 columns):
     #   Column                    Non-Null Count  Dtype  
    ---  ------                    --------------  -----  
     0   Distance (km)             64 non-null     float64
     1   Average Speed (km/h)      64 non-null     float64
     2   Max. Speed (km/h)         63 non-null     float64
     3   Elevation Gain (m)        62 non-null     float64
     4   Elevation Loss (m)        63 non-null     float64
     5   Max. Heart Rate (bpm)     63 non-null     float64
     6   Average Heart Rate (bpm)  63 non-null     float64
     7   Calories                  64 non-null     float64
     8   VO2max                    63 non-null     float64
     9   Aerobic Training Effect   63 non-null     float64
     10  Avg. Run Cadence          63 non-null     float64
     11  Max. Run Cadence          63 non-null     float64
     12  Stride Length             63 non-null     float64
     13  Steps                     63 non-null     float64
     14  duration_sec              64 non-null     float64
    dtypes: float64(15)
    memory usage: 10.1 KB
    None
:::
:::

::: {.cell .markdown}
#### Data imputation and scaling
:::

::: {.cell .markdown}
OK, once we have our dataset ready, let\'s impute the NAs with the
median. We have very little NA values in a few varaibles, so the median
imputation is a good approach and more robust than the mean. If we had
way more missing values, it might be interesting to check for any
pattern of our missing values (MCAR, MAR or MNAR) and implement a more
sophisticated imputation technique, since it would not reduce the
variability of the data.
:::

::: {.cell .markdown}
On the other hand, all ML algorithms that depend on computing the
Euclidean distance are sensitive to features magnitudes. If we wouldn\'t
scale the variables\' values, those with bigger values would dominate
these distances values. That\'s why it is important to scale al the
predictors.
:::

::: {.cell .code execution_count="142"}
``` python
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score


num_cols = df_clean.select_dtypes(include=['float64','int64']).columns          # Extract numeric variables
imputer = SimpleImputer(strategy='median')                                      # impute median,more robust than the mean! Very little NAs
scaler  = StandardScaler()                                                      # Scale features: mean 0, SD 1

X_num = imputer.fit_transform(df_clean[num_cols])
X_scaled = scaler.fit_transform(X_num)
```
:::

::: {.cell .markdown}
#### Setting the algorithm parameters
:::

::: {.cell .markdown}
Once we\'ve prep\'d our data, lets find the best parametrs for our
model. I don\'t expect to see more thatn 5 groups, so the best approach
is to see if my runs can be grouped in as much as 5 groups. This is
represented by k, which represents the number of clusters the algorithm
will be looking.

By assuming each value of k (between 2 to 5), we can calculate the
Shiloutte score. The Shiloutte score can take values from -1 to 1. A
shiloutte value near to 1 means that each observation has been assigned
correctly to a group (close to their group and away from others),
whereas -1 means the opposite. Values near 0 menas that there is some
overlapping between clusters.
:::

::: {.cell .code execution_count="143"}
``` python
# Search for the best shiloutte for 2 to 5 k-groups
sil_scores = {}
for k in range(2, 6):                                       
    km = KMeans(n_clusters=k, random_state=42)
    labels = km.fit_predict(X_scaled)
    sil_scores[k] = silhouette_score(X_scaled, labels)

print(sil_scores)
```

::: {.output .stream .stdout}
    {2: 0.38860006103135847, 3: 0.1948720148072741, 4: 0.20560691061946854, 5: 0.19176365089895936}
:::
:::

::: {.cell .markdown}
Apparently, the best k value is 2. With this ifnormation, let\'s build
the clusters. These clusters are built by computing the centroid of each
cluster. This centroid is a point in space which represents the center
of each cluster. These centroids minimize the cuadratic sum of distances
within each cluster.

#### Run the model

Once computed, we assign to each observation a cluster (either cluster
#1 or #2 in this particular case)
:::

::: {.cell .code execution_count="144"}
``` python

best_k = 2
km_final = KMeans(n_clusters=best_k, random_state = 123) # Initialize the algorithm and we add a seed for reproducibility
labels = km_final.fit_predict(X_scaled)                  # Fit the model.

# We create a new variable to assign each observation to a cluster
df_clean['cluster'] = labels

print("Cluster size:\n", df_clean['cluster'].value_counts())

# Pull the centroids of each cluster
centers_scaled = km_final.cluster_centers_
# Invert the scale applied before
centers_orig = scaler.inverse_transform(centers_scaled)
centers_df = pd.DataFrame(centers_orig, columns=num_cols)
print("\nCentroid of each cluster:\n", centers_df)
```

::: {.output .stream .stdout}
    Cluster size:
     cluster
    0    50
    1    14
    Name: count, dtype: int64

    Centroid of each cluster:
        Distance (km)  Average Speed (km/h)  Max. Speed (km/h)  Elevation Gain (m)  \
    0       6.306795             10.570933          14.289048           10.160000   
    1      14.022177             10.418734          14.513914           80.071429   

       Elevation Loss (m)  Max. Heart Rate (bpm)  Average Heart Rate (bpm)  \
    0                15.8             165.280000                150.000000   
    1                86.5             172.357143                157.928571   

          Calories     VO2max  Aerobic Training Effect  Avg. Run Cadence  \
    0   513.340000  51.100000                 3.306000        159.197000   
    1  1175.714286  50.928571                 4.192857        162.057143   

       Max. Run Cadence  Stride Length     Steps  duration_sec  
    0        177.540000     110.536000   5746.32   2149.920000  
    1        177.071429     107.351429  13131.00   4868.285714  
:::
:::

::: {.cell .markdown}
Apparently, we have two clusters: one for short runs, with very little
elevation gain and loss, which seem to be recovery runs or easy runs. On
the other hand, we have longer runs that have a bigger elevation change.
This makes a lot of sense! Let\'s plot
:::

::: {.cell .code execution_count="145"}
``` python
# Change back to minutes instead of seconds for better readibility
mins = df_clean['duration_sec'] / 60

plt.figure(figsize=(8,6))
plt.scatter(mins, df_clean['Distance (km)'], c=df_clean['cluster'], cmap='Set1', alpha=0.7)
plt.xlabel('Duration (min)')
plt.ylabel('Distance (km)')
plt.title('Running clusters')
plt.show()
```

::: {.output .display_data}
![](vertopal_23ee9b4a2eb5440eae219cceb0059e7b/1d91c8def3a6b269efeff48bbcc943fb73a8d43e.png)
:::
:::
