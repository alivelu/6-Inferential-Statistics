
## Stroop Effect 

In a Stroop task, participants are presented with a list of words, with each word displayed in a color of ink. The participant’s task is to say out loud the color of the ink in which the word is printed. The task has two conditions: a congruent words condition, and an incongruent words condition. In the congruent words condition, the words being displayed are color words whose names match the colors in which they are printed: for example RED, BLUE. In the incongruent words condition, the words displayed are color words whose names do not match the colors in which they are printed: for example PURPLE, ORANGE. In each case, we measure the time it takes to name the ink colors in equally-sized lists. Each participant will go through and record a time from each condition.

### 1. What is our independent variable? What is our dependent variable?

##### Independent Variable

* Congruent words or Incongruent words
* In this experiment we are examining whether the name of the word's color and the font color are same or different?  

##### Dependent Variable

The dependent variable is the reaction time that the user takes to read (i.e name the font color) in both types of lists.

### 2. What is an appropriate set of hypotheses for this task? What kind of statistical test do you expect to perform? Justify your choices.

##### Why?

To examine if there will be any difference in reaction time between the congruent and incongruent words 

##### What type of test?

After observing the data in Stroop data set, I would like to perform

* One-tailed hypothesis t-test

* Here we have chosen t-test because population parameters are unknown
* And the data provided was just the samples of population
* Here the same subject is exposed to two conditions and tested.
* Sample set is less than 30

* According to the Null hypothesis: There is no difference or interference in reaction time after the interference or it will be less than Incongruent.
* Meantime took for incongruent condition: μi 
* Meantime took under congruent condition: μc 
* Null Hypotheses: (H0) : μi - μC <= 0 or μi <= μC

* According to Alternate Hypothesis: There is a significant increase in reaction time for the incongruent words condition.
* Alternate Hypothesis HA: μi - μC > 0 or μi > μC

A right-tailed test (sometimes called an upper test) is where your hypothesis statement contains a greater than (>) symbol. In other words, the inequality points to the right. 
Ex: Null hypothesis: No change (H0 = 1).
Ex: Alternate hypothesis: (H1) > 1.

The important factor here is that the alternate hypothesis(H1) determines if you have a right-tailed test, not the null hypothesis.



```python
#Importing the required libraries
import pandas as pd
import numpy as np
from math import sqrt
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
sns.set_style("white")
```

* We are using Stroop dataset here
* We will print out first few records in data frame
* Here you can see variables in Stroop data frame


```python
#Read the data into a pandas dataframe and add a subject column
stroop_data = pd.read_csv('C:\\Users\\alive\\Documents\\Data Analyst Nano Degree\\inferential statistics\\stroopdata.csv')
stroop_data['Subject'] = stroop_data.index + 1
stroop_data.head()
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
      <th>Congruent</th>
      <th>Incongruent</th>
      <th>Subject</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12.079</td>
      <td>19.278</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>16.791</td>
      <td>18.741</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9.564</td>
      <td>21.214</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8.630</td>
      <td>15.687</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>14.669</td>
      <td>22.803</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Additional column that shows the time difference between congruent and Incongruent

stroop_data['time_diff'] = stroop_data['Congruent'] - stroop_data['Incongruent']
stroop_data
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
      <th>Congruent</th>
      <th>Incongruent</th>
      <th>Subject</th>
      <th>time_diff</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12.079</td>
      <td>19.278</td>
      <td>1</td>
      <td>-7.199</td>
    </tr>
    <tr>
      <th>1</th>
      <td>16.791</td>
      <td>18.741</td>
      <td>2</td>
      <td>-1.950</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9.564</td>
      <td>21.214</td>
      <td>3</td>
      <td>-11.650</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8.630</td>
      <td>15.687</td>
      <td>4</td>
      <td>-7.057</td>
    </tr>
    <tr>
      <th>4</th>
      <td>14.669</td>
      <td>22.803</td>
      <td>5</td>
      <td>-8.134</td>
    </tr>
    <tr>
      <th>5</th>
      <td>12.238</td>
      <td>20.878</td>
      <td>6</td>
      <td>-8.640</td>
    </tr>
    <tr>
      <th>6</th>
      <td>14.692</td>
      <td>24.572</td>
      <td>7</td>
      <td>-9.880</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8.987</td>
      <td>17.394</td>
      <td>8</td>
      <td>-8.407</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9.401</td>
      <td>20.762</td>
      <td>9</td>
      <td>-11.361</td>
    </tr>
    <tr>
      <th>9</th>
      <td>14.480</td>
      <td>26.282</td>
      <td>10</td>
      <td>-11.802</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>18.200</td>
      <td>35.255</td>
      <td>15</td>
      <td>-17.055</td>
    </tr>
    <tr>
      <th>15</th>
      <td>12.130</td>
      <td>22.158</td>
      <td>16</td>
      <td>-10.028</td>
    </tr>
    <tr>
      <th>16</th>
      <td>18.495</td>
      <td>25.139</td>
      <td>17</td>
      <td>-6.644</td>
    </tr>
    <tr>
      <th>17</th>
      <td>10.639</td>
      <td>20.429</td>
      <td>18</td>
      <td>-9.790</td>
    </tr>
    <tr>
      <th>18</th>
      <td>11.344</td>
      <td>17.425</td>
      <td>19</td>
      <td>-6.081</td>
    </tr>
    <tr>
      <th>19</th>
      <td>12.369</td>
      <td>34.288</td>
      <td>20</td>
      <td>-21.919</td>
    </tr>
    <tr>
      <th>20</th>
      <td>12.944</td>
      <td>23.894</td>
      <td>21</td>
      <td>-10.950</td>
    </tr>
    <tr>
      <th>21</th>
      <td>14.233</td>
      <td>17.960</td>
      <td>22</td>
      <td>-3.727</td>
    </tr>
    <tr>
      <th>22</th>
      <td>19.710</td>
      <td>22.058</td>
      <td>23</td>
      <td>-2.348</td>
    </tr>
    <tr>
      <th>23</th>
      <td>16.004</td>
      <td>21.157</td>
      <td>24</td>
      <td>-5.153</td>
    </tr>
  </tbody>
</table>
<p>24 rows × 4 columns</p>
</div>



#### Summary Statistics that describe variable's numeric values


```python
stroop_data.shape
```




    (24, 4)



i.e 24 rows 3 columns


```python
stroop_data.columns
```




    Index(['Congruent', 'Incongruent', 'Subject', 'time_diff'], dtype='object')



Above are the column nmaes



```python
# Sum method (total of columns)
stroop_data.sum()
```




    Congruent      337.227
    Incongruent    528.382
    Subject        300.000
    time_diff     -191.155
    dtype: float64



### 3. Report some descriptive statistics regarding this dataset. Include at least one measure of central tendency and at least one measure of variability.



```python
# Median method for middle value
stroop_data.median()
```




    Congruent      14.3565
    Incongruent    21.0175
    Subject        12.5000
    time_diff      -7.6665
    dtype: float64




```python
# Mean of columns
stroop_data. mean()
```




    Congruent      14.051125
    Incongruent    22.015917
    Subject        12.500000
    time_diff      -7.964792
    dtype: float64




```python
stroop_data.count()
```




    Congruent      24
    Incongruent    24
    Subject        24
    time_diff      24
    dtype: int64




```python
# Max values of each variable in data frame
stroop_data.max()

```




    Congruent      22.328
    Incongruent    35.255
    Subject        24.000
    time_diff      -1.950
    dtype: float64




```python
#call the.idx method to identify the row of max value
congruent_max  = stroop_data.Congruent
congruent_max.idxmax()
```




    10



10 represents index value of row where the max value is


```python
#call the.idx method to identify the row of max value
incongruent_max  = stroop_data.Incongruent
incongruent_max.idxmax()
```




    14



14 represents index value of row where the max value is


```python
#.std method calculates standaard deviation for ecah column
stroop_data.std()
```




    Congruent      3.559358
    Incongruent    4.797057
    Subject        7.071068
    time_diff      4.864827
    dtype: float64




```python
#.var method calculates variance in columns
stroop_data.var()
```




    Congruent      12.669029
    Incongruent    23.011757
    Subject        50.000000
    time_diff      23.666541
    dtype: float64




```python
#unique values in a variable (congruent)
congruent_unique = stroop_data.Congruent
congruent_unique.value_counts().head()

```




    8.987     1
    12.944    1
    18.495    1
    8.630     1
    12.079    1
    Name: Congruent, dtype: int64




```python
#unique values in a variable(incongruent)
incongruent_unique = stroop_data.Incongruent
incongruent_unique.value_counts().head()

```




    17.960    1
    18.741    1
    20.429    1
    22.158    1
    20.330    1
    Name: Incongruent, dtype: int64




```python
#Entire statistical decriotion
stroop_data.describe()
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
      <th>Congruent</th>
      <th>Incongruent</th>
      <th>Subject</th>
      <th>time_diff</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>24.000000</td>
      <td>24.000000</td>
      <td>24.000000</td>
      <td>24.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>14.051125</td>
      <td>22.015917</td>
      <td>12.500000</td>
      <td>-7.964792</td>
    </tr>
    <tr>
      <th>std</th>
      <td>3.559358</td>
      <td>4.797057</td>
      <td>7.071068</td>
      <td>4.864827</td>
    </tr>
    <tr>
      <th>min</th>
      <td>8.630000</td>
      <td>15.687000</td>
      <td>1.000000</td>
      <td>-21.919000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>11.895250</td>
      <td>18.716750</td>
      <td>6.750000</td>
      <td>-10.258500</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>14.356500</td>
      <td>21.017500</td>
      <td>12.500000</td>
      <td>-7.666500</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>16.200750</td>
      <td>24.051500</td>
      <td>18.250000</td>
      <td>-3.645500</td>
    </tr>
    <tr>
      <th>max</th>
      <td>22.328000</td>
      <td>35.255000</td>
      <td>24.000000</td>
      <td>-1.950000</td>
    </tr>
  </tbody>
</table>
</div>



* The difference in mean of time taken in performing incongruent and congruent tests = 7.96
* And the difference of standard deviation = 4.86

### 4. Provide one or two visualizations that show the distribution of the sample data. Write one or two sentences noting what you observe about the plot or plots.


```python
stroop_data['Congruent'].plot(kind='hist')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a0eb0d1080>




![png](output_38_1.png)


Mostly the test completes in between 9 and 18 seconds, i.e  around the mean of 14 seconds.


```python
stroop_data['Incongruent'].plot(kind='hist')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a0ec3996a0>




![png](output_40_1.png)


* As you can see, the test completes in between 16 and 25 seconds, I.e right around the mean of 22 seconds
* The data also has some outliers at around 35 seconds. 
* So we can say from the above two graphs that the Congruent data was read faster than the incongruent data.



```python
stroop_data[['Congruent','Incongruent']].boxplot();
```


![png](output_42_0.png)


Above in the boxplot diagram, we can see a clear difference between the mean(Q2) of the congruent and the incongruent boxplots. 

* The Congruent Boxplot has no outliers
* While the incongruent plot shows two outliers at around 35 seconds.

The congruent data plot is slightly negatively skewed.
As the Mean (14.0511) is smaller than the Median (14.3565). We can say that It's proved

The incongruent data plot is slightly positively skewed.
As the Median (21.0175) is smaller than the Mean (22.0159)

### 5. Now, perform the statistical test and report your results. What is your confidence level and your critical statistic value? Do you reject the null hypothesis or fail to reject it? Come to a conclusion in terms of the experiment task. Did the results match up with your expectations?¶

##### Degrees of freedom:
The degree of freedom in our case is n − 1, where n represents the number of pairs (subjects in this case).


```python
n=24
df = n-1
df
```




    23




```python
mean_of_differences =   -7.964792
std_of_differences =   4.864827
print("The mean of the difference: {:.4f}".format(mean_of_differences))
print("The standard deviation of the difference: {:.4f}".format(std_of_differences))
```

    The mean of the difference: -7.9648
    The standard deviation of the difference: 4.8648
    


```python
#Standard Error of the mean
Stderr_mean = std_of_differences/float(sqrt(n))
print("Standard Error of the mean value: {:.4f}".format(Stderr_mean))
```

    Standard Error of the mean value: 0.9930
    


```python
# Calculate t-statistic

t_statistic_one_tail = mean_of_differences/float(Stderr_mean)

print("t-statistic value: {:.4f}".format(t_statistic_one_tail))
```

    t-statistic value: -8.0207
    


```python
from scipy import stats

# t-critical values at alpha = 0.05 and n = 24 for one-tailed t-test, q = Quantile to check

t_critical_one_tail =stats.t.ppf(1-0.05, 23)  
print("t-critical values at alpha of 0.05 for one-tailed t-test:\
{:.4f}".format(t_critical_one_tail))
```

    t-critical values at alpha of 0.05 for one-tailed t-test:1.7139
    

So I have conducted a one-tailed test:
Sample size n = 24 
Degrees of freedom df = 23
𝞪 = 0.05
t critical = -1.714. 
𝞵c - 𝞵i is based on our samples and equal to -7.97. Sample standard deviation of the differences (std) = 4.86
t-statistic  = -8.02 

If the p value is less than Alpha null should be rejected


```python
#Cumulative distribution function. 

pval = stats.t.cdf(t_statistic_one_tail, df)*2

print("p-value: {:.4e}".format(pval))
```

    p-value: 4.1030e-08
    

We got the result of p-value as 4.1030e-08. This means we'd expect a 0.000004103 chance of null hypothesis to be true. Our p-value is way lower than our significance level α (0.05) so we should reject the null hypothesis. That means participants need more time to say the color of the ink in the Incongruent words list.Asp < 0.05 it's positive direction.


```python
#Paired t-test on response time for congruent vs incongruent words
print(stats.ttest_rel(stroop_data['Congruent'],stroop_data['Incongruent']))
```

    Ttest_relResult(statistic=-8.020706944109957, pvalue=4.1030005857111781e-08)
    


```python
#Confidence intervals (CI) are a useful statistic to include 
#because they indicate where the true population mean might be. 
#It is common to report 95% confidence intervals.
stats.norm.interval(0.95, loc = mean_of_differences, scale = Stderr_mean)
```




    (-9.9110923956457153, -6.0184916043542858)



Confidence interval = (-9.91, -6.02)
The users who participated in testing has a delay of 9.9 to 6 seconds in reading the Incongruent words condition. I.e Incongruent words took more time to read when compared to Congruent words.

#### 6. Optional: What do you think is responsible for the effects observed? Can you think of an alternative or similar task that would result in a similar effect?


When we read anything brain automatically understands the meaning of words.
Where as recognizing colors is not an “automatic process”.  Especially when the brain has to read the wrongly colored words. I.e When the word color is different from word(name of the colour). So the experiment has proved that when a color word is printed in the same color as the word, people can name the ink color more quickly 
Similar effects:

* Compare normal words with  words turned upside down
* Compare full words with  their corresponding shorcut words.


##### References

https://en.wikipedia.org/wiki/Stroop_effect

http://www.statstutor.ac.uk/resources/uploaded/paired-t-test.pdf

http://blog.minitab.com/blog/adventures-in-statistics-2/understanding-t-tests%3A-1-sample%2C-2-sample%2C-and-paired-t-tests

http://www.statisticshowto.com/p-value/

http://www.statisticshowto.com/how-to-decide-if-a-hypothesis-test-is-a-left-tailed-test-or-a-right-tailed-test/

https://www.youtube.com/watch?v=rWFDXt-MlNs


