---
title: Handling Data with Python
description: Review User Feedback Data to Create New Software Requirements
date: 2022-12-13T06:40:11.505Z
---
P﻿roblem:



A software company has a popular mobile app which has generated some user feedback data in the form of over 6000 user reviews. Management wants you to derive new requirements from the user  reviews. You can manually read and sort each piece of data, or you can find your own solution. The goal is to improve the app with the new requirements derived from the data



### Solution:



My code sorts the data points based on a given criteria and puts that data into new Excel files. I used Python with the 'Panda' and 'Textblob' py-modules. Panda was used for handling the data, Textblob was used to run a sentiment analysis on the data.



I started off by reducing the number of data points. First I removed all the reviews that contained less than 4 words. I did this because you need at least a subject, verb, and object to form a complete sentence. For example, "this app stinks" is technically a sentence but it doesn't tell us much besides that the review is negative. 4 word phrases like: "the search is broken", or "the feed loads slowly", are both complete sentences that actually tell us something important; albeit vague, we can still use these data points to help discover requirements. After the first reduction, I ended up with 2300 data points.



Then I did a second reduction by creating a word bank and removing all the reviews that do not contain 1 of those words. The word bank is: “wish|want|help|please|broke|feature|perform|new|fix|bug|lost|add|comp|better|problem|hope|upgrade|competitors|additional|more|extra”. I selected these words because they show a need or problem. After the second reduction, I ended up with around 500 data points.

Then I split the 500 data points into 3 chunks. As the data was chunked into each piece, a sentiment analysis was ran on each piece; this shows the subjectivity and polarity of each review. Then, I manually reviewed the chunks of data.

Results:
Within the user reviews there was evidence of commonly occuring issues from which we could create new requirements.

1. Fix the bugs related to cloud storage of the app data.
2. Update the grind system where players can earn more fishbucks daily.
3. Fix the bugs related to the fishtanks, there have been multiple reports of people losing their fish tank data. 
4. Fix bug where users lose fishbucks when updates occur. 
5. Add a new feature to allow fishtank customization.

`﻿``py

#~2300 words after first reduction
#import pandas library
import pandas as pd

#create a data object with the data from excel file in desktop filepath
data = pd.read_excel('C:/Users/Astro/Desktop/data.xlsx')

#remove all rows in data with less than 3 words in the 'Review' column
data = data[data['Review'].str.split().str.len() > 3]

#add data into a new excel file and save it to desktop
data.to_excel('C:/Users/Astro/Desktop/reduce1.xlsx', index=False)

#show the new excel file in file explorer
import subprocess
subprocess.Popen(r'explorer /select,"C:\Users\Astro\Desktop\reduce1.xlsx"')



#display the data
print(data)


#~500 words after second reduction
#import pandas library
import pandas as pd

#create a data object with the data from excel file
data = pd.read_excel('C:/Users/Astro/Desktop/reduce1.xlsx')

#reduction 2
data = data[data['Review'].str.contains('wish|want|help|please|broke|feature|perform|new|fix|bug|lost|add|comp|better|problem|hope|upgrade|competitors|additional|more|extra', case=False)]


#add data into a new excel file and save it to desktop
data.to_excel('C:/Users/Astro/Desktop/reduce2.xlsx', index=False)


#show the new excel file in file explorer
import subprocess

subprocess.Popen(r'explorer /select,"C:\Users\Astro\Desktop\reduce2.xlsx"')



#display the data
print(data)


#~250 words after chunked

#import pandas library
import pandas as pd

#create a data object with the data from excel file in desktop filepath
data = pd.read_excel('C:/Users/Astro/Desktop/reduce2.xlsx')

data = data[data['Review'].str.contains('wish|want|help|please|broke|feature|perform|new', case=False)]

#save data to a new excel file
data.to_excel('C:/Users/Astro/Desktop/first-chunk.xlsx', index=False)

#run a sentiment analysis on the data
from textblob import TextBlob
data['polarity'] = data['Review'].apply(lambda x: TextBlob(x).sentiment.polarity)
data['subjectivity'] = data['Review'].apply(lambda x: TextBlob(x).sentiment.subjectivity)

#save the sentiment analysis data to a new excel file
data.to_excel('C:/Users/Astro/Desktop/first-chunk-analysis.xlsx', index=False)


 
#display the data
print(data)


#~300 words after chunked

#import pandas library
import pandas as pd

#create a data object with the data from excel file in desktop filepath
data = pd.read_excel('C:/Users/Astro/Desktop/reduce2.xlsx')

data = data[data['Review'].str.contains('fix|bug|lost|add|comp|better|problem', case=False)]

#save data to a new excel file
data.to_excel('C:/Users/Astro/Desktop/second-chunk.xlsx', index=False)

#run a sentiment analysis on the data
from textblob import TextBlob
data['polarity'] = data['Review'].apply(lambda x: TextBlob(x).sentiment.polarity)
data['subjectivity'] = data['Review'].apply(lambda x: TextBlob(x).sentiment.subjectivity)

#save the sentiment analysis data to a new excel file
data.to_excel('C:/Users/Astro/Desktop/second-chunk-analysis.xlsx', index=False)



#display the data
print(data)

#~150 words after chunked

#import pandas library
import pandas as pd

#create a data object with the data from excel file in desktop filepath
data = pd.read_excel('C:/Users/Astro/Desktop/reduce2.xlsx')

data = data[data['Review'].str.contains('hope|upgrade|competitors|additional|more|extra', case=False)]

#save data to a new excel file
data.to_excel('C:/Users/Astro/Desktop/third-chunk.xlsx', index=False)

#run a sentiment analysis on the data
from textblob import TextBlob
data['polarity'] = data['Review'].apply(lambda x: TextBlob(x).sentiment.polarity)
data['subjectivity'] = data['Review'].apply(lambda x: TextBlob(x).sentiment.subjectivity)

#save the sentiment analysis data to a new excel file
data.to_excel('C:/Users/Astro/Desktop/third-chunk-analysis.xlsx', index=False)



#display the data
print(data)

#combine the chunkies
#import pandas library
import pandas as pd

#create a data object with the data from excel file in desktop filepath
data = pd.read_excel('C:/Users/Astro/Desktop/data/first-chunk-analysis.xlsx')

data2 = pd.read_excel('C:/Users/Astro/Desktop/data/second-chunk-analysis.xlsx')

data3 = pd.read_excel('C:/Users/Astro/Desktop/data/third-chunk-analysis.xlsx')

#combine data from all 3 chunks into one excel file
data4 = pd.concat([data, data2, data3])

#save data to a new excel file
data4.to_excel('C:/Users/Astro/Desktop/data/combined-chunks.xlsx', index=False)

#display the data
print(data4)
`﻿``

 :metal: