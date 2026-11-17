# Ex.No: 03   COMPUTE THE AUTO FUNCTION(ACF)
Date: 

### AIM:
To Compute the AutoCorrelation Function (ACF) of the data for the first 35 lags to determine the model
type to fit the data.
### REQUIREMENTS: VS CODE EDITOR
### DATASET: ADANI PORTS STOCK PRICE
### ALGORITHM:
1. Import the necessary packages
2. Find the mean, variance and then implement normalization for the data.
3. Implement the correlation using necessary logic and obtain the results
4. Store the results in an array
5. Represent the result in graphical representation as given below.
### PROGRAM:
~~~

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# 1. Load the dataset
# We read the CSV, parse the dates, and set the 'Date' column as the index.
df = pd.read_csv('ADANIPORTS.csv', parse_dates=['Date'], index_col='Date')

# Extract the 'Prev Close' prices as a raw mathematical array.
# .dropna() removes any blank rows to prevent mathematical errors.
# .values converts it from a Pandas Series to a NumPy array for faster calculation.
data = df['Prev Close'].dropna().values

N = len(data)

# Define lags: we will look at how the price correlates with itself up to 34 days in the past.
lags = range(35)

# Pre-allocate a list to store our calculated autocorrelation values
autocorr_values = []

# Calculate the overall mean (average) and variance (spread) of the data
mean_data = np.mean(data)
variance_data = np.var(data)

# 2. Calculate Autocorrelation for each lag
for lag in lags:
    if lag == 0:
        # A lag of 0 means comparing today's price exactly with today's price. 
        # A variable is always 100% correlated with itself, so it is exactly 1.
        autocorr_values.append(1.0)
    else:
        # Array Slicing to align the time steps:
        # data[:-lag] cuts off the last 'lag' days.
        # data[lag:] cuts off the first 'lag' days.
        # This effectively aligns Today's price side-by-side with the price 'lag' days ago.
        
        # Calculate Autocovariance: the average product of the deviations from the mean
        auto_cov = np.sum((data[:-lag] - mean_data) * (data[lag:] - mean_data)) / N
        
        # Autocorrelation is Autocovariance normalized by the Variance.
        # This mathematically forces the result to be a clean ratio between -1 and +1.
        autocorr_values.append(auto_cov / variance_data)

# 3. Display the graph
plt.figure(figsize=(10, 6))

# plt.stem creates a "lollipop" chart (vertical lines from a baseline to a point).
# This is the standard graphical format in time series for discrete lag values.
plt.stem(lags, autocorr_values)
plt.title('Autocorrelation of ADANIPORTS Previous Close Prices')
plt.xlabel('Lag (Days)')
plt.ylabel('Autocorrelation Coefficient')
plt.grid(True)
plt.show()
~~~

### OUTPUT:

<img width="1237" height="802" alt="image" src="https://github.com/user-attachments/assets/e495bc0a-3420-4011-95aa-eff2d452f0be" />


### RESULT:
        Thus we have successfully implemented the auto correlation function in python.
