## Tor Des Geants
Prior to the analysis of the data, several data exploration and cleaning was conducted. All attempts were made to make this cleaning process as transparent as possible.
This is a brief summary of the most important steps.
If you wish to explore the Python code,  please head over to Github https://github.com/kjonina/Tor_Des_Geants

### Extracting DUV-Statistics and ITRA data
Although it was considered to write Python code to scrape the data, it was decided that it would be too time consuming to be of benefit. It was decided to just copy and paste the data into a excel or text file. 

The initial reason for grabbing this data was to get the age and year of birth (YOB) of the participants. This quickly was abandoned for several reasons: 
- DUV only had YOB of finishers
- ITRA had only 2 years of our nearly 15 years that had DNFs also supplied in the results. 
- Several isues with the format from 100x100trail data and ITRA or DUV data to make the analysis of age any use. 

However, the data became very useful in other ways:
- ITRA data had the names of the participants who had been stopped by the organisation due to weather conditions in Rifugio Frassati and Bosses.
- DUV data was used for 2 reasons: 1) it contained official start and end dates of each race which was used to established an accurate Start Date for 100x100trail 2) used to accurately replace 'D3' amd 'D4' in Cut-offs Timetable

- Using DUV datasest, a simple dataset was created for each year to establish the start date of each race for each year. 

![DUV Start Date](https://github.com/kjonina/Tor_Des_Geants/Methodology Pictures/DUV Start Date.PNG)

![DUV D#](https://github.com/kjonina/Tor_Des_Geants/Methodology Pictures/DUV D#.PNG)

### Timetable - Cut-offs/ Elevation / Distance
Standard excel file was downloaded from the website to get info on the cut-offs, elevation and distances. 

![Timetable data original](https://github.com/kjonina/Tor_Des_Geants/Methodology Pictures/TOR330 Timetable Original.PNG)

Manual cleaning in Excel was conducted to make it a bit more readable.
(Again, it's possible to clean the dataset in Python but it's just be easier to clean with excel.

![Timetable data clean](https://github.com/kjonina/Tor_Des_Geants/Methodology Pictures/TOR330 Timetable Clean.PNG)

D3 is different every year so dates from DUV dataset were used to accurately get the dates.

### 100x100trail
This is the goldmine that contains all the times for each checkpoint of the race. This was scraped using Python code, and relevant information regarding checkpoint and time was extracted for each participants. 

I must admit that I thought this would be cleaning and analysis would be A BREEZE. Boy, was I wrong! Here I will try my very best to explain the logic. Ha ha ha ha!

#### Rows in the dataset
Number of rows in DUV and ITRA (each line in each dataset was one finisher) should that there were errors to look out for in 100x100trail dataset that was scraped!

![Nubmer of rows in each DUV, ITRA and 100x100trail](https://github.com/kjonina/Tor_Des_Geants/Methodology Pictures/ITRA_DUV_TOR_rows comparison.png)
Running the data in Python, showed that in 2021 there were 3 extra participants labelled as finished in the 100x100trail, in 2023  has 9 partipants and in 2024 has 3 participants with the same error.
In 2022, it looked like a similar issue, however, when exploring ITRA result further, it clear that the number of finishers (at Courmayer) that were submitted to ITRA and DUV were  the same.  

![Nubmer of rows in each ITRA](https://github.com/kjonina/Tor_Des_Geants/Methodology Pictures/ITRA_2022_finishers.png)

This means that we will have to go and look for those individuals who were incorrectly labelled as finished.


Some participants were easy to spot: filter by column 'Status' is TRUE AND column 'FINISH' (timestamp at the finish line) had a value (because they couldnt have finished but not have a timestamp at the finish line).
(Only 4 partipants in 2023 had this issue, so I first examined the rows to ensure that participants didnt have timestamps at other checkpoints. 
Other participants has Status is TRUE, I randomly picked three or four checkpoints and stated to select all that were NULL. 

Describe the cleaning process.

#### Start Date
Immediately issues with Start Date - it is unclear why there are issues with Start Date. 

(picture)Show the issues with Start Date

Start times were allocated based on Bibs, so bib numbers from 1 to 1000 were changed to 10:00:00 and bib numbers from 1001 onwards were changed to 12:00:00. 
This cleaning process was applied to every year.

(picture)Show email of the bib email

(Needs clarification for 2021 and 2022)

#### Aid Stations - RITIRIO
After extracting the name of the checkpoints, it was also noticed that some aid stations also were contained ' - RITIRIO' ('retired' in Italian). 
If the checkpoint contained this ' - RITIRIO', it was placed into a new column. In 2023, even though nearly 535 participants dropped out, there were only 135 participants with ' -  RITIRIO'.

(picutre) show example of JS

Although this runner has gotten  'Donnas OUT - RITIRIO', I am 100% sure that the individual dropped out at Chardonney (I ran with this runner).
From Chardonney, he got a lift to Donnas and hopped on the bus back to Courmayer. It does makes sense to place retired at Donnas, considering that was the lifebase he dropped out at. 
This makes this column less relevant - it does not add any benefit to the analysis TBH.


Issuses with the POLISH FELLA in Gressoney - got to Gressoney but didnt get a timestamp at gressoney for retiring. I know beacuse I ran up to him and was all excited when I was leaving. He was just coming in

#### Last Checkpoint for DNFs
Code was written to spot the very last checkpoint with a timestamp for DNFs. THis would mean that this is the last checkpoint arrived in.
This was placed into a column for each DNF. 
THis was created beacuse '- RITIRIO' was only available for 135 participants (out of 535 DNFs).


#### Teleportation issues
Some teleportation issue were spotted. Some participants after DNF'ing were scanned in miles away from their true last place of DNFing.

(picutre) show example of PW.

This is different to issues of wrongly labelling as retired, case of HBG.

(picutre) show example of HBG.

It was decided to save an excel file of all DNFs for each year and manually explore the teleportation issue. 

#### Missing times
There were also some participants who had missing times for different checkpoints. 

(picutre) show an example of a few missing.

Even though I don't know the true explanation for this, this could be down to simple issues related to manual data entry. 
Both runners and volunteers are really tired, especially towards the end of the race so sometimes both the runners and the volunteers would probably forget to scan the person through. 

This made it difficult to explore the times between checkpoints. 

(picutre) show missing values with FINISHERS. 



It makes sense to focus on the analysis from life base to life base. 

(picutre) show missing values for life basis for FINISHERS.



#### Cut-Off analysis
How many people missed the cut-offs?

Show Gressoney OUT person who completely missed their cut-off from Wave 1 and still finished. 

Sometimes, innocence is best.










## References
DUV-Statistics

TOR130
https://statistik.d-u-v.org/getresulteventalltime.php?event=111662

TOR330
https://statistik.d-u-v.org/getresulteventalltime.php?event=25042

TOR450
https://statistik.d-u-v.org/getresulteventalltime.php?event=91888


ITRA
https://itra.run/Races/RaceDetails/74639

100x100trail
https://100x100trail.com/json/TOR3302024.json


Timetable - Cut-offs/ Elevation / Distance
https://www.torxtrail.com/it/content/downloads