# Natural-Disasters
Analysis of natural disasters from 1900-2021.

## *Dataset*
**Source:**
EM-DAT, CRED / UCLouvain, Brussels, Belgium 
https://www.emdat.be 
Version: 2022-08-07 
File creation: Sun, 07 Aug 2022 09:04:19 CEST 
Num. of records: 25650 

## *Ask*

**1.**  Where in the world are the highest number of natural disasters happening?

**2.**  How have natural disasters changed over time, and have they increased or decreased?

**3.**  Which types of natural disasters happen the most frequently, and has this changed over time?


## *Clean Data*

Counted rows to confirm that the *Dis_No* colum was unique.
```
SELECT COUNT("Dis No") 
FROM Disasters;

SELECT COUNT(DISTINCT("Dis No")) 
FROM Disasters;
```

Confirmed the time frame I am looking at.
```
SELECT DISTINCT("Start Year")
FROM Disasters
ORDER BY "Start Year";
```

Identified the fields that I want to see data on.
```
SELECT "Dis No","Disaster Group", "Disaster Type", "Country", "Region", "Continent","Start Year", "End Year", "Total Deaths", "Total Affected", "Total Damages, Adjusted ('000 US$)"
FROM Disasters
LIMIT 1000;
```

Renamed *Total Damages, Adjusted ('000 US$)* to *Total Damages* so that it is shorter and clearer.
```
ALTER TABLE Disasters
RENAME COLUMN "Total Damages, Adjusted ('000 US$)" TO "Total Damages"
```

Replaced NULLs with "Unknown" in *Total Deaths*, *Total Affected* and *Total Damages* columns so that we have a value if needed on later visulazation.
```
UPDATE Disasters
SET "Total Deaths" = "Unknown"
WHERE "Total Deaths" IS NULL;

UPDATE Disasters
SET "Total Affected" = "Unknown"
WHERE "Total Affected" IS NULL;

UPDATE Disasters 
SET "Total Damages" = "Unknown"
WHERE "Total Damages" IS NULL;
```

Removed "(the)" from the *Country* column to make sure there are no issues for mapping in visualizations
```
UPDATE Disasters 
SET "Country" = REPLACE(Country, ' (the)', '');

UPDATE Disasters 
SET "Country" = REPLACE(Country, ' (Province of China)', '');

UPDATE Disasters 
SET "Country" = REPLACE(Country, ' (Islamic Republic of)', '');

UPDATE Disasters 
SET "Country" = REPLACE(Country, ' of Great Britain and Northern Ireland', '');
```

Identifyed the data that I want to export to work with. 
```
SELECT "Dis No","Disaster Group", "Disaster Type", "Country", "Region", "Continent","Start Year", "End Year", "Total Deaths", "Total Affected", "Total Damages"
FROM Disasters
ORDER BY "Start Year", "Continent","Country","Region","Disaster Group", "Disaster Type";
```

## *Analysis*

**1.  Where on the globe are the highest number of natural disasters happening?**  
Per the recorded data, natural disasters have occured the most in the US, China,India, Phillipines and Indonesia from 1900 to 2021.

![image](https://user-images.githubusercontent.com/110743067/185026599-8651fdb4-eaa1-49fe-863a-c284b2e44187.png)

##

**2.  How have natural disasters changed over time, and have they increased or decreased?**   
The data is showing that natural diasters have steadly increased starting in the 1950s, with a more dramatic incline starting around 1997. This could be because collecting data about disasters has become easier than it was in the early-mid 1900s. Furthur reseach would need to be done to conclude this.

![image](https://user-images.githubusercontent.com/110743067/185026686-61a5a6a3-5800-4187-8163-651e41958f67.png)

##

**3.  Which types of natural disasters happen the most frequently, and has this changed over time?**   
Floods and storm account for 63.6% of natural disasters worldwide. Both have seen increases starting around the 1950s. 

![image](https://user-images.githubusercontent.com/110743067/185026708-b25829a4-dcea-4add-8fc1-c8a6d68316a6.png)

![image](https://user-images.githubusercontent.com/110743067/185026717-b9404835-b78d-4c19-a7d8-a432b026fd7e.png)
