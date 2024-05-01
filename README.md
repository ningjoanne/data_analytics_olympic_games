
Data Analyst Project – Olympic Games Analyst
============================================

![](https://ningjoanne.files.wordpress.com/2024/03/pbi-snap.png?w=750)

![](https://ningjoanne.files.wordpress.com/2024/03/sql-snap.png?w=1024)

![](https://ningjoanne.files.wordpress.com/2024/03/data-model.png?w=1024)

* * *

Business Probelm
----------------

**“As a data analyst working at a news company you are asked to visualize data that will help readers understand how countries have performed historically in the summer Olympic Games.**

**You also know that there is an interest in details about the competitors, so if you find anything interesting then don’t hesitate to bring that in also.**

**The main task is still to show historical performance for different countries, with the possibility to select your own country.”**

* * *

Processing Data & Table Structure
---------------------------------

The initial step is loading the necessary data into a SQL database and subsequently transforming it as follows:

**Olympic Games View**

    SELECT 
      ID, 
      Name AS 'Competitor Name', 
      CASE WHEN Sex = 'M' THEN 'Male' ELSE 'Female' END AS Sex, 
      Age, 
      CASE WHEN Age < 18 THEN 'Under 18' WHEN Age BETWEEN 18 
      AND 25 THEN '18-25' WHEN Age BETWEEN 26 
      AND 30 THEN '26-30' ELSE 'Over 30' END AS [Age Group], 
      Height, 
      Weight, 
      NOC AS 'National Code', 
      LEFT(
        Games, 
        CHARINDEX(' ', Games)-1
      ) AS 'Year', 
      --Split column to Year, based on space
      RIGHT(
        Games, 
        CHARINDEX(
          ' ', 
          REVERSE(Games)
        )-1
      ) AS 'Season', 
      --Split column to Season, based on space
      Sport, 
      Event, 
      CASE WHEN Medal = 'NA' THEN 'Not Registered' ELSE Medal END AS Medal --replace NA with No Registered
    FROM 
      [olympic_games].[dbo].[athletes_event_results] 
    WHERE 
      RIGHT(
        Games, 
        CHARINDEX(
          ' ', 
          REVERSE(Games)
        )-1
      ) = 'Summer' -- filter the season for Summer

* * *

Data Model & Calculations
-------------------------

The query from the previous step is imported into Power BI, where dimensions and facts can be merged, resulting in a data model that consists of just one table in this instance.

![](https://ningjoanne.files.wordpress.com/2024/04/data-model-corp.png?w=888)

The calculations are created in Power BI using Data Analysis Expressions (DAX). To simplify coding tasks, emphasis is placed on the reuse of measures, a practice known as “measure branching.”

**Number of Competitors:**

\# of Competitors =DISTINCTCOUNT( ‘Olympic Games Data'\[ID\])

**Number of Medals:**

\# of Medals = COUNTROWS(‘Olympic Games Data’)

\# of Medals (Registered) = CALCULATE (\[# of Medals\], FILTER (‘Olympic Games Data’, ‘Olympic Games Data'\[Medal\] = “Bronze” || ‘Olympic Games Data'\[Medal\] = “Gold” || ‘Olympic Games Data'\[Medal\] = “Silver”))

* * *

Dashboard – Olympic Games Analysis
----------------------------------

The completed dashboard comprises visualizations and filters, providing end users with an intuitive way to explore the history of the summer Olympic games. Users can filter by period using the year, focus on a specific country using the nation code, or delve into the performance of individual competitors or sports over time.

**[Click here to access the dashboard and give it a try!](https://app.powerbi.com/view?r=eyJrIjoiYWNiMGM0MmEtMWQ5ZC00OWIzLWI4NTktNjZkYzEwMjY0MTdhIiwidCI6Ijg5NGIzZjE2LTg2MTMtNDljZS05MzZjLTc5ZGYxMjY2M2ViNiJ9)**

[![](https://ningjoanne.files.wordpress.com/2024/03/pbi-snap.png?w=750)](https://app.powerbi.com/view?r=eyJrIjoiYWNiMGM0MmEtMWQ5ZC00OWIzLWI4NTktNjZkYzEwMjY0MTdhIiwidCI6Ijg5NGIzZjE2LTg2MTMtNDljZS05MzZjLTc5ZGYxMjY2M2ViNiJ9)

* * *

References:

[www.youtube.com/@iamaliahmad](http://www.youtube.com/@iamaliahmad)

