---
layout: page
title: "Carpentry@UiO R and Tidyverse (Novices), Day 2 Q&A"
---

## Summarising data with {dplyr}

[Lesson materials](https://athanasiamo.github.io/swc.tidyverse/materials/005_dplyr_summaries.html)

### Challenge 1
1a: First start by trying to summarise a single column, body_mass_g, by calculating its mean in kilograms.

1b: Add a column with the standard deviation of body_mass_g on kilogram scale.

1c: Now add the same two metrics for flipper_length_mm on centimeter scale and give the columns clear names. Why could the drop_na() step give us wrong results?

**Questions**

- Does `drop_na()` remove NA only for the column specified or the whole row containing NA in the specified column?
    - it will remove the entire row. So use carefully and when you know you are not interested in the remaining data if that value is missing.
    - this is also the reason we show you the use of `na.rm = TRUE`, which will only remove NAs for that specific summary function and column.

- When I wrote the script, and selected a certain phrase (such as "na.rm = TRUE"), the same phrase in all the lines within the script became highlighted (or boxed). Is this possible to remove all "na/rm = TRUE" from all the lines in the script in this way?
    - the most efficient way of doing this, is using a search and replace in RStudio. You can toggle this functionality by the keystroke `ctrl/cmd + f` on the keyboard. This will open a small bar on the top on your script with a place to write the text you want to search for, and another where you can add the text you want to replace it with. To delete the text, leave the second replacement text as empty. And press "replace" for it to happen. I recommend running through each replacement individually rather than doing "replace all" if your script is long, as you might end up deleting something you didnt mean to.


- Can you drop_na from  the two  variables at the same time?
    - Yes, but beware! That will remove the entire rows where there are NA is  _any_ of the columns.There are definitely reasons you might want that, but its good to be sure. For instance, some analyses do not deal will with missing values, so you will need to remove all missing for them to work. Then drop_na is great!

- Sorry, you may have said this already, but is there a reason you now Write summarise With a 'z'?
    - There is no difference between `summarize` and `summarise`
    - This is likely happening because I (Mo) made the materials and use British spelling, while Raoul uses American spelling when he works :D

- I might have missed it, but what is the difference between data frame and tibble?
    - a tibble is a special case of a data.frame. So it works just like a data.frame with small differences. Everything you can do to a data.frame you can do to a tibble, but a tibble can do a little more (like being grouped).

- Can we plot the summaries in histograms?
    - Mo will cover that in the next session

- Can you please repeat why in the records =n() inside n nothig was specified?
    - n() gives the current group size
    - So n() is a very special function, that does not need a specific input
    - Raoul first grouped by species and island, and with n() you will give you the sample size for each species e.g. Adelie by Island e.g. Biscoe N = 44
    - It doesn’t not require input, because it takes the number of rows in the group it has. Running on an ungrouped dataset, will return the number of rows

- Can we use nrows() or other functions instead of n()
    - Raoul mentioned using `length(column_name)`, but `nrow(.)` will not respect grouped data.


- Can you use across() in summarise()?
     - Yes! The days became too full so we omitted that, but it is absolutely possible, and some of us do it a lot! I'll give an example code, for those who want to try!
```
penguins %>% 
    group_by(species) %>% 
    summarise(across(.cols = ends_with("mm"),
                     .fns = list("Mean" = mean,
                                 "SD"   = sd,
                                 "Min"  = min,
                                 "Max"  = max),
                     na.rm = TRUE))
```

- in this example we summarise everything ending with "mm", and get the full descriptive statistics, and give the columns a suffix based on the function list. so all columns with means are suffixed with "Mean", all sd's suffixed with "SD" etc. and I have applied `na.rm = TRUE` to all the across functions. NOTE! This will only work if all the functions have this `na.rm` option. If you add n() in there, it will fail, because it does not have that option!

## Combining everything we have learned

[Lesson materials](https://athanasiamo.github.io/swc.tidyverse/materials/006_combining.html)

**Questions**

- why not just use summary(penguins) for all the mean values?
    - `summary()` is absolutely a function to use for a quick idea of what your dataset contains.

### Challenge 1
1a: In our code making the summary table. Add another summary column for the number of records, giving it the name n. _hint: try the n() function.

1b: Try grouping by more variables, like species and island, is the output what you would expect it to be?

1c: Create another summary table, with the same descriptive statistics (mean, sd ,min,max and n), but for all numerical variables. Grouped only by the variable names.



**Questions**
- Some people use OneDrive to store their data, R and the palmerpenguins and have trouble loading the data from there. Is there a solution for this? (Sorry, this is a bit unspecific.)
    - if it were me, I'd likely make my project inside onedrive and work from there. That gives you also a type of version control on your scripts. (Mo)

- How do you change the apperance of R Studio and colors in the code?
    - You go to Tools -> Global Options -> Apperance, and choose the theme from the Editor theme list (Mo is using "Tomorrow" theme for teaching)

- Does n() function need to be clean of NAs too?
  - the `n()` function is content-agnostic, which means it does *not* qualitatively evaluate its input, but only returns the number of observations (*i.e.*, rows) in the (grouped) data set. As such it will also count all `NA`s. 



### Challenge 2
 2a: Create a bar chart based om the penguins summary data, where the standard deviations are on the x axis and species are on the y axis. Make sure to dodge the bar for easier comparisons. Create subplots on the different metrics (Hint: use facet_wrap(). 
 
 2b: Change it so that species is both on the x-axis and the fill for the bar chart. What argument do you need to add to facet_wrap() to make the y-axis scale vary freely between the subplots? Why is this plot misleading? 
 
 **Questions**

- Can you set the binning of the bars?
    - when you use geom_histogram you can set the bins through the `bins` argument, or through the `binwdith` argument. When using your owns statistics (like stat = "identity"), then binning is already done, so there is no such option.

- Sometimes you use the aes inside ggplot and sometimes inside the plot you close)
    - if you use aes inside ggplot, like this: ggplot(aes(x = )), you are setting up the aesthetics for the whole ggplot; if you use aes inside the specific ggplot layer, e.g. ggplot () + geom_point(aes(x = )), you are setting up the aesthetics only for that layer

- `facet_wrap(~name + stat)` works as same as (stat ~ name)? 
    - nice! so `facet_wrap()` will only take something we call a one-sided formula, meaning `~` with something on its right. Which is why we do `facet_wrap(~ name + species ). for `facet_grid()` its a little different. it takes a two-sided formula, meaning something on the left AND right. And you can read it like `facet_grid(row_column ~ column_column)`. Meaning it will make a grid structure , while `facet_wrap` wraps around in a new line after 4 plots. Its hard to explain in words! But if you run facet_grid(name ~ stat), you will see that the subplots that are like columns come from the name column, and the rows come from the stat column. 

- I need to know what Mo's break program is!
     - https://hovancik.net/stretchly/
     - I forgot to pause them, which I usually do when I teach! But its a great program to remind you to take some breaks during your day, and for me has been very important in. getting a more healthy working day

### R User Groups in Oslo

- Oslo useR! Group: https://www.meetup.com/Oslo-useR-Group/
- R-Ladies Oslo: https://www.meetup.com/rladies-oslo/


### Learning more

The [tidyverse webpage](https://www.tidyverse.org/) offers lots of resources on learning the tidyverse way of working, and information about what great things you can do with this collection of packages. 
There is an [R for Datascience](https://www.rfordatasci.com/) learning community that is an excellent and welcoming community of other learners navigating the tidyverse. We wholeheartedly recommend joining this community!
The [Rstudio community](https://community.rstudio.com/) is also a great place to ask questions or look for solutions for questions you may have, and so is [stackoverflow](https://stackoverflow.com/).

## Feedback
*Please give us positive and constructive feedback*

Thanks for an amazing course! Its been super good organised, the pace has been working very well for me, and the level of advance. Learned so much! 

Thank you for the great course, it was very useful!

Thank you Universitetsbiblioteket!

Thanks alot guys! this was amazing! Although at the very start it was not that much interesting, day nr2 and 3 was perfect, so many shortcuts and very interesting commands :) hope to see some more workshops soon :+1: 