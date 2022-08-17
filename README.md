# Natural-Disasters
Analysis of natural disasters throughout the the last decade (1900-2021).

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


## *Process*

```
SELECT "Total Damages"
FROM Disasters
LIMIT 100;
```

```
#Counted rows to confirm that the Dis_No colum was unique
SELECT COUNT("Dis No") 
FROM Disasters;

SELECT COUNT(DISTINCT("Dis No")) 
FROM Disasters;
```

```
#Identifying the time frame I am looking at
SELECT DISTINCT("Start Year")
FROM Disasters
ORDER BY "Start Year";
```

```
#Identifying the fields that I want to see data on
SELECT "Dis No","Disaster Group", "Disaster Type", "Country", "Region", "Continent","Start Year", "End Year", "Total Deaths", "Total Affected", "Total Damages, Adjusted ('000 US$)"
FROM Disasters
LIMIT 1000;
```

```
#Renamed column to make it shorter and clearer
ALTER TABLE Disasters
RENAME COLUMN "Total Damages, Adjusted ('000 US$)" TO "Total Damages"
```

```
#Added "Unknown" in place of Total Deaths, Total Affected and Total Damages so that we have a value if needed on later visulazation
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

```
#Identifying the data that I want to export to turn into a visualization 
SELECT "Dis No","Disaster Group", "Disaster Type", "Country", "Region", "Continent","Start Year", "End Year", "Total Deaths", "Total Affected", "Total Damages"
FROM Disasters
ORDER BY "Start Year", "Continent","Country","Region","Disaster Group", "Disaster Type";
```

```
# Removed (the) from Country column to make sure there are no issues for visualization
UPDATE Disasters 
SET "Country" = REPLACE(Country, ' (the)', '');

UPDATE Disasters 
SET "Country" = REPLACE(Country, ' (Province of China)', '');

UPDATE Disasters 
SET "Country" = REPLACE(Country, ' (Islamic Republic of)', '');

UPDATE Disasters 
SET "Country" = REPLACE(Country, ' of Great Britain and Northern Ireland', '');
```


![image](https://user-images.githubusercontent.com/110743067/185005689-54478223-d14f-4d5f-b73a-e07b8229bcdd.png)
![image](https://user-images.githubusercontent.com/110743067/185005701-abad42b1-97aa-4848-8215-0a182583985d.png)
![image](https://user-images.githubusercontent.com/110743067/185005707-ce518229-afa8-487d-b4a3-354938694366.png)
![image](https://user-images.githubusercontent.com/110743067/185007216-9d1652fd-1a59-412c-b939-b501c7d6c0f8.png)

ACT
