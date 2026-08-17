****Respiratory Medical Equipment Sales****


```python
import pandas as pd
import numpy as np

# Import and read the file
df = pd.read_csv('/Users/.../respiratory_medical_equipment_sales.csv')
df
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
      <th>Order_ID</th>
      <th>Order_Date</th>
      <th>Device_Type</th>
      <th>Manufacturer_Vendor</th>
      <th>Hospital_Name</th>
      <th>Hospital_Type</th>
      <th>Department</th>
      <th>Region</th>
      <th>Quantity_Sold</th>
      <th>Unit_Cost</th>
      <th>...</th>
      <th>Total_Cost</th>
      <th>Revenue</th>
      <th>Gross_Profit</th>
      <th>Service_Maintenance_Cost</th>
      <th>Net_Profit</th>
      <th>Profit_Margin_Pct</th>
      <th>Contract_Type</th>
      <th>Sales_Channel</th>
      <th>Warranty_Years</th>
      <th>Sales_Representative</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ORD-2024-1001</td>
      <td>2024-01-02</td>
      <td>Patient Monitor</td>
      <td>Mindray</td>
      <td>Eastern Province Advanced Care Clinic</td>
      <td>Specialized Center</td>
      <td>PICU</td>
      <td>Dammam</td>
      <td>6</td>
      <td>6331.42</td>
      <td>...</td>
      <td>37988.52</td>
      <td>47636.28</td>
      <td>9647.76</td>
      <td>2452.98</td>
      <td>7194.78</td>
      <td>15.10</td>
      <td>Annual Contract</td>
      <td>Direct Sales</td>
      <td>3</td>
      <td>Khaled Al-Shehri</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ORD-2024-1002</td>
      <td>2024-01-02</td>
      <td>HFNC</td>
      <td>Fisher &amp; Paykel</td>
      <td>King Fahd Hospital</td>
      <td>Government</td>
      <td>ICU</td>
      <td>Madinah</td>
      <td>6</td>
      <td>5600.43</td>
      <td>...</td>
      <td>33602.58</td>
      <td>45681.48</td>
      <td>12078.90</td>
      <td>2182.44</td>
      <td>9896.46</td>
      <td>21.66</td>
      <td>Annual Contract</td>
      <td>Direct Sales</td>
      <td>2</td>
      <td>Fahad Al-Otaibi</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ORD-2024-1003</td>
      <td>2024-01-02</td>
      <td>Patient Monitor</td>
      <td>GE Healthcare</td>
      <td>Prince Sultan Military Medical City</td>
      <td>Government</td>
      <td>NICU</td>
      <td>Riyadh</td>
      <td>1</td>
      <td>5313.75</td>
      <td>...</td>
      <td>5313.75</td>
      <td>6941.66</td>
      <td>1627.91</td>
      <td>553.68</td>
      <td>1074.23</td>
      <td>15.48</td>
      <td>Government Tender</td>
      <td>Government Tender</td>
      <td>3</td>
      <td>Sara Al-Ghamdi</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ORD-2024-1004</td>
      <td>2024-01-05</td>
      <td>Suction Machine</td>
      <td>AtmOS</td>
      <td>Saudi German Hospital</td>
      <td>Private</td>
      <td>ER</td>
      <td>Jeddah</td>
      <td>5</td>
      <td>1042.35</td>
      <td>...</td>
      <td>5211.75</td>
      <td>6962.75</td>
      <td>1751.00</td>
      <td>228.49</td>
      <td>1522.51</td>
      <td>21.87</td>
      <td>Annual Contract</td>
      <td>Direct Sales</td>
      <td>1</td>
      <td>Omar Al-Zahrani</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ORD-2024-1005</td>
      <td>2024-01-09</td>
      <td>BiPAP</td>
      <td>Philips Respironics</td>
      <td>King Abdulaziz Medical City</td>
      <td>Government</td>
      <td>ICU</td>
      <td>Jeddah</td>
      <td>13</td>
      <td>4198.11</td>
      <td>...</td>
      <td>54575.43</td>
      <td>71068.66</td>
      <td>16493.23</td>
      <td>3184.34</td>
      <td>13308.89</td>
      <td>18.73</td>
      <td>Annual Contract</td>
      <td>Direct Sales</td>
      <td>2</td>
      <td>Reem Al-Dossary</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>495</th>
      <td>ORD-2024-1496</td>
      <td>2026-06-15</td>
      <td>HFNC</td>
      <td>Dräger</td>
      <td>Riyadh Respiratory Care Center</td>
      <td>Specialized Center</td>
      <td>NICU</td>
      <td>Riyadh</td>
      <td>19</td>
      <td>6957.40</td>
      <td>...</td>
      <td>132190.60</td>
      <td>173423.83</td>
      <td>41233.23</td>
      <td>11583.01</td>
      <td>29650.22</td>
      <td>17.10</td>
      <td>One-Time Purchase</td>
      <td>Direct Sales</td>
      <td>2</td>
      <td>Fahad Al-Otaibi</td>
    </tr>
    <tr>
      <th>496</th>
      <td>ORD-2024-1497</td>
      <td>2026-06-16</td>
      <td>BiPAP</td>
      <td>Philips Respironics</td>
      <td>King Faisal Specialist Hospital &amp; Research Centre</td>
      <td>Government</td>
      <td>Home Care</td>
      <td>Riyadh</td>
      <td>13</td>
      <td>4349.87</td>
      <td>...</td>
      <td>56548.31</td>
      <td>72701.20</td>
      <td>16152.89</td>
      <td>3856.57</td>
      <td>12296.32</td>
      <td>16.91</td>
      <td>Government Tender</td>
      <td>Government Tender</td>
      <td>5</td>
      <td>Omar Al-Zahrani</td>
    </tr>
    <tr>
      <th>497</th>
      <td>ORD-2024-1498</td>
      <td>2026-06-18</td>
      <td>HFNC</td>
      <td>Dräger</td>
      <td>Mouwasat Hospital</td>
      <td>Private</td>
      <td>Pulmonology</td>
      <td>Dammam</td>
      <td>5</td>
      <td>6935.13</td>
      <td>...</td>
      <td>34675.65</td>
      <td>46989.45</td>
      <td>12313.80</td>
      <td>2477.10</td>
      <td>9836.70</td>
      <td>20.93</td>
      <td>Annual Contract</td>
      <td>Direct Sales</td>
      <td>1</td>
      <td>Sara Al-Ghamdi</td>
    </tr>
    <tr>
      <th>498</th>
      <td>ORD-2024-1499</td>
      <td>2026-06-21</td>
      <td>Nebulizer</td>
      <td>Omron Medical</td>
      <td>Saudi German Hospital</td>
      <td>Private</td>
      <td>ER</td>
      <td>Jeddah</td>
      <td>32</td>
      <td>357.39</td>
      <td>...</td>
      <td>11436.48</td>
      <td>15668.48</td>
      <td>4232.00</td>
      <td>753.06</td>
      <td>3478.94</td>
      <td>22.20</td>
      <td>Annual Contract</td>
      <td>Direct Sales</td>
      <td>2</td>
      <td>Sara Al-Ghamdi</td>
    </tr>
    <tr>
      <th>499</th>
      <td>ORD-2024-1500</td>
      <td>2026-06-25</td>
      <td>HFNC</td>
      <td>Vapotherm</td>
      <td>Dr. Sulaiman Al Habib Hospital</td>
      <td>Private</td>
      <td>NICU</td>
      <td>Riyadh</td>
      <td>2</td>
      <td>4753.04</td>
      <td>...</td>
      <td>9506.08</td>
      <td>12658.02</td>
      <td>3151.94</td>
      <td>973.99</td>
      <td>2177.95</td>
      <td>17.21</td>
      <td>Rental</td>
      <td>Direct Sales</td>
      <td>1</td>
      <td>Fahad Al-Otaibi</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 21 columns</p>
</div>



**Simple data cleaning or check**


```python
# Null values
null_summary = df.isnull().sum()
total_nulls = null_summary.sum()

print('Missing data assessment')
if total_nulls == 0:
    print('Results: 0 missing data detected')
else:
    print('Results: ' + str(total_nulls) + ' missing data detected')

num_cols = df.select_dtypes(include=[np.number]).columns
cat_cols = df.select_dtypes(include=['object']).columns

df[num_cols] = df[num_cols].fillna(df[num_cols].median())
df[cat_cols] = df[cat_cols].fillna('Unknown')

print("Automated imputation: Median for numeric features, 'Unknown' for categories.")

```

    Missing data assessment
    Results: 0 missing data detected
    Automated imputation: Median for numeric features, 'Unknown' for categories.



```python
#Dulblicate check
duplicates = df.duplicated().sum()
print('\n Dublicates check ')
if duplicates == 0:
    print('Result: 0 duplicate detected.')
else:
    print('Warning:' + str(duplicates) + ' duplicate detected.')
    df.drop_duplicates(inplace=True)
    print('Duplicates removed successfully.')
```

    
     Dublicates check 
    Result: 0 duplicate detected.



```python
# Data type and business logic validation
print('\n Data type and logic validation')
df['Order_Date'] = pd.to_datetime(df['Order_Date'])

#Revenue logic validation: (Quantity * price) == revenue
calculate_rev = round(df['Quantity_Sold']*df['Unit_Selling_Price'], 2)
mismatches = (df['Revenue'] != calculate_rev).sum()

print(f"Date Column Converted to Datetime: {df['Order_Date'].dtype}")
print(f"Business Rule Validation (Revenue calculation mismatches): {mismatches}")
```

    
     Data type and logic validation
    Date Column Converted to Datetime: datetime64[ns]
    Business Rule Validation (Revenue calculation mismatches): 0


***Export clean file***


```python
import os

output_path = 'data/processed_respiratory_sales.csv'
os.makedirs(os.path.dirname(output_path), exist_ok=True)

#Save file
df.to_csv(output_path, index=False)
print(f"\nAudit Complete! Clean file saved to '{output_path}'.")
```

    
    Audit Complete! Clean file saved to 'data/processed_respiratory_sales.csv'.

