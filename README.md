# 12 things to do with R

R is a programming language widely used in data journalism, typically for data analysis and sometimes for visualisation. It isn't the only programming language: Python or JavaScript can both perform similar tasks, but each has their strengths and weaknesses. And if you already know one or both of those you should be able to understand R relatively quickly.

This post explains some key processes to get started with R.

## Install R Console or R Studio

Most people use R within software like R Studio or R Console. Go and install that first.

## Create a new project

One of the advantages of R is that it works with 'projects' that save the history of all your actions. This means not only can you trace back what you have done, but you can share that history with others. So if an editor, colleague or reader asks 'How did you do that?', you can show them.

Before you start a new project create a folder for it to sit in. This might be within a broader project folder where you're storing other material related to your story, it might be within a folder where you do all your R work. Or you might be one of those people who puts everything on their desktop and then wonders why it takes so long for their computer to boot up (*ahem.*).

Then, to create a new project in R Studio click on **File > New project...**, click **Existing Directory** on the window that appears, then browse to the folder you have created for your project and select it.

Once you've created a new project R will create a new file in that folder ending with the extension *.Rproj*. This is where all the history is stored, and something you can share with others or store as a backup.

## Import and store data

Like most programming languages, R allows you to import and store data in 'objects' called **variables**. 

The easiest way to import data is to put it in the same folder as your project. This way you don't have to describe a path to data somewhere else.

If you've created a project in R Studio, and got it open, in the *console* on the left you should see a `>` and a flashing cursor. 

To import data into a variable, type something like this:

`yourvariable <- read.csv('yourdata.csv')`

The bit that says `'yourdata.csv'` should obviously be the name of your file: note that it should be placed in inverted commas. In this example I'm using a spreadsheet in CSV format. (More on other formats below.)

The bit that says `yourvariable` is just a name that you choose. It can be almost anything, but choose something meaningful.

The bit that says `read.csv` is a **function**. Functions are basically recipes: the `read.csv` function is a recipe for reading CSV files. If you wanted to read other formats you can use other functions but I'll come onto that below. 

The `<-` bit is taking the result of reading that CSV and putting it into your variable. You can actually just use `=` if you're used to that from other languages, but `<-` has a certain appeal.

Once you've got it into a variable you can do all sorts of other things with it, as we'll see...

## Generate summary statistics

R was originally designed as a language for statistical analysis, and one of its most basic functions allows you to get a quick overview of a dataset's statistical qualities, such as averages, medians, modes, and the largest and smallest values.

That function is `summary()`

To summarise the data in the variable you just created just put it in parentheses like so:

`summary(yourvariable)`

When you press enter you should see a summary for each column in the data that you imported. For columns containing numerical data you will see:

* The minimum value
* The first quartile
* The median
* The mean
* The third quartile
* The maximum value
* How many N/A values

For columns containing text data you will see the six most common text entries with the number of times each one appears.

If you just want to know what's biggest, smallest, average, most common, most lacking data etc, this is a very quick way to do so.

## Join data

Quite often you need to join two datasets that have a column in common. 

For example, imagine one dataset has crimes in each area, and another has the population in each area. You need to combine the two datasets in order to calculate a crimes per person. The two datasets have a column in common: the name of the area.

To join these you first need to import each of them (as described earlier) into a variable in R. Let's say you call one *crimes* and the other *populations*. 

You can now merge those using the `merge` function like so:

`merge(crimes, populations)`

But this will only *display* what you get when you merge those two datasets. So instead you need to store the results at the same time, like so:

`newcombineddata <- merge(crimes, populations)`

*Note: the matches must be exact, so if an area is named slightly different in one dataset (for example it uses 'and' where the other data uses '&') then those rows won't merge properly*.


## Import different types of data

If you need to deal with data in formats other than CSV, R should be able to import it. 

Data from the statistical software SPSS, for example, can be imported using `read.SPSS` like so:

mylovelydata <- read.SPSS('somefile.spss')

The `read.table` function can also deal with a wide range of formats. This function, however, needs more information: specifically whether your data file has a header row, and what 'delimiter' is used.

A 'delimiter' is what is used to separate each column of data. In a CSV file, for example, a comma is used to separate each column like so: *column1,column2,column3*. CSV stands for Comma Separated Values.

A `read.table` function then might look like this:

`newdata <- ("mylovelydata.csv", "TRUE", ",")`

In those brackets are 3 ingredients: the name of the file, then after a comma, either `"TRUE"` (if it has headers) or `"FALSE"` (if it does not), then after another comma the delimiter in quotation marks: `","` means that the delimited is a comma. 

Other useful commands include `read.delim` (to import delimited data formats) and `read.fwf` (to import fixed width formats)

## Create a subset of your data

You can access individual columns in your data by using the `$` sign between the variable name and the column name like so:

`mydata$mycolumn`

That can be saved in a variable in the same way too:

`justonecolumn <- mydata$address`

The `subset` function can also be used to create a filtered version of your data based on one or more criteria. Here's an example:

`2014crimes <- subset(crimes, crimes$year == 2014)`

The `subset` function needs 2 ingredients: the name of the variable with your data, and after a comma, the criteria. 

Normally the criteria needs to specify a column within that data using the dollar sign. So `crimes$year == 2014` means the year column in the crimes dataset, and `==` means 'is the same as' whatever comes after it (2014 in this case). Note that we use two equals signs in programming to say 'is equal to'; one equals sign on its own is normally used to create a variable.

That double equals sign is called an **operator**. Other operators include:

* `>` - greater than
* `<` - less than
* `!=` - *not* equal to

If you want to write a criteria that specifies a text value rather than a numerical value then you need to use quotation marks like so:

`burglaries <- subset(crimes, crimes$category == "Burglary")`

Also, with text the match must be perfect not partial: even a space before or after 'Burglary' in the dataset would result in a non-match; and a lower case 'b' would also not match.

## Run queries and filters

Beyond the `summary` function mentioned above there are a whole host of functions to ask specific questions of specific parts of your data. These are very similar to spreadsheet functions like `=SUM`, `=MEDIAN`, `=MAX` and so on. Here are some examples:

`sum(mydata$mycolumn)` adds up all numbers in the specified column

`median(mydata$mycolumn)` calculates a median (the middle value)

`mean(mydata$mycolumn)` calculates a mean average

`max(mydata$mycolumn)` tells you the biggest number in that column

`min(mydata$mycolumn)` tells you the smallest

`range(mydata$mycolumn)` tells you the range of numbers (the biggest and smallest)

`sd(mydata$mycolumn)` tells you the standard deviation

`length(mydata$mycolumn)` tells you how many entries are in the field

## Create a chart

There are various ways to draw charts in R. Here's just one:

`barplot(height = dataset$numbercolumn)`

The `barplot` function will draw a bar chart, of sorts at least. It can take all sorts of ingredients but the key one is the height of each bar. This is done with `height =` followed by a data table variable and a column within that which is numeric.

As in previous examples, `dataset` above needs to be changed to the name of a specific variable you have created which holds some data. Likewise `numbercolumn` needs to be changed to the name of the column within that dataset which uses numbers.

If you have a lot of numbers the result will be very crowded, but fewer numbers will get you a clearer result.

Adding more ingredients allows you to control more about a chart. This example adds a label to the x axis:

`barplot(height = donations$amount, xlab = 'Amount')`

You can also add a y axis label, a legend, title, and limits to each axis, among other things. To find out how you need to read the **documentation** for the particular function. 

The image is normally shown in the bottom right corner of RStudio, in the tab 'plots'. If you want to save an image you can click 'Export' above the image to save it as an image or PDF.

## Export data

If you want to export the results of your work as a new CSV file, you can do so with the `write.csv` function like so:

`write.csv(yourvariablename, file='newdata.csv')`

This function has 2 ingredients: the name of the variable containing the data you want to export, and the name you want to give to the file you're exporting it as. That name comes in inverted commas after `file=`, and it needs to include the file extension. So you could say `file='newtextfile.txt'` if you wanted to save it as a text file, and so on.

Similar functions:

dump() and dget() include metadata in txt file
