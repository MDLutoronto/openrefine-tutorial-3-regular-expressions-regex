---
title: "OpenRefine Tutorial 3. Regular Expressions (Regex) Activity"
layout: "home"
description: ""
permalink: "/"  #! Remove this if not the homepage
---

# OpenRefine Tutorial 3. Regular Expressions (Regex) Activity

*This tutorial has been developed for OpenRefine version 3\.7\.5*

You were introduced to GREL in the previous activity, so you know that GREL is a powerful tool for cleaning/editing your data. You can make GREL even more powerful by learning how to use regular expressions (aka regex). A regular expression is a sequence of characters that define a search pattern – it is used to search for matches within text. In OpenRefine, you can use it in your GREL expressions to create sophisticated patterns describing what type of information you want to find within your dataset, then do something with the matching text (edit it, delete it, put it in a new column, etc.).

This activity assumes you have already completed the [Survey of Household Spending](https://mdl.library.utoronto.ca/technology/tutorials/openrefine-tutorial-1) and [Citizen Science](https://mdl.library.utoronto.ca/technology/tutorials/openrefine-tutorial-2) activities, have a familiarity with OpenRefine and know how to create simple GREL expressions. Before you begin, please download the[OpenRefine workshop sample datasets](https://maps.library.utoronto.ca/datapub/workshops/OpenRefineWorkshop.zip), if you have not already.

**In this activity, you are going to:**

[Open regex101\.com and load some sample data](#A)

[Practice some regex basics](#B)

[Use regex in OpenRefine](#C)

Open regex101\.com and load some sample data
--------------------------------------------

1\. Browse to the website **[regex101\.com](https://regex101.com/)**. The **REGULAR EXPRESSION** box at the top of the screen is where you will type your regular expressions. The **TEST STRING** box below that needs to have some text that you want to match. Let’s grab some sample data to use for matching.

2\. Imagine you’re helping a researcher who has collected some data, but it is a bit messy. The researcher had some graduate students go out and collect data that consists of a site name, a date, and an instrument reading. However, they each formatted their file differently.

For example, in Notebook 1, we have tabs separating columns of data, and the dates are written in international standard date format. Mostly this data file looks like it would be fairly decent to deal with.

Notebook 2 on the other hand, has slashes as column separators, and the dates are in a non\-standard format.  
![Side-by-side comparison of the two notebooks. On the left, Notebook 1 in standard ISO format: year-month-day. On the right, Notebook 2 in a non-standard format: month day, year.]({{ '/assets/images/openrefineworkshop_newnotebooks.png' | relative_url }})

3\. Open the file **sample\_data.txt** in a text editing program such as Notepad. This file contains the contents of Notebook 1 and Notebook 2\. Highlight the text and copy/paste it into the test string window in Regex101\. We are now ready to start writing regular expressions!

<img src='{{ '/assets/images/OpenRefine3_3.png' | relative_url }}' alt='A Regex101 window open with data pasted from the file' title='' width='80%' height='722' />

***Note:** the light grey arrows and dots between characters mark whitespace. Depending on your setup, you may or may not see them.*

Practice some regex basics
--------------------------

4\. Look at the regular expression box in Regex101\. Notice that the regular expression starts with a slash and ends with a slash. You type the regular expression between the slashes. The simplest possible regular expression is a single character. Type the letter **b** in the regular expression box.  
<img src='{{ '/assets/images/RegEx_tutorial_B_4.png' | relative_url }}' alt='Type the letter b in the regular expression box' title='' width='924' height='469' />

5\. You can use the boxes at the right\-hand side of the screen to gain information about what has happened. The **EXPLANATION** box will tell you what the regular expression you typed actually does, and the **MATCH INFORMATION** box will show you a list of all your matches.  
![Information about current regular expression and matches]({{ '/assets/images/RegEx_tutorial_B_5_1.png' | relative_url }})

 

<img src='{{ '/assets/images/RegEx_tutorial_B_5a.png' | relative_url }}' alt='Set regular expression options' title='' width='862' height='203' />

![Set global and case insensitive for matches]({{ '/assets/images/RegEx_tutorial_B_5b.png' | relative_url }})

 

1. If it looks like nothing happened when you typed the letter b: check the explanation box – does it say the match was case sensitive? If yes, you need to click on the **gm** icon at the far right end of the regular expression box and choose **insensitive** instead. Now you should see matches.
3. If it looks like the letter b only matched one time, even though there are several instances of the letter b in this text: click on the **m** icon at the far right end of the regular expression box, and choose **global** and **insensitive**as displayed above. Now it should match all cases of the letter b in the text.

The global and insensitive options are referred to as “mode modifiers”. They go after the second slash in the regex syntax.

6\. Let’s start making some more interesting matches. In order to build regular expressions we use combinations of normal strings of text, along with certain characters that have special meaning, known as **metacharacters**. A list of all the metacharacters can be found here: [http://](http://www.rexegg.com/regex-quickstart.html)[www.rexegg.com/regex\-quickstart.html](http://www.rexegg.com/regex-quickstart.html). Some of the common ones that we will be using include backslash, pipe (vertical bar), parentheses, period, asterisk, and question mark.

7\. Let’s start with some matches on the first research assistant’s data, which is easier to work with. (We’ll need a different set of regular expressions to match the second assistant’s data). First, we will find data from the month of May or the month of June. Use the **pipe** (vertical bar) metacharacter to represent OR. In the regular expression box, delete your first expression “b”, and type:

```

05|06
```
Have a look at the results in the Explanation and Match Information boxes.  
<img src='{{ '/assets/images/RegEx_tutorial_B_7.png' | relative_url }}' alt='Use pipe metacharacter to represent OR in regex' title='' width='923' height='238' />

8\. Notice that we also made a match for January 5th which is not a month, so that was not what was intended here. You can take advantage of context to make a more precise match. We know the month has a hyphen on either side of it, whereas the day only has a hyphen at the beginning of it. So to match months only, type:

```

-05-|-06-
```
<img src='{{ '/assets/images/RegEx_tutorial_B_8.png' | relative_url }}' alt='Use hyphens to restrict results to match months only' title='' width='922' height='235' />

 

 

9\. It would be more efficient to write this in parentheses, i.e., type the following instead:

```

-(05|06)-
```
This way you don’t have to repeat the same character more than once. The parentheses are also very handy because they create “groups”, which are then displayed in the Match information box. Have a look there right now – look at the difference between the **Full match** and the **Group 1** matches. Groups allow you to pull out discrete pieces of your expression so that you can do things with them individually.

<img src='{{ '/assets/images/OpenRefine3_9.png' | relative_url }}' alt='Use parentheses to save repetitive work' title='' width='100%' height='864' />

10\. Now let’s try to match entire dates. We can do this by taking advantage of context – we know the format of the dates is 4 digits, a hyphen, 2 digits, another hyphen, and then 2 more digits. We can use the **period** metacharacter as a stand\-in for any single character in any particular position. To try this out, type:

```

....-..-..
```
Even more useful would be to add groups, so change the expression to:

```

(....)-(..)-(..)
```
Now you will have 3 match groups, one for years, one for months, and one for days. We could use these to separate out the individual parts of the date in a tool like OpenRefine, if we wished to.

![Expression to match the entire dates]({{ '/assets/images/RegEx_tutorial_B_10_0.png' | relative_url }})

11\. Now that we have successfully matched the easy dates, let’s move on to research assistant 2’s data. They’re non\-standard so this will be a bit harder. As a starting point, we know that the dates are surrounded by a slash on either side. Remove the expression in the box right now and replace it with a slash character **/** You should see the error message “pattern error”. This is because the slash is used in the syntax for writing expressions (i.e., it appears at the beginning and the end of every expression). If we want to match slashes, we need to “escape” them using the backslash metacharacter – this provides an instruction to treat the slash as a regular character rather than as a special character. Type a backslash followed by a slash: **\\/**. The same rule would apply to any metacharacter you want to actually match – you must first escape it.

12\. Now we want to match all the characters between the two slashes. We don’t know exactly how many characters there are, because the months and days have a variable number of characters in them. We can use the **asterisk \*** metacharacter in combination with our period metacharacter, to match a date with any character, any number of times. Look at the results of typing:

```

\/.*\/
```
Let’s also use grouping to get a match that doesn’t actually include those slashes. Type:

```

\/(.*)\/
```
<img src='{{ '/assets/images/RegEx_tutorial_B_12_0.png' | relative_url }}' alt='Expression to match the entire dates between two slashes' title='' width='371' height='476' />

 

13\. Now let’s break out each part of the date into a separate group, the way we did with the standardized dates. We need to take advantage of the context again, noting the space between the month and the day, and the comma\+space between the day and the year. Look at the results when you type:

```

\/(.*) (.*), (.*)\/
```
<img src='{{ '/assets/images/RegEx_tutorial_B_13.png' | relative_url }}' alt='Break the previous expression into three groups to show month, date, and year' title='' width='359' height='435' />

 

14\. This is mostly working, but not entirely. Notice there are some typos in the data – for example, one row is missing the comma. If we want to account for this, we need to match dates when they have a comma and when they don’t. We can do this using the **question mark** metacharacter, which is used to make the preceding character optional. Type:

```

\/(.*) (.*),? (.*)\/
```
Look at the results.

![Use the question mark to solve the difference of comma between matches]({{ '/assets/images/RegEx_tutorial_B_14_0.png' | relative_url }})

 

15\. Now we’ve caught the date that is missing the comma, but we’ve introduced a new problem – group 2 now includes the commas when they exist. Making the comma optional means it is no longer a viable separator differentiating group 2 from group 3, and only the space is accomplishing this. We need another strategy.

Using the period metacharacter is always a bit error prone, because it matches any character at all and so is not very precise. We could try making our match more specific, for example, only allowing numbers. That will force the commas out. To do this, we can use the **square brackets** metacharacter, which can enclose a range indicating the specific kinds of characters we want to match. This is also known as “character sets”. Type:

```

\/(.*) ([0-9]*),? (.*)\/
```
Look at the results.

![Introduce square brackets to specify the numbers to let comma become a viable separator]({{ '/assets/images/RegEx_tutorial_B_15_1.png' | relative_url }})

 

 

Use regex in OpenRefine
-----------------------

16\. Now let’s put this into practice to do something useful in OpenRefine. If you don’t already have OpenRefine running on your computer, open it now. Create a new project, and choose the file **sample\_data.txt**. Click **Next**.

17\. In the preview view, notice that OpenRefine isn’t able to parse the data into columns because there is no consistent column separator (due to the two notebooks being pasted into one file even though their formatting is different). We will have to use regex to divide it into columns ourselves. Notice also that line\-based text files don’t give you the option to treat the first row as column headers, nor would it make sense to do so right now anyway. Check **Ignore first** (at the bottom of the screen) and type **1 line(s)** so that we don’t get the column names mixed in with our data. Give the project a name and then click **Create Project**.

<img src='{{ '/assets/images/OpenRefine3_17.png' | relative_url }}' alt='Sample txt file opened in OpenRefine' title='' width='100%' height='1305' />

18\. Let’s use the regex we worked on in the last section to pull out the date information and put it into a new Date column. From the **Column 1** pull down menu, select **Edit column \> Add column based on this column**.

<img src='{{ '/assets/images/OpenRefine3_18.png' | relative_url }}' alt='Edit Column menu under column 1 is open. Add column based on this column is selected' title='' width='60%' height='705' />

19\. The GREL expression\-builder window opens. We’re going to use a GREL function called **partition** to break out the data into 3 partitions: 1\. Everything before the dates (i.e., the site name), 2\. The dates, and 3\. Everything after the dates (i.e., the numeric measurements). To start writing a partition statement, type:

```

value.partition()
```
<img src='{{ '/assets/images/OpenRefine3_19.png' | relative_url }}' alt='A partition GREL statement in the expression box. ' title='' width='90%' height='750' />

20\. Inside the parentheses is where GREL expects to find a regular expression. Let’s start by putting the slashes to indicate the beginning and the end of a regex:

```

value.partition(//)
```
You can see that OpenRefine recognizes that this indicates a regex because some stuff appears in the preview column. But, it isn’t what we want yet.  
![Beginning to use Regex in OpenRefine]({{ '/assets/images/openrefineworkshop51.png' | relative_url }})

21\. Next, include the expression we used earlier that matched the international standard formatted dates:

```

value.partition(/....-..-../)
```
Notice that you’ve got some results that seem like they could be useful now. The partition function is able to partition our data into a) everything before our pattern match, b) the pattern match itself, and c) everything after the pattern match.

This output is what is known as an "array". An array is a collection of elements. It is notated by square brackets, with the different elements inside, separated by commas. Each element in the array can be referred to by a number (aka an index): the first element is 0, the second is 1 and so on. In order to create a column based on the dates, you need to tell OpenRefine to use the second element in the array – i.e., the element at index location 1\. Do this by typing:

```

value.partition(/....-..-../)[1]
```
![Basic Regex for one set of dates: /(....)-(..)-(..)/[1] works for the dates in International Standard Format, but not for the others]({{ '/assets/images/openrefineworkshop52a.png' | relative_url }})

 

23\. However, scroll down until you can see some data from the second research assistant, with the less structured date formats. The match isn’t working there. We need to combine the two regular expressions we developed earlier, one for each date format. For now, **remove the \[1]** so we can see all the elements of the match arrays.

24\. We need to make use of OR (remember this is the pipe or vertical bar symbol \|). Start by putting parentheses around the entire existing expression, add the pipe symbol, and then a second set of parentheses with the second expression within them. The result looks pretty complicated, but should be familiar:

```

value.partition(/(....-..-..)|(\/(.*) ([0-9]*),? (.*)\/)/)
```
![Regex: using the pipe character | as an "Or"]({{ '/assets/images/openrefineworkshop53a.png' | relative_url }})

 

25\. Have a look at the results. It mostly works, except the slashes are being included with the date matches for the second expression. This is because the partition GREL function is not able to make use of regex groups the way we were able to in regex101\.com. It only takes the entire match for the expression, so because we have included slashes in the expression, they match.

***Note:** there’s another GREL function, "match", that can use regex groups, but it is much more difficult to work with for this example).*

So, we need to find a way to remove the slashes and have the match still work. There’s a trick we can use here that we didn’t talk about earlier; it’s a special shortcut, **\\w**, that matches only “word” characters. If we replace the period in our first and last parentheses with a **\\w**, then instead of matching any character at all, it will only match letters and number and will exclude slashes. Then we will no longer need to explicitly include the slashes in the expression. Type:

```

value.partition(/(....-..-..)|((\w*) ([0-9]*),? (\w*))/)
```
![Regex: using w as a special character to avoid having slashes as part of the date field]({{ '/assets/images/openrefineworkshop54.png' | relative_url }})

 

26\. The final touch is to add the \[1] back to the end:

```

value.partition(/(....-..-..)|((\w*) ([0-9]*),? (\w*))/)[1]
```
![Regex: the final step]({{ '/assets/images/openrefineworkshop55.png' | relative_url }})

 

27\. Give the column the name **Date** and click **OK**.

28\. The dates are now in their own column. They aren’t all formatted the same way, and also note that OpenRefine is not recognizing them as dates yet (you can tell because they aren’t shown in green). To transform them to dates, from the **Date** column pull down menu, choose **Edit cells \> Common transformations \> To date**.

<img src='{{ '/assets/images/OpenRefine3_28.png' | relative_url }}' alt='OpenRefine after the partition' title='' width='55%' height='476' />

![Drop down on Date, Edit cells, Common transforms, To date]({{ '/assets/images/openrefineworkshop56.png' | relative_url }})

 

29\. For a further challenge, try to perform these steps again to create a column for the site name (array position \[0]) and the numeric measurements (array position \[2]). In each case, you’ll need to do some fiddling with the expression to keep the slashes out of the array element you’re trying to parse at that time.

That’s it for our Regular Expressions activity! You’re now able to perform matching operations on your data using GREL and regular expressions.

[**OpenRefine Tutorial 4: 311 Calls Activity**](https://mdl.library.utoronto.ca/technology/tutorials/openrefine-tutorial-4-311-calls-activity)

Technique: [Cleaning data](/technique/cleaning-data) \| Tools: [OpenRefine](/tools/openrefine)**Date Created:** 2019\-04\-01**Updated:** 2023\-10\-23
