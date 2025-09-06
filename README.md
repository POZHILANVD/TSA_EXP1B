# Ex.No: 1B CONVERSION OF NON STATIONARY TO STATIONARY DATA
### Developed by: POZHILAN V D
### Date: 31/08/2025

## AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
## ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
## PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from statsmodels.tsa.seasonal import seasonal_decompose

data = pd.read_excel('/content/Coffe_sales.xlsx')

data['date'] = pd.to_datetime(data['date'])


price_column_name = 'money'
daily_data = data.groupby('date')[price_column_name].mean().reset_index()

daily_data.set_index('date', inplace=True)

target_series_name = price_column_name
target_series = daily_data[target_series_name]

daily_data['price_diff'] = target_series - target_series.shift(1)


result_original = seasonal_decompose(target_series, model='additive', period=7)
daily_data['price_seasonal_resid'] = result_original.resid

daily_data['price_log'] = np.log(target_series)


daily_data['price_log_diff'] = daily_data['price_log'] - daily_data['price_log'].shift(1)


result_log_diff = seasonal_decompose(daily_data['price_log_diff'].dropna(), model='additive', period=7)
daily_data['price_log_seasonal_resid'] = result_log_diff.resid

plt.figure(figsize=(16, 18))

plt.subplot(6, 1, 1)
plt.plot(target_series, label='Original Price')
plt.legend(loc='best')
plt.title(f'Original {target_series_name} Data Over Time')
plt.xlabel('Date')
plt.ylabel('Price ($)')
plt.grid(True)


plt.subplot(6, 1, 2)
plt.plot(daily_data['price_diff'].dropna(), label='Regular Differencing')
plt.legend(loc='best')
plt.title('Regular Differencing of Price ($)')
plt.xlabel('Date')
plt.ylabel('Differenced Price ($)')
plt.grid(True)

plt.subplot(6, 1, 3)
plt.plot(daily_data['price_seasonal_resid'].dropna(), label='Seasonal Residuals (Original Series, period=7)')
plt.legend(loc='best')
plt.title('Seasonal Residuals (from Original Price Decomposition, period=7)')
plt.xlabel('Date')
plt.ylabel('Residual')
plt.grid(True)

plt.subplot(6, 1, 4)
plt.plot(daily_data['price_log'].dropna(), label='Log Transformed Price')
plt.legend(loc='best')
plt.title('Log Transformed Price ($)')
plt.xlabel('Date')
plt.ylabel('Log Price ($)')
plt.grid(True)

plt.subplot(6, 1, 5)
plt.plot(daily_data['price_log_diff'].dropna(), label='Log Transformation and Regular Differencing')
plt.legend(loc='best')
plt.title('Log Transformation and Regular Differencing of Price ($)')
plt.xlabel('Date')
plt.ylabel('RDiff(Log(Price ($))))')
plt.grid(True)

plt.subplot(6, 1, 6)
plt.plot(daily_data['price_log_seasonal_resid'].dropna(), label='Log, Reg. Diff. & Seasonal Diff. Residuals (period=7)')
plt.legend(loc='best')
plt.title('Log, Regular Differencing and Seasonal Differencing Residuals of Price ($) (period=7)')
plt.xlabel('Date')
plt.ylabel('SDiff(RDiff(Log(Price ($))))')
plt.grid(True)

plt.tight_layout()
plt.show()

data.plot(kind='line')

```
## OUTPUT:

### ORIGINAL:

<img width="1716" height="301" alt="image" src="https://github.com/user-attachments/assets/965eec9c-dea0-4eed-8135-7470113164ae" />

### REGULAR DIFFERENCING:

<img width="1710" height="304" alt="image" src="https://github.com/user-attachments/assets/23e75a8d-c69e-4e85-aeb3-5fbf9a151743" />

### SEASONAL ADJUSTMENT:

<img width="1653" height="302" alt="image" src="https://github.com/user-attachments/assets/c0346262-9298-430b-92d9-6a7f5fe96fd4" />

### LOG TRANSFORMATION:

<img width="1681" height="294" alt="image" src="https://github.com/user-attachments/assets/3ab9f2b6-b297-44f2-8b13-9c4a97e6d7a8" />

### Log Transformation and Regular Differencing:

<img width="1663" height="285" alt="image" src="https://github.com/user-attachments/assets/5f13019e-78cb-4586-8a93-5deae35bcb28" />

### Log Transformation + Regular & Seasonal Differencing:

<img width="1653" height="286" alt="image" src="https://github.com/user-attachments/assets/0aefa927-280a-4f2e-93d8-ffa2a25e191e" />

### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
