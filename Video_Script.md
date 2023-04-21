
### Read the dataset and view the data

Our first task will be to read the data from its source file. In this case a csv file named nyc_airbnb.csv.

To read the csv data, we need 

step 1: to import the pandas library.

    import pandas as pd

step 2: using the read_csv method in pandas, we read the data from the location its stored.

    pd.read_csv("../datasets/nyc_airbnb.csv")

step 4: we store the data into a variable `nyc`.

    nyc= pd.read_csv("../datasets/nyc_airbnb.csv")

step 5: we print the content stored in the variable `nyc`.

    nyc

### Check for outliers in `price` data

#### using strip plot


**put in notebook**
Since we are looking to find outliers in `price` data, a good way to observe their existence is using visualizations. To see where the outliers lie in our data, strip plots can be very useful to see how the datapoints are spread. Lets plot a strip plot for the `price` data.
**end**

For the strip plot, we first

step 1: import the plotly library.

    import the plotly.express library as px 

step 2: using px, we call the strip() method to generate the strip plot
    
    px.strip()
    
step 3: inside the strip method, the parameters will be,
        `nyc`: the variable where the data is stored
        `price`: the column data to plot in the y axis
        
    px.strip(nyc,y='price')

step 4: Next we store the plot into a variable named `price_strip` 

    price_strip = px.strip(nyc, y='price')

step 5: Now, we view the strip plot using price_strip variable by calling the show() method

    price_strip.show()
    
If we look at the strip plot, we see that the data range is between 0-10,000. The majority of the datapoints are within the range of 0-2000 and a few of them are over 4000. We can  definitely say there exists some outliers in the price data. The points beyond 4000 can be probable outliers for `price` data. But we need to know the exact outlier points to apply clipping to the data.

Now, Let us plot a boxplot on the price data and compare it to the boxplot explained above.    
            
#### using boxplot

For the boxplot, we 

step 1: call the boxplot() method to generate the boxplot

    boxplot()
    
step 2: inside the method, the parameters will be,
    
column: the column data to plot for the boxplot
figsize(optional): to define the size of the figure in terms of width and height
fontsize(optional): to show the texts size in the figure
vert(optional): the allignment(x or y axis) of the plot. Value False means horizontal(x axis) alignment,whereas, True means vertical alignment

    boxplot(column='price',figsize=(30,10), fontsize='30', vert=False)
    
step 3: apply the boxplot() method to the variable `nyc`, where our data is stored

    nyc.boxplot(column='price',figsize=(30,10), fontsize='30', vert=False)

step 4: store the boxplot in a variable `box_price`

    box_price =  nyc.boxplot(column='price',figsize=(30,10), fontsize='30', vert=False)

step 5: use the `box_price` variable to display the boxplot

    box_price
    
From the boxplot, we can see that a huge number of `price` datapoints are outside the right whisker lines (these are actually the outliers). Our valid range of values (that are not outliers) are within the whisker points (range of Q1-1.5IQR and Q3 + 1.5IQR). The datapoints outside this range are the outlier points.

Our task now is to find these outlier range points to remove the outliers from `price` data.

### Use describe for `price` distribution:

To find the exact outlier points range in `price`, we need to see its number distribution in the quartile division. This will give us a better understanding of where the outliers lie.For this, We will use the `describe()` method to know about the distribution of price data.

To see the `price` distribution, we

step 1: select the price column from the variable `nyc`
    
    nyc['price']
    
step 2: use the describe() method on the price column. 

    nyc['price'].describe()

**put in notebook**
Looking at the price distribution, we can see the min, 25%, 50%, 75% and max price value of price. The price values ranges from 0 to 10000 and the 75% percentile value of `price` is 175. This basically means 75% of our values is within 0 -175. So, the remaining 25% of values is in the range 175-1000. There is a high chance that our outliers lie mostly in this region of values. 

Now lets try to find the outlier points based on the `quantile` formula.
**end**

### Find the Outliers:

**put in notebook**
To find the outliers using the `quantile` formula, we first find out the interquartile(IQR) range for `price` data. For that , we calculate the `q3` and `q1` points in the `price` data.
**end**

#### calculate Q3:
**put in notebook**
**end**

**put in notebook**
The q3 point is the data point that represents the 75th percentile value in a data variable.

to calculate q3 for `price`, we first
**end**

step 1: select the `price` column from the variable `nyc`

    nyc['price']
    
step 2: then use the `quantile()` method on the price data

    nyc['price'].quantile()
    
step 3: inside the quantile method, the parameter will be,
     - the percentile value whose datapoint we want to find(75% for q3)

    nyc['price'].quantile(.75)
    
step 4: Now, we store the result in a variable `q3`  

    q3= nyc['price'].quantile()

step 5: print the variable `q3` and see the q3 point

    q3

#### calculate Q1:

**put in notebook**
The q1 point is the data point that represents the 25th percentile value in a data variable.
to calculate q1, we first
**end**

step 1: select the `price` column from the variable `nyc`

    nyc['price']
    
step 2: then use the `quantile()` method on the price data

    nyc['price'].quantile()
    
step 3: inside the quantile method, the parameter will be,
     - the percentile value whose datapoint we want to find(25% for q3)

    nyc['price'].quantile(.25)
    
step 4: Again, we store the result in a variable `q1`  

    q1= nyc['price'].quantile()

step 5: print the variable `q1` and see the q1 point

    q1

### Find the interquartile range:


**put on notebook**
After calculating `q1` and `q3`, we find out the interquartile range which is the difference between these two points. This range will determine the outlier points(upper and lower bound) for our `price` data.
**end**

For finding the IQR, we 

step 1: substract `q3` from `q1`
    
    q3-q1
step 2: store the result in a variable `iqr`

    iqr=q3-q1
step 3: and print the variable `iqr` and see the interquartile range

    print("iqr: ",iqr)


### Calculate the upper and lower bound for outliers:

**put on notebook**
After finding the interquartile range(iqr), our next task is to calculate the upper and lower bound points to find out the outlier range for `price` data. We will use the `iqr` and the calculated q1,q3 data points to find out the lower and upper bound points.
**end**

for the `upper bound`, we

step 1: define the range, q3 + 1.5*iqr

    q3 + 1.5*iqr
    
step 2: store it in a variable `upper_bound`
    
    upper_bound =  q3 + 1.5*iqr  
    
step 3: print the `upper_bound`

    print("upper bound",upper_bound)


for the `lower bound`, we

step 1: define the range, q1 - 1.5*iqr

    q1 - 1.5*iqr
    
step 2: store it in a variable `lower_bound`
    
    lower_bound =  q3 + 1.5*iqr  

step 3: print the `lower_bound`

    print("lower bound",lower_bound)

### Find the Clipping points:

**put in notebook**
Now that we have found the upper and lower bound points, we need to find the clipping points for the `price` data.
The clipping points for the `price` data will be as follows:
**end**


**show formula image**
lower_point= max(lower_bound, nyc['price'].min())
upper_point= min(upper_bound, nyc['price'].max())

For the lower_point formula, we are taking the max value between the minimum price value (0) and the `lower_bound` because we can go as far as the minimum data point that we have. In this example, we found that our `lower_bound` is -90. But our lowest data point is 0. So we dont need the lower bound as -90. We need to go only as far as the lowest data point that is 0.

So for lower_point calculation, we first

step 1: use the max() function

    max()

step 2: the function parameters will be,
    - `lower_bound`: we calculated in the previous step
    - `nyc['price'].min()` : the minimum value in the `nyc['price']` column, the lowest data point

    max(lower_bound,nyc['price'].min())
    
step 3: store the function into variable `lower_point`

    lower_point= max(lower_bound,nyc['price'].min())
    
step 4: print the variable `lower_point`

    print("lower_point", lower_point)


For the upper_point, the formula is similar where we will take the minimum of the highest datapoint of `price` and the calculated upper_bound. For the `price` data, the maximum datapoint we have is 10,000 and our upper bound is 334. Since  we need to go as far as the upper bound, we will take the minimum from these two points.

So for upper_point, we

step 1: use the min() function

    min()

step 2: the function parameters will be,
    - `upper_bound`: calculated in the previous step
    - `nyc['price'].max()` : the minimum value in the `nyc['price']` data

    min(upper_bound,nyc['price'].max())
    
step 3: store the function into variable `upper_point`
    
    upper_point= min(upper_bound, nyc['price'].max())


step 4: print the variable `upper_point`

    print("upper_point", upper_point)

### Clip outliers using the clipping points:

Now, we have our lower and upper point. We will clip our data according to them. After clipping, as shown in our introductory example, the price datapoints less than the lower_point will be set to the `lower_point` value and the price datapoints greater than the upper_point will be set to the `upper_point` value. By doing this , it will remove the outlier points from the `price` data. This method of removing outliers from data is called `winsorizing`. 

For clipping the data, we

step 1: select the `price` column from `nyc` variable

    nyc['price']

step 2: call the `clip()` method on the `price` column

    nyc['price'].clip()
    
step 3: in the clip method, the parameters will be,
    - `lower_point`: the calculated lower point of price data
    - `upper_point`: the calculated upper point of price data

    nyc['price'].clip(lower_point, upper_point)
    
step 4: set the result to the `price` column of `nyc` variable to make the changes permanent 

    nyc['price']= nyc['price'].clip(lower_point, upper_point)

## Check final clipped data:

After clipping, We check the five number summary of `price` data using the `describe` method.

For that, we 

Step 1: select the `price` column from the variable `nyc`

     nyc['price']

Step 2: use the `describe()` method on the `price` data 
    
    nyc['price'].describe()

After clipping the `price` data, we can see the distribution has become more dense (0-334) which was a lot more spread out(0-10000) before. Let us now use the scatter plot again to observe its spread.

## Visualize the clipped data:

#### with strip plot

Let us see how our strip plot looks now compared to the intial one we plotted and see the difference.

step 1: using `px`, call the `strip()` method to generate the strip plot

    px.strip()
    
step 2: inside the method, the parameters will be,
    - `nyc`: variable where the data is stored
    - `price`: column data to plot in the y axis

    px.strip(nyc, y='price')
    
step 3: store the result into the variable `price_strip2`

    price_strip2= px.strip(nyc, y='price')
    
step 4: display the variable `price_strip2` using the `show()` method 

    price_strip2.show()

In the strip plot , we can also see the density of the datapoints are heavily densed ranging from 0 to 300+. It shows the outlier points have been removed from our data.

#### with box plot


Lets visualize the boxplot to confirm if outliers exist in the `price` data.

We
step 1: call the boxplot() method to generate the boxplot

    boxplot()
    
step 2: inside the method, the parameters will be,
    
`column`: the column data to plot for the boxplot
`figsize`(optional): to define the size of the figure in terms of width and height
`fontsize`(optional): to show the texts size in the figure
`vert`(optional): the allignment(x or y axis) of the plot. Value False means horizontal(x axis) alignment,whereas, True means vertical alignment

    boxplot(column='price',figsize=(30,10), fontsize='30', vert=False)
    
step 3: apply the `boxplot()` method to the variable `nyc`, where our data is stored

    nyc.boxplot(column='price',figsize=(30,10), fontsize='30', vert=False)

step 4: store the boxplot in a variable `box_price2`

    box_price2 =  nyc.boxplot(column='price',figsize=(30,10), fontsize='30', vert=False)

step 5: use the `box_price2` variable to display the boxplot

    box_price2

From the new boxplot above, we can see that there are no outlier points (dotted points) beyond the whisker marks which indicates that the outliers points have been removed by applying clipping/winsorizing.


## Conclusion:

By using the clip method, we have removed our outliers from the price data. Now using this dataset will give us good predictions of airbnb hotel prices in New York.



