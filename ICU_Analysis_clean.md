Deal with the multiple files


```python
import os
import pandas as pd

patients = pd.read_csv('/Users/.../mimic-iii-clinical-database-demo-1.4/PATIENTS.csv')
admissions = pd.read_csv('/Users/.../mimic-iii-clinical-database-demo-1.4/ADMISSIONS.csv')
icustays = pd.read_csv('/Users/.../mimic-iii-clinical-database-demo-1.4/ICUSTAYS.csv')

print(patients.shape, admissions.shape, icustays.shape)
print(
    patients.columns.tolist(),
    admissions.columns.tolist(),
    icustays.columns.tolist())
```

    (100, 8) (129, 19) (136, 12)
    ['row_id', 'subject_id', 'gender', 'dob', 'dod', 'dod_hosp', 'dod_ssn', 'expire_flag'] ['row_id', 'subject_id', 'hadm_id', 'admittime', 'dischtime', 'deathtime', 'admission_type', 'admission_location', 'discharge_location', 'insurance', 'language', 'religion', 'marital_status', 'ethnicity', 'edregtime', 'edouttime', 'diagnosis', 'hospital_expire_flag', 'has_chartevents_data'] ['row_id', 'subject_id', 'hadm_id', 'icustay_id', 'dbsource', 'first_careunit', 'last_careunit', 'first_wardid', 'last_wardid', 'intime', 'outtime', 'los']



```python
df = admissions.merge(patients, on='subject_id', how='left')
df = df.merge(icustays, on=['subject_id', 'hadm_id'], how='left')

print(df.shape)
print(df.head())

df.to_csv('ICU_Analysis.csv', index=False)
```

    (136, 36)
       row_id_x  subject_id  hadm_id            admittime            dischtime  \
    0     12258       10006   142345  2164-10-23 21:09:00  2164-11-01 17:15:00   
    1     12263       10011   105331  2126-08-14 22:32:00  2126-08-28 18:59:00   
    2     12265       10013   165520  2125-10-04 23:36:00  2125-10-07 15:13:00   
    3     12269       10017   199207  2149-05-26 17:19:00  2149-06-03 18:42:00   
    4     12270       10019   177759  2163-05-14 20:43:00  2163-05-15 12:00:00   
    
                 deathtime admission_type         admission_location  \
    0                  NaN      EMERGENCY       EMERGENCY ROOM ADMIT   
    1  2126-08-28 18:59:00      EMERGENCY  TRANSFER FROM HOSP/EXTRAM   
    2  2125-10-07 15:13:00      EMERGENCY  TRANSFER FROM HOSP/EXTRAM   
    3                  NaN      EMERGENCY       EMERGENCY ROOM ADMIT   
    4  2163-05-15 12:00:00      EMERGENCY  TRANSFER FROM HOSP/EXTRAM   
    
      discharge_location insurance  ... row_id icustay_id dbsource first_careunit  \
    0   HOME HEALTH CARE  Medicare  ...  12742     206504  carevue           MICU   
    1       DEAD/EXPIRED   Private  ...  12747     232110  carevue           MICU   
    2       DEAD/EXPIRED  Medicare  ...  12749     264446  carevue           MICU   
    3                SNF  Medicare  ...  12754     204881  carevue            CCU   
    4       DEAD/EXPIRED  Medicare  ...  12755     228977  carevue           MICU   
    
      last_careunit first_wardid last_wardid               intime  \
    0          MICU           52          52  2164-10-23 21:10:15   
    1          MICU           15          15  2126-08-14 22:34:00   
    2          MICU           15          15  2125-10-04 23:38:00   
    3           CCU            7           7  2149-05-29 18:52:29   
    4          MICU           15          15  2163-05-14 20:43:56   
    
                   outtime      los  
    0  2164-10-25 12:21:07   1.6325  
    1  2126-08-28 18:59:00  13.8507  
    2  2125-10-07 15:13:52   2.6499  
    3  2149-05-31 22:19:17   2.1436  
    4  2163-05-16 03:47:04   1.2938  
    
    [5 rows x 36 columns]



```python
df = df.drop_duplicates()

df['dob'] = pd.to_datetime(df['dob'], errors='coerce')
df['admittime'] = pd.to_datetime(df['admittime'], errors='coerce')
df['dischtime'] = pd.to_datetime(df['dischtime'], errors='coerce')
df['intime'] = pd.to_datetime(df['intime'], errors='coerce')
df['outtime'] = pd.to_datetime(df['outtime'], errors='coerce')

df['hospital_los_days'] = (
    df['dischtime'] - df['admittime']
).dt.total_seconds() / 86400

df['icu_los_days'] = (
    df['outtime'] - df['intime']
).dt.total_seconds() / 86400
```


```python
print(df[['hospital_los_days', 'icu_los_days']].describe())
```

           hospital_los_days  icu_los_days
    count         136.000000    136.000000
    mean            9.747769      4.452461
    std            12.733673      6.196832
    min             0.038194      0.105926
    25%             3.519444      1.233504
    50%             6.790625      2.111447
    75%            11.403299      4.329063
    max           123.984722     35.406516



```python
df = df[
    (df['hospital_los_days'] >= 0) &
    (df['icu_los_days'] >= 0)
]
```


```python
df.to_csv('ICU_Analysis_Clean.csv', index=False)
```


```python
print(df.columns.tolist())
```

    ['row_id_x', 'subject_id', 'hadm_id', 'admittime', 'dischtime', 'deathtime', 'admission_type', 'admission_location', 'discharge_location', 'insurance', 'language', 'religion', 'marital_status', 'ethnicity', 'edregtime', 'edouttime', 'diagnosis', 'hospital_expire_flag', 'has_chartevents_data', 'row_id_y', 'gender', 'dob', 'dod', 'dod_hosp', 'dod_ssn', 'expire_flag', 'row_id', 'icustay_id', 'dbsource', 'first_careunit', 'last_careunit', 'first_wardid', 'last_wardid', 'intime', 'outtime', 'los', 'hospital_los_days', 'icu_los_days']



```python
keep_columns = [
    'subject_id',
    'hadm_id',
    'icustay_id',
    'gender',
    'admission_type',
    'admission_location',
    'discharge_location',
    'insurance',
    'ethnicity',
    'diagnosis',
    'hospital_expire_flag',
    'expire_flag',
    'first_careunit',
    'last_careunit',
    'admittime',
    'dischtime',
    'intime',
    'outtime',
    'hospital_los_days',
    'icu_los_days'
]

df = df[keep_columns]
```


```python
df.to_csv('ICU_Analysis_Clean.csv', index=False)
```

Clean the data set


```python
missing = df.isnull().sum()
missing = missing[missing > 0].sort_values(ascending=False)

print(missing)
```

    Series([], dtype: int64)



```python
df = df.drop_duplicates()

# Keep needed columns
keep_columns = [
    'subject_id',
    'hadm_id',
    'icustay_id',
    'gender',
    'admission_type',
    'admission_location',
    'discharge_location',
    'insurance',
    'ethnicity',
    'diagnosis',
    'hospital_expire_flag',
    'expire_flag',
    'first_careunit',
    'last_careunit',
    'admittime',
    'dischtime',
    'intime',
    'outtime',
    'hospital_los_days',
    'icu_los_days'
]

df = df[keep_columns]

# Convert dates
date_columns = [
    'admittime',
    'dischtime',
    'intime',
    'outtime'
]

for column in date_columns:
    df[column] = pd.to_datetime(df[column], errors='coerce')

# Calculate LOS
df['hospital_los_days'] = (
    df['dischtime'] - df['admittime']
).dt.total_seconds() / 86400

df['icu_los_days'] = (
    df['outtime'] - df['intime']
).dt.total_seconds() / 86400

# Remove impossible LOS
df = df[
    (df['hospital_los_days'] >= 0) &
    (df['icu_los_days'] >= 0)
]

# Replace empty text values
text_columns = df.select_dtypes(include='object').columns

for column in text_columns:
    df[column] = df[column].fillna('Unknown')

# Fill binary flags
df['hospital_expire_flag'] = df['hospital_expire_flag'].fillna(0)
df['expire_flag'] = df['expire_flag'].fillna(0)

# Save
df.to_csv('ICU_Analysis_Clean.csv', index=False)

# Quick final check
print('Rows:', len(df))
print('Columns:', len(df.columns))
print(df.head())
```

    Rows: 136
    Columns: 20
       subject_id  hadm_id  icustay_id gender admission_type  \
    0       10006   142345      206504      F      EMERGENCY   
    1       10011   105331      232110      F      EMERGENCY   
    2       10013   165520      264446      F      EMERGENCY   
    3       10017   199207      204881      F      EMERGENCY   
    4       10019   177759      228977      M      EMERGENCY   
    
              admission_location discharge_location insurance  \
    0       EMERGENCY ROOM ADMIT   HOME HEALTH CARE  Medicare   
    1  TRANSFER FROM HOSP/EXTRAM       DEAD/EXPIRED   Private   
    2  TRANSFER FROM HOSP/EXTRAM       DEAD/EXPIRED  Medicare   
    3       EMERGENCY ROOM ADMIT                SNF  Medicare   
    4  TRANSFER FROM HOSP/EXTRAM       DEAD/EXPIRED  Medicare   
    
                    ethnicity            diagnosis  hospital_expire_flag  \
    0  BLACK/AFRICAN AMERICAN               SEPSIS                     0   
    1   UNKNOWN/NOT SPECIFIED          HEPATITIS B                     1   
    2   UNKNOWN/NOT SPECIFIED               SEPSIS                     1   
    3                   WHITE     HUMERAL FRACTURE                     0   
    4                   WHITE  ALCOHOLIC HEPATITIS                     1   
    
       expire_flag first_careunit last_careunit           admittime  \
    0            1           MICU          MICU 2164-10-23 21:09:00   
    1            1           MICU          MICU 2126-08-14 22:32:00   
    2            1           MICU          MICU 2125-10-04 23:36:00   
    3            1            CCU           CCU 2149-05-26 17:19:00   
    4            1           MICU          MICU 2163-05-14 20:43:00   
    
                dischtime              intime             outtime  \
    0 2164-11-01 17:15:00 2164-10-23 21:10:15 2164-10-25 12:21:07   
    1 2126-08-28 18:59:00 2126-08-14 22:34:00 2126-08-28 18:59:00   
    2 2125-10-07 15:13:00 2125-10-04 23:38:00 2125-10-07 15:13:52   
    3 2149-06-03 18:42:00 2149-05-29 18:52:29 2149-05-31 22:19:17   
    4 2163-05-15 12:00:00 2163-05-14 20:43:56 2163-05-16 03:47:04   
    
       hospital_los_days  icu_los_days  
    0           8.837500      1.632546  
    1          13.852083     13.850694  
    2           2.650694      2.649907  
    3           8.057639      2.143611  
    4           0.636806      1.293843  



```python
import shutil
output_path = 'data/ICU_Analysis_Clean.csv'
os.makedirs(os.path.dirname(output_path), exist_ok=True)

df.to_csv(output_path, index=False)

desktop_path = os.path.expanduser('~/Desktop/ICU_Analysis_Clean.csv')
shutil.copy('data/ICU_Analysis_Clean.csv', desktop_path)
```




    '/Users/manalali/Desktop/ICU_Analysis_Clean.csv'


