# Music and Mental Health Analysis Using SQL

Here, I asked several questions with respect to the dataset, and on that basis, I came up with answers.

##[Link To Dataset](https://www.kaggle.com/datasets/catherinerasgaitis/mxmh-survey-results)

![image](https://user-images.githubusercontent.com/72240938/213400119-00552bda-e884-459c-ba61-b3d48ac61c82.png)


There is a single dataset on which I worked with.

1. Create an Age Group column and assign “Under 20”, “20–30”, “31–50”, and “Above 50” according to the Age column.

## Ans:

## Code:

ALTER table `mxmh_survey_results`

add column `Age Group` varchar(10);

UPDATE `mxmh_survey_results`

SET `Age Group` = “Under 20”

where Age<20;

UPDATE `mxmh_survey_results`

SET `Age Group` = “20–30”

where Age>=20 and Age<=30;

UPDATE `mxmh_survey_results`

SET `Age Group` = “21–30”

where Age>=31 and Age<=50;

UPDATE `mxmh_survey_results`

SET `Age Group` = “Above 50”

where Age>50;

2. Find the Age Group with Average Hours Per Day spent.

Ans:

Code:

SELECT `Age Group`, Avg(`Hours per day`) as AvgHoursPerDay

from `mxmh_survey_results`

group by `Age Group`

order by AvgHoursPerDay desc;

## Output:

![image](https://user-images.githubusercontent.com/72240938/213400570-6664894a-24ff-4b0d-b4c4-86ecebe136b6.png)

3. Find the primary streaming service, and fav genre with their Average Hours Per Day spent on them.

## Ans:

## Code:

SELECT `Primary streaming service`, `Fav genre`,

Avg(`Hours per day`) over(partition by `Primary streaming service`, `Fav genre`) as Avg_Hours_Per_Day

from mxmh_survey_results;

Output:

Average Hours Per Day for Apple Music for different genres:

Country: 5.67

EDM: 6.5

Folk: 1.5

Hip Hop: 4.25

K Pop: 4

Latin: 5

Lofi: 4

Metal: 3.6

Pop: 3.2

R&B: 4

Rap: 1.25

Rock: 4.21

Video Game Music: 1

Average Hours Per Day for Pandora for different genres:

Classical: 4

Gospel: 1.5

Hip Hop: 1

Pop: 2

R&B: 3.5

Rock: 1.5

Average Hours Per Day for Spotify for different genres:

Classical: 2.79

Country: 2.63

EDM: 4.52

Folk: 4.16

Gospel: 5

Hip Hop: 4.16

Jazz: 6.42

K Pop: 4

Lofi: 4.6

Metal: 3.78

Pop: 3.02

R&B: 3.57

Rap: 8.1

Rock: 4.04

Video Game Music: 3.38

Average Hours Per Day for YouTube Music for different genres:

Classical: 2.57

Country: 4.5

EDM: 6.4

Folk: 2.3

Hip Hop: 3.3

Jazz: 1.75

K Pop: 6.5

Metal: 3.57

Pop: 3.42

R&B: 3

Rap: 2.5

Rock: 3.28

Video Game Music: 1.88

Note: Here, I didn’t include Streaming Service “No Streaming Service”, or “Other Streaming Service”.

I used the named streaming services only.


4. Find no. of people who watch foreign languages per day.

## Ans:

## Code:

SELECT `Foreign Languages`, count(*) as Foreign_Languages_Listeners_Count

from mxmh_survey_results

group by `Foreign Languages`;

## Output:

![image](https://user-images.githubusercontent.com/72240938/213401107-6c5de5f9-3037-4381-a7e8-7436be58c67f.png)

5. Find the Top 5 Favorite Genres by the count of them.

## Ans:

## Code:

SELECT `Fav genre`, count(*) as Fav_Genre_count

from mxmh_survey_results

group by `Fav genre`

order by Fav_Genre_count desc;

## Output:

![image](https://user-images.githubusercontent.com/72240938/213401344-bea3aaa7-ba93-46b4-ac54-fae54d3e19c6.png)

6. Find Age Group by Count.

## Ans:

## Code:

SELECT `Age Group`, count(*) as Total_Age_Count

from mxmh_survey_results

group by `Age Group`

order by Total_Age_Count desc;

## Output:

![image](https://user-images.githubusercontent.com/72240938/213401579-ee080b00-1d41-43fc-abd8-5b84eae179ab.png)

7. Find Primary Streaming Service with its count of it.

## Ans:

## Code:

SELECT `Primary streaming service`, count(*) as Total_StreamingService_Count

from mxmh_survey_results

group by `Primary streaming service`

order by Total_StreamingService_Count desc;


## Output:

![image](https://user-images.githubusercontent.com/72240938/213401708-ec17634c-8af5-4140-9ac1-acc147de05c4.png)

8. Find avg BPM with respect to all genres

## Ans:

## Code:

SELECT avg(Total_BPM) as Avg_BPM_for_all_genres from

(select `fav genre`, SUM(`BPM`) as Total_BPM

from `mxmh_survey_results`

group by `fav genre`

order by Total_BPM desc) x;


## Output:

![image](https://user-images.githubusercontent.com/72240938/213401818-a410cb92-5837-49bd-b81e-7277d47a27d0.png)

9. Find avg hours per day with respect to all genres.

## Ans:

## Code:

SELECT avg(TotalHours) as Avg_Hours_Per_Day from

(select `fav genre`, SUM(`Hours per day`) as TotalHours

from `mxmh_survey_results`

group by `fav genre`

order by TotalHours desc) x;

## Output:

![image](https://user-images.githubusercontent.com/72240938/213401950-ff0264d1-8877-4c02-a512-1b9afd787698.png)

10. Find music effect count.

## Ans:

## Code:

SELECT `Music effects`, COUNT(*) AS TotalCount

from `mxmh_survey_results`

group by `Music effects`;

## Output:

![image](https://user-images.githubusercontent.com/72240938/213402057-8e47555a-4aba-4ea4-8403-f61b5d091421.png)

11. Find fav genre with respect to music effect count.

## Ans:

## Code:

SELECT `fav genre`, `Music effects`,

COUNT(*) over(partition by `fav genre`, `Music effects`) as Total_Count

from `mxmh_survey_results`;

Output:

For Genre: Classical

Improve: 28

No Effect: 11

Worsen: 1

For Genre: Country

Improve:18

No Effect: 3

For Genre: EDM

Improve: 30

No Effect: 6

For Genre: Folk

Improve: 20

No Effect: 5

For Genre: Gospel

Improve: 4

For Genre: Hip Hop

Improve: 28

No Effect: 4

For Genre: Jazz

Improve: 16

No Effect: 3

For Genre: K Pop

Improve: 19

No Effect: 4

For Genre: Latin

Improve: 1

No Effect: 1

For Genre: Lofi

Improve: 10

For Genre: Metal

Improve: 58

No Effect: 20

For Genre: Pop

Improve: 75

No Effect: 20

Worsen: 2

For Genre: R&B

Improve: 22

No Effect: 8

For Genre: Rap

Improve: 15

No Effect: 4

Worsen: 1

For Genre: Rock

Improve: 107

No Effect: 36

Worsen: 7

For Genre: Video Game Music

Improve: 20

No Effect: 13

Worsen: 4

12. Find the Top 5 Genres with respect to the most avg hours watched.

## Ans:

## Code:

SELECT `Fav genre`, Avg(`Hours per day`) as AvgHoursPerDay

from `mxmh_survey_results`

group by `Fav genre`

order by AvgHoursPerDay desc

limit 5;

## Output:

![image](https://user-images.githubusercontent.com/72240938/213402283-10e760bf-ba90-4577-83a0-5391876a4fca.png)

13. Find the Bottom 5 Genres with respect to the least avg hours watched.

## Ans:

## Code:

SELECT `Fav genre`, Avg(`Hours per day`) as AvgHoursPerDay

from `mxmh_survey_results`

group by `Fav genre`

order by AvgHoursPerDay asc

limit 5;

## Output:

![image](https://user-images.githubusercontent.com/72240938/213402378-246a879d-d90c-467f-bf27-03c80378cf29.png)


## Key Insights:

• With respect to Age Groups, Under 20 people participated the most and those Above 50 participated the least in the survey. Most people who are Under 20 have the most number of avg hours per day watched and 20–30 has the least number of avg hours per day.

• With respect to Genre, Rock genre viewing people are the most.

And, with respect to avg hours watched, Latin has the most average hours watched whereas the Classical genre average hours watched is the least.

With respect to the Primary Streaming Service count, Spotify's count is the highest whereas Pandora's count is the lowest.
And, with respect to average hours watched with respect to Primary Streaming Services, Spotify has the highest number of average hours watched and Pandora has the least number of average hours watched.

• Streaming Service which is the most widely used among people is Spotify and the least used Streaming Service is Pandora.

• Foreign Language listener count of people is more than non-listeners.

• Music has overall improved the psychological health of people. Rock Genre has improved the most among all genres, whereas Videogame Music Genre has worsened the most among all genres.



















