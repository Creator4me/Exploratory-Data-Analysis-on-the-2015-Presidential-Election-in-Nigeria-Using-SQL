# Exploratory-Data-Analysis-on-the-2015-Presidential-Election-in-Nigeria-Using-SQL

## Table Of Content
 -[Project Overview](#project-overview)
 
 -[Data Sources](#data-source)

 -[Tool Used](#tool-used)

 -[Data Cleaning And Preparation](#data-cleaning-and-preparation)

 -[Exploratory Data Analysis](#exploratory-data-analysis)

 -[Data Analysis](#data-analysis)

 -[Result and Findings](#result-and-findings)

 -[Recommendation](#recommendation)

 -[Limitation](#limitations)

 -[References](#references)

### Project Overview

The provided queries analyze the 2015 Presidential Election in Nigeria. Various aspects of the election data have been explored to understand voter turnout, party performances, regional patterns, and more.

<img width="728" alt="Election2015 png1" src="https://github.com/Creator4me/Exploratory-Data-Analysis-on-the-2015-Presidential-Election-in-Nigeria-Using-SQL/assets/140057655/d5dc3c5b-6dbb-4c14-8825-0b6326204f7e">

<img width="731" alt="Election2015 png2" src="https://github.com/Creator4me/Exploratory-Data-Analysis-on-the-2015-Presidential-Election-in-Nigeria-Using-SQL/assets/140057655/634b772e-1b39-47a3-8454-80ffd2731ef3">

### Data Source

2015 Presidential Election:The core dataset employed for this investigative review is the "2015election.csv" file, capturing the performance of the two main political parties during the 2015 presidential election.

### Tool used

- Excel - Data Cleaning
  - [Download Here](https://microsoft.com)
- SQL Server - Data Analysis
- Power BI - Used For Creating Reports
  - [Download Here](https://powerbi.microsoft.com)
    
### Data Cleaning And Preparation.

In the initial Data Preparation Phase,The following task were performed
1.Data Loading and Inspection
2.Data Cleaning and Formatting

### Exploratory Data Analysis (EDA)
This involves exploring the election data to answer key questions such as:
- Number Of State?
- Number Of Geopolitical Zones?
- Distribution Of Potential Voters?
- Distribution Of Accredited Voters?
- Distribution Of Votes Cast?
- Distribution of Valid Votes Cast?
- Distribution of Rejected Votes?
- Regional Voting Patterns?
- Voter Turnout Rate?
- What state has the highest Percentage of Vote in PDP?
- What state has the highest Percentage of Vote in APC?
- Marginal Difference of Votes Between Both Parties?
- What is the Percentage of Rejected Votes Across the States?
- Geopolitical Zone Trend?
  
### Data Analysis
Code/Features Worked With
```SQL
--Number of States
Query: SELECT COUNT(state) FROM Election_2015;

--Number of Geopolitical Zones
Query: SELECT COUNT(DISTINCT geopolitical_zone) FROM Election_2015;

--Distribution of Potential Voters
 SELECT state, num_of_reg_voters AS potential_Voters FROM Election_2015 ORDER BY num_of_reg_voters DESC;

--Distribution of Accredited Voters
 SELECT state, accredited_voters FROM Election_2015 ORDER BY accredited_voters DESC;

--Distribution of Votes Cast
 SELECT state, votes_cast FROM Election_2015 ORDER BY votes_cast DESC;

--Distribution of Valid Votes Cast
 SELECT state, valid_votes FROM Election_2015 ORDER BY valid_votes DESC;

--Distribution of Rejected Votes
 SELECT state, rejected_votes FROM Election_2015 ORDER BY rejected_votes DESC;

--Regional Voting Patterns--
SELECT geopolitical_zone,
       SUM(pdp_votes) as total_pdp_votes,
	   SUM(apc_votes)as total_apc_votes,
	   ROUND(SUM(pdp_votes) *100.0/SUM(valid_votes),2) as pdp_percentage,
	   ROUND(SUM(apc_votes) *100.0/SUM(valid_votes),2) as apc_percentage
FROM Election_2015
GROUP BY geopolitical_zone
ORDER BY geopolitical_zone;

--Voter Turnout Rate--
SELECT  state,(accredited_voters::FLOAT/num_of_reg_voters)* 100 as turnout_rate
FROM Election_2015
ORDER BY turnout_rate DESC;

--State With Highest Percentage Of Vote in PDP--
SELECT state, pct_of_pdp_votes
FROM Election_2015
ORDER BY pct_of_pdp_votes DESC
LIMIT 1;

--State With Highest Percentage Of Vote in APC--
SELECT state, pct_of_apc_votes
FROM Election_2015
ORDER BY pct_of_apc_votes DESC
LIMIT 1;

--Marginal Difference Of Votes Between Both Parties--Threshold 10%
SELECT state, ABS(pct_of_pdp_votes - pct_of_apc_votes) AS vote_difference
FROM Election_2015
WHERE ABS(pct_of_pdp_votes - pct_of_apc_votes) < 10 -- Adjust the threshold (5 in this case) for the maximum acceptable difference
ORDER BY vote_difference;

--Marginal Difference Of Votes Betwen Both Parties--Threshold 5%
SELECT state, ABS(pct_of_pdp_votes - pct_of_apc_votes) AS vote_difference
FROM Election_2015
WHERE ABS(pct_of_pdp_votes - pct_of_apc_votes) < 5 -- Adjust the threshold (5 in this case) for the maximum acceptable difference
ORDER BY vote_difference;

--Percentage Of Rejected Votes Across The States
SELECT state,
       (rejected_votes::FLOAT / votes_cast) * 100.0 AS pct_rejected_votes
FROM Election_2015;

--Percentage Of Valid Votes Across The States
SELECT state,
       (valid_votes::FLOAT / votes_cast) * 100.0 AS pct_valid_votes
FROM Election_2015
order by  pct_valid_votes 

--Geopolitical Zone Trend--
SELECT geopolitical_zone,
       CASE
           WHEN MAX(pdp_votes) > MAX(apc_votes) THEN 'PDP'
           WHEN MAX(apc_votes) > MAX(pdp_votes) THEN 'APC'
           ELSE 'Equal Dominance'  -- If votes for PDP and APC are equal
       END AS predominant_party
FROM Election_2015
GROUP BY geopolitical_zone;
```
### Result/Findings
The Analysis results are summarised as follows

1.Concerning the distribution of potential voters among states, Lagos State stood out with the highest count of potential voters among all 37 states, while Bayelsa had the lowest count.

2.Examining the distribution of accredited voters, Kano recorded both the highest number of accredited voters and votes cast. Conversely, Ekiti had the lowest count of accredited voters and votes cast.

3.Notably, Kano had a substantial count of valid votes, followed by Kaduna. On the other hand, FCT and Ekiti had notably lower valid vote counts.

4.Lagos State observed the highest number of rejected votes, while Bayelsa had the lowest count of rejected votes.

5.Analyzing the regional voting patterns, PDP secured more votes in the South-South, while APC received higher votes in the Northwest. Votes in the Southwest and North Central were nearly equivalent. In terms of voter participation, Rivers State displayed significantly higher voter turnout compared to Lagos, which had the lowest turnout.

6.Ebonyi State noted the highest percentage of rejected votes, while states like Akwa Ibom had notably low rejected votes.

7.Examining the voter difference across states, PDP garnered higher votes in states such as Anambra, Rivers, Akwa Ibom, Abia, Enugu, and Bayelsa. Conversely, APC obtained more votes in states like Borno, Yobe, Bauchi, Kano, and Jigawa, while the FCT showed the least vote difference between both parties.

### Recommendation

According to the analysis, I suggest the following:

 - Enhance voter education in states with higher rates of rejected votes, educating them on the voting process.
 - Raise awareness in states with lower voter turnout about the importance of participating in elections.

### Limitations

 -We excluded records from other parties to prioritize our focus on the two main parties.

 ### References
 1.Independent National Electoral Commission









