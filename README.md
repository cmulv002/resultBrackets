# Head to Head

This is a single file piece of code designed to generate head-to-head data for matches for tournaments for an entire year. The code relies on lxml and python for the parsing of the websites and openpyxl for writing the data to a spreadsheet. The spreadsheet included in the repository is a direct result of the code listed. The program functions using data parsed from smash.gg. 


## Prerequisites

```
lxml - https://lxml.de/
openpyxl - https://openpyxl.readthedocs.io/en/stable/
```

To run this program, you must use these two libraries. You can install them via pip if you know what you're doing with python. You will also need the requests and string library. Including those is shown in the code. 


##The Code

[Github](https://github.com/cmulv002/resultBrackets/blob/master/headtohead.py)

This code is basically complete and will, if a specific bug is remedied, collect data for all the included variable tournaments and output them to a spreadsheet. To update this code with more tournaments, just update the tournament variables. To change the featured players, append the Top100List. If you plan to change the number of people on the list, ctrl-f for 102 and change it up or down as many people as you are adding or subtracting.


## Sample Output

![alt text](https://github.com/cmulv002/resultBrackets/blob/master/scoresheet.PNG?raw=true)

[Spreadsheet](https://github.com/cmulv002/resultBrackets/blob/master/ScoreSheet8-13-2018.xlsx)

This is the output of the spreadsheet using the program. The output is generated entirely using the python script.

## Known Bugs

1. This program currently does not work due to an error found in the CSSSelector functionality. 

```
for el in root.cssselect('div.match-affix-wrapper'):
              

        for elemCheck1 in el.cssselect('div.matchSectionWrapper'):
            incrementNameList.append(elemCheck1.text_content())
            
        for elemCheck3 in el.cssselect('div.match-player.winner'):
            for elemCheck4 in elemCheck3.cssselect('i.fa.fa-check.text-success'):
                winnerCheck = elemCheck3.text_content()
                #print(winnerCheck)
                noReport = True
                
 ```
 
 This piece of code is where the bug is found. in root.cssselect('div.matchSectionWrapper'), normally the program would begin to iterate through the for loop for all of the matches contained in div classes match-affix-wrapper. This was the core functionality of the parser. Since this code lost fucnctionality over the span of the day, there also may have been a change in website permissions or an outside error preventing me from utilizing this function. I intend to further pursue this bug in hopes of restoring the program to its full potential. Unfortunately, at this time, this bug appears to be a result of the website itself not allowing for lxml parsing.
 
 2. This program does not support differentiating between losses and disqualifications. The program counts disqualifications as losses.
