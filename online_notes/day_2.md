---
layout: page
title: "Carpentry@UiO R and Tidyverse (Novices), Day 2 Q&A"
---

## Adding and altering variables with {dplyr}

[Lesson materials](https://athanasiamo.github.io/swc.tidyverse/materials/003_dplyr_mutating.html) 

**Questions**

- Can you repeat what scale does?
    - `scale()` transforms any numeric variable so that is gets an average of 0 and standard deviation of 1. This is a common transformation to do before statistical analyses, to help statistical models work more efficiently. 
    - to learn more about any function, try running `?function_name` in the Console, and the Help file for the function will open up. Here's a little more information on how to look for help in R:
   [help in R](https://www.r-bloggers.com/2018/08/5-ways-to-get-help-in-r/).

- Can we reorder the columns, so that we see the new ones “closer” to the beginning of the dataset?
    - yes! The `select()` function can also be used to reorder columns! Combining selects of specific columns, and the select helpers can make re-arranging your columns very convenient.
    - `penguins %>% select(year, body_mass_g, size, everything())` . When you want to keep all the columns, but just want some specific columns at the start, the helper functions `everything()` will add all other columns after your other selections.

- How to type ( ~ ):
    - Win (Norwegian keyboard): "alt gr" + ^
    - Mac (Norwegian keyboard): option + ^ 


### Challenge 1

1a: Create a column named bill_ld_ratio_log that is the natural logarithm (using the log() function) of bill_length_mm divided by bill_depth_mm

1b: Create a new column called body_type, where animals below 3 kg are small, animals between 3 and 4.5 kg are normal, and animals larger than 4.5 kg are large. In the same command, create a new column named biscoe and its content should be TRUE if the island is Biscoe and FALSE for everything else.

1c: Transform all the columns with millimeters measurements so they are scaled, and add the prefix `sc_` to the columns names.

**Questions**

- (copied from chat) wrote this instead of «Biscoe» : `biscoe = if_else(condition = island == 1` Does that give the same answer?
    - In case of this `penguins` dataset, the `island` variable is factorial with 3 levels (you can see in "Environment" tab) and "Biscoe" is 1, so it works. 
- Regarding the 1b, the `case_when`'s "TRUE" looks like almost "FALSE" as they represent those which did not fall under the previous conditions. And "NA" also went to "Large" in our room. Did we do anything wrong? Or is there any way to avoid to put NA observation not to put to "Large"?
    - Yes, this `TRUE ~ "something"` in `case_when` is a little counter intuitive in the beginning, but it means "everything not mentioned above should get this 'something' value". So that goes for everything, including `NA`! 
    - what you can do instead of using  this last line, you could use `!is.na(bill_ratio) ~ "stumped")` , which says "everything that is not `NA` (!is.na) in the bill_ratio after the first two conditions should be large"
    - its covered in the materials, but we had to skip because we are running over time. https://athanasiamo.github.io/swc.tidyverse/materials/003_dplyr_mutating.html#Adding_new_variables,_part_three
- Row 4: Body mass is NA, but boy_type is large when we follow the code shown. How do we get code this right?
    - (this question is answered above to another similar question :) )
- (copied from chat) we create all these variables and we see them on the tibble, but they are not included on the “penguins” dataset. What if we want to create new variables and  include them in the dataset, so that we can continue working with them later on?
    - great question! That is true, most of this course we are working on "temporary" objects, i.e. we are not saving them in the environment. If you want to save the result of your pipeline, you can do that by going to the very top, your first call to `penguins` . 
    - `penguins_more <- penguins %>% mutate(variables = 1)`
    - This will create a new object in your environment called `penguis_more`, you can give it any name you like

## Reading in data and pivoting / reshaping with {tidyr}
[Lesson materials](https://athanasiamo.github.io/swc.tidyverse/materials/004_tidyr_pivoting.html) 

**Questions**
- (copied from chat) How about defining the wording directory?
    - For reference: https://martinctc.github.io/blog/rstudio-projects-and-working-directories-a-beginner%27s-guide/
    - another ref: https://www.tidyverse.org/blog/2017/12/workflow-vs-script/

### Challenge 1
1a: Read in the penguins data set from file.
1c: Read in the penguins data set from file and assign it to the object penguins

### Challenge 2
2a: Pivot longer all columns containing an underscore (_) in their name.
2b: Pivot the penguins data so that all the bill measurements (starts with “bill”) are in the same column.
2c: As mentioned, pivot_longer accepts tidy-selectors. Pivot longer all numerical columns.

**Questions**
- We tried `penguins %>%
  pivot_longer(select(where(is.numeric)))`. Why does it not work with select in the Pivot function? Is "select" in there implicitly without specifying?
  - `pivot_longer()` itself already selects columns based on the `cols` parameter. You can use `where()` directly without having to wrap it into a `select()`, *e.g.*, `penguins %>% pivot_longer(cols = where(is.numeric))`. 

### Challenge 5 
5a: Turn the penguins_long dataset back to its original state

**Questions**
- Question on the beginning of the day: What does the `height = 0` parameter in the `geom_jitter` do again?
    - the height controls the amount of jitter along the y-axis, while the width controls the amount of jitter (random noise) along the x-axis

- One thing that was unclear for some people was whether they were also supposed to remove the ID column since it was supposed to go to the original penguins format
    - I did not think of that, but that is an excellent question. ideally, yes, I think I'll add that to the solution also (and make it clearer in the question). Because technically, the original dataset does not have it.

- What is the difference between read.csv and read_csv?
    - read.csv comes with R (not in tidyverse) and works just fine. there are some small differences in how the data is read in, and the default options in read_csv are very carefully picked and works well. read_csv is also faster, if your dataset is large.

- Very basic question, but, I'm using a mac, and when I specify the file path, it doesn't seem to work with "~". Like, read_csv("/Library/Frameworks/... works, but read_csv("~/Library/Frameworks/... doesn't. What am I doing wrong?
    - another good question. So this was poorly explained by me (Mo), but the `~` is a pointer to your home folder, which for a Mac usually is "/Users/username". So the `~` is a shorthand for writing that path. In the example I showed, my Downloads folder is in my home folder. So typing "~/Downloads"  is equivalent to typing `/Users/username/Downloads` . The penguins csv file is _not_ stored in your home folder, but in an entirely different place, namely in your Library. So adding `~` will point to the wrong place, namely `/Users/username/Library/...` which does not exist. 
    - I'm sorry for the poor explanation, I was thinking as I typed, which can go wrong like this!
        - Thanks! That makes sense.

- Thanks. Let's say one of the columns contains unstructured text like observation, such as "this sample was spotted near the river". And then, you later think that closeness to the river may be important for your analysis. Is there any way to make a new column from unstructured text, based on the occurrence of the word "river?"
    - oh this is a good one. Yes there absolutely is! String handling like this can be quite complex depending on what you want to do, but it is absolutely something many of us do a lot. Lets say this freetext data is in a column named "comments". You can use a function used `grepl` to search a column for specific strings: 
    ```
    penguins %>% 
      mutate(place = grepl("river", comments))
    ```
    - This will make a column named "place" which is `TRUE` if the word "river" is in the comments column, and `FALSE` if not.



## Feedback
*Please include something positive and if applicable something constructive :heart: (not negative :wink: )*

- Im super happy with the course, learning so much! If there is one thing I would change, it would be that the challenges could not be the same as what you guys just went through. Because then I get lazy and dont re-type it. Rather just say "Ah, thats what they already did". So perhaps just force us to modify it a bit. But other than that everythiung is so helpful! 

- Thank you for the great course. In my opinion, maybe having longer breakout sessions would give us more time to figure out the exercises on our own.

- Great course! Very clear and informative and nice pace for us beginners:)

- Brilliant course - the pace is just right and you are pedagogy rock stars! Many thanks!

- Thanks for two great days of coding. A very well-designed and well-paced course for me at least.

- I really appreciate the course, I learn so much and it will be so useful for my future work! Great instructors, easy to follow along. Thank you! <3

- Great course! Very inspiring and informative. Thank you all.