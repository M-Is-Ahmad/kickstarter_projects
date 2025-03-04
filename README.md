# Kickstarter Projects Analysis

This repository contains SQL queries used to analyze a dataset of Kickstarter projects. The analysis includes various insights related to project success rates, funding trends, and the impact of backers on project success. The data covers Kickstarter projects from 2009 to 2017.

## Queries Overview

The following SQL queries are used to answer different analysis questions based on the Kickstarter dataset:

### 1. **Which category has the highest success percentage? How many projects have been successful?**
   - **Query:** Calculates the success percentage for each project category and determines the number of successful projects.
   - **Purpose:** Helps identify which categories have the highest success rates.
   
   ```sql
   SELECT 
       Category,
       ROUND(COUNT(CASE WHEN State = 'Successful' THEN 1 END) * 100.0 / COUNT(*), 2) AS success_percentage,
       COUNT(CASE WHEN State = 'Successful' THEN 1 END) AS successful_projects
   FROM Kickstarter_projects
   GROUP BY Category
   ORDER BY success_percentage DESC;

### 2. **What project with a goal over $1,000 USD had the biggest Goal Completion % (Pledged / Goal)? How much money was pledged?**
   - **Query:** Identifies the project with the highest goal completion percentage where the goal exceeds $1,000.
   - **Purpose:** Assesses the most successfully funded projects relative to their goal.

```sql
SELECT 
    Name,
    Goal,
    Pledged,
    ROUND(Pledged * 1.0 / Goal * 100, 2) AS goal_completion_percentage
FROM Kickstarter_projects
WHERE Goal > 1000
ORDER BY goal_completion_percentage DESC
LIMIT 1;

### 3. **Can you identify any trends in project success rates over the years (2009-2017)?**
   - **Query:** Analyzes success rates for Kickstarter projects by year.
   - **Purpose:** Helps to identify patterns or trends in project success over time.

```sql
SELECT 
    strftime('%Y', Launched) AS Year,
    ROUND(COUNT(CASE WHEN State = 'Successful' THEN 1 END) * 100.0 / COUNT(*), 2) AS success_percentage
FROM Kickstarter_projects
GROUP BY Year
ORDER BY Year;

### 4. **What types of projects should you, as an investor, focus on to ensure future success?**
   - **Query:** Calculates the success rate for each category or subcategory and helps investors determine which types of projects are most likely to succeed.
   - **Purpose:** Provides guidance for investors on which project categories show the highest success rates.

```sql
SELECT 
    Category,
    ROUND(COUNT(CASE WHEN State = 'Successful' THEN 1 END) * 100.0 / COUNT(*), 2) AS success_percentage
FROM Kickstarter_projects
GROUP BY Category
ORDER BY success_percentage DESC;

### 5. **How do funding success rates differ between different years (2009-2017)?**
   - **Query:** Analyzes how success rates vary across different years.
   - **Purpose:** Helps understand how funding success rates change over time.

```sql
SELECT 
    strftime('%Y', Launched) AS Year,
    ROUND(COUNT(CASE WHEN State = 'Successful' THEN 1 END) * 100.0 / COUNT(*), 2) AS success_percentage
FROM Kickstarter_projects
GROUP BY Year
ORDER BY Year;

### 6. **What is the correlation between the number of backers and project success?**
   - **Query:** Compares success rates based on different ranges of backers.
   - **Purpose:** Identifies whether the number of backers correlates with project success.

```sql
SELECT 
    CASE 
        WHEN Backers < 50 THEN '1-49'
        WHEN Backers BETWEEN 50 AND 499 THEN '50-499'
        WHEN Backers BETWEEN 500 AND 999 THEN '500-999'
        WHEN Backers >= 1000 THEN '1000+'
    END AS Backer_Range,
    ROUND(COUNT(CASE WHEN State = 'Successful' THEN 1 END) * 100.0 / COUNT(*), 2) AS success_percentage
FROM Kickstarter_projects
GROUP BY Backer_Range
ORDER BY Backer_Range;

### 7. **What is the average pledge per backer across different categories?**
   - **Query:** Calculates the average pledge per backer for each project category.
   - **Purpose:** Analyzes how different categories perform in terms of average pledge per backer.

```sql
SELECT 
    Category,
    SUM(Pledged) AS total_pledged,
    SUM(Backers) AS total_backers,
    ROUND(SUM(Pledged) * 1.0 / SUM(Backers), 2) AS avg_pledge_per_backer
FROM Kickstarter_projects
WHERE Backers > 0  -- To avoid division by zero
GROUP BY Category
ORDER BY avg_pledge_per_backer DESC;

### 8. **What are the top 10 countries with the highest number of successful projects?**
   - **Query:** Lists the top 10 countries with the highest number of successful projects.
   - **Purpose:** Helps identify which countries are most successful on Kickstarter.

```sql
SELECT 
    Country,
    COUNT(*) AS successful_projects
FROM Kickstarter_projects
WHERE State = 'Successful'  -- Filter for successful projects
GROUP BY Country
ORDER BY successful_projects DESC
LIMIT 10;