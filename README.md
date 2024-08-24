# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA

# Developed By: Keerthi Vasan A

# Register No: 212222240048

# Date: 

### AIM:
To perform regular differencing, seasonal adjustment, and log transformation on Apple stock price dataset.

### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differencing, seasonal adjustment, and log transformation.
4. Plot the data according to need, before and after regular differencing, seasonal adjustment, and log transformation.
5. Display the overall results.

### PROGRAM:
#### IMPORTING PACKAGES:
```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.stattools import adfuller

%matplotlib inline

```
#### Preprocessing:
```py
train = pd.read_csv('/content/apple_stock.csv')

train['Date'] = pd.to_datetime(train['Date'], format='%Y-%m-%d')
train.index = train['Date']
train.drop('Date', axis=1, inplace=True)

train.head()
print(train.columns)
train.columns = train.columns.str.strip()

train['Adj Close'].plot(title='Adjusted Close Price Over Time')


```
#### REGULAR DIFFERENCING:
```py
# Regular differencing
from statsmodels.tsa.stattools import adfuller

def adf_test(timeseries):
    print("Results of Dickey-Fuller Test:")
    dftest = adfuller(timeseries, autolag="AIC")
    dfoutput = pd.Series(dftest[0:4], index=["Test Statistic", "p-value", "#Lags Used", "Number of Observations Used"])
    for key, value in dftest[4].items():
        dfoutput["Critical Value (%s)" % key] = value
    print(dfoutput)

adf_test(train['Adj Close'])

train['Adj Close_diff'] = train['Adj Close'] - train['Adj Close'].shift(1)
train['Adj Close_diff'].dropna().plot(title='Regular Differenced Adjusted Close Price')

```
#### SEASONAL DIFFERENCING:
```py
#SEASONAL DIFFERENCING
n = 7  # Assuming a weekly seasonality for demonstration

train['Adj Close_seasonal_diff'] = train['Adj Close'] - train['Adj Close'].shift(n)
train['Adj Close_seasonal_diff'].dropna().plot(title='Seasonal Differenced Adjusted Close Price')

```
#### LOG TRANSFORMATION:
```py
#Log transformation
train['Adj Close_log'] = np.log(train['Adj Close'])

train['Adj Close_log_diff'] = train['Adj Close_log'] - train['Adj Close_log'].shift(1)
train['Adj Close_log_diff'].dropna().plot(title='Log Differenced Adjusted Close Price')

```


### OUTPUT:


REGULAR DIFFERENCING:

![Screenshot 2024-08-24 104919](https://github.com/user-attachments/assets/49d4197d-53a5-4703-a74d-288d4db9d16a)


SEASONAL ADJUSTMENT:

![Screenshot 2024-08-24 104953](https://github.com/user-attachments/assets/a6704191-fcee-4809-8c3c-2c50d0dcdad2)


LOG TRANSFORMATION:

![Screenshot 2024-08-24 105025](https://github.com/user-attachments/assets/3353df3e-fa80-450e-bbc2-a8a038656b85)


### RESULT:
Thus we have successfully created the Python code for the conversion of non-stationary to stationary data of Apple stock price dataset.
