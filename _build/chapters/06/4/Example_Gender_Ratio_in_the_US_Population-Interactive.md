---
redirect_from:
  - "/chapters/06/4/example-gender-ratio-in-the-us-population-interactive"
interact_link: content/chapters/06/4/Example_Gender_Ratio_in_the_US_Population-Interactive.ipynb
title: 'Interactive Example: Trends in Gender'
prev_page:
  url: /chapters/06/4/Example_Gender_Ratio_in_the_US_Population
  title: 'Example: Trends in Gender'
next_page:
  url: /chapters/07/Visualization
  title: 'Visualization'
comment: "***PROGRAMMATICALLY GENERATED, DO NOT EDIT. SEE ORIGINAL FILES IN /content***"
---







# Example: Trends in Gender

We are now equipped with enough coding skills to examine features and trends in subgroups of the U.S. population. In this example, we will look at the distribution of males and females across age groups. We will continue using the `us_pop` table from the previous section, but with all years.



{:.input_area}
```python
us_pop
```





<div markdown="0" class="output output_html">
<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SEX</th> <th>AGE</th> <th>2010</th> <th>2011</th> <th>2012</th> <th>2013</th> <th>2014</th> <th>2015</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0   </td> <td>0   </td> <td>3951330</td> <td>3963087</td> <td>3926540</td> <td>3931141</td> <td>3949775</td> <td>3978038</td>
        </tr>
        <tr>
            <td>0   </td> <td>1   </td> <td>3957888</td> <td>3966551</td> <td>3977939</td> <td>3942872</td> <td>3949776</td> <td>3968564</td>
        </tr>
        <tr>
            <td>0   </td> <td>2   </td> <td>4090862</td> <td>3971565</td> <td>3980095</td> <td>3992720</td> <td>3959664</td> <td>3966583</td>
        </tr>
        <tr>
            <td>0   </td> <td>3   </td> <td>4111920</td> <td>4102470</td> <td>3983157</td> <td>3992734</td> <td>4007079</td> <td>3974061</td>
        </tr>
        <tr>
            <td>0   </td> <td>4   </td> <td>4077551</td> <td>4122294</td> <td>4112849</td> <td>3994449</td> <td>4005716</td> <td>4020035</td>
        </tr>
        <tr>
            <td>0   </td> <td>5   </td> <td>4064653</td> <td>4087709</td> <td>4132242</td> <td>4123626</td> <td>4006900</td> <td>4018158</td>
        </tr>
        <tr>
            <td>0   </td> <td>6   </td> <td>4073013</td> <td>4074993</td> <td>4097605</td> <td>4142916</td> <td>4135930</td> <td>4019207</td>
        </tr>
        <tr>
            <td>0   </td> <td>7   </td> <td>4043046</td> <td>4083225</td> <td>4084913</td> <td>4108349</td> <td>4155326</td> <td>4148360</td>
        </tr>
        <tr>
            <td>0   </td> <td>8   </td> <td>4025604</td> <td>4053203</td> <td>4093177</td> <td>4095711</td> <td>4120903</td> <td>4167887</td>
        </tr>
        <tr>
            <td>0   </td> <td>9   </td> <td>4125415</td> <td>4035710</td> <td>4063152</td> <td>4104072</td> <td>4108349</td> <td>4133564</td>
        </tr>
    </tbody>
</table>
<p>... (296 rows omitted)</p>
</div>



As we know from having examined this dataset earlier, a [description of the table](http://www2.census.gov/programs-surveys/popest/datasets/2010-2015/national/asrh/nc-est2015-agesex-res.pdf) appears online. Here is a reminder of what the table contains. 

Each row represents an age group. The `SEX` column contains numeric codes: `0` stands for the total, `1` for male, and `2` for female. The `AGE` column contains ages in completed years, but the special value `999` represents the entire population regardless of age. The rest of the columns contain estimates of the US population.

### Understanding `AGE` = 100
As a preliminary, let's interpret data in the final age category in the table, where `AGE` is 100. The code below extracts the rows for the combined group of men and women (`SEX` code 0) for the highest ages.



{:.input_area}
```python
us_pop.where('SEX', are.equal_to(0)).where('AGE', are.between(97, 101))
```





<div markdown="0" class="output output_html">
<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SEX</th> <th>AGE</th> <th>2010</th> <th>2011</th> <th>2012</th> <th>2013</th> <th>2014</th> <th>2015</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0   </td> <td>97  </td> <td>68893</td> <td>73274</td> <td>77156</td> <td>79953</td> <td>83089</td> <td>92377</td>
        </tr>
        <tr>
            <td>0   </td> <td>98  </td> <td>47037</td> <td>50670</td> <td>54509</td> <td>57015</td> <td>59726</td> <td>61991</td>
        </tr>
        <tr>
            <td>0   </td> <td>99  </td> <td>32178</td> <td>33636</td> <td>36779</td> <td>39271</td> <td>41468</td> <td>43641</td>
        </tr>
        <tr>
            <td>0   </td> <td>100 </td> <td>54410</td> <td>57702</td> <td>61821</td> <td>66189</td> <td>71626</td> <td>76974</td>
        </tr>
    </tbody>
</table>
</div>



Not surprisingly, the numbers of people are smaller at higher ages – for example, there are fewer 99-year-olds than 98-year-olds. 

It does come as a surprise, though, that the numbers for `AGE` 100 are quite a bit larger than those for age 99. A closer examination of the documentation shows that it's because the Census Bureau used 100 as the code for everyone aged 100 or more. 

The row with `AGE` 100 doesn't just represent 100-year-olds – it also includes those who are older than 100. That is why the numbers in that row are larger than in the row for the 99-year-olds.

### Overall Proportions of Males and Females
We will now begin looking at gender ratios in any year (you can select the year e.g. 2014). First, let's look at all the age groups together. Remember that this means looking at the rows where the "age" is coded 999. The function `all_ages` returns a table containing this information. There are three rows: one for the total of both genders, one for males (`SEX` code 1), and one for females (`SEX` code 2).



{:.input_area}
```python
def all_ages(year="2014"):
    us_pop_year = us_pop.select(list(us_pop.labels[:2])+[year])
    return us_pop_year.where('AGE', are.equal_to(999))
interact(all_ages, year=list(us_pop.labels[2:]));
```



{:.output .output_data_text}
```
interactive(children=(Dropdown(description='year', index=4, options=('2010', '2011', '2012', '2013', '2014', '…
```


Row 0 of `all_ages` contains the total U.S. population in each of the two years. The United States had just under 319 million in 2014.

Row 1 contains the counts for males and Row 2 for females. Compare these two rows to see that in 2014, there were more females than males in the United States. 

The population counts in Row 1 and Row 2 add up to the total population in Row 0. 

For comparability with other quantities, we will need to convert these counts to percents out of the total population. Let's access the total for a particular year and name it `pop_base`. Then, we'll show a population table with a proportion column. Consistent with our earlier observation that there were more females than males, about 50.8% of the population in 2014 was female and about 49.2% male in each of the two years. 



{:.input_area}
```python
def all_ages_proportion(year="2014"):
    pop_base = all_ages(year).column(year).item(0)
    return all_ages(year).with_column('Proportion', all_ages(year).column((year))/pop_base).set_format('Proportion', PercentFormatter)
interact(all_ages_proportion, year=list(us_pop.labels[2:]));
```



{:.output .output_data_text}
```
interactive(children=(Dropdown(description='year', index=4, options=('2010', '2011', '2012', '2013', '2014', '…
```


### Proportions of Boys and Girls among Infants

When we look at infants, however, the opposite is true. Let's define infants to be babies who have not yet completed one year, represented in the row corresponding to `AGE` 0. Here are their numbers in the population. You can see that male infants outnumbered female infants.



{:.input_area}
```python
def infants(year="2014"):
    us_pop_year = us_pop.select(list(us_pop.labels[:2])+[year])
    return us_pop_year.where('AGE', are.equal_to(0))
interact(infants, year=list(us_pop.labels[2:]));
```



{:.output .output_data_text}
```
interactive(children=(Dropdown(description='year', index=4, options=('2010', '2011', '2012', '2013', '2014', '…
```


As before, we can convert these counts to percents out of the total numbers of infants. The resulting table shows that in 2014, just over 51% of infants in the U.S. were male. 



{:.input_area}
```python
def infants_proportion(year="2014"):
    infants_year = infants(year).column(year).item(0)
    return infants(year).with_column('Proportion', infants(year).column(year)/infants_year).set_format('Proportion', PercentFormatter)
interact(infants_proportion, year=list(us_pop.labels[2:]));
```



{:.output .output_data_text}
```
interactive(children=(Dropdown(description='year', index=4, options=('2010', '2011', '2012', '2013', '2014', '…
```


In fact, it has long been observed that the proportion of boys among newborns is slightly more than 1/2. The reason for this is not thoroughly understood, and [scientists are still working on it](http://www.npr.org/sections/health-shots/2015/03/30/396384911/why-are-more-baby-boys-born-than-girls).

### Female:Male Gender Ratio at Each Age

We have seen that while there are more baby boys than baby girls, there are more females than males overall. So it's clear that the split between genders must vary across age groups.

To study this variation, we will separate out the data for the females and the males, and eliminate the row where all the ages are aggregated and `AGE` is coded as 999.

The tables `females` and `males` contain the data for each the two genders.



{:.input_area}
```python
def females(year="2014"):
    us_pop_year = us_pop.select(list(us_pop.labels[:2])+[year])
    return us_pop_year.where('SEX', are.equal_to(2)).where('AGE', are.not_equal_to(999))
interact(females, year=list(us_pop.labels[2:]));
```



{:.output .output_data_text}
```
interactive(children=(Dropdown(description='year', index=4, options=('2010', '2011', '2012', '2013', '2014', '…
```




{:.input_area}
```python
def males(year="2014"):
    us_pop_year = us_pop.select(list(us_pop.labels[:2])+[year])
    return us_pop_year.where('SEX', are.equal_to(1)).where('AGE', are.not_equal_to(999))
interact(males, year=list(us_pop.labels[2:]));
```



{:.output .output_data_text}
```
interactive(children=(Dropdown(description='year', index=4, options=('2010', '2011', '2012', '2013', '2014', '…
```


The plan now is to compare the number of women and the number of men at each age, for each of the two years. Array and Table methods give us straightforward ways to do this. Both of these tables have one row for each age.



{:.input_area}
```python
males('2014').column('AGE')
```





{:.output .output_data_text}
```
array([  0,   1,   2,   3,   4,   5,   6,   7,   8,   9,  10,  11,  12,
        13,  14,  15,  16,  17,  18,  19,  20,  21,  22,  23,  24,  25,
        26,  27,  28,  29,  30,  31,  32,  33,  34,  35,  36,  37,  38,
        39,  40,  41,  42,  43,  44,  45,  46,  47,  48,  49,  50,  51,
        52,  53,  54,  55,  56,  57,  58,  59,  60,  61,  62,  63,  64,
        65,  66,  67,  68,  69,  70,  71,  72,  73,  74,  75,  76,  77,
        78,  79,  80,  81,  82,  83,  84,  85,  86,  87,  88,  89,  90,
        91,  92,  93,  94,  95,  96,  97,  98,  99, 100])
```





{:.input_area}
```python
females('2014').column('AGE')
```





{:.output .output_data_text}
```
array([  0,   1,   2,   3,   4,   5,   6,   7,   8,   9,  10,  11,  12,
        13,  14,  15,  16,  17,  18,  19,  20,  21,  22,  23,  24,  25,
        26,  27,  28,  29,  30,  31,  32,  33,  34,  35,  36,  37,  38,
        39,  40,  41,  42,  43,  44,  45,  46,  47,  48,  49,  50,  51,
        52,  53,  54,  55,  56,  57,  58,  59,  60,  61,  62,  63,  64,
        65,  66,  67,  68,  69,  70,  71,  72,  73,  74,  75,  76,  77,
        78,  79,  80,  81,  82,  83,  84,  85,  86,  87,  88,  89,  90,
        91,  92,  93,  94,  95,  96,  97,  98,  99, 100])
```



For any given age, we can get the Female:Male gender ratio by dividing the number of females by the number of males. To do this in one step, we can use `column` to extract the array of female counts and the corresponding array of male counts, and then simply divide one array by the other. Elementwise division will create an array of gender ratios for all the years.



{:.input_area}
```python
def ratios(year="2014"):
    return Table().with_columns('AGE', females(year).column('AGE'),year+' F:M RATIO', females(year).column(year)/males(year).column(year))
interact(ratios, year=list(us_pop.labels[2:]));
```



{:.output .output_data_text}
```
interactive(children=(Dropdown(description='year', index=4, options=('2010', '2011', '2012', '2013', '2014', '…
```


You can see from the display that the ratios are all around 0.96 for children aged nine or younger. When the Female:Male ratio is less than 1, there are fewer females than males. Thus what we are seeing is that there were fewer girls than boys in each of the age groups 0, 1, 2, and so on through 9. Moreover, in each of these age groups, there were about 96 girls for every 100 boys.

So how can the overall proportion of females in the population be higher than the males? 

Something extraordinary happens when we examine the other end of the age range. Here are the Female:Male ratios for people aged more than 75.



{:.input_area}
```python
ratios("2014").where('AGE', are.above(75)).show()
```



<div markdown="0" class="output output_html">
<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>AGE</th> <th>2014 F:M RATIO</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>76  </td> <td>1.23487       </td>
        </tr>
        <tr>
            <td>77  </td> <td>1.25797       </td>
        </tr>
        <tr>
            <td>78  </td> <td>1.28244       </td>
        </tr>
        <tr>
            <td>79  </td> <td>1.31627       </td>
        </tr>
        <tr>
            <td>80  </td> <td>1.34138       </td>
        </tr>
        <tr>
            <td>81  </td> <td>1.37967       </td>
        </tr>
        <tr>
            <td>82  </td> <td>1.41932       </td>
        </tr>
        <tr>
            <td>83  </td> <td>1.46552       </td>
        </tr>
        <tr>
            <td>84  </td> <td>1.52048       </td>
        </tr>
        <tr>
            <td>85  </td> <td>1.5756        </td>
        </tr>
        <tr>
            <td>86  </td> <td>1.65096       </td>
        </tr>
        <tr>
            <td>87  </td> <td>1.72172       </td>
        </tr>
        <tr>
            <td>88  </td> <td>1.81223       </td>
        </tr>
        <tr>
            <td>89  </td> <td>1.91837       </td>
        </tr>
        <tr>
            <td>90  </td> <td>2.01263       </td>
        </tr>
        <tr>
            <td>91  </td> <td>2.09488       </td>
        </tr>
        <tr>
            <td>92  </td> <td>2.2299        </td>
        </tr>
        <tr>
            <td>93  </td> <td>2.33359       </td>
        </tr>
        <tr>
            <td>94  </td> <td>2.52285       </td>
        </tr>
        <tr>
            <td>95  </td> <td>2.67253       </td>
        </tr>
        <tr>
            <td>96  </td> <td>2.87998       </td>
        </tr>
        <tr>
            <td>97  </td> <td>3.09104       </td>
        </tr>
        <tr>
            <td>98  </td> <td>3.41826       </td>
        </tr>
        <tr>
            <td>99  </td> <td>3.63278       </td>
        </tr>
        <tr>
            <td>100 </td> <td>4.25966       </td>
        </tr>
    </tbody>
</table>
</div>


Not only are all of these ratios greater than 1, signifying more women than men in all of these age groups, many of them are considerably greater than 1. 

- At ages 89 and 90 the ratios are close to 2, meaning that there were about twice as many women as men at those ages in 2014.
- At ages 98 and 99, there were about 3.5 to 4 times as many women as men. 

If you are wondering how many people there were at these advanced ages, you can use Python to find out:



{:.input_area}
```python
males("2014").where('AGE', are.between(98, 100))
```





<div markdown="0" class="output output_html">
<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SEX</th> <th>AGE</th> <th>2014</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1   </td> <td>98  </td> <td>13518</td>
        </tr>
        <tr>
            <td>1   </td> <td>99  </td> <td>8951 </td>
        </tr>
    </tbody>
</table>
</div>





{:.input_area}
```python
females("2014").where('AGE', are.between(98, 100))
```





<div markdown="0" class="output output_html">
<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SEX</th> <th>AGE</th> <th>2014</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2   </td> <td>98  </td> <td>46208</td>
        </tr>
        <tr>
            <td>2   </td> <td>99  </td> <td>32517</td>
        </tr>
    </tbody>
</table>
</div>



The graph below shows the gender ratios plotted against age. The blue curve shows the 2014 ratio by age.

The ratios are almost 1 (signifying close to equal numbers of males and females) for ages 0 through 60, but they start shooting up dramatically (more females than males) starting at about age 65.

That females outnumber males in the U.S. is partly due to the marked gender imbalance in favor of women among senior citizens.



{:.input_area}
```python
def ratios_plot(year="2014"):
    return ratios(year).plot('AGE')
interact(ratios_plot, year=list(us_pop.labels[2:]));
```



{:.output .output_data_text}
```
interactive(children=(Dropdown(description='year', index=4, options=('2010', '2011', '2012', '2013', '2014', '…
```




{:.input_area}
```python
import nbinteract as nbi
def normal(mean, sd):
    '''Returns 1000 points drawn at random fron N(mean, sd)'''
    return np.random.normal(mean, sd, 1000)
# Pass in the `normal` function and let user change mean and sd.
# Whenever the user interacts with the sliders, the `normal` function
# is called and the returned data are plotted.
nbi.hist(normal, mean=(0, 10), sd=(0.1, 2.0))

# Clicking the Show widget button below loads all widgets on the page.
# Widgets will automatically load for all subsequent pages until you close
# the tab/window.
```



{:.output .output_data_text}
```
interactive(children=(IntSlider(value=5, description='mean', max=10), FloatSlider(value=1.05, description='sd'…
```



{:.output .output_data_text}
```
Figure(axes=[Axis(label='X', scale=LinearScale()), Axis(label='Y', orientation='vertical', scale=LinearScale()…
```

