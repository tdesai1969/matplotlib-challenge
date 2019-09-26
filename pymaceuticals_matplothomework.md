

```python
# Dependencies and Setup
%matplotlib inline
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

# Hide warning messages in notebook
import warnings
warnings.filterwarnings('ignore')

# File to Load (Remember to Change These)
mouse_drug_data_to_load = "data/mouse_drug_data.csv"
clinical_trial_data_to_load = "data/clinicaltrial_data.csv"

# Read the Mouse and Drug Data and the Clinical Trial Data
mouse_drug_data_df = pd.read_csv(mouse_drug_data_to_load)
clinical_trial_data_df = pd.read_csv(clinical_trial_data_to_load)

# Combine the data into a single dataset
combined_mouse_clinical_df = pd.merge(clinical_trial_data_df,mouse_drug_data_df, how='left', on='Mouse ID')

# Display the data table for preview
combined_mouse_clinical_df.head()

```




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
      <th>Mouse ID</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
      <th>Drug</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>b128</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <td>1</td>
      <td>f932</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
      <td>Ketapril</td>
    </tr>
    <tr>
      <td>2</td>
      <td>g107</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
      <td>Ketapril</td>
    </tr>
    <tr>
      <td>3</td>
      <td>a457</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
      <td>Ketapril</td>
    </tr>
    <tr>
      <td>4</td>
      <td>c819</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
      <td>Ketapril</td>
    </tr>
  </tbody>
</table>
</div>



## Tumor Response to Treatment


```python
# Store the Mean Tumor Volume Data Grouped by Drug and Timepoint 
mean_tumor_volume = combined_mouse_clinical_df.groupby(['Drug','Timepoint'])['Tumor Volume (mm3)'].mean()

# Convert to DataFrame
mean_tumor_volume_df  = pd.DataFrame(mean_tumor_volume)
mean_tumor_volume_df

# Preview DataFrame
mean_tumor_volume_df=mean_tumor_volume_df.reset_index()
mean_tumor_volume_df.head()


```




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
      <th>Drug</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Capomulin</td>
      <td>0</td>
      <td>45.000000</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Capomulin</td>
      <td>5</td>
      <td>44.266086</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Capomulin</td>
      <td>10</td>
      <td>43.084291</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Capomulin</td>
      <td>15</td>
      <td>42.064317</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Capomulin</td>
      <td>20</td>
      <td>40.716325</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Store the Standard Error of Tumor Volumes Grouped by Drug and Timepoint
mean_tumor_volume_ste = combined_mouse_clinical_df.groupby(['Drug','Timepoint'])['Tumor Volume (mm3)'].sem()

# Convert to DataFrame

mean_tumor_volume_std_error = pd.DataFrame(mean_tumor_volume_ste).reset_index()

# Preview DataFrame

mean_tumor_volume_std_error.head()
```




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
      <th>Drug</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Capomulin</td>
      <td>0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Capomulin</td>
      <td>5</td>
      <td>0.448593</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Capomulin</td>
      <td>10</td>
      <td>0.702684</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Capomulin</td>
      <td>15</td>
      <td>0.838617</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Capomulin</td>
      <td>20</td>
      <td>0.909731</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Minor Data Munging to Re-Format the Data Frames

mean_tumor_volume_pivot_df = mean_tumor_volume_df.pivot_table('Tumor Volume (mm3)', ['Timepoint'], 'Drug')[["Capomulin", "Infubinol", "Ketapril","Placebo"]]

# Preview that Reformatting worked

mean_tumor_volume_pivot_df


```




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
      <td>0</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
    </tr>
    <tr>
      <td>5</td>
      <td>44.266086</td>
      <td>47.062001</td>
      <td>47.389175</td>
      <td>47.125589</td>
    </tr>
    <tr>
      <td>10</td>
      <td>43.084291</td>
      <td>49.403909</td>
      <td>49.582269</td>
      <td>49.423329</td>
    </tr>
    <tr>
      <td>15</td>
      <td>42.064317</td>
      <td>51.296397</td>
      <td>52.399974</td>
      <td>51.359742</td>
    </tr>
    <tr>
      <td>20</td>
      <td>40.716325</td>
      <td>53.197691</td>
      <td>54.920935</td>
      <td>54.364417</td>
    </tr>
    <tr>
      <td>25</td>
      <td>39.939528</td>
      <td>55.715252</td>
      <td>57.678982</td>
      <td>57.482574</td>
    </tr>
    <tr>
      <td>30</td>
      <td>38.769339</td>
      <td>58.299397</td>
      <td>60.994507</td>
      <td>59.809063</td>
    </tr>
    <tr>
      <td>35</td>
      <td>37.816839</td>
      <td>60.742461</td>
      <td>63.371686</td>
      <td>62.420615</td>
    </tr>
    <tr>
      <td>40</td>
      <td>36.958001</td>
      <td>63.162824</td>
      <td>66.068580</td>
      <td>65.052675</td>
    </tr>
    <tr>
      <td>45</td>
      <td>36.236114</td>
      <td>65.755562</td>
      <td>70.662958</td>
      <td>68.084082</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Minor Data Munging to Re-Format the Data Frames

mean_tumor_volume_pivot_error_df=mean_tumor_volume_std_error.pivot_table('Tumor Volume (mm3)', ['Timepoint'], 'Drug')[["Capomulin", "Infubinol", "Ketapril","Placebo"]]


# Preview that Reformatting worked

mean_tumor_volume_pivot_error_df.head()

```




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
      <td>0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>5</td>
      <td>0.448593</td>
      <td>0.235102</td>
      <td>0.264819</td>
      <td>0.218091</td>
    </tr>
    <tr>
      <td>10</td>
      <td>0.702684</td>
      <td>0.282346</td>
      <td>0.357421</td>
      <td>0.402064</td>
    </tr>
    <tr>
      <td>15</td>
      <td>0.838617</td>
      <td>0.357705</td>
      <td>0.580268</td>
      <td>0.614461</td>
    </tr>
    <tr>
      <td>20</td>
      <td>0.909731</td>
      <td>0.476210</td>
      <td>0.726484</td>
      <td>0.839609</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Generate the Plot (with Error Bars)

plt.errorbar(mean_tumor_volume_pivot_df.index, mean_tumor_volume_pivot_df['Capomulin'], 
             yerr=mean_tumor_volume_pivot_error_df['Capomulin'],
             color='red', marker='+', markersize=5, linestyle='-', linewidth=1,label='Capomulin')
plt.errorbar(mean_tumor_volume_pivot_df.index, mean_tumor_volume_pivot_df['Infubinol'], 
             yerr=mean_tumor_volume_pivot_error_df['Infubinol'],
             color='blue', marker='o', markersize=5, linestyle='-', linewidth=1,label='Infubinol')
plt.errorbar(mean_tumor_volume_pivot_df.index, mean_tumor_volume_pivot_df['Ketapril'], 
             yerr=mean_tumor_volume_pivot_error_df['Ketapril'],
             color='green', marker='x', markersize=5, linestyle='-', linewidth=1,label='Ketapril')
plt.errorbar(mean_tumor_volume_pivot_df.index, mean_tumor_volume_pivot_df['Placebo'], 
             yerr=mean_tumor_volume_pivot_error_df['Placebo'],
             color='black', marker='^', markersize=5, linestyle='-', linewidth=1,label='Placebo')

# Create labels for the X and Y axis

plt.xlabel("Time(Days)")
plt.ylabel("Tumor Volume (mm3")
plt.title("Tumor Response to Treatment")
plt.xlim(0,50)
plt.ylim(35,75)
plt.grid()

# Set our legend to where the chart thinks is best
plt.legend(loc='best')


# Save the Figure
plt.savefig("../Pymaceuticals/Images/tumor_response.png")

# Show the Figure
plt.show()
```


![png](output_6_0.png)


![Tumor Response to Treatment](../Images/treatment.png)

## Metastatic Response to Treatment


```python
# Store the Mean Met. Site Data Grouped by Drug and Timepoint 

mean_met_site = combined_mouse_clinical_df.groupby(['Drug','Timepoint'])['Metastatic Sites'].mean()
mean_met_site

# Convert to DataFrame

mean_met_site_df = pd.DataFrame(mean_met_site)

# Preview DataFrame

mean_met_site_df.head()
```




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
      <th></th>
      <th>Metastatic Sites</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th>Timepoint</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="5" valign="top">Capomulin</td>
      <td>0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>5</td>
      <td>0.160000</td>
    </tr>
    <tr>
      <td>10</td>
      <td>0.320000</td>
    </tr>
    <tr>
      <td>15</td>
      <td>0.375000</td>
    </tr>
    <tr>
      <td>20</td>
      <td>0.652174</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Store the Standard Error associated with Met. Sites Grouped by Drug and Timepoint 

mean_met_site_err = combined_mouse_clinical_df.groupby(['Drug','Timepoint'])['Metastatic Sites'].sem()
mean_met_site_err


# Convert to DataFrame

mean_met_site_err_df = pd.DataFrame(mean_met_site_err)

# Preview DataFrame

mean_met_site_err_df
```




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
      <th></th>
      <th>Metastatic Sites</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th>Timepoint</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="5" valign="top">Capomulin</td>
      <td>0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>5</td>
      <td>0.074833</td>
    </tr>
    <tr>
      <td>10</td>
      <td>0.125433</td>
    </tr>
    <tr>
      <td>15</td>
      <td>0.132048</td>
    </tr>
    <tr>
      <td>20</td>
      <td>0.161621</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td rowspan="5" valign="top">Zoniferol</td>
      <td>25</td>
      <td>0.236621</td>
    </tr>
    <tr>
      <td>30</td>
      <td>0.248168</td>
    </tr>
    <tr>
      <td>35</td>
      <td>0.285714</td>
    </tr>
    <tr>
      <td>40</td>
      <td>0.299791</td>
    </tr>
    <tr>
      <td>45</td>
      <td>0.286400</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 1 columns</p>
</div>




```python
# Minor Data Munging to Re-Format the Data Frames

rows_to_column_met_pivot = mean_met_site_df.pivot_table('Metastatic Sites', ['Timepoint'], 'Drug')[["Capomulin", "Infubinol", "Ketapril","Placebo"]]
mean_met_site_pivot_df = pd.DataFrame(rows_to_column_met_pivot)


# Preview that Reformatting worked
mean_met_site_pivot_df.head()
```




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
      <td>0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>5</td>
      <td>0.160000</td>
      <td>0.280000</td>
      <td>0.304348</td>
      <td>0.375000</td>
    </tr>
    <tr>
      <td>10</td>
      <td>0.320000</td>
      <td>0.666667</td>
      <td>0.590909</td>
      <td>0.833333</td>
    </tr>
    <tr>
      <td>15</td>
      <td>0.375000</td>
      <td>0.904762</td>
      <td>0.842105</td>
      <td>1.250000</td>
    </tr>
    <tr>
      <td>20</td>
      <td>0.652174</td>
      <td>1.050000</td>
      <td>1.210526</td>
      <td>1.526316</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Minor Data Munging to Re-Format the Data Frames

rows_to_column_met_err_pivot = mean_met_site_err_df.pivot_table('Metastatic Sites', ['Timepoint'], 'Drug')[["Capomulin", "Infubinol", "Ketapril","Placebo"]]
mean_met_site_pivot_err_df = pd.DataFrame(rows_to_column_met_err_pivot)


# Preview that Reformatting worked
mean_met_site_pivot_err_df.head()
```




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
      <td>0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>5</td>
      <td>0.074833</td>
      <td>0.091652</td>
      <td>0.098100</td>
      <td>0.100947</td>
    </tr>
    <tr>
      <td>10</td>
      <td>0.125433</td>
      <td>0.159364</td>
      <td>0.142018</td>
      <td>0.115261</td>
    </tr>
    <tr>
      <td>15</td>
      <td>0.132048</td>
      <td>0.194015</td>
      <td>0.191381</td>
      <td>0.190221</td>
    </tr>
    <tr>
      <td>20</td>
      <td>0.161621</td>
      <td>0.234801</td>
      <td>0.236680</td>
      <td>0.234064</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Generate the Plot (with Error Bars)

plt.errorbar(mean_met_site_pivot_df.index, mean_met_site_pivot_df['Capomulin'], yerr=mean_met_site_pivot_err_df['Capomulin'],
             color='red', marker='+', markersize=5, linestyle='-', linewidth=1,label='Capomulin')
plt.errorbar(mean_met_site_pivot_df.index, mean_met_site_pivot_df['Infubinol'], yerr=mean_met_site_pivot_err_df['Infubinol'],
             color='blue', marker='o', markersize=5, linestyle='-', linewidth=1,label='Infubinol')
plt.errorbar(mean_met_site_pivot_df.index, mean_met_site_pivot_df['Ketapril'], yerr=mean_met_site_pivot_err_df['Ketapril'],
             color='green', marker='x', markersize=5, linestyle='-', linewidth=1,label='Ketapril')
plt.errorbar(mean_met_site_pivot_df.index, mean_met_site_pivot_df['Placebo'], yerr=mean_met_site_pivot_err_df['Placebo'],
             color='black', marker='^', markersize=5, linestyle='-', linewidth=1,label='Placebo')

# Create labels,Limits for the X and Y axis

plt.xlabel("Treatment Duration (Days)")
plt.ylabel("Met.Sites")
plt.title("Matastatic Spread During Treatment")
plt.xlim(0,50)
plt.ylim(0,4)
plt.grid()
plt.legend(loc="best")

# Save the Figure

plt.savefig("../Pymaceuticals/Images/Matastatic_Spread.png")
# Show the Figure
plt.show()

```


![png](output_13_0.png)


![Metastatic Spread During Treatment](../Images/spread.png)

## Survival Rates


```python
# Store the Count of Mice Grouped by Drug and Timepoint (W can pass any metric)

mice_count = combined_mouse_clinical_df.groupby(['Drug','Timepoint'])['Mouse ID'].count()
mice_count

# Convert to DataFrame

mice_count_df = pd.DataFrame(mice_count).reset_index()

# Preview DataFrame

mice_count_df.head()

```




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
      <th>Drug</th>
      <th>Timepoint</th>
      <th>Mouse ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Capomulin</td>
      <td>0</td>
      <td>25</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Capomulin</td>
      <td>5</td>
      <td>25</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Capomulin</td>
      <td>10</td>
      <td>25</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Capomulin</td>
      <td>15</td>
      <td>24</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Capomulin</td>
      <td>20</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Minor Data Munging to Re-Format the Data Frames

rows_to_column_mice_pivot = mice_count_df.pivot_table('Mouse ID', ['Timepoint'], 'Drug')\
[["Capomulin", "Infubinol", "Ketapril","Placebo"]]

mice_count_format_df = pd.DataFrame(rows_to_column_mice_pivot)


# Preview the Data Frame
mice_count_format_df
```




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
      <td>0</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
    </tr>
    <tr>
      <td>5</td>
      <td>25</td>
      <td>25</td>
      <td>23</td>
      <td>24</td>
    </tr>
    <tr>
      <td>10</td>
      <td>25</td>
      <td>21</td>
      <td>22</td>
      <td>24</td>
    </tr>
    <tr>
      <td>15</td>
      <td>24</td>
      <td>21</td>
      <td>19</td>
      <td>20</td>
    </tr>
    <tr>
      <td>20</td>
      <td>23</td>
      <td>20</td>
      <td>19</td>
      <td>19</td>
    </tr>
    <tr>
      <td>25</td>
      <td>22</td>
      <td>18</td>
      <td>19</td>
      <td>17</td>
    </tr>
    <tr>
      <td>30</td>
      <td>22</td>
      <td>17</td>
      <td>18</td>
      <td>15</td>
    </tr>
    <tr>
      <td>35</td>
      <td>22</td>
      <td>12</td>
      <td>17</td>
      <td>14</td>
    </tr>
    <tr>
      <td>40</td>
      <td>21</td>
      <td>10</td>
      <td>15</td>
      <td>12</td>
    </tr>
    <tr>
      <td>45</td>
      <td>21</td>
      <td>9</td>
      <td>11</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
mice_count_format_first= mice_count_format_df.iloc[0,1]

# Convert mice_count_format_df to percentage
percentage_mice_count= (mice_count_format_df/mice_count_format_first)*100
percentage_mice_count
```




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
      <td>0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <td>5</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>92.0</td>
      <td>96.0</td>
    </tr>
    <tr>
      <td>10</td>
      <td>100.0</td>
      <td>84.0</td>
      <td>88.0</td>
      <td>96.0</td>
    </tr>
    <tr>
      <td>15</td>
      <td>96.0</td>
      <td>84.0</td>
      <td>76.0</td>
      <td>80.0</td>
    </tr>
    <tr>
      <td>20</td>
      <td>92.0</td>
      <td>80.0</td>
      <td>76.0</td>
      <td>76.0</td>
    </tr>
    <tr>
      <td>25</td>
      <td>88.0</td>
      <td>72.0</td>
      <td>76.0</td>
      <td>68.0</td>
    </tr>
    <tr>
      <td>30</td>
      <td>88.0</td>
      <td>68.0</td>
      <td>72.0</td>
      <td>60.0</td>
    </tr>
    <tr>
      <td>35</td>
      <td>88.0</td>
      <td>48.0</td>
      <td>68.0</td>
      <td>56.0</td>
    </tr>
    <tr>
      <td>40</td>
      <td>84.0</td>
      <td>40.0</td>
      <td>60.0</td>
      <td>48.0</td>
    </tr>
    <tr>
      <td>45</td>
      <td>84.0</td>
      <td>36.0</td>
      <td>44.0</td>
      <td>44.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Generate the Plot (Accounting for percentages)

plt.errorbar(percentage_mice_count.index, percentage_mice_count['Capomulin'], 
             color='red', marker='+', markersize=5, linestyle='-', linewidth=1,label='Capomulin')

plt.errorbar(percentage_mice_count.index, percentage_mice_count['Infubinol'], 
             color='blue', marker='o', markersize=5, linestyle='-', linewidth=1,label='Infubinol')

plt.errorbar(percentage_mice_count.index, percentage_mice_count['Ketapril'], 
             color='green', marker='x', markersize=5, linestyle='-', linewidth=1,label='Ketapril')

plt.errorbar(percentage_mice_count.index, percentage_mice_count['Placebo'], 
             color='black', marker='^', markersize=5, linestyle='-', linewidth=1,label='Placebo')

# Create labels,Limits for the X and Y axis, Chart title

plt.title("Survival During Treatment")
plt.xlabel("Treatment Duration (Days)")
plt.ylabel("Survival Rate (%)")
plt.grid()
plt.legend(loc='best')

# Save the Figure
plt.savefig("../Pymaceuticals/Images/survival_treatment.png")

# Show the Figure
plt.show()
```


![png](output_19_0.png)


![Metastatic Spread During Treatment](../Images/survival.png)

## Summary Bar Graph


```python
# Calculate the percent changes for each drug
change_tumor_volume_first= mean_tumor_volume_pivot_df.iloc[0,:]
#print(change_tumor_volume_first)
change_tumor_volume_end= mean_tumor_volume_pivot_df.iloc[-1,:]
#print(change_tumor_volume_end)


percentage_change_tumor_volume= ((change_tumor_volume_end-change_tumor_volume_first)/change_tumor_volume_first)*100


# Display the data to confirm
round(percentage_change_tumor_volume,5)
```




    Drug
    Capomulin   -19.47530
    Infubinol    46.12347
    Ketapril     57.02879
    Placebo      51.29796
    dtype: float64




```python
percentage_change_tumor_volume.index
drug=tuple(percentage_change_tumor_volume.index)
```


```python
value = list(percentage_change_tumor_volume)
value
value2=[]
[ ]
for x in value:
    number=round(x,2)
    value2.append(number)
value2
value3 =tuple(value2)
value3
```




    (-19.48, 46.12, 57.03, 51.3)




```python
# Store all Relevant Percent Changes into a Tuple
# Helped from TA in order to get the values in the graph

print(drug)
print(value3)


# Splice the data between passing and failing drugs

plt.bar(drug, value3, color=["green","red","red","red"], align="center")


# Orient widths. Add labels, tick marks, etc. 
tick_locations = [value for value in drug]
plt.xticks(tick_locations, drug)
plt.xlim(-0.50, len(drug)-0.50)
plt.ylim(-25, max(value)+5.0)

plt.title("Tumor Change Over 45 Day Treatment")
plt.ylabel("% Tumor Volume Change")
plt.grid()


# Use functions to label the percentages of changes
for a,b in zip(drug, value3):
    plt.text(a, b+1, str(b))

# Call functions to implement the function calls


# Save the Figure

plt.savefig("../Pymaceuticals/Images/tumor_change.png")
# Show the Figure
plt.show()
```

    ('Capomulin', 'Infubinol', 'Ketapril', 'Placebo')
    (-19.48, 46.12, 57.03, 51.3)
    


![png](output_25_1.png)


![Metastatic Spread During Treatment](../Images/change.png)
