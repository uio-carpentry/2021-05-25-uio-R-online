---
layout: page
title: "Carpentry@UiO R and Tidyverse (Novices), Day 1 Q&A"
---

## Visualisation with {ggplot2}
[Lesson materials](https://athanasiamo.github.io/swc.tidyverse/materials/001-ggplot2.html)

### Challenge 1
1a: How has bill length changed over time? What do you observe?
1b: Try a different geom_ function called `geom_jitter`. It will spread the points apart a little bit using random noise.

### Challenge 2:
2a: What will happen if you switch colour to also be by year? Is the graph still useful? Why or why not?
2b: What if you map colour aesthetic to species? What has changed? How is year different from species or island? What is the limitation of the colour aesthetic, when used to visualize different types of data?


### Challenge 4:
4a: Change the transparency (alpha) of the data points by year. _Hint: alpha takes a value from 0 (transparent) to 1 (solid).
4b: Move the transparency outside the aes() and set it to 0.7. What can we benefit of each one of these methods?


**Question**
- Using ‘geom_point’ with a continuous variable  on  the x-axis would give the same plot as using ‘geom_jitter’, right?
    - geom_jitter It adds a small amount of random variation to the location of each point so if the two are continuous variable they will look the same  . geom_jitter is a useful way of handling overplotting

- This is room 5. Can we control how wide the jitter can be, can we set a value?
    - ggplot(data = penguins) + geom_jitter(mapping = aes(x = year,
                           y = bill_length_mm),
              **width = 0.2**)
    - if in doubt about a functionality, you can always check the function's help page. In this case, `?geom_jitter` will take you to the help page. There you will find information for the two parameters `width` and `height` (and a lot of other parameters). 

- Is it possible to display two distinct plots next to each other in two distinct panes/panels for better comparison (without using extra functions like 'grid.arrange()')?
    - For arranging individual ggplots, I (Raoul) would recommend using the `wrap_plots()` functionlity of the {patchwork} package. This package is developed in very close contact with the {ggplot2} developers and currently provides the best functionality to arrange ggplots. Edit: {patchwork} is *not* part of the {tidyverse} package collection and needs to be installed seperately.
    - if you want to make subplots based on the same general plot, you can use the `facet_wrap()` function based on variables in your dataset. 
      ggplot(data = penguins) + 
         geom_jitter(mapping = aes(x = year,
                                   y = bill_length_mm)) + 
         facet_wrap(~species)
         
- Hi another question from room 5: 
  For highlighting the data points from only one year (e.g., 2008) and making the data from otheryears transparent, can we modify the following code somehow? ggplot(data=penguins) 
  +
  ggplot(data=penguins) +
  geom_jitter(mapping =aes(x=bill_depth_mm, 
                           y=bill_length_mm,
                           colour=species,
                           alpha=year))
   - excellent question! We can, but it is a little advanced. Now the jitter aes includes a bit of code similar to what Raoul will be covering today.
    ggplot(data=penguins) +
      geom_jitter(mapping = aes(x = bill_depth_mm, 
                                y = bill_length_mm,
                                colour = species,
                                alpha = if_else(year == 2008, "2008", "Other"))
      )

                         
## Data subsetting with dplyr
[lesson material](https://athanasiamo.github.io/swc.tidyverse/materials/002_dplyr_subsetting.html)


### Challenge 1
1a: Select from the penguins data set only columns that are factors.
1b: Now we lost flipper length! To make sure we keep flipper length, instead select columns what end with “mm”.
1c: Now select the columns island, species, and all numeric columns


**Questions**
- Hi! Question about is.numeric function: can we also add an argumet to this function, for example to choose only numeric columns from the first five columns? 
 - Good question! no, this is not part of its functionality. You notice how the function inside the where `is.numeric` does not have parentheses after it, because it cannot take more arguments. if you want to do something like that, you will need to create your own specialised function that can do that. But that is quite advanced!

- Does this do the trick? `select(penguins, 1:4 & where(is.numeric))`
  - That is some trick! cool! I didnt know! (Mo)
  - Me neither! :D Trial & error


### Challenge 2
2a: Using a comma (‘,’), each expression must be TRUE for the end result. Choose all data where flipper length is larger or equal to 180, and the species is “Gentoo”
2b: Do the same using the & (and) sign.
2c: Filter the penguins data so that you have either chinstrap penguins, or penguins with body mass below or equal to 3 kilos.


**Questions**
- Hi, is it possible to remove rows with "NA"?
    - A: Yes, it is. I believe Raoul will cover that :)
        - Thanks

### Challenge 3
3a: Arrange the penguins data by body mass.
3b: Arrange the penguins data by descending order of flipper length.
3c: You can arrange on multiple columns. Try arranging the data by ascending island and descending flipper length, using a comma between the two arguments.

**Questions**
- How can we see all the data but not only the first 10 observations?
    - in a tibble (the type of data.frame used here) its a little tricky. But the best hack to see everything is to ask for it to look like a standard data.frame by `penguins %>% as.data.frame()` Please note that this will flood yout console with a lot of information. You can also click on the object name in the RStudio environment, which will open it in a spreadsheet view. (Mo)
    - from Raoul! `penguins %>% print(n = Inf)`

- In this script you have defined 3 objects with the name `chinstraps`. Will this be confusing for R?
    - Every time new `chinstraps` are defined (i.e. with `<-` and new conditions), the `chinstraps` is overwritten.
- If I forget which package a function belongs to, is there a way to ask in R which package function xyz belongs to? 
    - The best way to find something like that out is to search the help. Either search for the function in the search bar under the "packages" pane, or type `?xyz` in the console. This opens a functions help. If it cannot find `xyz` it might be because it is not loaded. Typing `??xyz` will search in also unloaded packages. This list can be LONG. But at the top of each help, next to the functions' name there is the name of the package in curly braces `{}`