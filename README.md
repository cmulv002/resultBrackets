# Head to Head

This is a single file piece of code designed to generate head-to-head data for matches for tournaments for an entire year. The code relies on lxml and python for the parsing of the websites and openpyxl for writing the data to a spreadsheet. The spreadsheet included in the repository is a direct result of the code listed. The program functions using data parsed from smash.gg. 

## Credits


[LXML Documentation](https://lxml.de/)

Anything you need to know about lxml can be found here. The HTML stuff is the only stuff that I used. 

[Web Scraping with Python/LXML Tutorial from Stanford](http://stanford.edu/~mgorkove/cgibin/rpython_tutorials/webscraping_with_lxml.php)

This is a great starting point for learning to use lxml. Since lxml does not have as much documentation as other parsing libraries, I would recommend starting here for some functioning examples and ideas on how to get started. I personally relied on the CSSSelector examples I learned here to gather the data. 

[Automate the Boring Stuff with Python Chapter 12](https://automatetheboringstuff.com/chapter12/)

This is a great place to learn the openpyxl(exporting to excel) that you need. I personally recommend going through this chapter and learning it because you'll end up referencing it all the time in any case. 





## Prerequisites

```
lxml - https://lxml.de/
openpyxl - https://openpyxl.readthedocs.io/en/stable/
```

To run this program, you must use these two libraries. You can install them via pip if you know what you're doing with python. You will also need the requests and string library. Including those is shown in the code. 


## The Code

[Github](https://github.com/cmulv002/resultBrackets/blob/master/headtohead.py)

This code is basically complete and will, if a specific bug is remedied, collect data for all the included variable tournaments and output them to a spreadsheet. To update this code with more tournaments, just update the tournament variables. To change the featured players, append the Top100List. If you plan to change the number of people on the list, ctrl-f for 102 and change it up or down as many people as you are adding or subtracting. 

Here are the core functions of the program.

### Separating Winners and Losers

'''
  for el in root.cssselect('div.match-affix-wrapper'): #div.match-affix-wrapper is the match selector
        

        for elemCheck1 in el.cssselect('div.matchSectionWrapper'): 
            incrementNameList.append(elemCheck1.text_content())
            
        for elemCheck3 in el.cssselect('div.match-player.winner'): #
            for elemCheck4 in elemCheck3.cssselect('i.fa.fa-check.text-success'): #checks if there is a non-reported match
                winnerCheck = elemCheck3.text_content()
                #print(winnerCheck)
                noReport = True
            
        
        #print(incrementNameList)
        
        if(incrementNameList[0] == winnerCheck and noReport == True): #if score is not reported
            winnerList.append(winnerCheck+str(0))
            loserList.append(incrementNameList[1]+str(0))
            
            noReport = True
            falseStrTxt = True
            falseStrTxt2 = True
            
        elif(incrementNameList[0] != winnerCheck and noReport == True): #if score is reported
            loserList.append(incrementNameList[0]+str(0))
            winnerList.append(incrementNameList[1]+str(0))
            
            noReport = True
            falseStrTxt = True
            falseStrTxt2 = True
            
        incrementNameList = []
                    
                        
        
        for elem1 in el.cssselect('div.match-player.winner'): #
            
            if falseStrTxt == False:
                winnerList.append(elem1.text_content())
            falseStrTxt = False
            
            
                    
                
             
        
            
        for elem2 in el.cssselect('div.match-player.loser'):
            if falseStrTxt2 == False:
                loserList.append(elem2.text_content())
            falseStrTxt2 = False
        falseStrTxt = False
        falseStrTxt2 = False
        noReport = False    
        incrementNameList = []
        winnercheck = None
        x = x+1
'''
  
This function is rather daunting so here's an overview. This function first creates a list of all of the matches by using the div tag match-affix-wrapper. After that, it parses through to detect which player won the match, and which lost, and following that ammends the winner list or loser list. 


'''
 aCoord = 'A'
    colLetter = 'A'
    
    k = 0
    for j in range(2, 102):
        colLetter = get_column_letter(j)
        
        top100Coord1  = aCoord + str(j)
        top100Coord2 = colLetter + '1'
        sheet[top100Coord1] = top100List[k]
        sheet[top100Coord2] = top100List[k]
        k = k + 1
    k = 0
    
     for e in range(0, len(winnerList)):
        for f in range(0, len(top100List)):
            if loserList[e] == top100List[f]:
                deleteFlag2 = True
            if winnerList[e] == top100List[f]:
                deleteFlag1 = True
                
        if (deleteFlag1 == True) and (deleteFlag2 == True):
            newWinnerList.append(winnerList[e])
            newLoserList.append(loserList[e])
            
        #else:
            #print("Delete ", winnerList[e], " beat ", loserList[e])
           
        deleteFlag2 = False
        deleteFlag1 = False

'''

This function writes the names of the players to the spreadsheet. get_column_letter in openpyxl returns a letter for the matching
column(1 = A, 2 = B, etc). It writes the names of the players in the first colum vertically and in the first crow horizontally 
to make the spreadsheet navigable in either the column or the row. 


'''

 for a in range(0, winnerLength):
        for colCellObj in sheet['A2':'A101']:
            for cellObj1 in colCellObj:
                #print(str(cellObj1.value), '-', str(winnerList[a]))
                if cellObj1.value ==  winnerList[a]:
                    winnerCoordinate = cellObj1.coordinate
                    winnerCoordNum = winnerCoordinate.replace("A","")
                    #print(cellObj1.coordinate, winnerList[a], " Winner ")
                    fixerFlag1 = True
                    s=s+1
                    
                if cellObj1.value == loserList[a]:
                    loserCoordinate = cellObj1.coordinate
                    loserCoordNum = loserCoordinate.replace("A","")
                    #print(cellObj1.coordinate, loserList[a], " Loser ")
                    fixerFlag2 = True
                    t = t+1
                    
                    
                    
                #------SCOREDISP-------#
                #print(a)
        
            
            
        if fixerFlag1 == True and fixerFlag2 == True:
                winnerY = get_column_letter(int(winnerCoordNum))
                #print(winnerY, winnerCoordNum)
                loserY = get_column_letter(int(loserCoordNum))
                #print(loserY, loserCoordNum)
                fourCoordTwo = winnerY + loserCoordNum
                
                
                fourCoordOne = loserY + winnerCoordNum
                #print(fourCoordOne, fourCoordTwo)
                #print(fourCoordTwo)
                #print(fourCoordOne)
                fixerFlag1 == False
                fixerFlag2 == False
                
                #print(fourCoordOne, fourCoordTwo)
                #FOURCOORD1 is WINNER +
                #FOURCOORD2 is LOSER +
                scoreShow = sheet[fourCoordOne].value
                
                #print(scoreShow)
                if scoreShow is None:
                    
                    sheet[fourCoordOne] = '1-0'
                    sheet[fourCoordTwo] = '0-1'
                    #print(sheet[fourCoordTwo].value)
                else:
                    #print(scoreShow)
                    r = str(scoreShow)
                    splitList = r.split("-")
                    #print(splitList)
                    
                    ogWinScore = splitList[0]
                    ogWinScore = int(ogWinScore)
                    ogLoseScore = splitList[1]
                    ogWinScore = str(ogWinScore+1)
                    sheet[fourCoordOne] = ogWinScore + '-' + ogLoseScore
                    sheet[fourCoordTwo] = ogLoseScore + '-' + ogWinScore
        elif fixerFlag1 == True and fixerFlag2 == False:
            
            fixerFlag1 == False
            fixerFlag2 == False
        elif fixerFlag1 == False and fixerFlag2 == True:
            fixerFlag1 == False
            fixerFlag2 == False
            


'''

This part of the program is used to update scores on the spreadsheet. Many flags were necessary to iron out errors, so the code is rather confusing. Please ask for elaboration if you need it.


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
