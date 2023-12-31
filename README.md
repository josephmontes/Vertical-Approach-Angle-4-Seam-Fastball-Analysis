# 0. INTRO
- Thank you for checking out my first research project using R
  - All the code I wrote to create this can be viewed in the 'Research_Code.R' file
- The Statcast data used for this research is pitch by pitch data from 2019 to (August 10) 2023 scraped using the baseballr package
  - Baseball Savant was also used as a reference for this research
- I have a strong interest in analyzing trends that are happening within the pitcher batter matchup across MLB
  - Some examples of its evolution are batters striking out more but hitting more homeruns, and pitchers not throwing as many innings but throwing higher quality 'stuff'
- In this project, I analyze 4-Seam Fastballs with a focus on their Vertical Approach Angle (VAA)
  - Vertical Approach Angle (VAA) describes the angle that the ball crosses the plate at
    - If VAA = 0°, then the ball is traveling completely parallel to the ground
    - The VAA for 4-Seam Fastballs is typically between -5.6° to -4.2°
  - The idea behind analyzing the VAA for 4-Seamers is that a 'flatter' (closer to 0°) 4-Seam Fastball should be harder to hit because it approaches the batter at an angle that is not on the same plane as their swing, since swings naturally have an **uppercut angle**
    - Also, batters are likely to be deceived if a 4-Seam Fastball has a flatter VAA than average because the batter will anticipate the pitch to come in at an angle that it does not 
- I identify and analyze pitchers who exhibit the 'optimal' VAA characteristics on their 4-Seam Fastball vs. pitchers who do not
- Plus, I identify and analyze batters, more specifically, the type of batters who *have* and *have not* had success vs. the 'optimal' VAA 4-Seam Fastball
- I followed this formula posted on FanGraphs by Alex Chamberlain for my calculation of VAA:

   ![Screenshot 2023-08-30 141448](https://github.com/josephmontes/VAA/assets/125607783/bbb0ffe8-bc70-42df-a561-49f7aba21a57)
   

# 1. IDENTIFYING THE VAA TREND

#### Despite 4-Seam Fastball usage slightly declining in recent years, it is still the most frequently thrown pitch at 32.6%
  - In 2023, it is being thrown at least twice as much as every pitch besides Sliders (17.8%)

![Screenshot 2023-08-29 225522](https://github.com/josephmontes/VAA/assets/125607783/0aa01456-74b1-4f70-b819-32711fa79a47)


#### Likely contributing to 4-Seam Fastball usage decline is the fact that it is hit at an above average rate according to wOBA
   - Average wOBA is 0.320
   - Average wOBA against 4-Seam Fastballs is .352 as of August 10, 2023
     
![Screenshot 2023-09-14 000958](https://github.com/josephmontes/VAA/assets/125607783/8e810026-affe-450a-96f1-d8155e44ef08)


#### However, when 4-Seamers are broken down by their VAA, we see that wOBA (and xwOBA) decrease as a 4-Seam Fastball's VAA approaches 0°

![Screenshot 2023-08-29 231032](https://github.com/josephmontes/VAA/assets/125607783/af21f4d3-00f8-4194-bd6b-6d7a5632bd99)
  
- wOBA and xwOBA are plate appearance rated, so I wanted to look at a couple of swing rated stats
   - **Whiff%** increases as a 4-Seam Fastball's VAA approaches 0°
     - League Average Whiff% is 21%
   - **Barrel% (per swing)** also decreases as VAA approaches 0°
     - League Average Barrel% per swing is 3.5%
  
![Screenshot 2023-09-13 171651](https://github.com/josephmontes/VAA/assets/125607783/7a500deb-acaf-40fa-b52a-6eb8b92b005e)
![Screenshot 2023-09-11 150924](https://github.com/josephmontes/VAA/assets/125607783/98cd5a83-ffaf-4051-ba9c-7077c0e7b8c9)


####  In each of these graphs, the VAA value where wOBA, xwOBA, Whiff%, Barrel% per swing cross their league average thresholds is consistently when VAA = -4.5° 
   - The table below shows the wOBA against 4-Seam Fastballs that have a VAA >= -4.5° (vaa_fb = 1) vs. VAA <= -4.5° (vaa_fb = 0)
        - 'freq' column shows the frequency as a percentage
     
![Screenshot 2023-09-13 134910](https://github.com/josephmontes/VAA/assets/125607783/417f5ae5-2239-49ce-8855-7353b48d7822)

#### The next table shows that the league average VAA for 4-Seam Fastball is steadily trending towards -4.5° 
 - The column 'inches' refers to *inches* from the top of the zone (I prefer using this measurement over using the plate_z coordinate because I do not want noise from the batter's height to affect the analysis)
     - 4-Seam Fastballs are being thrown closer to the top of the zone each season

    ![Screenshot 2023-08-29 235028](https://github.com/josephmontes/VAA/assets/125607783/7c540b71-5599-49a5-9fae-60d39f39eafc)

    - There's a notable jump in 'Avg_VAA' and 'inches' after the 2021 season
   
#### But are all 4-Seam Fastballs thrown with a VAA >= -4.5° equally effective? 
   - Following the trend in the table above that shows 4-Seam Fastballs being thrown closer to the top of the zone, I investigated how wOBA changes as pitch location changes for 4-Seam Fastballs with a VAA >= -4.5° 
   - The midpoint wOBA value for this graph is set at .303 to represent the average wOBA on 4-Seam Fastballs with -4.5° VAA (shown in 2nd table up)
   - Minimum 100 pitch frequency

![Screenshot 2023-09-05 165702](https://github.com/josephmontes/VAA/assets/125607783/bb108dac-bc73-4f47-9399-82613ec9608c)

- The blue colored dots being in between -5 & 5.5 on the y-axis indicates that the 5 inches below and 5.5 inches above the top of the strikezone is the most effective location for 4-Seam Fastballs with a VAA >=-4.5°  

#### The following table supports this by showing the wOBA value for -4.5° VAA 4-Seam Fastballs inside of that -5 to 5.5 inch zone (vaa_fb = 1) vs. -4.5° VAA 4-Seam Fastballs outside of that zone (vaa_fb = 0)

![Screenshot 2023-09-13 141225](https://github.com/josephmontes/VAA/assets/125607783/60f6d66e-b97f-400f-b82e-105548a95345)

- The 66 point difference in wOBA highlights that all 4-Seam Fastballs with a VAA >= -4.5° are **not** equally effective, and it emphasizes the importance of getting it in the top parts of the zone
- Going forward, the terms 'optimal' and 'good' type of 4-Seam Fastballs will be used to refer to the 4-Seam Fastballs with a VAA >=--4.5° **and** located in that -5 to 5.5 inch area at the top of their strikezone

# 2. PITCHERS

#### So who is throwing this pitch?
- Below is the list of pitching seasons where the pitcher threw this 'optimal' 4-Seam Fastball at least **30%** of the time that they threw a 4-Seam Fastball (represented in the column of the table below titled 'Rate'), followed by the list of pitching seasons where they threw it less than **8%** of the time
   - Both lists of pitching seasons will be divided into 2 lists: those who threw it effectively, and those who threw it ineffectively
   - 'Effective' is defined as below the average 4-Seam Fastball wOBA value: 0.350
   - At least 600 4-Seam Fastballs thrown to stabilize the wOBA value
   - 'Next wOBA' column contains the value representing that pitcher's wOBA against their 4-Seam Fastball in the next season
      - '*' means that the value is from the next season that they threw a qualified amount of 4-Seam Fastballs
      - '**' means that they have not thrown another qualified amount of 4-Seam Fastballs, so the value is just from the next chronological season
    
## List of pitchers with 30%+ 'Rate' & wOBA < 0.350 on their 4-Seam Fastball

| Season | Player Name     | wOBA  | Next wOBA | Rate |
|-----------|-----------------|-------|-----------|------|
| 2019      | Emilio Pagán    | 0.247 | 0.360*        | 33%  |
| 2019      | Trevor May      | 0.248 | 0.301*        | 31%  |
| 2019      | Gerrit Cole     | 0.264 | 0.319         | 34%  |
| 2019      | Jacob deGrom    | 0.302 | 0.224*        | 33%  |
| 2019      | John Brebbia    | 0.314 | 0.307**       | 31%  |
| 2019      | Sean Doolittle  | 0.329 | 0.352*        | 33%  |
| 2019      | Edwin Díaz      | 0.332 | 0.327*        | 31%  |
| 2020      | Gerrit Cole (2x)| 0.319 | 0.298         | 37%  |
| 2021      | Jacob deGrom (2x)| 0.224 | 0.283**      | 32%  |
| 2021      | Liam Hendriks   | 0.265 | 0.310**       | 35%  |
| 2021      | Ian Kennedy     | 0.279 | 0.339         | 35%  |
| 2021      | Johnny Cueto    | 0.282 | 0.227**       | 33%  |
| 2021      | Michael Kopech  | 0.297 | 0.300         | 34%  |
| 2021      | Gerrit Cole (3x)| 0.298 | 0.308         | 33%  |
| 2021      | Trevor May (2x) | 0.298 | 0.417**       | 34%  |
| 2021      | Jake Odorizzi   | 0.314 | 0.343         | 31%  |
| 2021      | Anthony DeSclafani| 0.325 | 0.761**     | 31%  |
| 2021      | Bailey Ober     | 0.326 | 0.321*        | 34%  |
| 2021      | Edwin Díaz (2x)  | 0.327 | 0.353**      | 31%  |
| 2021      | Carlos Hernández | 0.327 | 0.447**      | 30%  |
| 2021      | Tyler Mahle     | 0.336 | 0.292         | 32%  |
| 2021      | Cristian Javier | 0.349 | 0.280         | 36%  |
| 2022      | Alexis Díaz     | 0.244 | 0.340**       | 38%  |
| 2022      | Alex Vesia      | 0.257 | 0.350**       | 34%  |
| 2022      | George Kirby    | 0.258 | 0.282         | 30%  |
| 2022      | Joe Ryan        | 0.267 | 0.306         | 39%  |
| 2022      | Aaron Nola      | 0.269 | 0.293**       | 30%  |
| 2022      | Cristian Javier (2x)| 0.280 | 0.327     | 37%  |
| 2022      | José Quijada    | 0.282 | 0.278**       | 36%  |
| 2022      | Brandon Woodruff| 0.286 | 0.352**       | 31%  |
| 2022      | Freddy Peralta  | 0.290 | 0.318         | 31%  |
| 2022      | Tyler Mahle (2x)| 0.292 | 0.324**       | 36%  |
| 2022      | Michael Kopech (2x)| 0.300 | 0.377      | 32%  |
| 2022      | Phil Bickford   | 0.329 | 0.288         | 32%  |
| 2023      | Luis Castillo   | 0.270 | NA            | 38%  |
| 2023      | George Kirby (2x)| 0.282 | NA           | 34%  |
| 2023      | Zack Wheeler    | 0.282 | NA            | 33%  |
| 2023      | Phil Bickford (2x) | 0.288 | NA         | 38%  |
| 2023      | Joe Ryan (2x)   | 0.306 | NA            | 41%  |
| 2023      | Pablo López     | 0.306 | NA            | 32%  |
| 2023      | Freddy Peralta (2x)| 0.318 | NA         | 34%  |
| 2023      | Andrew Heaney   | 0.322 | NA            | 39%  |
| 2023      | Bryce Miller    | 0.323 | NA            | 33%  |
| 2023      | Cristian Javier (3x) | 0.327 | NA       | 37%  |
| 2023      | Bailey Ober (2x)| 0.327 | NA            | 36%  |
| 2023      | Max Scherzer    | 0.337 | NA            | 30%  |
| 2023      | Yusei Kikuchi   | 0.346 | NA            | 30%  |

### NOTES
- 32 pitching seasons
  - 18 pitchers
    - Of the 18 pitchers, only 3! pitchers ended up throwing their 4-Seam Fastball ineffectively in a future qualifying season:
      - Emilio Pagán, Sean Doolittle, Michael Kopech

  
## PLAYER SPOTLIGHT
### Emilio Pagán
  - In 2019, the wOBA against his 4-Seam Fastball was 0.247
  - In 2021, the next season that he threw his 4-Seam Fastball a qualifying amount of times, it was ineffective but the wOBA value against it was only a few points above the .350 average wOBA, so it's not like it was getting crushed
    - In this 'ineffective' 2021 season, his 'good' 4-Seam Fastball 'rate' dropped to 27% from 33% in 2019 

    #### 2021 
    ![Screenshot 2023-09-11 195336](https://github.com/josephmontes/VAA/assets/125607783/a656c9f9-cd8b-43f2-8c83-d4900494d23b)
      
- What's more interesting is that, although he has not thrown a qualified amount of 4-Seam Fastballs since 2021, in 2022 and 2023 his 'good' 4-Seam Fastball 'Rate' has gone back over 30% **and** the wOBA value against his 4-Seam Fastball has progressed back to being well below the league average .350 wOBA value
  - **IMPORTANT:** The column 'w_woba' stands for 'weighted wOBA', so when the 2 rows in that column are added together it equals the overall wOBA against their 4-Seam Fastball
    - 2022: *0.319*, 2023: *0.298*
    #### 2022
    ![Screenshot 2023-09-11 195308](https://github.com/josephmontes/VAA/assets/125607783/ba485bb2-a786-4f30-b165-0e1c10222713)
    #### 2023
    ![Screenshot 2023-09-11 195320](https://github.com/josephmontes/VAA/assets/125607783/d5abbfba-005b-4fdb-964b-3c0e7f3b2937)

        
### Sean Doolittle
  - In 2019, the wOBA against his 4-Seam Fastball was 0.329
  - In 2021, the next season that he threw his 4-Seam Fastball a qualifying amount of time, it was ineffective but the wOBA value against it was also only a few points above the league average .350 wOBA value
    - In that 2021 season, his 'good' 4-Seamer rate also dropped over 6% like Emilio Pagán's
      #### 2021
      ![Screenshot 2023-09-11 200535](https://github.com/josephmontes/VAA/assets/125607783/d4ce8ae1-6732-4ce6-8b1e-d6c4e767388c)

- What I want to highlight here is that his 4-Seam Fastballs that were categorized as 'good', according to VAA and location, were still extremely effective, while the ineffective ones were hit at 0.377 wOBA
  - It is safe to assume that if Sean Doolittle, and Emilio Pagán, were able to maintain their 30%+ 'Rate', then their 4-Seam Fastball would not have ended up regressing in this 2021 season
- Sean Doolittle has not thrown more than 50 4-Seam Fastballs in a season since this 2021 season
   
       
## List of pitchers with 30%+ 'Rate' & wOBA > 0.350 on their 4-Seam Fastball
- '***' next to Hunter Greene's 'Next wOBA' value is because there is something wrong with the 2023 data I have scraped for him

| Season    | Player Name       | wOBA  | Next wOBA       | Rate |
|-----------|-------------------|-------|-----------------|------|
| 2019      | Elieser Hernández | 0.419 | 0.361**         | 30%  |
| 2019      | Trevor Bauer      | 0.418 | 0.301*          | 32%  |
| 2019      | Freddy Peralta    | 0.373 | 0.290*          | 34%  |
| 2021      | Josiah Gray       | 0.419 | 0.489           | 31%  |
| 2021      | Andrew Heaney     | 0.371 | 0.352           | 33%  |
| 2021      | Luis Castillo     | 0.361 | 0.208           | 34%  |
| 2022      | Josiah Gray (2x)  | 0.489 | 0.437**         | 32%  |
| 2022      | JP Sears          | 0.378 | 0.367           | 30%  |
| 2022      | Hunter Greene     | 0.369 | 0.371***        | 30%  |
| 2022      | Andrew Heaney (2x)| 0.352 | 0.322           | 38%  |
| 2023      | Grayson Rodriguez | 0.434 | NA              | 33%  |
| 2023      | Logan Allen       | 0.383 | NA              | 34%  |
| 2023      | Michael Kopech    | 0.377 | NA              | 31%  |
| 2023      | JP Sears (2x)     | 0.367 | NA              | 33%  |

### NOTES
- 14 total pitching seasons
   - 11 pitchers
     - 1 pitcher never got another qualified season (Elieser Hernandez)
     - 4 pitchers have successfully progressed by having a below average wOBA value against their 4-Seam Fastball in a following qualified season and not going back above:
       - Trevor Bauer, Freddy Peralta, Andrew Heaney, Luis Castillo
     - 3 have not yet gotten below the average wOBA value against their 4-Seam Fastball in a qualifying season:
       - Josiah Gray, Hunter Greene, JP Sears
     - The remaining 3 pitchers are from 2023:
       - Grayson Rodriguez, Logan Allen, Michael Kopech
    - These 6 pitchers (Gray, Greene, Sears, Rodriguez, Allen, Kopech) can be viewed at as progression candidates based on the fact that similar 4-Seam Fastballs typically have much better success, and based on the 4 pitchers from this short list that have progressed
    - Also, these 6 pitchers are a group of very young pitchers, which signals to me that there is more room than average for them to benefit and to learn from experience using their pitch arsenal against MLB batters

## PLAYER SPOTLIGHT
### Josiah Gray
  - His 4-Seam Fastball was hit alarmingly hard in his first 2 seasons despite exhibiting 'good' 4-Seam Fastball VAA at a 'rate' higher than most pitchers
    - wOBA against his 4-Seam Fastball: 2021=*0.419*, 2022=*0.489*
    #### 2021
    ![Screenshot 2023-09-11 202536](https://github.com/josephmontes/VAA/assets/125607783/22b47900-db8b-49af-ae2a-26ec2fd2f6e8)

    #### 2022
    ![Screenshot 2023-09-11 202547](https://github.com/josephmontes/VAA/assets/125607783/ab8361f3-778e-4d4d-ae20-a4dba58ebe69)

- With a young pitcher especially, as long as he keeps his 'rate' high, you should expect the wOBA against his 4-Seam Fastball to improve
  - But his 4-Seam Fastball usage has gone from 51.6% in 2021 to 18.7%! usage in 2023
  - Primarily throws a Slider now
- Additionally, in 2023, his 'good' 4-Seam Fastball 'rate' has dropped over 6% and is not above that 30% threshold like it used to be
  #### 2023
  ![Screenshot 2023-09-11 202403](https://github.com/josephmontes/VAA/assets/125607783/6c15fc7b-9245-4142-9584-91c9a61bf4b3)


### Michael Kopech
  - I want to highlight the 4-Seam Fastball height for Michael Kopech in 2021 and 2022 when it was effective vs. 2023 when it has been ineffective
    #### 2021
    ![Screenshot 2023-09-11 201243](https://github.com/josephmontes/VAA/assets/125607783/5dfe20d8-cab9-462d-8ee5-9f9b2ed70787)
    #### 2022
    ![Screenshot 2023-09-11 201256](https://github.com/josephmontes/VAA/assets/125607783/12e9cc63-1375-421f-b08e-6b82f28a2da2)
    - When he throws the 'good' 4-Seam Fastball ('g_fb' = 1), the column 'inches' shows that he is locating the pitch just fractions of an inch below the top of the zone
    #### 2023
    ![Screenshot 2023-09-11 201310](https://github.com/josephmontes/VAA/assets/125607783/70802e8a-de38-4275-a2d4-d6e9c1cee737)
    - In 2023, when g_fb = 1, the 'inches' column reveals that, on average, he is missing above the zone with this pitch now
    - This insight corresponds with how his overall BB% is trending: 2021=*8.4%*, 2022=*11.5%*, 2023=*15.7%*
- Additionally, he consistently throws his 4-Seam Fastball over 60% of the time each season, so it may be the case that hitters are sitting on it and he needs to incorporate another reliable pitch to keep hitters honest
       
### Spencer Strider
  - If you read this list like me and were also wondering where Spencer Strider is...
  - He throws the 'good' 4-Seam Fastball 28% of the time with it being extremely effective
    #### 2022
    ![Screenshot 2023-09-11 203427](https://github.com/josephmontes/VAA/assets/125607783/70f03a98-2fc3-4327-a6da-c73333a1011c)
    #### 2023
    ![Screenshot 2023-09-11 203435](https://github.com/josephmontes/VAA/assets/125607783/c9e327da-3b1e-498f-b0b3-3456a5497587)

- His 4-Seam Fastball's regression in the 2023 season is coming from the times that he does *not* throw this 'good' type of 4-Seam Fastball, since the wOBA value when g_fb = 0 has ballooned from .290 in 2022 to .404 in 2023
  - He could be suffering from what was mentioned earlier with Michael Kopech where he throws his 4-Seam Fastball at such a high rate (60% of the time), batters are sitting on it and able to punish the 'bad' ones

### So who is NOT throwing this pitch?

## List of pitching seasons with a 8% 'Rate' or lower & wOBA > 0.350 on their 4-Seam Fastball

| Season    |  Player Name           | wOBA  | Next wOBA | Rate |
|-----------|-----------------------|-------|-----------------|------|
| 2019      | Dereck Rodríguez      | 0.462 | 0.688**         | 7%   |
| 2019      | Taylor Clarke         | 0.439 | 0.356**         | 7%   |
| 2019      | Tommy Milone          | 0.431 | 0.396**         | 5%   |
| 2019      | Antonio Senzatela     | 0.444 | 0.374           | 4%   |
| 2019      | Peter Lambert         | 0.420 | 0.700**         | 1%   |
| 2019      | Cole Hamels           | 0.415 | NA              | 6%   |
| 2019      | Michael Wacha         | 0.415 | 0.318*          | 2%   |
| 2019      | Joe Musgrove          | 0.407 | 0.422*          | 0%   |
| 2019      | Andrew Cashner        | 0.384 | NA              | 4%   |
| 2019      | Kyle Freeland         | 0.382 | 0.366*          | 7%   |
| 2019      | Jason Vargas          | 0.380 | NA              | 7%   |
| 2019      | Max Fried             | 0.374 | 0.318*          | 5%   |
| 2019      | Patrick Corbin        | 0.369 | 0.402*          | 6%   |
| 2019      | Shaun Anderson        | 0.366 | 0.506**         | 3%   |
| 2019      | Wilmer Font           | 0.366 | 0.479**         | 7%   |
| 2019      | Germán Márquez        | 0.364 | 0.364*          | 6%   |
| 2019      | Justin Verlander      | 0.357 | 0.247*          | 3%   |
| 2019      | Aníbal Sánchez        | 0.354 | 0.544**         | 7%   |
| 2019      | Tyler Skaggs          | 0.353 | NA              | 1%   |
| 2019      | Sean Newcomb          | 0.350 | 0.533**         | 4%   |
| 2020      | Antonio Senzatela (2x)| 0.374 | 0.327           | 7%   |
| 2021      | Joe Musgrove (2x)     | 0.422 | 0.318           | 2%   |
| 2021      | Jon Lester            | 0.420 | NA              | 5%   |
| 2021      | Austin Gomber         | 0.417 | 0.471           | 1%   |
| 2021      | Matt Harvey           | 0.407 | NA              | 6%   |
| 2021      | Brad Keller           | 0.405 | 0.316           | 7%   |
| 2021      | Matt Moore            | 0.403 | 0.310**         | 7%   |
| 2021      | Patrick Corbin (2x)   | 0.402 | 0.418**         | 2%   |
| 2021      | Chi Chi González      | 0.401 | 0.359**         | 2%   |
| 2021      | Garret Richards       | 0.389 | 0.484**         | 6%   |
| 2021      | Ross Stripling        | 0.383 | 0.318           | 1%   |
| 2021      | Kwang Hyun Kim        | 0.371 | NA              | 7%   |
| 2021      | Huascar Ynoa          | 0.368 | 0.547**         | 7%   |
| 2021      | Germán Márquez (2x)   | 0.365 | 0.413           | 5%   |
| 2021      | Michael Pineda        | 0.361 | 0.448**         | 1%   |
| 2021      | Hyun Jin Ryu          | 0.356 | 0.416**         | 5%   |
| 2021      | Chris Flexen          | 0.346 | 0.398           | 1%   |
| 2022      | Austin Gomber (2x)    | 0.471 | 0.452           | 1%   |
| 2022      | Antonio Senzatela (3x)| 0.444 | 0.515**         | 4%   |
| 2022      | Brent Suter           | 0.402 | 0.314**         | 1%   |
| 2022      | Chris Flexen (2x)     | 0.398 | 0.521**         | 0%   |
| 2022      | Justin Steele         | 0.393 | 0.328           | 5%   |
| 2022      | Michael Wacha (2x)    | 0.380 | 0.264**         | 5%   |
| 2022      | Tyler Wells           | 0.355 | 0.315           | 6%   |
| 2022      | Zack Greinke          | 0.351 | 0.381**         | 2%   |
| 2023      | Austin Gomber (3x)    | 0.452 | NA              | 1%   |
| 2023      | Blake Snell           | 0.375 | NA              | 6%   |

### NOTES
- 47 pitching seasons
  - 38 pitchers
  - Of the 38 pitchers, only 9! have had qualifying seasons afterwards where they progressed and threw their 4-Seam effectively:
    - Michael Wacha, Max Fried, Justin Verlander, Antonio Senzatela, Joe Musgrove, Brad Keller, Ross Stripling, Justin Steele, Tyler Wells
  - Of these 9 pitchers, 2 ended up regressing in a qualifying season after:
    - Antonio Senzatela and Michael Wacha
  - Of the remaining 7 pitchers, 2 do not qualify, but have shown notable regression in their 200+ 4-Seamers during the 2023 season:
    - Brad Keller and Ross Stripling
  - The remaining 5 pitchers:
    - Max Fried, Justin Verlander, Joe Musgrove, Justin Steele, Tyler Wells


## PLAYER SPOTLIGHT
### Blake Snell
  - Despite his 4-Seam Fastball getting hit hard in this 2023 season (.375 wOBA against), it has been an effective pitch for him in 2019, 2021 and 2022 seasons
  - It usually works in the middle parts of the plate because of the deception Snell creates by tunneling it with his higher quality breaking stuff and his Changeup 
    - In 2021 and 2022 he started throwing his 4-Seam Fastball more towards his arm side of the plate, likely because he started incorporating his Changeup less and his Slider more in 2021 and 2022 
    - The table below shows the plate coordinates of his 4-Seam Fastball by season
      - plate_x = 0 means it is in the middle, plate_x > 0 means that it is to the right (catchers perspective) and plate_x < 0 means that it is to the left (inside to RHB)
        
        ![Screenshot 2023-09-13 151308](https://github.com/josephmontes/VAA/assets/125607783/0a621958-e72b-4395-b19c-4f4f2d2fb9ab)
    
      - Note that in 2019 it was working in the top-middle part of the zone, in 2021 and 2022 it was working lower in the zone on the arm side of the plate, and in 2023 he is throwing it more middle-middle
        - In 2023, he is not throwing his Slider at the same rate that he was in 2021 and 2022 (24% -> 14%) and he is throwing his Curveball more (14% -> 19%) and Changeup more (7% -> 20%)
        - This could help explain the notable change in 4-Seam Fastball location from year to year, since he gets his 4-Seam Fastball to work by playing off of these pitches

  - In 2023, the usage rate of his 4-Seam Fastball is under 50% for the first time since his Cy Young season in 2018 (according to Baseball Savant)
    - Batters currently have a 0.375 wOBA against his 4-Seam Fastball with a 0.409 xwOBA
      - These are the highest marks against his 4-Seam Fastball in his career
    - His Curveball, Slider and Changeup are legitimate plus pitches, and throwing them more has worked really well for him in 2023, despite not having an overly effective 4-Seam Fastball, as he is currently an NL Cy Young front-runner
   

  

### Justin Steele
  - In 2022, his 4-Seam Fastball was not an effective pitch for him (.393 wOBA against)
  - He primarily throws 2 pitches, with his Slider being his better pitch
    - He uses his Slider over 30% of the time
    - This is a theme with a lot of guys who end up finding success with this less 'optimal' 4-Seam Fastball, they get the most out of it by using it to complement their arsenal rather than leading their arsenal the way the pitchers in the 30%+ lists use it
  - One big difference in his 4-Seam Fastball from 2022 to 2023 (when it has been very effective) is the location of it
    - In 2022 you can see he was leaving it more in the middle, but in 2023 it is closer to the edge of the zone
    #### 2022
    ![Screenshot 2023-09-12 014743](https://github.com/josephmontes/VAA/assets/125607783/03404ebc-e2d7-40a8-8ee4-116733de8695)
    #### 2023
    ![Screenshot 2023-09-12 014721](https://github.com/josephmontes/VAA/assets/125607783/4e782343-cb7e-4e95-b0f3-a2226010f435)

  - The following table shows this numerically in the 'plate_x' column
    - plate_x = 0 would be middle, so plate_x = -2.8 in 2023 shows that it is typically in that left third of the plate
      
    ![Screenshot 2023-09-13 151643](https://github.com/josephmontes/VAA/assets/125607783/ef5c0919-f9a9-448b-b778-bac91083f18f)

    - 'in_pct' value represents the frequency his 4-Seam Fastball was located in that left inner third of the plate as a percentage
    - Note how the vertical pitch location, 'inches', is basically the same
  - Justin Steele has found an effective location for his 4-Seam Fastball to play off the way he throws his Slider, which has translated into improved wOBA against his 4-Seam Fastball (.328) and Cy Young candidacy in 2023, 

      
## List of pitching seasons with a 8% 'Rate' or lower & wOBA < 0.350 on their 4-Seam Fastball

| Season    | Player Name             | wOBA  | Next wOBA  |  Rate |
|-----------|-------------------------|-------|-----------------|------|
| 2019      | Giovanny Gallegos       | 0.271 | 0.262*          | 4%   |
| 2019      | Lucas Giolito           | 0.291 | 0.342*          | 7%   |
| 2019      | Daniel Ponce de Leon    | 0.295 | 0.366**         | 3%   |
| 2019      | Zack Greinke            | 0.303 | 0.346*          | 1%   |
| 2019      | Blake Snell             | 0.319 | 0.327*          | 6%   |
| 2019      | Javy Guerra             | 0.324 | 0.321**         | 3%   |
| 2019      | Michael Feliz           | 0.330 | 0.678**         | 7%   |
| 2019      | Clayton Kershaw         | 0.332 | 0.355*          | 7%   |
| 2019      | Cole Irvin              | 0.333 | 0.330*          | 5%   |
| 2019      | John Means              | 0.335 | 0.348*          | 3%   |
| 2019      | Miles Mikolas           | 0.345 | 0.282*          | 1%   |
| 2019      | Michael Pineda          | 0.348 | 0.365*          | 0%   |
| 2021      | Giovanny Gallegos (2x)  | 0.262 | 0.296**         | 6%   |
| 2021      | Carlos Rodón            | 0.280 | 0.290           | 7%   |
| 2021      | Max Fried               | 0.321 | 0.316           | 7%   |
| 2021      | Brent Suter             | 0.325 | 0.405           | 0%   |
| 2021      | Antonio Senzatela       | 0.327 | 0.444           | 5%   |
| 2021      | Blake Snell (2x)        | 0.327 | 0.352           | 7%   |
| 2021      | Cole Irvin (2x)         | 0.333 | 0.289           | 5%   |
| 2021      | Chris Flexen            | 0.346 | 0.398           | 1%   |
| 2021      | Zack Greinke (2x)       | 0.346 | 0.376           | 0%   |
| 2022      | Justin Verlander        | 0.247 | 0.330           | 2%   |
| 2022      | Joe Musgrove            | 0.280 | 0.265**         | 3%   |
| 2022      | Miles Mikolas (2x)      | 0.281 | 0.321**         | 1%   |
| 2022      | Brad Keller             | 0.316 | 0.468**         | 4%   |
| 2022      | Félix Bautista          | 0.318 | 0.269           | 6%   |
| 2022      | Ross Stripling          | 0.318 | 0.423**         | 0%   |
| 2022      | Andre Pallante          | 0.328 | 0.324**         | 5%   |
| 2022      | Spenser Watkins         | 0.349 | NA              | 5%   |
| 2023      | Tyler Wells             | 0.315 | NA              | 2%   |
| 2023      | Justin Steele           | 0.328 | NA              | 2%   |
| 2023      | Justin Verlander (2x)   | 0.330 | NA              | 1%   |

### NOTES
- 32 pitching seasons
  - 26 pitchers
  - 8 pitchers have not thrown another qualifying season:
    - Daniel Ponce de Leon, Javy Guerra, Michael Feliz, Joe Musgrove, Brad Keller, Ross Stripling, Andre Pallante, Spenser Watkins
  - Of the remaining 18 pitchers, 8 have regressed to above league average in a following qualified season:
    - Clayton Kershaw, John Means, Michael Pineda, Brent Suter, Antonio Senzatela, Blake Snell, Chris Flexen, Zack Greinke
  - 2 pitchers are from 2023:
    - Tyler Wells, Justin Steele
  - The remaining 8 pitchers have maintained their effectiveness in a qualified season:
    - Giovanny Gallegos, Lucas Giolito, Cole Irvin, Miles Mikolas, Carlos Rodon, Max Fried, Justin Verlander, Félix Bautista
      - Of these remaining 8 pitchers, 4 although unqualified, have shown notable regression on the effectiveness of their 4-Seam Fastballs in 2023:
        - Lucas Giolito, Cole Irvin, Carlos Rodón, Miles Mikolas
    - The remaining 4 pitchers:
      - Giovanny Gallegos, Max Fried, Justin Verlander, Félix Bautista

## PLAYER SPOTLIGHT
### Zack Greinke, Justin Verlander, Clayton Kershaw
  - I am spotlighting these players here because I feel that they represent the most important takeaways from these 8% 'rate' pitchers
    - If you do not have this optimal 4-Seam Fastball, then it will likely require a 'Hall of Fame' level understanding of what you do throw, 'Hall of Fame' level complimentary pitches, or 'Hall of Fame' level command to get by with it for a season
      - And many of the other pitchers on this list have exhibited one of those qualities, at least for a season or two
        - Carlos Rodón took a few years of struggling in the league before he put it together into two very dominant seasons in 2021 and 2022
        - Félix Bautista's Splitter, Joe Musgrove's Slider, Max Fried's Curveball, even Antonio Senzatela's Slider in 2021 and Tyler Wells' Changeup are examples of very high-quality complimentary pitches that elevated the effectiveness of their 4-Seam Fastball for at least a season
        - Justin Steele's improved 4-Seam Fastball location in 2023 from the previous Player Spotlight is an example of high-quality or 'Hall of Fame' level command leading to increased effectiveness
    - It is only a question of how long they are able to sustain that level, which we saw many of the pitchers on this list do fall off of it
  - This list of pitchers reveals those who are utilizing the art/craft of pitching more than just throwing high-quality stuff over the plate, since they do not have that as high a quality of stuff to their 4-Seam Fastball
    - Therefore, these pitchers could be viewed at as having more potential than the pitchers on the 30% lists because **if** you teach someone like Joe Musgrove how to throw the 'optimal' 4-Seam Fastball at a higher 'rate', then he should be more likely to figure out how to use it effectively than if it was taught to someone else, since he has displayed an ability to get the most out of lesser quality stuff

---------------------------------------------------------------------

# 3. Batters 

### The story is different for batters
- Batters will appear against every pitch type which dilutes the necessity to crush this more 'effective' type of 4-Seam Fastball
  - Whereas pitchers that throw 4-Seam Fastballs would benefit from it more since they do not throw every pitch type
- Also, for positive offensive production, it is more important to hit the 'ineffective' type of 4-Seam Fastballs than it is to hit the 'effective' ones since **batters will see the ineffective ones more often**
     - 36% of 4-Seam Fastballs have a VAA >= -4.5° 
     - Of that 36%, 56% are in the most 'optimal' location, which ends up being about **18%** of all 4-Seam Fastballs
- As long as the batter is able to do at least league average damage on the 'suboptimal' type of 4-Seamers, then an ability to hit the more effective version of it would unlock a unique ceiling that a batter can get their offensive production to
  - Also, it is reasonable to anticipate that this trend will continue, presenting a favorable outlook for the batter in future seasons
- However, I am more interested in what *type* of batters are successful vs. this 'good' VAA 4-Seam Fastball moreso than what *specific* batters
- To achieve this, I will **not** look at who has the highest wOBA vs. this type of 4-Seam Fastball because then I will just get a list of the best batters, instead, I will look at who has the biggest *difference* in wOBA between the way they hit 'good' 4-Seamers vs. all other 4-Seam Fastballs
  - Every batters's wOBA vs. 4-Seam Fastballs will be viewed in the same two categories that have been used the whole time:
    
    `1.` VAA >= -4.5° and location within the -5 to 5.5 inches from the top of the strikezone ('g_fb' = 1)
    
    `2.` Any other 4-Seam Fastball ('g_fb' = 0)
    
- The table below shows the league-wide wOBA against this 'good' 4-Seam Fastball ('g_fb' = 1) vs. every other 4-Seam Fastball ('g_fb' = 0)
  
  ![Screenshot 2023-09-11 164418](https://github.com/josephmontes/VAA/assets/125607783/63d1b65a-c3fb-4c6d-964d-54a5640684c8)

  - In this table the difference between the wOBA values is -0.096
    - If the difference between a batter's wOBAs is **> -0.025**, I will say he hits it **better** than average
    - If the difference between a batter's wOBAs is **< -0.150**, I will say he hits it **worse** than average
- 'g_wOBA' is wOBA vs. 'good' 4-Seam Fastball
- 'b_wOBA' is wOBA vs. 'bad' 4-Seam Fastball
- 'd_wOBA' is the difference between 'g_wOBA' and 'b_wOBA'
- minimum 600 4-Seam Fastballs
- Only batting seasons from 2021 and 2022 seasons were used here because: there were a lot more qualifying batting seasons than pitching seasons, 2020 is confusing to evaluate, and this way every batter can be evaluated in the next season too
  
## List of batting seasons where d_wOBA >= -0.025

| Season    | Player Name         | wOBA  | g_wOBA | b_wOBA | d_wOBA |
|-----------|---------------------|-------|--------|--------|--------|
| 2021      | Yoán Moncada        | 0.330 | 0.440  | 0.305  | 0.135  |
| 2021      | Carlos Santana      | 0.359 | 0.464  | 0.329  | 0.135  |
| 2021      | Manny Machado       | 0.385 | 0.487  | 0.355  | 0.132  |
| 2021      | Jose Altuve         | 0.451 | 0.556  | 0.432  | 0.124  |
| 2021      | Jake Cronenworth    | 0.394 | 0.497  | 0.378  | 0.119  |
| 2021      | Yordan Alvarez      | 0.388 | 0.484  | 0.372  | 0.112  |
| 2021      | Yuli Gurriel        | 0.326 | 0.412  | 0.307  | 0.105  |
| 2021      | Marcus Semien       | 0.429 | 0.490  | 0.417  | 0.073  |
| 2021      | Miguel Rojas        | 0.343 | 0.403  | 0.336  | 0.067  |
| 2021      | Max Muncy           | 0.465 | 0.520  | 0.454  | 0.066  |
| 2021      | Will Smith          | 0.445 | 0.495  | 0.437  | 0.058  |
| 2021      | Kyle Schwarber      | 0.399 | 0.442  | 0.387  | 0.055  |
| 2021      | Teoscar Hernández   | 0.423 | 0.463  | 0.416  | 0.047  |
| 2021      | Hunter Renfroe      | 0.411 | 0.436  | 0.404  | 0.032  |
| 2021      | Josh Rojas          | 0.382 | 0.408  | 0.377  | 0.031  |
| 2021      | Bo Bichette         | 0.350 | 0.373  | 0.345  | 0.028  |
| 2021      | Franmil Reyes       | 0.448 | 0.465  | 0.443  | 0.021  |
| 2021      | Brett Gardner       | 0.368 | 0.383  | 0.364  | 0.019  |
| 2021      | Vladimir Guerrero Jr.| 0.520| 0.532  | 0.517  | 0.015  |
| 2021      | Jared Walsh         | 0.361 | 0.372  | 0.358  | 0.014  |
| 2021      | Lourdes Gurriel Jr. | 0.281 | 0.291  | 0.278  | 0.013  |
| 2021      | DJ LeMahieu         | 0.353 | 0.361  | 0.350  | 0.011  |
| 2021      | Jurickson Profar    | 0.344 | 0.348  | 0.343  | 0.005  |
| 2021      | Josh Donaldson      | 0.388 | 0.391  | 0.388  | 0.004  |
| 2021      | Amed Rosario        | 0.391 | 0.387  | 0.392  | -0.005 |
| 2021      | Isiah Kiner-Falefa  | 0.353 | 0.349  | 0.354  | -0.005 |
| 2021      | Xander Bogaerts     | 0.389 | 0.385  | 0.391  | -0.006 |
| 2021      | Jazz Chisholm Jr.   | 0.371 | 0.365  | 0.372  | -0.007 |
| 2021      | Omar Narváez        | 0.403 | 0.395  | 0.405  | -0.010 |
| 2021      | Andrew Benintendi   | 0.348 | 0.339  | 0.350  | -0.011 |
| 2021      | Mike Yastrzemski    | 0.401 | 0.390  | 0.404  | -0.014 |
| 2021      | Cody Bellinger      | 0.267 | 0.255  | 0.271  | -0.016 |
| 2021      | Eric Hosmer         | 0.406 | 0.393  | 0.409  | -0.016 |
| 2021      | David Fletcher      | 0.283 | 0.268  | 0.285  | -0.017 |
| 2021      | Christian Yelich    | 0.366 | 0.352  | 0.371  | -0.018 |
| 2021      | José Ramírez        | 0.435 | 0.418  | 0.440  | -0.022 |
| 2021      | Michael Conforto    | 0.367 | 0.348  | 0.371  | -0.023 |
| 2021      | Aaron Judge         | 0.465 | 0.445  | 0.469  | -0.024 |
| 2021      | Bryan Reynolds      | 0.452 | 0.431  | 0.454  | -0.024 |
| 2022      | Manny Machado (2x)  | 0.311 | 0.456  | 0.270  | 0.186  |
| 2022      | Rafael Devers       | 0.366 | 0.505  | 0.320  | 0.185  |
| 2022      | Mark Canha          | 0.345 | 0.450  | 0.319  | 0.131  |
| 2022      | Carlos Santana (2x) | 0.364 | 0.457  | 0.333  | 0.123  |
| 2022      | Ronald Acuña Jr.    | 0.338 | 0.433  | 0.321  | 0.112  |
| 2022      | Miguel Cabrera      | 0.302 | 0.386  | 0.279  | 0.107  |
| 2022      | Ke'Bryan Hayes      | 0.329 | 0.418  | 0.314  | 0.104  |
| 2022      | Josh Bell           | 0.372 | 0.452  | 0.348  | 0.104  |
| 2022      | Aaron Hicks         | 0.327 | 0.408  | 0.305  | 0.103  |
| 2022      | Owen Miller         | 0.299 | 0.379  | 0.280  | 0.100  |
| 2022      | Brandon Drury       | 0.372 | 0.452  | 0.353  | 0.099  |
| 2022      | Josh Donaldson (2x) | 0.335 | 0.408  | 0.310  | 0.098  |
| 2022      | Trea Turner         | 0.413 | 0.495  | 0.401  | 0.095  |
| 2022      | Kyle Schwarber      | 0.392 | 0.454  | 0.379  | 0.075  |
| 2022      | DJ LeMahieu (2x)    | 0.393 | 0.449  | 0.379  | 0.069  |
| 2022      | Adolis García       | 0.325 | 0.370  | 0.308  | 0.062  |
| 2022      | Myles Straw         | 0.293 | 0.341  | 0.281  | 0.061  |
| 2022      | Max Muncy           | 0.359 | 0.407  | 0.349  | 0.058  |
| 2022      | Sean Murphy         | 0.375 | 0.421  | 0.364  | 0.057  |
| 2022      | Kolten Wong         | 0.427 | 0.473  | 0.417  | 0.057  |
| 2022      | Whit Merrifield     | 0.341 | 0.388  | 0.331  | 0.056  |
| 2022      | Brandon Nimmo       | 0.390 | 0.436  | 0.381  | 0.055  |
| 2022      | J.P. Crawford       | 0.347 | 0.391  | 0.338  | 0.053  |
| 2022      | Jeff McNeil         | 0.357 | 0.392  | 0.346  | 0.046  |
| 2022      | Anthony Santander   | 0.328 | 0.364  | 0.320  | 0.045  |
| 2022      | Rhys Hoskins        | 0.373 | 0.403  | 0.367  | 0.037  |
| 2022      | Dansby Swanson      | 0.383 | 0.409  | 0.378  | 0.031  |
| 2022      | Jorge Polanco       | 0.383 | 0.407  | 0.377  | 0.030  |
| 2022      | Brendan Donovan     | 0.402 | 0.423  | 0.396  | 0.027  |
| 2022      | Seth Brown          | 0.433 | 0.456  | 0.429  | 0.027  |
| 2022      | Riley Greene        | 0.345 | 0.366  | 0.341  | 0.025  |
| 2022      | Adley Rutschman     | 0.421 | 0.433  | 0.418  | 0.015  |
| 2022      | Corey Seager        | 0.426 | 0.433  | 0.424  | 0.009  |
| 2022      | Yoán Moncada (2x)   | 0.278 | 0.284  | 0.277  | 0.007  |
| 2022      | Ty France           | 0.369 | 0.374  | 0.368  | 0.006  |
| 2022      | Matt Chapman        | 0.282 | 0.285  | 0.281  | 0.004  |
| 2022      | Steven Kwan         | 0.389 | 0.392  | 0.389  | 0.004  |
| 2022      | Jose Altuve (2x)    | 0.459 | 0.462  | 0.459  | 0.003  |
| 2022      | Isiah Kiner-Falefa (2x)|0.346| 0.344 | 0.346  | -0.002 |
| 2022      | Bryan Reynolds (2x) | 0.424 | 0.422  | 0.425  | -0.003 |
| 2022      | Juan Soto           | 0.485 | 0.480  | 0.486  | -0.006 |
| 2022      | Josh Rojas (2x)     | 0.331 | 0.323  | 0.333  | -0.010 |
| 2022      | Gio Urshela         | 0.324 | 0.312  | 0.326  | -0.014 |
| 2022      | Eugenio Suárez      | 0.349 | 0.336  | 0.352  | -0.015 |
| 2022      | Lourdes Gurriel Jr. (2x)| 0.370 | 0.354 | 0.374 | -0.019 |
| 2022      | Luis Arraez         | 0.444 | 0.429  | 0.448  | -0.019 |
| 2022      | Josh Naylor         | 0.344 | 0.327  | 0.348  | -0.021 |
| 2022      | Randy Arozarena     | 0.415 | 0.398  | 0.420  | -0.022 |
| 2022      | Matt Olson          | 0.403 | 0.386  | 0.408  | -0.022 |
| 2022      | Elvis Andrus        | 0.395 | 0.376  | 0.399  | -0.023 |
| 2022      | Justin Turner       | 0.338 | 0.320  | 0.345  | -0.025 |

### NOTES
- First thing I noticed is that there is a lot of the quintessential contact hitters on this list
  - Luis Arraez, Steven Kwan, DJ LeMahieu, Jake Cronenworth, Andrew Benintendi, Gio Urshela, Isiah Kiner-Falefa, Jeff McNeil, Whit Merrifield, David Fletcher, Myles Straw, Josh Rojas, Elvis Andrus, Bryan Reynolds, Brandon Nimmo, Trea Turner, Brendan Donovan, Brett Gardner, Dansby Swanson, Jose Altuve, Ty France, and more
  - So I investigated their K% and found that the K% for this list of hitters is 19.6%!
    - League average K% is 22.1% 
    - To further support this, the Whiff% for this list of hitters is 21.4%
      - League average Whiff% is 24.8%
- Every hitter on this list has a positive outlook in regard to their production against 4-Seam Fastballs because of their displayed ability to hit this more difficult type of 4-Seam Fastball that is trending
  - Since it is statistically easier to hit the other type of 4-Seam Fastball that these hitters are not hitting as well, it is a safer bet to make on these batters that they will be above average 4-Seam Fastball hitters going forward


## PLAYER SPOTLIGHT
### Max Muncy & Kyle Schwarber
  - These 2 names really stuck out to me because their batting profiles contrast with most of the other hitters on this list
  - However, the ability to hit this more effective type of 4-Seam Fastball may be what separates them from other 'Three True Outcome' kind of guys like Joey Gallo & Miguel Sanó (who we actually see on the following list below)
    - Note that when 'g_fb'=1, both of their wOBA values are decreasing each season **and** the pitch's average height above the zone ('inches') is doubling each season
      - Schwarber: wOBA: *.474 -> .409 -> .311* inches: *.106 -> .369 -> .674*
      - Muncy: wOBA: *.473 -> .394 -> .242* inches: *.241 -> .526 -> 1.48*
    - This indicates that pitchers are throwing this pitch a little bit higher in the zone each year vs. these batters and essentially baiting them into swinging at an effective pitch that they would normally spoil
    #### Kyler Schwarber 2021
    ![Screenshot 2023-09-12 183324](https://github.com/josephmontes/VAA/assets/125607783/f9015b27-a522-4bdf-8aca-2278eac0af77)
    #### Kyler Schwarber 2022
    ![Screenshot 2023-09-12 183329](https://github.com/josephmontes/VAA/assets/125607783/709500b9-3986-4311-9005-3d4301b2238a)
    #### Kyler Schwarber 2023
    ![Screenshot 2023-09-12 183336](https://github.com/josephmontes/VAA/assets/125607783/212f9d60-ac85-42e5-a226-25b5d7dcff9f)
    #### Max Muncy 2021
    ![Screenshot 2023-09-12 183343](https://github.com/josephmontes/VAA/assets/125607783/9b0a3893-1b6a-48ce-9594-68a033dcdbbe)
    #### Max Muncy 2022
    ![Screenshot 2023-09-12 183353](https://github.com/josephmontes/VAA/assets/125607783/5e08b938-37fa-49d5-b32e-e7cd62292e8a)
    #### Max Muncy 2023
    ![Screenshot 2023-09-12 183419](https://github.com/josephmontes/VAA/assets/125607783/0178dd97-3d0d-4265-824c-f003c079c59f)

    - In 2023, both of these players have become less effective against the 'good' 4-Seam Fastball, and this decline in their performance is likely due to pitchers increasingly throwing the 4-Seam Fastball higher above the zone, which results in the players making lower-quality contact.

## List of batting seasons where d_wOBA <= -0.150

| Season    |   Player Name     | wOBA  | g_wOBA | b_wOBA | d_wOBA |
|-----------|-------------------|-------|--------|--------|--------|
| 2021      | Nick Castellanos  | 0.488 | 0.213  | 0.544  | -0.331  |
| 2021      | Tyler Naquin      | 0.369 | 0.109  | 0.436  | -0.328  |
| 2021      | Tim Anderson      | 0.362 | 0.107  | 0.414  | -0.307  |
| 2021      | Bobby Dalbec      | 0.324 | 0.091  | 0.383  | -0.292  |
| 2021      | Brandon Belt      | 0.374 | 0.137  | 0.424  | -0.287  |
| 2021      | Tyler O'Neill     | 0.400 | 0.166  | 0.449  | -0.283  |
| 2021      | J.T. Realmuto     | 0.375 | 0.143  | 0.426  | -0.282  |
| 2021      | Eugenio Suárez    | 0.380 | 0.170  | 0.434  | -0.264  |
| 2021      | Freddie Freeman   | 0.447 | 0.232  | 0.495  | -0.264  |
| 2021      | Randy Arozarena   | 0.426 | 0.214  | 0.475  | -0.261  |
| 2021      | Juan Soto         | 0.532 | 0.304  | 0.562  | -0.258  |
| 2021      | Eduardo Escobar   | 0.405 | 0.184  | 0.439  | -0.255  |
| 2021      | Jorge Soler       | 0.378 | 0.176  | 0.430  | -0.254  |
| 2021      | Corey Dickerson   | 0.280 | 0.070  | 0.321  | -0.251  |
| 2021      | Ozzie Albies      | 0.388 | 0.179  | 0.428  | -0.249  |
| 2021      | Trevor Story      | 0.334 | 0.141  | 0.386  | -0.245  |
| 2021      | Manuel Margot     | 0.358 | 0.172  | 0.415  | -0.243  |
| 2021      | David Peralta     | 0.359 | 0.145  | 0.387  | -0.242  |
| 2021      | Joey Votto        | 0.473 | 0.288  | 0.526  | -0.238  |
| 2021      | Pavin Smith       | 0.336 | 0.139  | 0.374  | -0.235  |
| 2021      | Rafael Devers     | 0.386 | 0.198  | 0.433  | -0.235  |
| 2021      | Kolten Wong       | 0.391 | 0.191  | 0.426  | -0.235  |
| 2021      | Andrew Vaughn     | 0.376 | 0.178  | 0.412  | -0.234  |
| 2021      | Gary Sánchez      | 0.325 | 0.139  | 0.371  | -0.233  |
| 2021      | Ryan McMahon      | 0.360 | 0.172  | 0.402  | -0.230  |
| 2021      | Ian Happ          | 0.397 | 0.223  | 0.449  | -0.226  |
| 2021      | Bryce Harper      | 0.481 | 0.300  | 0.524  | -0.224  |
| 2021      | Justin Turner     | 0.378 | 0.198  | 0.419  | -0.222  |
| 2021      | Kyle Tucker       | 0.416 | 0.229  | 0.449  | -0.219  |
| 2021      | Carlos Correa     | 0.360 | 0.179  | 0.398  | -0.219  |
| 2021      | Willson Contreras | 0.385 | 0.217  | 0.425  | -0.209  |
| 2021      | Brandon Lowe      | 0.420 | 0.244  | 0.450  | -0.206  |
| 2021      | Joey Wendle       | 0.365 | 0.194  | 0.397  | -0.203  |
| 2021      | Martín Maldonado  | 0.343 | 0.167  | 0.369  | -0.202  |
| 2021      | Jorge Polanco     | 0.399 | 0.233  | 0.433  | -0.200  |
| 2021      | Miguel Sanó       | 0.465 | 0.298  | 0.494  | -0.196  |
| 2021      | Jean Segura       | 0.374 | 0.200  | 0.394  | -0.194  |
| 2021      | Josh Harrison     | 0.364 | 0.207  | 0.401  | -0.194  |
| 2021      | Akil Baddoo       | 0.369 | 0.206  | 0.399  | -0.194  |
| 2021      | Francisco Lindor  | 0.362 | 0.198  | 0.391  | -0.193  |
| 2021      | Chris Taylor      | 0.381 | 0.221  | 0.414  | -0.193  |
| 2021      | Kyle Seager       | 0.410 | 0.251  | 0.443  | -0.192  |
| 2021      | Christian Walker  | 0.299 | 0.147  | 0.337  | -0.190  |
| 2021      | Josh Bell         | 0.401 | 0.247  | 0.437  | -0.189  |
| 2021      | Nick Solak        | 0.375 | 0.227  | 0.410  | -0.184  |
| 2021      | Adolis García     | 0.315 | 0.167  | 0.349  | -0.182  |
| 2021      | Miguel Cabrera    | 0.340 | 0.190  | 0.370  | -0.180  |
| 2021      | Jonathan Villar   | 0.372 | 0.228  | 0.407  | -0.179  |
| 2021      | Nathaniel Lowe    | 0.389 | 0.245  | 0.421  | -0.176  |
| 2021      | Christian Vázquez | 0.357 | 0.211  | 0.386  | -0.175  |
| 2021      | César Hernández   | 0.339 | 0.189  | 0.361  | -0.171  |
| 2021      | Willy Adames      | 0.430 | 0.288  | 0.455  | -0.167  |
| 2021      | Joey Gallo        | 0.411 | 0.282  | 0.447  | -0.165  |
| 2021      | Ty France         | 0.415 | 0.275  | 0.438  | -0.163  |
| 2021      | Nelson Cruz       | 0.420 | 0.291  | 0.452  | -0.161  |
| 2021      | Matt Chapman      | 0.351 | 0.228  | 0.388  | -0.159  |
| 2021      | Jed Lowrie        | 0.346 | 0.222  | 0.380  | -0.158  |
| 2021      | Ryan Mountcastle  | 0.382 | 0.258  | 0.416  | -0.158  |
| 2021      | José Abreu        | 0.410 | 0.280  | 0.432  | -0.152  |
| 2022      | Joey Gallo (2x)   | 0.353 | 0.109  | 0.463  | -0.354  |
| 2022      | Mike Trout        | 0.386 | 0.107  | 0.461  | -0.354  |
| 2022      | Brandon Marsh     | 0.328 | 0.076  | 0.404  | -0.328  |
| 2022      | Nelson Cruz       | 0.306 | 0.066  | 0.362  | -0.297  |
| 2022      | Pete Alonso       | 0.379 | 0.174  | 0.434  | -0.260  |
| 2022      | Ji Man Choi       | 0.317 | 0.137  | 0.357  | -0.219  |
| 2022      | Mookie Betts      | 0.387 | 0.209  | 0.427  | -0.218  |
| 2022      | José Ramírez      | 0.374 | 0.213  | 0.428  | -0.215  |
| 2022      | Yuli Gurriel      | 0.282 | 0.111  | 0.323  | -0.212  |
| 2022      | Will Smith        | 0.384 | 0.224  | 0.430  | -0.206  |
| 2022      | Randal Grichuk (2x)| 0.312| 0.136  | 0.339  | -0.204  |
| 2022      | Trevor Story (2x) | 0.296 | 0.137  | 0.336  | -0.200  |
| 2022      | Amed Rosario      | 0.353 | 0.186  | 0.385  | -0.199  |
| 2022      | Nathaniel Lowe (2x)| 0.363| 0.217  | 0.410  | -0.194  |
| 2022      | Taylor Walls      | 0.302 | 0.155  | 0.343  | -0.188  |
| 2022      | Nick Gordon       | 0.370 | 0.230  | 0.419  | -0.188  |
| 2022      | Brendan Rodgers   | 0.336 | 0.184  | 0.372  | -0.188  |
| 2022      | César Hernández   | 0.324 | 0.168  | 0.354  | -0.186  |
| 2022      | J.T. Realmuto (2x)| 0.456 | 0.310  | 0.494  | -0.184  |
| 2022      | Yordan Alvarez    | 0.499 | 0.342  | 0.525  | -0.183  |
| 2022      | Andrew Benintendi | 0.375 | 0.230  | 0.411  | -0.181  |
| 2022      | Paul Goldschmidt  | 0.422 | 0.282  | 0.459  | -0.178  |
| 2022      | Marcus Semien     | 0.335 | 0.191  | 0.368  | -0.178  |
| 2022      | Kyle Tucker (2x)  | 0.378 | 0.229  | 0.406  | -0.177  |
| 2022      | Ian Happ (2x)     | 0.383 | 0.237  | 0.410  | -0.174  |
| 2022      | MJ Melendez       | 0.336 | 0.189  | 0.362  | -0.174  |
| 2022      | Austin Riley      | 0.358 | 0.224  | 0.393  | -0.169  |
| 2022      | Jonathan Schoop   | 0.237 | 0.109  | 0.275  | -0.167  |
| 2022      | Taylor Ward       | 0.368 | 0.227  | 0.394  | -0.167  |
| 2022      | Xander Bogaerts   | 0.398 | 0.270  | 0.434  | -0.164  |
| 2022      | Ryan McMahon (2x) | 0.320 | 0.191  | 0.351  | -0.160  |
| 2022      | Robbie Grossman   | 0.322 | 0.190  | 0.350  | -0.160  |
| 2022      | Andrew Vaughn (2x)| 0.372 | 0.236  | 0.395  | -0.159  |
| 2022      | Geraldo Perdomo   | 0.260 | 0.136  | 0.290  | -0.154  |
| 2022      | Bryson Stott      | 0.261 | 0.147  | 0.300  | -0.153  |


### NOTES
- I was expecting to see a bias towards power hitters on this list, and it does seem that way
  - Mike Trout, Nathaniel Lowe, Austin Riley, Paul Goldschmidt, Pete Alonso, Joey Gallo, Miguel Sanó, Nelson Cruz, Ryan Mountcastle, Brandon Lowe, Bryce Harper, Willson Contreras, Rafael Devers, Randy Arozarena, Brandon Belt, Ryan McMahon, Gary Sanchez
- This list of batters has an average HR rate (3.8%) higher than the batters on the first list (3.3%)
- The overall wOBA (not just on 4-Seam Fastballs) for this list of hitters is higher (0.347) than the first list of hitters (0.344), supporting the notion that these hitters are more power-type of bats and the other list more contact-type
    - Another interesting note: the next year's overall wOBA (not just on 4-Seam Fastballs) for these lists of hitters: 'contact' list 1: **.338** vs. 'power' list 2: **.329!**
      - In other words, in the next season, the 'contact' list of hitters actually collectively had a higher wOBA than the list with a bias towards power hitters
      - Could this switch really be guided by their ability to counter this more effective 4-Seam Fastball combined with an increased rate of those 4-Seam Fastballs occurring?
  - I wish that I had access to Attack Angle and hand position measurements because I would expect that this second list of hitters has a steeper Attack Angle and lower hand positioning before their swing
    - However, I did find a hint that Attack Angle could be a factor by finding that the Flyball% for this 2nd 'power' list is 28.3% vs. 25.6%! for the first 'contact' list
        - The idea is that batters with a steeper Attack Angle tend to hit more Flyballs

## PLAYER SPOTLIGHT
- The 20 hitters that were on both lists
  - Grouped by the ones whose 'g_wOBA' declined from season to season vs. those whose 'g_wOBA' improved
  
### List of batters on both lists whose 'g_wOBA' decreased from season to season

| Season    |   Player Name      | wOBA   | g_wOBA | b_wOBA | d_wOBA |
|-----------|--------------------|--------|--------|--------|--------|
| 2021      | Yordan Alvarez     | 0.388  | 0.484  | 0.372  | 0.112  | 
| 2022      | Yordan Alvarez     | 0.499  | 0.342  | 0.525  | -0.183 | 
| 2021      | Yuli Gurriel       | 0.326  | 0.412  | 0.307  | 0.105  | 
| 2022      | Yuli Gurriel       | 0.282  | 0.111  | 0.323  | -0.212 | 
| 2021      | Marcus Semien      | 0.429  | 0.49   | 0.417  | 0.073  | 
| 2022      | Marcus Semien      | 0.335  | 0.191  | 0.368  | -0.178 | 
| 2021      | Will Smith         | 0.445  | 0.495  | 0.437  | 0.058  |
| 2022      | Will Smith         | 0.384  | 0.224  | 0.43   | -0.206 |
| 2021      | Amed Rosario       | 0.391  | 0.387  | 0.392  | -0.005  |
| 2022      | Amed Rosario       | 0.353  | 0.186  | 0.385  | -0.199  | 
| 2021      | Xander Bogaerts    | 0.389  | 0.385  | 0.391  | -0.006  |
| 2022      | Xander Bogaerts    | 0.398  | 0.27   | 0.434  | -0.164  |
| 2021      | Andrew Benintendi  | 0.348  | 0.339  | 0.35   | -0.011  |
| 2022      | Andrew Benintendi  | 0.375  | 0.23   | 0.411  | -0.181  |
| 2021      | José Ramírez       | 0.435  | 0.418  | 0.44   | -0.022  |
| 2022      | José Ramírez       | 0.374  | 0.213  | 0.428  | -0.215  |

### NOTES
- 8 batters
  - 1 batter is currently in a different stratosphere than basically every other batter: Yordan Alvarez
    - For the purposes of this analysis, I am comfortable excluding him because he is on this list due to his 0.525 b_wOBA and not because he had a below average g_wOBA, which resulted in a large d_wOBA discrepancy
  - Of the other 7 hitters, 6! of them are in the middle of regressing in regard to their wOBA against 4-Seam Fastballs in 2023 (wOBA value vs. 4-Seam Fastballs are as of September 9, 2023 according to Baseball Savant)
    - Yuli Gurriel: 0.241
    - Marcus Semien: 0.331
    - Will Smith: 0.340
    - Amed Rosario: 0.315
    - Xander Bogaerts: 0.325
    - Andrew Benintendi: 0.344 
    - Jose Ramirez: 0.367
  - Despite displaying a declining ability to hit this 'good' 4-Seam Fastball from season to season, all of these hitters have previously displayed an above average ability to hit that more effective type of 4-Seam Fastball in a recent season, so there is reason to believe that they are able to replicate that performance to some extent in a future season, but it is interesting how most of them have not done that in 2023
 
### List of batters on both lists whose 'g_wOBA' increased from season to season
      
| Season    |   Player Name      | wOBA   | g_wOBA | b_wOBA | d_wOBA |
|-----------|--------------------|--------|--------|--------|--------|
| 2021      | Rafael Devers      | 0.386  | 0.198  | 0.433  |-0.235  |
| 2022      | Rafael Devers      | 0.366  | 0.505  | 0.32   | 0.185  |
| 2021      | Miguel Cabrera     | 0.34   | 0.19   | 0.37   |-0.180  |
| 2022      | Miguel Cabrera     | 0.302  | 0.386  | 0.279  | 0.107  |
| 2021      | Josh Bell          | 0.401  | 0.247  | 0.437  |-0.189  |
| 2022      | Josh Bell          | 0.372  | 0.452  | 0.348  | 0.104  |
| 2021      | Adolis García      | 0.315  | 0.167  | 0.349  |-0.182  |
| 2022      | Adolis García      | 0.325  | 0.37   | 0.308  | 0.062  |
| 2021      | Kolten Wong        | 0.391  | 0.191  | 0.426  |-0.235  |
| 2022      | Kolten Wong        | 0.427  | 0.473  | 0.417  | 0.057  |
| 2021      | Jorge Polanco      | 0.399  | 0.233  | 0.433  |-0.200  |
| 2022      | Jorge Polanco      | 0.383  | 0.407  | 0.377  | 0.030  |
| 2021      | Ty France          | 0.415  | 0.275  | 0.438  |-0.163  |
| 2022      | Ty France          | 0.369  | 0.374  | 0.368  | 0.006  |
| 2021      | Matt Chapman       | 0.351  | 0.228  | 0.388  |-0.159  |
| 2022      | Matt Chapman       | 0.282  | 0.285  | 0.281  | 0.004  |
| 2021      | Juan Soto          | 0.532  | 0.304  | 0.562  | -0.258 |
| 2022      | Juan Soto          | 0.485  | 0.48   | 0.486  | -0.006 |
| 2021      | Eugenio Suárez     | 0.38   | 0.17   | 0.434  | -0.264 |
| 2022      | Eugenio Suárez     | 0.349  | 0.336  | 0.352  | -0.015 |
| 2021      | Randy Arozarena    | 0.426  | 0.214  | 0.475  | -0.261 |
| 2022      | Randy Arozarena    | 0.415  | 0.398  | 0.42   | -0.022 |
| 2021      | Justin Turner      | 0.378  | 0.198  | 0.419  | -0.222 |
| 2022      | Justin Turner      | 0.338  | 0.32   | 0.345  | -0.025 |


### NOTES
- 12 batters
  - I will exclude Juan Soto for the same reasons as Yordan Alvarez
  - Of the remaining 11 batters, 6 have 'above average' wOBA against 4-Seam Fastballs in 2023 
    - Rafael Devers: 0.363
    - Miguel Cabrera: 0.394
    - Josh Bell: 0.422
    - Kolten Wong: 0.393
    - Jorge Polanco: 0.447
    - Randy Arozarena: 0.426
  - Of the other 5 batters that are 'below average', 4 of them have an xwOBA above average (xwOBA in parentheses)
    - Ty France: 0.331 (.356)
    - Matt Chapman: 0.335 (.365)
    - Eugenio Suarez: 0.347 (.409)
    - Justin Turner: 0.322 (.352)
    - Adolis Garcia: 0.318 (.325)
  - The results from this second list do not seem as definitive as the first list, but it is still promising when you consider that 4 of the 5 batters who currently have a below average wOBA against 4-Seam Fastballs in 2023 come with an above average xwOBA


## CONCLUSIONS
### VAA TREND
- 4-Seam Fastballs are being thrown less in general, but 4-Seam Fastballs with a VAA >= -4.5° are being thrown more
  - This means that a higher percentage of the 4-Seam Fastballs thrown will be 'optimal' or 'good' 4-Seam Fastballs
- Many statistics support the fact that this type of 4-Seam Fastball is considerably more favorable for pitchers
  - Especially when it is thrown anywhere within about 5 inches of the top of the batter's strikezone
- Due to the evidence supporting this type of 4-Seam Fastball's current effectiveness, this trend is likely not going to stop until batters show an increased ability to counter it in future seasons
### PITCHERS
- **70% of the pitchers with a 'Rate' of 30% or greater** finished the season with *above average* 4-Seam Fastball effectiveness compared to only **40% of the pitchers with a 'Rate' of 8% or less**
- For the list of effective pitching seasons where their 'rate' was **less than 8%**, the wOBA against it was a combined .316, but the wOBA next year inflated to .360!
  - The wOBA against 4-Seam Fastballs for the list of *ineffective* pitching seasons where their 'rate' was **30%+**, the wOBA against it was .394, but their next year wOBA was .342!
      - This supports the notion that there could be predictive value to throwing this type of 4-Seam Fastball
- Pitchers not having success with this flatter type of 4-Seam Fastball should trust in these numbers that emphasize the pitch's above average effectiveness across the league, and focus more on potential issues with the way they are using it
  - Many of the effective pitchers in the 30% category use their 4-Seam Fastball as their primary pitch (Gerrit Cole, Cristian Javier, Michael Kopech, etc.)
- Pitchers having success *without* throwing their 4-Seam Fastball this way might be better at the art of pitching, but they should be wary that they might also have been getting lucky because the league typically hits 4-Seam Fastballs that look like theirs for much more damage
  - The effectiveness of their 4-Seam Fastball is at risk of declining if they do not locate it as carefully next season or even if they just slightly lose effectiveness with their *other* pitches
  - Many of the effective pitchers in the 'less than 8% 'rate'' category have success with their 4-Seam Fastball by using it as a complimentary pitch to their more high-quality pitches (Zack Greinke, Justin Verlander, Clayton Kershaw, Justin Steele, etc.)
- I noticed that there are a ton of young pitchers throwing this 'better' 4-Seam Fastball right now, so, again, I do not see this trend going away (Bryce Miller, Mason Miller, Bobby Miller, Hunter Greene, Bryan Woo, JP Sears, Spencer Strider, etc.)
- I do not have access to stats such as Arm Angle because I do not have measurements of where the pitcher is on the rubber nor measurements of their shoulder height, but if I did, I expect that I would see a strong correlation between lowering a pitcher's Arm Angle at the point of release and having an optimal VAA on their 4-Seam Fastball
  - There is a correlation between release height and VAA that you can find in my code!
- Also, I cannot calculate spin efficiency measurements with my data because I do not have the directional components of spin rate, just an overall spin rate, but if I did, I would expect that having a highly spin efficient 4-Seam Fastball helps a pitcher's VAA to be flatter because highly spin efficient 4-Seam Fastballs counteract the drag force on the pitched ball, enabling the lift force, leading to a pitched ball that drops less, and therefore has a flatter VAA
### BATTERS
- The significant Flyball% discrepancy between the lists of batters mentioned earlier made sense, to me, of *why* this trend is happening: it is to counter all the launch angle efforts by the batter and counter the increasing Homerun environment created in the last 5 seasons
  - That explains why there is a skew towards contact hitters having the ability to hit this more effective 4-Seam Fastball, but that does not tell the whole story
    - This Flyball% discrepancy between the list of batters is a hint that Attack Angle measurements would be able to reveal more about which batters are having success/can have success vs. this type of 4-Seam Fastball
    - More specifically, the ability for a batter to have a *range* of Attack Angles at their disposal due to their swing mechanics would be a major key in being able to take advantage of this trend, rather than just being a batter that has a flatter Attack Angle on average
      - ***Range*** of Attack Angles is emphasized because if a batter just has an overall flatter Attack Angle, then he is likely to struggle on the rest of the 90% of pitches that are not this specific type of 4-Seam Fastball, but if he has more diversity in his Attack Angles, then that will enable him to be able to get to this type of pitch when he needs to while maintaining an effective approach vs. all other pitch types
- Another interesting note is that the BB% for the first list of 'contact' batters (9.2%) was slightly higher than the second list of 'power' batters (8.7%)
  - I found this notable because you would likely assume that between 2 lists of batters, the list with more power bats would have the higher walk rate than the contact hitters
  - Also, part of the idea behind this analysis is that a flatter VAA on 4-Seam Fastballs makes them harder to hit because the pitch is not breaking as the batter is expecting
    - This implies that if the batter has an ability to see the ball better, which a higher BB% implies, then he can give himself a better chance to identify and ultimately hit this type of 4-Seam Fastball
- I was hesitant to say that the first list of 'contact' hitters should be viewed as progression candidates because of my assumption that the ability to hit this type of pitch will have a lot of variability from season to season, and because it is only 6% of **all** pitches
    - I was also assuming that if a guy hits it more effectively one season, then he should regress to some extent the following season just based on the fact that this specific type of 4-Seam Fastball is statistically harder to hit
  - But what I found was that, although the overall wOBA for the first list of hitters was lower (.344) than it was for that second list of 'power' bats (.347), the overall wOBA in the next year for the first list of hitters (.338) was better than the second list's (.329), so maybe there *is* predictive value in a batter's ability to hit this type of 4-Seam Fastball
- Until this trend plateaus or declines, it should be a priority for every MLB front office to understand which of their batters are currently showing an ability to hit this type of 4-Seam Fastball, have displayed this ability in the past, and the ones without it
  - From the last Player Spotlight section where I looked at the batters on both lists, we see that **it is possible for batters to improve their ability to hit this pitch and sustain that production into the following season**


#### If there are any Questions, Comments, Ideas, or any pitcher/batter you would like to see their VAA breakdown of, please feel free to email me at josephmontes.baseball@gmail.com

