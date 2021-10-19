# What Really Affects Walmart Sales
Historical Weekly Sales data of 45 Walmart Stores located in different region. This Data includes Holiday Events, Weather Temperature, Fuel Price, Consumer Price Index, and Unemployment Rate Data to analyze whether these datas affecting the weekly sales in Walmart Store. This Data gathered from February 2010 until October 2012.

**Holiday Events**
- Super Bowl (on February)
- Labour Day (on September)
- Thanksgiving (on November)
- Christmas (December)

**Weather Temperature represent in Fahrenheit**
- Normal Weather Temperature 32C will be around 90F

**Fuel Price represent in Dollar per Gallon**

**Consumer Price Index**
- Measure of the average change over time in the prices paid by urban consumers for a market basket of consumer goods and services. 
- If CPI is higher means that price of consumer goods are increasing higher.

**Unemployment Rate represent in Percentage**
- Higher Unemployment Rate means that there are more jobless people in an area.

## Investigation of Walmart Weekly Sales Whether Affected by These Categories or Not

**1. Data Preparation**
- Check the Data Type

```python
df.info()
```
![Data Type](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Data%20Type.JPG)

- Check the Missing Value and Drop Duplicates

```python
dataDesc = []

for i in df.columns:
    dataDesc.append([
        i,
        df[i].dtypes,
        df[i].isna().sum(),
        round((((df[i].isna().sum()) / len(df)) *100),2),
        df[i].nunique(),
        df[i].drop_duplicates().sample(2).values
    ])

pd.DataFrame(dataDesc,columns=[
    "Data Features",
    "Data Type",
    "Null",
    "Null Percentage",
    "Unique",
    "Unique Sample"
])
```
![Data Description 1](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Data%20Description%20(1).JPG)

- Investigate the Data Description (Continuous Numerical and Object Type)

![Data Description 2](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Data%20Description.JPG)


_What we get in Data Preparation_

- Changing Data Type on Holiday_Flag Column and Store from int64 to object type
- There is no Missing Value and Duplicate Value
- There is Outliers on Temperature, Unemployment, and Weekly_Sales but we are going to see how these outliers affect on the Sales


**2. Date and Time Conversion and Extraction**
```python
df['Date'] = pd.to_datetime(df['Date'],format='%d-%m-%Y')

date = df['Date'].dt

df['Month'] = date.month
df['Month_Name'] = date.month_name()
df['Year'] = date.year
df['Quarter'] = date.to_period('Q')
```

**3. Average Weekly Sales VS Holiday Event**
```python
pd.crosstab(index = df['Holiday_Flag'], columns = 'Average Weekly Sales',values = df['Weekly_Sales'],aggfunc='mean').round(2)
```
![Average Weekly Sales - Holiday](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Average%20Weekly%20Sales%20VS%20Holiday%20Flag.JPG)

_0 mark represent there is no Holiday Event,_
_1 mark represent there is a Holiday Event_

_There is 7.8% difference in Average Weekly Sales between Holiday and no-Holiday_

**4. Store that has highest Weekly Sales and Lowest Weekly Sales**

![Store](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Total%20Weekly%20Sales%20VS%20Store.JPG)

_Store 20 has the highest Total Weekly Sales, meanwhile Store 33 has the lowest Total Weekly Sales_

**5. Total Weekly Sales By Month**

![Total Weekly Sales by Month](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Total%20Weekly%20Sales%20VS%20Month.JPG)

_If the Holiday Events were subjected on February, September, November, and December, there will be an increasing Sales on these months. However this chart shows that Holiday Events were not affected Weekly Sales. But in Point No. 3, we have an insight that showed us Average Weekly Sales was 7.8% higher in Holiday Week. It was because the data gathered from February 2010 until October 2012, so there was a void in this period_

![Total Weekly Sales by Month Yearly](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Total%20Weekly%20Sales%20VS%20Month%20(Yearly).JPG)

_So, if we categorized these Weekly Sales by Month in a Year Period, we can get an insight that Increasing Weekly Sales was spotted on Christmas Holiday on December_

**6. Correlation Between Weekly Sales and (Temperature, Fuel Price, CPI, and Unemployment)**

![Correlation](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Data%20Correlation.JPG)

_From this Heatmap, we can see that there is no Correlation at all between Weekly Sales and Temperature, Fuel Price, CPI, Unemployment. Does that means these Data not affecting Weekly Sales at all? NO._

**7. Data Distribution of Temperature, Fuel Price, Weekly Sales. And The Growth Rate of each Data.**

![Temperature Distribution](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Temperature%20Data%20Distribution.JPG)

![Fuel Price Distribution](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Fuel%20Price%20Data%20Dsitribution.JPG)

![CPI Distribution](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/CPI%20Data%20Distribution.JPG)

![Unemployment Distribution](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Unemployment%20Data%20Distribution.JPG)

![Fuel Price Growth Rate](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Fuel%20Price%20Growth%20Rate.JPG)

![CPI Growth Rate](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/CPI%20Growth%20Rate.JPG)

![Unemployment Growth Rate](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Unemployment%20Growth%20Rate.JPG)

_Based on the Data Distribution, we will convert Continuous Numerical Data to a Categorical Data, Exp : Temperature Categorical_

```python
def CategoryTemp(x):
    if (x <= 20):
        return ('A -2 - 20')
    elif (x > 20) & (x <= 40):
        return ('B 20 - 40')
    elif (x > 40) & (x <= 60):
        return ('C 40 - 60')
    elif (x > 60) & (x <= 80):
        return ('D 60 - 80')
    elif (x > 80) & (x <= 101):
        return ('E 80-101')

df['CategoryTemp'] = df['Temperature'].apply(CategoryTemp)
```
![Weekly Sales VS Temperature Table](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Total%20Weekly%20Sales%20VS%20Temperature%20(Table).JPG)

![Weekly Sales VS Temperature Visual](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Total%20Weekly%20Sales%20VS%20Temperature%20(Visual).JPG)

_From these, we can get an insight that people are tend to going out shopping in a normal weather temperature, and this temperature is a ratio which means that it will be shown periodically every year, so we don't need to see the growth rate of temperature data_

![Weekly Sales VS Fuel Price Table](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Total%20Weekly%20Sales%20VS%20Fuel%20Price%20(Table).JPG)

![Weekly Sales VS Fuel Price Visual](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Total%20Weekly%20Sales%20VS%20Fuel%20Price%20(Visual).JPG)

_From these, we can also get an insight that people are not going out for shopping if the Fuel Price was high. The Growth Rate of Fuel Price that shown increasing every year, was the reason why the Category C Fuel Price was very high in term of Weekly Sales_

![Weekly Sales VS CPI Table](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Total%20Weekly%20Sales%20VS%20CPI%20(Table).JPG)

![Weekly Sales VS CPI Visual](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Total%20Weekly%20Sales%20VS%20CPI%20(Visual).JPG)

_From these, we can also get an insight that people are tend to going out for shopping if the CPI was low. The Growth Rate of CPI because of Inflation of course was the reason why the even CPI reached 200, there was so much Sales in Walmart_

![Weekly Sales VS Unemployment Table](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Total%20Weekly%20Sales%20VS%20Unemployment%20(Table).JPG)

![Weekly Sales VS Unemployment Visual](https://github.com/tommysachi/What_Really_Affects_Walmart_Sales/blob/main/Table%20%26%20Visual/Total%20Weekly%20Sales%20VS%20Unemployment%20(Visual).JPG)

_From these, we can also get an insight that people are tend to going out for shopping if the Unemployment Rate was low. The Declining Rate of Unemployment will be really affect Walmart Sales in the future in term of increasing_

**8. Review and Suggestion**
- If Walmart really want to Increase the Sales, they should put attention on when to launch their new products or planning for the campaign, they should do it in Christmas Holiday, with the Weather Temperature not too low, when the CPI index is low and where is the region that has a Fuel Price lower than Average.
- Out of those Category, if Walmart needs to increase their Sales, they need to do some Promotions or Interesting Discounts, so that people are tend to going out for shopping even Temperature dropping to low or even going to high or even Fuel Price, CPI, and Unemployment Rate are increasing.
