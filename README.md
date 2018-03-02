
# PYMACEUTICALS

#### Data Oberservations
* The Capomulin treatment decreased cancer tumor volume by 17% over a 45 day period in mice trials.
* In all drug trials, metastatic sites increase over the 45 day period, however, with use of the medication Infubinol metastatic sites started to decrease at Day 30. 
* Survival rate of the mice was highest with the medication Capomulin.

### Data Import & Checks


```python
#import dependencies
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
#read in csvs
clinical_trial_df = pd.read_csv("clinicaltrial_data.csv")
mouse_data_df = pd.read_csv("mouse_drug_data.csv")
```


```python
#check data
clinical_trial_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Mouse ID</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>b128</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>f932</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>g107</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>a457</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>c819</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#check data
mouse_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Mouse ID</th>
      <th>Drug</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>f234</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>1</th>
      <td>x402</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>2</th>
      <td>a492</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>3</th>
      <td>w540</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>4</th>
      <td>v764</td>
      <td>Stelasyn</td>
    </tr>
  </tbody>
</table>
</div>




```python
#merge csvs on Mouse ID
cancer_df = pd.merge(mouse_data_df, clinical_trial_df, on = "Mouse ID", how = "outer")

#check dataframe
cancer_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Mouse ID</th>
      <th>Drug</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>f234</td>
      <td>Stelasyn</td>
      <td>0</td>
      <td>45.000000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>f234</td>
      <td>Stelasyn</td>
      <td>5</td>
      <td>47.313491</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>f234</td>
      <td>Stelasyn</td>
      <td>10</td>
      <td>47.904324</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>f234</td>
      <td>Stelasyn</td>
      <td>15</td>
      <td>48.735197</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>f234</td>
      <td>Stelasyn</td>
      <td>20</td>
      <td>51.112713</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



### Tumor Volume Response to Treatment


```python
#create separate dataframe with needed information
tumor_response = cancer_df.loc[:, ["Drug","Timepoint", "Tumor Volume (mm3)"]]

#drop unneeded drugs
tumor_response = tumor_response.loc[(tumor_response["Drug"] == "Capomulin")| 
                                    (tumor_response["Drug"] == "Infubinol")|
                                    (tumor_response["Drug"] == "Ketapril")|
                                    (tumor_response["Drug"] == "Placebo")]

#group by drug type and timepoint
tumor_response = tumor_response.groupby(["Drug", "Timepoint"])

#tumor volume central tendenacies 
tumor_central = tumor_response.median()
tumor_central = tumor_central.unstack(level = 0)
tumor_central
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">Tumor Volume (mm3)</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Placebo</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>45.597064</td>
      <td>46.877243</td>
      <td>47.059664</td>
      <td>46.989764</td>
    </tr>
    <tr>
      <th>10</th>
      <td>43.421014</td>
      <td>49.471244</td>
      <td>49.797416</td>
      <td>49.109721</td>
    </tr>
    <tr>
      <th>15</th>
      <td>42.798160</td>
      <td>51.265440</td>
      <td>52.246310</td>
      <td>51.271314</td>
    </tr>
    <tr>
      <th>20</th>
      <td>40.716428</td>
      <td>53.862724</td>
      <td>54.250054</td>
      <td>53.006865</td>
    </tr>
    <tr>
      <th>25</th>
      <td>40.224165</td>
      <td>55.924633</td>
      <td>56.957917</td>
      <td>57.106418</td>
    </tr>
    <tr>
      <th>30</th>
      <td>39.260371</td>
      <td>59.133640</td>
      <td>60.296505</td>
      <td>59.916934</td>
    </tr>
    <tr>
      <th>35</th>
      <td>38.360455</td>
      <td>60.722723</td>
      <td>62.539154</td>
      <td>62.970450</td>
    </tr>
    <tr>
      <th>40</th>
      <td>36.843898</td>
      <td>63.344283</td>
      <td>66.229606</td>
      <td>66.287744</td>
    </tr>
    <tr>
      <th>45</th>
      <td>37.311846</td>
      <td>66.083066</td>
      <td>69.872251</td>
      <td>69.042841</td>
    </tr>
  </tbody>
</table>
</div>




```python
#format dataframe for plotting
tumor_timepoint = tumor_central.reset_index()
tumor_timepoint.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>Timepoint</th>
      <th colspan="4" halign="left">Tumor Volume (mm3)</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th></th>
      <th>Capomulin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Placebo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>45.597064</td>
      <td>46.877243</td>
      <td>47.059664</td>
      <td>46.989764</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>43.421014</td>
      <td>49.471244</td>
      <td>49.797416</td>
      <td>49.109721</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15</td>
      <td>42.798160</td>
      <td>51.265440</td>
      <td>52.246310</td>
      <td>51.271314</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20</td>
      <td>40.716428</td>
      <td>53.862724</td>
      <td>54.250054</td>
      <td>53.006865</td>
    </tr>
  </tbody>
</table>
</div>




```python
#create plot
cap, = plt.plot(tumor_timepoint["Timepoint"], 
                tumor_timepoint["Tumor Volume (mm3)"]["Capomulin"], linestyle = ":", 
                linewidth= 1 , label= "Capomulin", color = "#006d2c", marker = "s")

inf, = plt.plot(tumor_timepoint["Timepoint"], 
                tumor_timepoint["Tumor Volume (mm3)"]["Infubinol"], linestyle = ":", 
                linewidth = 1 ,label = "Infubinol", color = "#2ca25f", marker = "s")

ket, = plt.plot(tumor_timepoint["Timepoint"], 
                tumor_timepoint["Tumor Volume (mm3)"]["Ketapril"], linewidth = 1, 
                label = "Ketapril", color = "#66c2a4", marker = "s")

pla, = plt.plot(tumor_timepoint["Timepoint"], 
                tumor_timepoint["Tumor Volume (mm3)"]["Placebo"], linewidth = 1, 
                label = "Placebo", color = "#bdbdbd", marker = "s")

plt.title("Tumor Response to Treatment")
plt.xlabel("Time (Days)")
plt.ylabel("Tumor Volume (mm3)")
plt.grid(True)
plt.legend(handles = [cap,inf,ket,pla], loc = "best")
plt.show()
```


![png](output_11_0.png)


### Metastatic Site Response to Treatment


```python
#create separate dataframe with needed information
metastatic_response = cancer_df.loc[:, ["Drug","Timepoint", "Metastatic Sites"]]

#drop unneeded drugs
metastatic_response = metastatic_response.loc[(metastatic_response["Drug"] == "Capomulin")| 
                                              (metastatic_response["Drug"] == "Infubinol")|
                                              (metastatic_response["Drug"] == "Ketapril")|
                                              (metastatic_response["Drug"] == "Placebo")]

#group by drug type and timepoint
metastatic_group = metastatic_response.groupby(["Drug", "Timepoint"])

#tumor volume central tendenacies 
metastatic_sum = metastatic_group.sum()
metastatic_sum = metastatic_sum.unstack(level = 0)
metastatic_sum
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">Metastatic Sites</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Placebo</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>4</td>
      <td>7</td>
      <td>7</td>
      <td>9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>8</td>
      <td>14</td>
      <td>13</td>
      <td>20</td>
    </tr>
    <tr>
      <th>15</th>
      <td>9</td>
      <td>19</td>
      <td>16</td>
      <td>25</td>
    </tr>
    <tr>
      <th>20</th>
      <td>15</td>
      <td>21</td>
      <td>23</td>
      <td>29</td>
    </tr>
    <tr>
      <th>25</th>
      <td>18</td>
      <td>23</td>
      <td>31</td>
      <td>33</td>
    </tr>
    <tr>
      <th>30</th>
      <td>24</td>
      <td>27</td>
      <td>37</td>
      <td>34</td>
    </tr>
    <tr>
      <th>35</th>
      <td>26</td>
      <td>20</td>
      <td>39</td>
      <td>37</td>
    </tr>
    <tr>
      <th>40</th>
      <td>29</td>
      <td>21</td>
      <td>41</td>
      <td>38</td>
    </tr>
    <tr>
      <th>45</th>
      <td>31</td>
      <td>19</td>
      <td>37</td>
      <td>36</td>
    </tr>
  </tbody>
</table>
</div>




```python
#format dataframe for plotting
metastatic_timepoint = metastatic_sum.reset_index()
metastatic_timepoint.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>Timepoint</th>
      <th colspan="4" halign="left">Metastatic Sites</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th></th>
      <th>Capomulin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Placebo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>4</td>
      <td>7</td>
      <td>7</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>8</td>
      <td>14</td>
      <td>13</td>
      <td>20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15</td>
      <td>9</td>
      <td>19</td>
      <td>16</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20</td>
      <td>15</td>
      <td>21</td>
      <td>23</td>
      <td>29</td>
    </tr>
  </tbody>
</table>
</div>




```python
#create plot
cap, = plt.plot(metastatic_timepoint["Timepoint"], 
                metastatic_timepoint["Metastatic Sites"]["Capomulin"], linestyle = ":", 
                linewidth=1, label = "Capomulin", color = "#006d2c", marker = "s")

inf, = plt.plot(metastatic_timepoint["Timepoint"], 
                metastatic_timepoint["Metastatic Sites"]["Infubinol"], linestyle = ":", 
                linewidth = 1, label = 'Infubinol', color = "#2ca25f", marker = "s")

ket, = plt.plot(metastatic_timepoint["Timepoint"], 
                metastatic_timepoint["Metastatic Sites"]["Ketapril"], linestyle = ":", 
                linewidth = 1, label = "Ketapril", color = "#66c2a4", marker = "s")

pla, = plt.plot(metastatic_timepoint["Timepoint"], 
                metastatic_timepoint["Metastatic Sites"]["Placebo"], linestyle = ":", 
                linewidth = 1, label = "Placebo", color = "#bdbdbd", marker = "s")

plt.title('Metastatic Sites to Treatment')
plt.xlabel('Time (Days)')
plt.ylabel('Number of Metastatic Sites')
plt.grid(True)
plt.legend(handles=[cap,inf,ket,pla], loc='best')
plt.show()
```


![png](output_15_0.png)


### Survival Rate Response to Treatment


```python
#create separate dataframe with needed information
survival_rate = cancer_df.loc[:, ["Drug","Timepoint", "Mouse ID"]]

#drop unneeded drugs
survival_rate = survival_rate.loc[(survival_rate["Drug"] == "Capomulin")|
                                  (survival_rate["Drug"] == "Infubinol")|
                                  (survival_rate["Drug"] == "Ketapril")|
                                  (survival_rate["Drug"] == "Placebo")]

#group by drug type and timepoint
survival_rate = survival_rate.groupby(["Drug", "Timepoint"])

#count mouse ids
mouse_count = survival_rate.count()
mouse_count = mouse_count.unstack(level = 0)
mouse_count
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">Mouse ID</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Placebo</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>25</td>
      <td>25</td>
      <td>23</td>
      <td>24</td>
    </tr>
    <tr>
      <th>10</th>
      <td>25</td>
      <td>21</td>
      <td>22</td>
      <td>24</td>
    </tr>
    <tr>
      <th>15</th>
      <td>24</td>
      <td>21</td>
      <td>19</td>
      <td>20</td>
    </tr>
    <tr>
      <th>20</th>
      <td>23</td>
      <td>20</td>
      <td>19</td>
      <td>19</td>
    </tr>
    <tr>
      <th>25</th>
      <td>22</td>
      <td>18</td>
      <td>19</td>
      <td>17</td>
    </tr>
    <tr>
      <th>30</th>
      <td>22</td>
      <td>17</td>
      <td>18</td>
      <td>15</td>
    </tr>
    <tr>
      <th>35</th>
      <td>22</td>
      <td>12</td>
      <td>17</td>
      <td>14</td>
    </tr>
    <tr>
      <th>40</th>
      <td>21</td>
      <td>10</td>
      <td>15</td>
      <td>12</td>
    </tr>
    <tr>
      <th>45</th>
      <td>21</td>
      <td>9</td>
      <td>11</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#formate dataframe for plotting
mouse_timepoint = mouse_count.reset_index()
mouse_timepoint.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>Timepoint</th>
      <th colspan="4" halign="left">Mouse ID</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th></th>
      <th>Capomulin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Placebo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>25</td>
      <td>25</td>
      <td>23</td>
      <td>24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>25</td>
      <td>21</td>
      <td>22</td>
      <td>24</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15</td>
      <td>24</td>
      <td>21</td>
      <td>19</td>
      <td>20</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20</td>
      <td>23</td>
      <td>20</td>
      <td>19</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
#create plot
cap, = plt.plot(mouse_timepoint["Timepoint"], 
                mouse_timepoint["Mouse ID"]["Capomulin"], linestyle = ":", 
                linewidth = 1,label = "Capomulin", color = "#006d2c", marker = "s")

inf, = plt.plot(mouse_timepoint["Timepoint"], mouse_timepoint["Mouse ID"]["Infubinol"],
                linewidth = 1, label = "Infubinol", color = "#2ca25f", marker = "s")

ket, = plt.plot(mouse_timepoint["Timepoint"], mouse_timepoint["Mouse ID"]["Ketapril"], 
                linestyle = ":", linewidth = 1, label = "Ketapril", color = "#66c2a4", marker = "s")

pla, = plt.plot(mouse_timepoint["Timepoint"], mouse_timepoint["Mouse ID"]["Placebo"], 
                linestyle = ":", linewidth = 1, label = "Placebo", color = "#bdbdbd", marker = "s")

plt.title("Survival Rate Response to Treatment")
plt.xlabel("Time (Days)")
plt.ylabel("Number of Mice")
plt.grid(True)
plt.legend(handles=[cap,inf,ket,pla], loc = "best")
plt.show()
```


![png](output_19_0.png)


### Percent Change in Tumor Size Based on Treatment


```python
#take dataframe from tumor volume plot and calcuate percent change over timeperiod
tumor_response.head()
tumor_change = tumor_response.median()
tumor_pct_change = tumor_change.apply(lambda x: x.div(x.iloc[0]).subtract(1).mul(100))
tumor_pct_change.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Tumor Volume (mm3)</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th>Timepoint</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Capomulin</th>
      <th>0</th>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1.326808</td>
    </tr>
    <tr>
      <th>10</th>
      <td>-3.508857</td>
    </tr>
    <tr>
      <th>15</th>
      <td>-4.892979</td>
    </tr>
    <tr>
      <th>20</th>
      <td>-9.519049</td>
    </tr>
  </tbody>
</table>
</div>




```python
#format dataframe for plotting
tumor_table = tumor_pct_change.reset_index()
tumor_table.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Drug</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Capomulin</td>
      <td>0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Capomulin</td>
      <td>5</td>
      <td>1.326808</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Capomulin</td>
      <td>10</td>
      <td>-3.508857</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Capomulin</td>
      <td>15</td>
      <td>-4.892979</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Capomulin</td>
      <td>20</td>
      <td>-9.519049</td>
    </tr>
  </tbody>
</table>
</div>




```python
#pull out final percent changes
tumor_final_change = tumor_table.iloc[9]
tumor_final_change = tumor_final_change.tolist()
tumor_final_change = tumor_final_change[0:]
tumor_final_change
```




    [-17.084787168000005,
     46.851257533333325,
     55.27166843266669,
     53.428535150444475]




```python
plot_df = pd.DataFrame({"Drug": ["Capomulin", "Infubinol", "Ketapril", "Placebo"],
                       "Percent Change": tumor_final_change})

plot_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Drug</th>
      <th>Percent Change</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Capomulin</td>
      <td>-17.084787</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Infubinol</td>
      <td>46.851258</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Ketapril</td>
      <td>55.271668</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Placebo</td>
      <td>53.428535</td>
    </tr>
  </tbody>
</table>
</div>




```python
#create basic bar plot structure
axis = range(len(plot_df))

bar1 = plt.bar(0, plot_df["Percent Change"][0], color = "#41ab5d", alpha = 1, align = "edge", 
               edgecolor = "black", width=1)
bar2 = plt.bar(1, plot_df["Percent Change"][1], color = "#cb181d", alpha = 1, align = "edge", 
               edgecolor = "black", width=1)
bar3 = plt.bar(2, plot_df["Percent Change"][2], color= "#d73027", alpha = 1, align = "edge", 
               edgecolor = "black", width=1)
bar4 = plt.bar(3, plot_df["Percent Change"][3], color= "#d73027", alpha = 1, align = "edge", 
               edgecolor = "black", width=1)

tick_locations = [value + 0.5 for value in axis]
plt.xticks(tick_locations, plot_df["Drug"])
plt.xlim(0, 4)
plt.ylim(-30, 70)

#add plot labels
plt.title("Percent Change in Tumor Volume Over 45 Days")
plt.xlabel("Drug")
plt.ylabel("% Change")

def autolabel(bar):
    for bar in bar:
        height = bar.get_height()
        plt.text(bar.get_x() + bar.get_width()/2., -8,
                '%d' % int(height) + "%", 
                ha='center', va='bottom', color='white', fontsize=14)

autolabel(bar1)

def autolabel(bar):
    for bar in bar:
        height = bar.get_height()
        plt.text(bar.get_x() + bar.get_width()/2., 2,
                '%d' % int(height) + "%", 
                ha='center', va='bottom', color='white', fontsize=14)

autolabel(bar2)
autolabel(bar3)
autolabel(bar4)

plt.show()
```


![png](output_25_0.png)

