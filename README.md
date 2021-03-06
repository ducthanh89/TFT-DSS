# TFT-DSS
Teamfight Tactics Decisive Support System  

10.01 TFTDSS.py file after refactoring.

29.11 new file botActions.py check description at the bottom.

15.11 First try to build champions detector based on CNN with FastAI. Results at the bottom.

This is my current project, and I will work on advices which champion should user pick from 5 random champions that user can buy.  

I will use my knowledge to build fuzzy logic system that will calculate uncertain utility for every single champion and their combinations as a team.  I dont recommend using it in ranked games especially its was tested only about 30 times in game on my personal machine.

You can track my progress in projects section.


When u want to update csv files by yourself, if not then scroll down to TFTDSS.py 


# Installation

I highly recommend you to create new virtual environment. I am using Anaconda distribution. Python 3.8.5 is working for me.

conda create -n tft python=3.8.5

conda activate tft

then go to https://pytorch.org/ and check INSTALL PYTORCH section.

"Note 1: for Windows, please install torch and torchvision first by following the official instruction here https://pytorch.org. On pytorch website, be sure to select the right CUDA version you have. If you intend to run on CPU mode only, select CUDA = None."
Reference from https://github.com/JaidedAI/EasyOCR

Then install requirements from requirements.txt

Run TFTDSS.py in your IDE(for me working in Spyder and in Pycharm). You need to use tft virtual env.

# V1DSS directory
It contains just decisive support system for game. It's for typical user. 



# Web scraping with DataMining.py
This file uses selenium, pandas, collections/namedtuple and time.

Opera webdriver is required, or u can change it to Chrome but you need to change it in code also.

Website where I am gathering data about champions is tftactics.gg

Run DataMining.py -> in the back there are gathered and refined data filled into pandas dataframe

This dataframe is converted into championsData.csv
This file contains csv such as: Champion(name), DPS, AS, DMG, Range, HP, Mana, Armor, MR, OriginPrimary, OriginSecondary, ClassPrimary, ClassSecondary, Cost, Tier

Pros: works

Cons: some code should be more automated

# FuzzyCalculation.py
This file uses pandas, numpy, skfuzzy, namedtuple.

championsData.csv file is also required which is obtained by running DataMining.py


What happens there?

-Effective HP calculation
-Scaling all stats from 0.0 to 1.0
-Using fuzzy system to set preference that MEANHP is more important than HP
-Total points for single champion calculation with formula that is based on my knowledge about game(my playstyle/preferences) and Tier from tftactics.gg


Run FuzzyCalculation.py -> return scaledChampionsdf.csv

# ChampionsDataAnalysis.py
This file uses pandas, numpy, matplotlib.

scaledChampionsdf.csv file is also required which is obtained by running FuzzyCalculation.py

Data about some(7) champions is visualized on radar plot u can take a look for example, just scroll down.

U can change argument in line 183 to pick champions cost tier that u want to plot.


Run ChampionsDataAnalysis.py -> return figure with radar plots.

# TFTDSS.py
This file uses tkinter, pandas, namedtuple, easyocr, cv2(openCV).

scaledChampionsdf.csv file is also required which is obtained by running FuzzyCalculation.py

note: you should use window mode on full hd resolution in game.
-------------------------------------------------------------
GUI

-Semi automated GUI building
-Champions as counters
-Classes and origins as counters

-User can add(1) or sub(1) from counter
-User can reset every counter on Champions to buy pool to 0 value with single button(reset)
-User can update classes and origins based on Champion pool counters with single button(update classes)
-User can show points for champions to buy(show points button) // points are calculated basing on champion pool, origin and class counters
-User can use OCR to update champions to buy counters(OCR button)
-User can visualize which champion to buy, where champ with most points is bordered with green rectangle, then 3 champions with worse points score are presented with blue rectangles, and the worst champion has red rectangle(draw rectangles button)
-Scan&go button is reseting champions to buy counters. Then updating champion pool, origin and class counters. Next it uses OCR to detect champions to buy from buy zone. Finally it shows points for champions to buy.
-User can add(1) to Champion pool counter by clicking champion name that appear with points


You should use +- buttons in champion pool to feed system with ur current champion pool.
Then scan&go, and after buttons named by champions appear click on that, that you will buy them.
-----------------------------------------------------------------

Points calculation with champion pool included(not single champion like in FuzzyCalculation.py file).

Formula is based on my experience in game and my playstyle.

Points = single champion points(already calculated in FuzzyCalculation.py file) + bonus for the same origin(counter * 0.2) + bonus for the same class(counter * 0.2) + bonus for the same champion in pool((counter -1) * 0.2)


# SShelper.py
This file is used for cropping screenshots to extract in game champion models which im going to feed neural network model.







# Data visualization.
![DataAnalysisTier4](https://user-images.githubusercontent.com/60773657/105047831-1e393c00-5a6b-11eb-9fb0-fbf8cf76db6e.png)


# Using DSS in game.


![TFTDSSinGame](https://user-images.githubusercontent.com/60773657/105048410-d5ce4e00-5a6b-11eb-84ab-0ba6b5fe86e4.jpg)



Results of first three attempts playing normal game with 54 points in silver 4.
You should pay attention that TFTDSS supported me only in choosing champion to buy. I took care about items and champions placement on the field myself.

![Scores](https://user-images.githubusercontent.com/60773657/86474581-6bf68d00-bd43-11ea-849a-700741035bc8.png)

# Draw rectangles button(points mode)
![DrawRectangles](https://user-images.githubusercontent.com/60773657/105048542-fd251b00-5a6b-11eb-8cd3-bcf2b701984b.jpg)


# Champion object detection
I think i need to collect more images.
![Champion-detection2](https://user-images.githubusercontent.com/60773657/99180678-7fb9f300-2728-11eb-80bf-87b75fb2a74e.JPG)

![Champion-detection3](https://user-images.githubusercontent.com/60773657/99180674-76308b00-2728-11eb-9309-a37063c0dfa2.JPG)



07.12.2020 object champion detection with YOLO
![ROTD1](https://user-images.githubusercontent.com/60773657/101356751-d71b3100-3898-11eb-9756-85f22e17dbd7.jpg)



# botActions.py
Working bot which buy preffered class of champions for example brawlers. Shuffling champions on playground and subsitutes bench after champion is bought. Also spending gold in predefined rounds for XP or refreshing champions to buy. This bot is mainly for automate screenshots gathering to feed my dataset to train object detection model, but it performs well and can beat some players. For sure it beats "afk bots".
For sure you need to clone directories with JPGs like TemplateMatchingClient.

Just be sure to set windowed mode in game, and after log in u can run this code.
