---
layout: post
title: 'Preparing a Text Document for a Spreadsheet Using Regexes'
date: 2015-07-24 09:30:29
categories: regex 
comments: true
---
One of the most frequently needed tasks is to prepare a text file to be used in a spreadsheet. This task can be one of the most tedious without using regexes. This post is a demonstration of how the process might occur. If you would like to learn more about regexes, I highly recommend the book ["The Bastards Book of Regular Expressions"][bbre] by Dan Nguyen.

## Scenario

We will be using a text file from the "Tillamook County Pioneer Museum" that contains a person's name, birth date and death dates. We will then be modifying this file in preparation to be placed into a spreadsheet. To do this, we will be running multiple regexes against the file. We would like this data to go into a spreadsheet but it also requires some changes. 

The initial input look like this:

```
1   First Name: Martin
2   Middle Initial: 
3   Last Name: AAMOTH
4   Birth Date: 2/4/1886
5   Death Date: 5/24/1966
6
7   First Name: Dorothy
8   Middle Initial: A
9   Last Name: AARBirth Date: 
10  Birth Date: 
11  Death Date: 10/7/1974
12
13  First Name: Charles
14  Middle Initial: H
15  Last Name: ABBEY
16  Birth Date: 8/8/1909
17  Death Date: 2/21/1982
18
19  First Name: Muriel
20  Middle Initial: I
21  Last Name: ABBEY
22  Birth Date: 7/18/1912
23  Death Date: 6/7/1992
24
25  First Name: John
26  Middle Initial: T
27  Last Name: ABBOTT
28  Birth Date: 10/12/1902
29  Death Date: 3/13/1996
```
You can download a copy [here](/assets/rifaso/regexinput.txt)

The final output will look like this.

![FinalOutput](/assets/rifaso/regexfinaloutput.png)

## Prerequisite

To follow along you will need a text editor that support Regexes. I will be using [Sublime Text](http://www.sublimetext.com), but any will do.

## Year, months, days

In our example we need to change the date format from MM/DD/YYYY to YYYY-MM-DD. Unfortunately not all our data meets this format. We have dates that include M/D/YYYY, M/DD/YYYY, and MM/D/YYYY Single digit dates and months will first need to be have a 0 placed in front of them. So 1/1/2000 becomes 01/01/2000.

`\b` denotes a word boundary either at the beginning or the end of a word. Words are any combination of consecutive letters, numbers or underscores.
`()` will capture anything between the parenthesis.
`\d` stands for a digit 0-9.
`\1` will use the captured data in the replacement.

Therefore we are searching for a single digit that has a word bound to the left or right of it and adding a 0 in front of it. We can use this regex to accomplish this.

Find: `\b(\d)\b`

Replace: `0/1`

This give us:

```
1   First Name: Martin
2   Middle Initial: 
3   Last Name: AAMOTH
4   Birth Date: 02/04/1886
5   Death Date: 05/24/1966
6
7   First Name: Dorothy
8   Middle Initial: A
9   Last Name: AARDE
10  Birth Date: 
11  Death Date: 10/07/1974
12
13  First Name: Charles
14  Middle Initial: H
15  Last Name: ABBEY
16  Birth Date: 08/08/1909
17  Death Date: 02/21/1982
18
19  First Name: Muriel
20  Middle Initial: I
21  Last Name: ABBEY
22  Birth Date: 07/18/1912
23  Death Date: 06/07/1992
24
25  First Name: John
26  Middle Initial: T
27  Last Name: ABBOTT
28  Birth Date: 10/12/1902
29  Death Date: 03/13/1996
```

Next we will change the mm/dd/yyyy yyyy-mm-dd

`{}` state how many times we are looking for the syntax immediately before it. So a `\d{2}` says we are a looking for a two digit number.
`()()` having multiple parenthesis will capture multiple groups. We use `\1`, `\2\` etc when referring to each group.

We are searching for all instances of two digit number, followed by a slash, followed by another two digit number, a slash, and then a four digit number. We will capture all the digits so we can rearrange them but we will not be capturing the slashes. We will be replacing the slashes with dashes. We can do all this with the following:

Find: `(\d{2})/(\d{2})/(\d{4})`

Replace `\3-\1-\2`

Our replacement regex states that we took the third group of parenthesis in the find and placed it first. This will be followed with a dash, the first captured group, another dash, and finally the second captured group. Our result will look like this now.

```
1   First Name: Martin
2   Middle Initial: 
3   Last Name: AAMOTH
4   Birth Date: 1886-02-04
5   Death Date: 1966-05-24
6
7   First Name: Dorothy
8   Middle Initial: A
9   Last Name: AARDE
10  Birth Date: 
11  Death Date: 1974-10-07
12
13  First Name: Charles
14  Middle Initial: H
15  Last Name: ABBEY
16  Birth Date: 1909-08-08
17  Death Date: 1982-02-21
18
19  First Name: Muriel
20  Middle Initial: I
21  Last Name: ABBEY
22  Birth Date: 1912-07-18
23  Death Date: 1992-06-07
24
25  First Name: John
26  Middle Initial: T
27  Last Name: ABBOTT
28  Birth Date: 1902-10-12
29  Death Date: 1996-03-13
```

## Names

We will now be switching the order of the names. We want to switch First name, Middle Initial and Last Name to Last Name, First Name, Middle Initial. This step may not be that useful in the long run as we plan to import this data into a spreadsheet document and spreadsheets make it trivial to move the columns around. However here we go.

`^` states that find has to start at the beginning of a line.
`.` finds any character except for a line break.
`*` finds zero or more of the syntax immediately before it.
`\n` finds a line break.

What we want to capture is the first three lines of each person. We do this with the following:

Find: `(^F.*\n)(.*\n)(.*\n)`

Replace: `\3\1\2`

We used `^F`, as we don't want to capture any three lines, we want to capture the three lines starting with First Name. The F is the least number of characters we need to distinguish the First Name row from any of the other rows. If another had a word that started with the letter F we would just have used more characters until we were sure we were finding the First Name row and not the other row. We then use .*\n to capture the rest of the row plus the line break. Capturing the line break allows us to continue or find to the next line, and it also allows us to use it when we do our replacement. 

After moving the third row before the first two our result is as follows:

```
1   Last Name: AAMOTH
2   First Name: Martin
3   Middle Initial: 
4   Birth Date: 1886-02-04
5   Death Date: 1966-05-24
6
7   Last Name: AARDE
8   First Name: Dorothy
9   Middle Initial: A
10  Birth Date: 
11  Death Date: 1974-10-07
12
13  Last Name: ABBEY
14  First Name: Charles
15  Middle Initial: H
16  Birth Date: 1909-08-08
17  Death Date: 1982-02-21
18
19  Last Name: ABBEY
20  First Name: Muriel
21  Middle Initial: I
22  Birth Date: 1912-07-18
23  Death Date: 1992-06-07
24
25  Last Name: ABBOTT
26  First Name: John
27  Middle Initial: T
28  Birth Date: 1902-10-12
29  Death Date: 1996-03-13
```

As we are only really moving the Last Name and the First Name and Middle Initial are essentially moving as a block, I was able to "simplify" the regex to this

Find: `(^F(?:.*\n){2})(.*\n)`

Replace: `\2\1`

`(?:)` says we will use this group in the search but don't need to capture it and use it in the replace. However, in our example I still captured the data as we had parenthesis inside parenthesis. So all we did was ensure that we didn't waste memory in capturing it twice.

Now though this is using less captured groups, I find the first regex we used is a bit more readable and easier to write.

## Preparing for a spreadsheet

We are going to use tabs for delineating our data. I personally perfer tab delineation as it gives us the option of importing or copy and pasting depending on the spreadsheet program of choice and requirements of the task.

`[]` donate a character can be a choice between whatever is inside the brackets. So, [ab] says the the character we are searching for is an 'a' or a 'b'. 
`\w` finds any letter, number or an underscore.
`+` finds one or more of the syntax immediately before it.
`\t` finds a tab or when used in replacement add's a tab

We want to find either a letter, number, underscore or space until we reach a colon. Then we want to capture everything else till the end of the line. We will be using these values in our tab delineated format. 

This is the regex we will use:

Find: `[\w ]+: (.*)\n`

When attempting our search we notice that the last option is not selected. 

![Regex Highlighted](/assets/rifaso/regexhighlightbefore.png)

The easiest way to correct this is to just modify the text by adding another line after the last line. This extra line break gives the `\n` something to find, ensuring we get everything.

![Regex Highlighted](/assets/rifaso/regexhighlightafter.png)

Now that we have found everything we can use this in our replace:

Replace: `\1\t`

And our results now look line this.

```
1  AAMOTH Martin    1966-05-24  1886-02-04  
2  AARDE  Dorothy A 1974-10-07    
3  ABBEY  Charles H 1982-02-21  1909-08-08  
4  ABBEY  Muriel  I 1992-06-07  1912-07-18  
5  ABBOTT John    T 1996-03-13  1902-10-12  
```

Though we don't see it, we have now ended up with an extra tab at the end. Most of the time this can be ignored, but for completeness lets get rid of it.

`$` is the opposite of ^. It states the search has to occur at the end of a line.

So we want all tabs that are at the end of the line. We can do this with:

Find: `\t$`

Replace:

We are replacing our search with nothing which will delete what we searched for, therefore removing the extra tabs.

```
1  AAMOTH Martin    1966-05-24  1886-02-04
2  AARDE  Dorothy A 1974-10-07
3  ABBEY  Charles H 1982-02-21  1909-08-08
4  ABBEY  Muriel  I 1992-06-07  1912-07-18
5  ABBOTT John    T 1996-03-13  1902-10-12
```

We can now either copy and paste the data in or import the text file into your spreadsheet program of choice.

## Conclusion

Though explaining everything took some time, this whole process is completed in minutes saving you from editing each record individually. If you have any questions, please feel free to add them in the comments. And again, if you would like to learn more about regexes the book ["The Bastards Book of Regular Expressions"][bbre] by Dan Nguyen is a great resource.

[bbre]:http://regex.bastardsbook.com/
