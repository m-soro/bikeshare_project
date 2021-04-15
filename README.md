# Introduction

This project took me about 4 days! I was working on it 5-6 hours a day or maybe even more(if you count little spurts of ideas during the day?). The only prep I did for this project was watching and re-watching the demo project more than once just to get the required specs and rubric expectations. 

I completely avoided looking at how others have done this project. Because I wanted this to be my work 100%! and I wanted to **challenge my:**

* **Logical** thinking
* **Research** and **problem-solving** skills
* **Creativity** to come up with other features to add. 

I found the preceeding set of quizzes helpful, because oftentimes its difficult in even where to begin!

## Project Overview

>Overview:
In this project, you will make use of Python to explore data related to bike share systems for three major cities in the United States—Chicago, New York City, and Washington. You will write code to import the data and answer interesting questions about it by computing descriptive statistics. You will also write a script that takes in raw input to create an interactive experience in the terminal to present these statistics.

## Project Expectations:

* **It has to be interactive** - asks for user's input
* **Does not crash** - handles errors
* **Avoids code duplications** - uses functions. *This is where I spent most of my time.* 
* **Use of correct data types**

I looked at the template provided to see the framework and questions provided.

I used all of the questions, but altered the control flow because I had different ideas for the project. 

## Details about my project

**Input Validation Functions** 

* Because this is interactive, I think it makes more sense to have functions that just validates answers. Like `y or n ?`, `input integer?` , `select city?`,  `select month?`, `select day?` and most of the time it really boils down to `y or n ?` question! Thanks to boolean logic!. I found it a lot easier to use these functions in conjuction with the `input` function. Oftentimes it easy to just execute this piece of program and see what and where errors occur.

**Functions in general**

* `lambdas` are like little magic tricks ! Multiple times I thought I was done re-factoring. Since my project has different features than the one shown in the demo. I needed to write a custom function for it. I could script it as pieces of code but I found it really hard to encapsulate them in a function. I did't know that `lambda` has to have a return value?! I had some confusion with the return value of functions, arguments and the scope of variables. 

```python

# Some lambda helper functions
get_value = lambda x,y,n :x.groupby([y])[y].count().sort_values(ascending=False).head(n).values
get_index = lambda x,y,n :x.groupby([y])[y].count().sort_values(ascending=False).head(n).index
str_cleaner = lambda x : x[8:x.find("']")]    

# Combine index which is-> pandas.series.index and value which is-> numpy.ndarray then print in friendly way
def iterate_idx_val(x,y,n, message):
    idx = get_index(x,y,n)
    val = get_value(x,y,n)
    idx_val = zip(idx, val)
    for data in idx_val:
       print_p(message.format(*data))

```
>Some of the custom functions that are doing the heavy lifting. There maybe a better way to this as with all implementations. 

```python
get_value = lambda x,y,n :x.groupby([y])[y].count().sort_values(ascending=False).head(n).values
```
* `get_value()` and `get_index()` are similar functions except they both return values and index separately. It uses count and sorts the values in ascending order, I used `head(n)` the variable `n` specifies how many rows to return since I wanted users an option to view up top 5 of the results.   
* `x` = is the DataFrame
* `y` = is the Column Name
* `n` = is how many rows to return

```python
def iterate_idx_val(x,y,n, message):
    idx = get_index(x,y,n)
    val = get_value(x,y,n)
    idx_val = zip(idx, val)
    for data in idx_val:
       print_p(message.format(*data))
```
* takes 4 arguments
* `x` = is the DataFrame
* `y` = is the Column Name
* `n` = is how many rows to return
* `message` = is how I wanted the tuples returned as a message.


```python
str_cleaner = lambda x : x[8:x.find("']")] 
``` 
* Is used to print the returned tuple. Where `x` is the string. It starts at index 8 then slices after finding `']`.

***I spent a lot of time in the following:*** 

**Refactoring codes**

* I looked up how to create lambdas and python's built in functions such as `zip`, `enumerate` and `comprehensions`. Most I did not end up using but, its a good refresher. I spent so many hours refactoring all the above codes except the string cleaner!

**Formatting and printing returned series and arrays** 

* The string manipulation part and sub-setting the selected DataFrame took a while too. 

**Capturing the returned value from a function**

* In `filter_data()` function instead of returning just the filtered DataFrame. I wanted to to capture that answers based on the input. So I returned it as a tuple of values and unpacked this tuple in the main `project()` functions 

**Error handling**

* Some of my functions have `try` and `except` clause but some does not. Some are done with a `while loop` and they both work. My common mistake about this is forgetting the input solicitation inside the `try` block.

**Print 5 consecutively**

* I found this task tough, I was so close in looking how others have done it! I tried experimenting with `enumerate` so that every rows have an index that I can call from my loop. I tried `generator` function since it returns a stream of data and remembers where it left off so I was thinking I can have a start and stop triggers in the loop where the generator's stream cuts off every 5 lines but, I could not get this to work! Finally I decided I'll implement this in a `while loop`. It took a couple of tries but it worked!

```python
def prompt_user_for_data(df):
    print_p('\n\n***Printing FIVE ROWS consecutively***\n\n')
    print_p("I have " + str(len(df)) + " total rows available to view. ")
    start = 0
    fiverr = 5
    while fiverr < len(df)+10:
        answer = y_or_n('\n\nView every 5 rows of --> ')
        if answer == 'y':
            pd.set_option('display.max_rows', None)
            pd.set_option('display.max_columns', None)
            pd.set_option('display.width', 800)
            pd.set_option('display.max_colwidth', None)
            print()
            print_p(df[start:fiverr])
            fiverr = fiverr + 5
            start = fiverr - 5
        elif answer == 'n':
            break  
```

I can control which sub-set of rows in the DataFrame are retuned by `df[start:stop]` but, I needed a way for `start to DEcrement by 5` and the `stop to INcrement by 5`, since the project calls to print every 5 lines. 

And, to do all of this up to the end of the DataFrame, so I use `len(df)` and added `+10` for good measure, since not all selected DataFrames can be printed or divided by chunks of 5. When the loop reaches `len(df)+10` it will just return an `Empty DataFrame` and the loop stops! 

I did not want a truncated version so I set all of the columns and rows to be visible. 

There maybe a pandas method for doing this easily??!

## Reference materials: 
 
1. Pandas official documentations 
2. Sites like realpython, kite, and stackOverflow. 
3. Udacity's python lessons! 

## Additional Features

* Informs the user what filters are selected and how many rows of data are being aggregated on.
* Applies time filter based on the answers if month and day are selected.
* An option to view top 5 result for some of the aggregations.
* Two ways to view the data:
    * By every 5 rows 
    * By specified amount of rows to return.
* Asks user if they want to see the data again.     


>Looking ahead: Maybe make this a Flask app!

# Conclusion

This has been a great project! It could be really tough to someone who does not have a lot of python exposure and I can't imagine working with messy data! Numpy and Pandas are super powerful and Python is just a fun language to write scripts and build things! I learned a lot by completing this project!












