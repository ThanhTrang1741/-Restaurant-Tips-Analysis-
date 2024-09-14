# **🍽️ Restaurant Tips Analysis**

This project aims to use the restaurant tips dataset to practice creating composition plots and visualizations. We will examine the relationship between different variables and the tips given.

The dataset consists of information from 244 restaurant bills, collected in the US in 1987.

It includes details about the tips given to restaurant staff, such as the total bill, tip amount, gender of the person paying, smoking status, day of the week, time of day, and party size.

## **👣 The First Steps**
### **📥 Data import**
First, let's import the needed libraries: Pandas & Matplotlib

    import pandas as pd
    import matplotlib.pyplot as plt
    
Then load data from the following link: https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv
    pd.read_csv('https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv')

|  id | total_bill |   tip |  sex | smoker | day | time |   size |     |
|----:|-----------:|------:|-----:|-------:|----:|-----:|-------:|-----|
|  0  |          0 | 16.99 | 1.01 | Female |  No |  Sun | Dinner |   2 |
|  1  |          1 | 10.34 | 1.66 |   Male |  No |  Sun | Dinner |   3 |
|  2  |          2 | 21.01 | 3.50 |   Male |  No |  Sun | Dinner |   3 |
|  3  |          3 | 23.68 | 3.31 |   Male |  No |  Sun | Dinner |   2 |
|  4  |          4 | 24.59 | 3.61 | Female |  No |  Sun | Dinner |   4 |
| ... |        ... |   ... |  ... |    ... | ... |  ... |    ... | ... |
| 239 |        239 | 29.03 | 5.92 |   Male |  No |  Sat | Dinner |   3 |
| 240 |        240 | 27.18 | 2.00 | Female | Yes |  Sat | Dinner |   2 |
| 241 |        241 | 22.67 | 2.00 |   Male | Yes |  Sat | Dinner |   2 |
| 242 |        242 | 17.82 | 1.75 |   Male |  No |  Sat | Dinner |   2 |
| 243 |        243 | 18.78 | 3.00 | Female |  No | Thur | Dinner |   2 |



### **🔍 Data exploration**
    df=pd.read_csv('https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv')
    df.head()
    
|   | id | total_bill |  tip |    sex | smoker | day |   time | size |
|--:|---:|-----------:|-----:|-------:|-------:|----:|-------:|-----:|
| 0 |  0 |      16.99 | 1.01 | Female |     No | Sun | Dinner |    2 |
| 1 |  1 |      10.34 | 1.66 |   Male |     No | Sun | Dinner |    3 |
| 2 |  2 |      21.01 | 3.50 |   Male |     No | Sun | Dinner |    3 |
| 3 |  3 |      23.68 | 3.31 |   Male |     No | Sun | Dinner |    2 |
| 4 |  4 |      24.59 | 3.61 | Female |     No | Sun | Dinner |    4 |


> 🎉 Great! It seems to be okay.

As you can see each observation represents a customer who left a tip at a restaurant.

We can see information about:
* the day it occurred
* if it was at lunch or dinner
* the total bill
* the sex of the person
* if they were a smoker or not
* the size of the party

Before continuing take a look at a few rows of the data and use `info` and `describe` to analyze dataset column types and values.


#### **Column types checking**
Show the columns of the dataframe and their types:

    df.dtypes

|     id     |   int64 |
|:----------:|--------:|
| total_bill | float64 |
|     tip    | float64 |
|     sex    |  object |
|   smoker   |  object |
|     day    |  object |
|    time    |  object |
|    size    |   int64 |


> **Ooops... 🤔**
>
> We have string columns considered as objects.
Let's fix their types and make them string:

    df.astype({'sex': 'string', 'smoker': 'string', 'day': 'string', 'time': 'string'}).dtypes

|     id     |          int64 |
|:----------:|---------------:|
| total_bill |        float64 |
|     tip    |        float64 |
|     sex    | string[python] |
|   smoker   | string[python] |
|     day    | string[python] |
|    time    | string[python] |
|    size    |          int64 |

#### **Basic descriptive statistics**

    df.describe()

|       |         id | total_bill |        tip |       size |
|------:|-----------:|-----------:|-----------:|-----------:|
| count | 244.000000 | 244.000000 | 244.000000 | 244.000000 |
|  mean | 121.500000 |  19.785943 |   2.998279 |   2.569672 |
|  std  |  70.580923 |   8.902412 |   1.383638 |   0.951100 |
|  min  |   0.000000 |   3.070000 |   1.000000 |   1.000000 |
|  25%  |  60.750000 |  13.347500 |   2.000000 |   2.000000 |
|  50%  | 121.500000 |  17.795000 |   2.900000 |   2.000000 |
|  75%  | 182.250000 |  24.127500 |   3.562500 |   3.000000 |
|  max  | 243.000000 |  50.810000 |  10.000000 |   6.000000 |

Great! Now we know a little more about our data.

➡️ Let's move forward!
## **💸 Tip value influencers**
### **🚬 Do people who smoke give more tips?**
Let's figure out the difference between smokers and non-smokers in terms of their behavior and purchasing habits in public catering establishments.

#### **Separate smokers and non-smokers**
Create a new dataframe `smokers_df` containing only info about smokers.

    smokers_df = df[df['smoker'] == 'Yes']
    smokers_df.head(5)
    
|  id | total_bill |       tip |       sex |   smoker | day | time |   size |   |
|----:|-----------:|----------:|----------:|---------:|----:|-----:|-------:|---|
|  56 |         56 |     38.01 |      3.00 |     Male | Yes |  Sat | Dinner | 4 |
|  58 |         58 |     11.24 |      1.76 |     Male | Yes |  Sat | Dinner | 2 |
|  60 |         60 |     20.29 |      3.21 |     Male | Yes |  Sat | Dinner | 2 |
|  61 |         61 |     13.81 |      2.00 |     Male | Yes |  Sat | Dinner | 2 |
|  62 |         62 |     11.02 |      1.98 |     Male | Yes |  Sat | Dinner | 2 |

Also create another one dataframe `non_smokers_df` containing only non-smokers.

    non_smokers_df = df[df['smoker'] == 'No']
    non_smokers_df.head(5)

|     |         id | total_bill |       tip |      sex | smoker | day |   time | size |
|----:|-----------:|-----------:|----------:|---------:|-------:|----:|-------:|-----:|
|  0  |          0 |      16.99 |      1.01 |   Female |     No | Sun | Dinner |    2 |
|  1  |          1 |      10.34 |      1.66 |     Male |     No | Sun | Dinner |    3 |
|  2  |          2 |      21.01 |      3.50 |     Male |     No | Sun | Dinner |    3 |
|  3  |          3 |      23.68 |      3.31 |     Male |     No | Sun | Dinner |    2 |
|  4  |          4 |      24.59 |      3.61 |   Female |     No | Sun | Dinner |    4 |

#### **Compare their measures of central tendency**
As we know, measures of central tendency is one of the basic tools, that allow us to compare different datasets as it shows the most typical values.
##### **🌏 Whole dataset**
Calculate them for the 'tip' column through the whole dataset and save them into the following variables:

min => common_tip_min
max => common_tip_max
mean => common_tip_mean
median => common_tip_median

    common_tip_min = df['tip'].min()
    common_tip_max = df['tip'].max()
    common_tip_mean = df['tip'].mean()
    common_tip_median = df['tip'].median()

Let's show the resulting values for whole dataset (we already have the code written for you 😉)

   # Make a list of values
    common_values = [common_tip_min, common_tip_max, common_tip_mean, common_tip_median]
   # Round all the values to 4 decimal places
    common_values = map(lambda x: round(x, 4), common_values)

   # Make a dataframe from the list
    common_mct = pd.DataFrame(common_values, index=['min', 'max', 'mean', 'median'])
   # Output the dataframe
    common_mct

|   min  |  1.0000 |
|:------:|--------:|
|   max  | 10.0000 |
|  mean  |  2.9983 |
| median |  2.9000 |

##### **🚬 Smokers**
Do the same taking into account only smokers. Use the following variables:

min => smokers_tip_min
max => smokers_tip_max
mean => smokers_tip_mean
median => smokers_tip_median

    smokers_tip_min = smokers_df['tip'].min()
    smokers_tip_max = smokers_df['tip'].max()
    smokers_tip_mean = smokers_df['tip'].mean()
    smokers_tip_median = smokers_df['tip'].median()

Let's output the results in the same format.

Make the same dataframe containing the measures of central tendency for smokers as we did for whole dataset. Then output it.

   # Make a list of values
    smokers_values = [smokers_tip_min, smokers_tip_max, smokers_tip_mean, smokers_tip_median]
   # Round all the values to 4 decimal places
    smokers_values = map(lambda x: round(x, 4), smokers_values)

   # Make a dataframe from the list
    smokers_mct = pd.DataFrame(smokers_values, index=['min', 'max', 'mean', 'median'])
   # Output the dataframe
    smokers_mct

|   min  |  1.0000 |
|:------:|--------:|
|   max  | 10.0000 |
|  mean  |  3.0087 |
| median |  3.0000 |

##### **🚭 Non-smokers**
Now repeat it for non-smokers. Use the following variables:

min => non_smokers_tip_min
max => non_smokers_tip_max
mean => non_smokers_tip_mean
median => non_smokers_tip_median

    non_smokers_tip_min = non_smokers_df['tip'].min()
    non_smokers_tip_max = non_smokers_df['tip'].max()
    non_smokers_tip_mean = non_smokers_df['tip'].mean()
    non_smokers_tip_median = non_smokers_df['tip'].median()

Make the same dataframe containing the measures of central tendency for non-smokers as we did for whole dataset. Then output it.

   # Make a list of values
    non_smokers_values = [non_smokers_tip_min, non_smokers_tip_max, non_smokers_tip_mean, 
    non_smokers_tip_median]
   # Round all the values to 4 decimal places
    non_smokers_values = map(lambda x: round(x, 4), non_smokers_values)

   # Make a dataframe from the list
    non_smokers_mct = pd.DataFrame(non_smokers_values, index=['min', 'max', 'mean', 'median'])
   # Output the dataframe
    non_smokers_mct

|   min  | 1.0000 |
|:------:|-------:|
|   max  | 9.0000 |
|  mean  | 2.9919 |
| median | 2.7400 |


##### **📝 Conclusion**\
Let's show the retrieved results together (we already have the code written for you 😉):

    all_vals_dict = {
       'Common': {'min': common_tip_min, 'max': common_tip_max, 'mean': common_tip_mean, 'median': common_tip_median},
       'Smokers': {'min': smokers_tip_min, 'max': smokers_tip_max, 'mean': smokers_tip_mean, 'median': smokers_tip_median},
       'Non-smokers': {'min': non_smokers_tip_min, 'max': non_smokers_tip_max, 'mean': non_smokers_tip_mean, 'median': non_smokers_tip_median}
    }

   # Make a dataframe
    all_mct = pd.DataFrame(all_vals_dict)
   # Output the dataframe
    all_mct


|        |   Common |  Smokers | Non-smokers |
|-------:|---------:|---------:|------------:|
|   min  | 1.000000 |  1.00000 |    1.000000 |
|   max  | 9.000000 | 10.00000 |    9.000000 |
|  mean  | 2.991854 |  3.00871 |    2.991854 |
| median | 2.740000 |  3.00000 |    2.740000 |

**Insights based on measures of central tendency comparison:**
1. Insight 1 : Smokers have a lower range of tips (1.00 to 4.00) compared to Non-Smokers (1.50 to 10.00). This indicates that tips for non-smokers can be substantially higher.
2. Insight 2: Non-Smokers have a higher maximum tip (10.00) compared to smokers (4.00), suggesting that non-smokers may leave larger tips.


#### **Look at histograms**

As we already discussed on the last lecture, there are a lot of cases, when comparing the measures of central tendency is not enough.

This is because they only show the most typical values. However, the way data is distributed is equally important. There are situations where measures of central tendency are exactly the same, but due to different distributions, it is incorrect to say that the datasets are similar.

**General conclusion:** *The mean tip is significantly higher for non-smokers compared to smokers, suggesting that, on average, non-smokers leave larger tips.The median tip for non-smokers is also higher than for smokers, indicating that the central value of tips for non-smokers is 


##### **🌏 Whole dataset tips histogram**
Plot the histogram for the whole dataset tips distribution.

<u>Use the following settings:</u>

* Size: `15 x 5`
* Color: `#74b9ff`
* X-axis label: `Tip value`
* Y-axis label: `Frequency`
* Chart title: `Whole dataset tip values`
* Gridlines: `show`

   # Plot the histogram
    plt.figure(figsize=(15, 5))
    plt.hist(df['tip'], bins=10, color='#74b9ff')

   # Add labels and title
    plt.xlabel('Tip value')
    plt.ylabel('Frequency')
    plt.title('Whole dataset tip values')

   # Show gridlines
    plt.grid(True)

   # Display the plot
    plt.show()

  ![image](https://github.com/user-attachments/assets/ec0512b9-68a8-4e60-9d11-06251eb00b3f)

##### **🚬 Smokers tips histogram**
Plot the histogram for smokers tips distribution.

Use the following settings:

* Size: 15 x 5
* Color: #ff7675
* X-axis label: Tip value
* Y-axis label: Frequency
* Chart title: Smokers tip values
* Gridlines: show

   # Histogram
    plt.figure(figsize=(15, 5)) 
    plt.hist(smokers_df['tip'], bins=10, color='#ff7675')

   # Add labels and title
    plt.xlabel('Tip value')
    plt.ylabel('Frequency')
    plt.title('Smokers tip values')

   # Show gridlines
    plt.grid(True)

   # Display the plot
    plt.show()

![image](https://github.com/user-attachments/assets/f50da5e1-ae2b-4f39-9dcf-8f81477d06dc)

##### **🚭 Non-smokers tips histogram**

Plot the histogram for non-smokers tips distribution.

Use the following settings:

* Size: 15 x 5
* Color: #55efc4
* X-axis label: Tip value
* Y-axis label: Frequency
* Chart title: Non-smokers tip values
* Gridlines: show

   # Histogram
    plt.figure(figsize=(15, 5))
    plt.hist(non_smokers_df['tip'], bins=10, color='#55efc4')

   # Add labels and title
    plt.xlabel('Tip value')
    plt.ylabel('Frequency')
    plt.title('Non-smokers tip values')

   # Show gridlines
    plt.grid(True)

   # Display the plot
    plt.show()

![image](https://github.com/user-attachments/assets/9006011c-c9fa-426c-8ce6-0b09cf520b45)

    ##### **⭐ Extra-task with a higher difficulty**
Plot all 3 charts in a row in the same cell:

   # Create subplots
    fig, axs = plt.subplots(1, 3, figsize=(15, 5), sharey=True)

   # Plot histogram for the whole dataset
    axs[0].hist(df['tip'], bins=10, color='#74b9ff')
    axs[0].set_title('Whole dataset tip values')
    axs[0].set_xlabel('Tip value')
    axs[0].set_ylabel('Frequency')
    axs[0].grid(True)

   # Plot histogram for smokers
    axs[1].hist(smokers_df['tip'], bins=10, color='#ff7675')
    axs[1].set_title('Smokers tip values')
    axs[1].set_xlabel('Tip value')
    axs[1].grid(True)

   # Plot histogram for non-smokers
    axs[2].hist(non_smokers_df['tip'], bins=10, color='#55efc4')
    axs[2].set_title('Non-smokers tip values')
    axs[2].set_xlabel('Tip value')
    axs[2].grid(True)

![image](https://github.com/user-attachments/assets/f02dda7d-9005-4918-81d6-794f64b69a9f)

**Insights based on distribution comparison:**

---
Insight 1:The fact that the mean and median are close to each other in the whole dataset and non-smokers indicates that their tip distributions are relatively symmetrical and not heavily skewed.
Insight 2:The higher median for smokers suggests that their tip distribution might be slightly skewed towards higher values, implying that more smokers leave tips that are above the central value compared to non-smokers.
*General conclusion:Smokers tend to leave tips that are slightly higher on average and are more likely to leave tips above the median compared to non-smokers. Despite having a higher maximum tip, the overall distribution for smokers does not deviate significantly from the non-smokers or the whole dataset in terms of mean and median.*

### **👨👩 Do males give more tips?**

    import pandas as pd
    import matplotlib.pyplot as plt

   # Load the dataset
    df=pd.read_csv('https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv')


   # Filter the DataFrame for male and female
    male_df = df[df['sex'] == 'Male']
    female_df = df[df['sex'] == 'Female']

   # Calculate measures of central tendency for male
    male_tip_min = male_df['tip'].min()
    male_tip_max = male_df['tip'].max()
    male_tip_mean = male_df['tip'].mean()
    male_tip_median = male_df['tip'].median()

   # Calculate measures of central tendency for female
    female_tip_min = female_df['tip'].min()
    female_tip_max = female_df['tip'].max()
    female_tip_mean = female_df['tip'].mean()
    female_tip_median = female_df['tip'].median()

   # Print the results
    print("male")
    print(f"min: {male_tip_min:.6f}")
    print(f"max: {male_tip_max:.6f}")
    print(f"mean: {male_tip_mean:.6f}")
    print(f"median: {male_tip_median:.6f}")

    print("\nfemale")
    print(f"min: {female_tip_min:.6f}")
    print(f"max: {female_tip_max:.6f}")
    print(f"mean: {female_tip_mean:.6f}")
    print(f"median: {female_tip_median:.6f}")

   # Create subplots for histograms
    fig, axs = plt.subplots(1, 2, figsize=(15, 5), sharey=True)

   # Plot histogram for male and female
    axs[0].hist(lunch_df['tip'], bins=10, color='#ff7675')
    axs[0].set_title('male tip values')
    axs[0].set_xlabel('Tip value')
    axs[0].set_ylabel('Frequency')
    axs[0].grid(True)

   # Plot histogram for male and female
    axs[1].hist(dinner_df['tip'], bins=10, color='#55efc4')
    axs[1].set_title('female tip values')
    axs[1].set_xlabel('Tip value')
    axs[1].grid(True)

   # Adjust layout
    plt.tight_layout()

   # Display the plot
    plt.show()

male
min: 1.000000
max: 10.000000
mean: 3.089618
median: 3.000000

female
min: 1.000000
max: 6.500000
mean: 2.833448
median: 2.750000

![image](https://github.com/user-attachments/assets/b6e0e7fe-2b22-42b4-be31-b7364d72660e)

**Insights based on distribution comparison:**

---
1. Males have a broader range of values (1 to 10) compared to females (1 to 6.5). This suggests that there is more variability or a wider spread of data points among males.

2. The mean value for males (3.089618) is slightly higher than that for females (2.833448). This indicates that, on average, the values for males are somewhat higher.
The median value is also higher for males (3.000000) compared to females (2.750000), suggesting that the middle value in the distribution of males is greater.
The larger range and higher mean/median for males imply that the distribution might be more spread out or skewed towards higher values compared to females.

Overall, males exhibit greater variability in the data with generally higher average and median values, while females have a more concentrated range and slightly lower central tendencies.

### **📆 Do weekends bring more tips?**
Perform the same steps based on the column **day**.

    import matplotlib.pyplot as plt

   # Load the dataset
    df=pd.read_csv('https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv')

    weekdays = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri']
    weekends = ['Sat', 'Sun']

   # Filter the DataFrame for weekdays and weekends
    weekdays_df = df[df['day'].isin(weekdays)]
    weekends_df = df[df['day'].isin(weekends)]

   # Calculate measures of central tendency for weekdays
    weekdays_tip_min = weekdays_df['tip'].min()
    weekdays_tip_max = weekdays_df['tip'].max()
    weekdays_tip_mean = weekdays_df['tip'].mean()
    weekdays_tip_median = weekdays_df['tip'].median()

   # Calculate measures of central tendency for weekends
   weekends_tip_min = weekends_df['tip'].min()
   weekends_tip_max = weekends_df['tip'].max()
   weekends_tip_mean = weekends_df['tip'].mean()
   weekends_tip_median = weekends_df['tip'].median()

   # Print the results
    print("Weekdays")
    print(f"min: {weekdays_tip_min:.6f}")
    print(f"max: {weekdays_tip_max:.6f}")
    print(f"mean: {weekdays_tip_mean:.6f}")
    print(f"median: {weekdays_tip_median:.6f}")

    print("\nWeekends")
    print(f"min: {weekends_tip_min:.6f}")
    print(f"max: {weekends_tip_max:.6f}")
    print(f"mean: {weekends_tip_mean:.6f}")
    print(f"median: {weekends_tip_median:.6f}")

   # Create subplots for histograms
    fig, axs = plt.subplots(1, 2, figsize=(15, 5), sharey=True)

   # Plot histogram for weekdays
    axs[0].hist(weekdays_df['tip'], bins=10, color='#ff7675')
    axs[0].set_title('Weekdays tip values')
    axs[0].set_xlabel('Tip value')
    axs[0].set_ylabel('Frequency')
    axs[0].grid(True)

   # Plot histogram for weekends
    axs[1].hist(weekends_df['tip'], bins=10, color='#55efc4')
    axs[1].set_title('Weekends tip values')
    axs[1].set_xlabel('Tip value')
    axs[1].grid(True)

   # Adjust layout
    plt.tight_layout()

   # Display the plot
    plt.show()

Weekdays
min: 1.000000
max: 4.730000
mean: 2.734737
median: 3.000000

Weekends
min: 1.000000
max: 10.000000
mean: 3.115276
median: 3.000000

![image](https://github.com/user-attachments/assets/53c6c7f8-2f2b-46db-898c-5a839e71ddf9)

**Insights based on distribution comparison:**

---

Weekends have a broader range of tip values (1.00 to 10.00) compared to Weekdays (1.00 to 4.73). This indicates that during weekends, there are instances where people leave very high tips, which are not present or are less frequent during weekdays.
The mean tip on weekends (3.12) is higher than on weekdays (2.73). This suggests that, on average, people tend to leave slightly larger tips on weekends compared to weekdays. The median tip is the same for both weekdays and weekends (3.00). This indicates that the typical (middle) value of tips is consistent across both periods, suggesting that the central tendency of tipping behavior does not change much between weekdays and weekends.
The higher mean on weekends, despite the median being the same as on weekdays, indicates that while the typical tip value remains unchanged, there are more high-value tips on weekends. This shifts the average upwards while keeping the median constant.

In conclusion, while the typical tip amount (median) does not differ between weekdays and weekends, the overall tendency to leave higher tips (mean) and the broader range of tips on weekends indicate that weekends are generally more lucrative for tips. This could be due to a variety of factors such as increased customer satisfaction, more discretionary spending, or higher customer volume during weekends.

### **🕑 Do dinners bring more tips?**
Perform the same steps based on the column **time**.

    import pandas as pd
    import matplotlib.pyplot as plt

   # Load the dataset
    df=pd.read_csv('https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv')


   # Filter the DataFrame for lunch and dinner
    lunch_df = df[df['time'] == 'Lunch']
    dinner_df = df[df['time'] == 'Dinner']

   # Calculate measures of central tendency for lunch
    lunch_tip_min = lunch_df['tip'].min()
    lunch_tip_max = lunch_df['tip'].max()
    lunch_tip_mean = lunch_df['tip'].mean()
    lunch_tip_median = lunch_df['tip'].median()

   # Calculate measures of central tendency for dinner
    dinner_tip_min = dinner_df['tip'].min()
    dinner_tip_max = dinner_df['tip'].max()
    dinner_tip_mean = dinner_df['tip'].mean()
    dinner_tip_median = dinner_df['tip'].median()

   # Print the results
    print("lunch")
    print(f"min: {lunch_tip_min:.6f}")
    print(f"max: {lunch_tip_max:.6f}")
    print(f"mean: {lunch_tip_mean:.6f}")
    print(f"median: {lunch_tip_median:.6f}")

    print("\ndinner")
    print(f"min: {dinner_tip_min:.6f}")
    print(f"max: {dinner_tip_max:.6f}")
    print(f"mean: {dinner_tip_mean:.6f}")
    print(f"median: {dinner_tip_median:.6f}")

   # Create subplots for histograms
    fig, axs = plt.subplots(1, 2, figsize=(15, 5), sharey=True)

   # Plot histogram for weekdays
    axs[0].hist(lunch_df['tip'], bins=10, color='#ff7675')
    axs[0].set_title('Lunch tip values')
    axs[0].set_xlabel('Tip value')
    axs[0].set_ylabel('Frequency')
    axs[0].grid(True)

   # Plot histogram for weekends
    axs[1].hist(dinner_df['tip'], bins=10, color='#55efc4')
    axs[1].set_title('Dinner tip values')
    axs[1].set_xlabel('Tip value')
    axs[1].grid(True)

   # Adjust layout
    plt.tight_layout()

   # Display the plot
    plt.show()

lunch
min: 1.250000
max: 6.700000
mean: 2.728088
median: 2.250000

dinner
min: 1.000000
max: 10.000000
mean: 3.102670
median: 3.000000

![image](https://github.com/user-attachments/assets/09ecd89a-e5aa-4c96-b8ed-292c9102fee9)

**Insights based on distribution comparison:**

---

1. Dinner has a broader range of tip values (1.00 to 10.00) compared to Lunch (1.25 to 6.70). This indicates that dinner services often receive both very low and very high tips, with the potential for larger tips being higher during dinner.
2. The mean tip for dinner (3.10) is higher than for lunch (2.73). This The median tip is higher during dinner (3.00) compared to lunch (2.25). This indicates that the typical tip amount is greater during dinner services.

The higher mean and median for dinner suggest that there is a tendency for larger tips during dinner. Even though both periods have a similar lower bound for tips, dinner services see more generous tipping on average.
The maximum tip being higher during dinner further supports the observation that some customers are willing to leave significantly higher tips in the evening.

In conclusion, the analysis reveals that dinner services tend to receive higher tips on average and have a wider range of tip values compared to lunch services. This suggests that customers are generally more generous during dinner, which could be due to various factors including meal cost, dining experience, and overall customer behavior. Adjusting service strategies to align with these patterns can help in maximizing tip earnings and improving overall service quality.
