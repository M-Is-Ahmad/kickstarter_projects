# 🚀 Kickstarter Projects Analysis

This repository contains SQL queries used to analyze a dataset of Kickstarter projects. The analysis includes various insights related to project success rates, funding trends, and the impact of backers on project success. The data covers Kickstarter projects from 2009 to 2017.

## 📌Queries Overview

The following SQL queries are used to answer different analysis questions based on the Kickstarter dataset:

### 1.  **Which category has the highest success percentage? How many projects have been successful?**
   - **🔍Query:** Calculates the success percentage for each project category and determines the number of successful projects.
   - **🎯Purpose:** Helps identify which categories have the highest success rates.
   
```sql
SELECT 
    Category, 
    COUNT(*) AS total_projects,
    SUM(CASE WHEN State = 'Successful' THEN 1 ELSE 0 END) AS successful_projects,
    ROUND((SUM(CASE WHEN State = 'Successful' THEN 1 ELSE 0 END) * 100.0) / COUNT(*), 2) AS success_percentage
FROM Kickstarter_projects
GROUP BY Category
ORDER BY success_percentage DESC;
   ```

### 2. **What are the Top 10 projects with a goal over $1,000 USD, had the biggest Goal Completion % (Pledged / Goal)? How much money was pledged?**
**
   - **🔍Query:** Identifies the project with the highest goal completion percentage where the goal exceeds $1,000.
   - **🎯Purpose:** Assesses the most successfully funded projects relative to their goal.

```sql
SELECT 
    Name, 
    Goal, 
    Pledged, 
    ROUND (((Pledged) / Goal),2) AS goal_completion_percentage
FROM Kickstarter_projects
WHERE Goal > 1000
ORDER BY goal_completion_percentage DESC
LIMIT 10;
```

### 3. **What are the trends in the success rates of Kickstarter projects over the years?**
   - **🔍Query:** Analyzes success rates for Kickstarter projects by year.
   - **🎯Purpose:** Helps to identify patterns or trends in project success over time.

```sql
SELECT 
    strftime('%Y', Launched) AS year,  -- Extract year from the Launched date
    COUNT(*) AS total_projects,        -- Total number of projects launched in that year
    SUM(CASE WHEN State = 'Successful' THEN 1 ELSE 0 END) AS successful_projects,  -- Count successful projects
    ROUND(((SUM(CASE WHEN State = 'Successful' THEN 1 ELSE 0 END) * 100.0) / COUNT(*)),2) AS success_rate  -- Success rate
FROM Kickstarter_projects
GROUP BY year
ORDER BY year;
```

### 4. **which categories (subcategories) tend to have the highest success rates?**
   - **🔍Query:** Calculates the success rate for each category or subcategory and helps investors determine which types of projects are most likely to succeed.
   - **🎯Purpose:** Provides guidance for investors on which project categories show the highest success rates.

```sql
SELECT 
    Category, Subcategory,
    COUNT(*) AS total_projects,
    SUM(CASE WHEN State = 'Successful' THEN 1 ELSE 0 END) AS successful_projects,
    ROUND((SUM(CASE WHEN State = 'Successful' THEN 1 ELSE 0 END) * 100.0) / COUNT(*), 2) AS success_percentage
FROM Kickstarter_projects
GROUP BY Subcategory
ORDER BY success_percentage DESC
LIMIT 10;
```

### 5. **How do funding success rates differ between different years (2009-2017)?**
   - **🔍Query:** Analyzes how success rates vary across different years.
   - **🎯Purpose:** Helps understand how funding success rates change over time.

```sql
SELECT 
    strftime('%Y', Launched) AS year,  
    COUNT(*) AS total_projects,       
    SUM(CASE WHEN State = 'Successful' THEN 1 ELSE 0 END) AS successful_projects,  
    ROUND((SUM(CASE WHEN State = 'Successful' THEN 1 ELSE 0 END) * 100.0) / COUNT(*), 2) AS success_rate  
FROM Kickstarter_projects
WHERE strftime('%Y', Launched) BETWEEN '2009' AND '2017'  -- Filter for years 2009-2017
GROUP BY year
ORDER BY year;
```

### 6. **What is the correlation between the number of backers and project success?**
   - **🔍Query:** Compares success rates based on different ranges of backers.
   - **🎯Purpose:** Identifies whether the number of backers correlates with project success.

```sql
SELECT 
    CASE 
        WHEN Backers BETWEEN 0 AND 10 THEN '0-10'
        WHEN Backers BETWEEN 11 AND 100 THEN '11-100'
        WHEN Backers BETWEEN 101 AND 500 THEN '101-500'
        WHEN Backers BETWEEN 501 AND 1000 THEN '501-1000'
        WHEN Backers BETWEEN 1001 AND 5000 THEN '1001-5000'
        WHEN Backers > 5000 THEN '5000+'
        ELSE 'No Backers' 
    END AS backer_range,
    COUNT(*) AS total_projects,
    SUM(CASE WHEN State = 'Successful' THEN 1 ELSE 0 END) AS successful_projects,
    ROUND((SUM(CASE WHEN State = 'Successful' THEN 1 ELSE 0 END) * 100.0) / COUNT(*), 2) AS success_rate
FROM Kickstarter_projects
GROUP BY backer_range
ORDER BY success_rate;
```

### 7. **What is the average pledge per backer across different categories?**
   - **🔍Query:** Calculates the average pledge per backer for each project category.
   - **🎯Purpose:** Analyzes how different categories perform in terms of average pledge per backer.

```sql
SELECT 
    Category,
    SUM(Pledged) AS total_pledged,
    SUM(Backers) AS total_backers,
    ROUND(SUM(Pledged) * 1.0 / SUM(Backers), 2) AS avg_pledge_per_backer
FROM Kickstarter_projects
WHERE Backers > 0  
GROUP BY Category
ORDER BY avg_pledge_per_backer DESC;
```
### 8. **What are the top 5 countries with the highest number of successful projects?**
   - **🔍Query:** Lists the top 10 countries with the highest number of successful projects.
   - **🎯Purpose:** Helps identify which countries are most successful on Kickstarter.

```sql
SELECT 
    Country,
    COUNT(*) AS successful_projects
FROM Kickstarter_projects
WHERE State = 'Successful' 
GROUP BY Country
ORDER BY successful_projects DESC
LIMIT 5;
```
## 🎯Conclusion

This analysis provides valuable insights into Kickstarter project success based on categories, goals, backers, and time periods. By identifying trends in funding success, we can better understand which factors contribute to project success. Key findings show how the success rate varies by category, the importance of backer support, and the funding trends over the years. This can help investors make informed decisions about which projects to support and guide creators in improving their chances of success.

## 🎯Target Audience

This analysis is aimed at:

- **📈Investors** looking to understand trends and identify high-success categories for future funding.
- **📢Kickstarter project creators** who want to improve their project's chances of success by learning from historical data.
- **📚Researchers** exploring crowdfunding platforms and their impacts over time.

This analysis is useful for anyone seeking to make informed decisions about participating in or analyzing Kickstarter campaigns.

## 📌Additional Notes

- The analysis was conducted using [SQLite](https://sqliteviz.com/), which provided an efficient platform for querying and analyzing the dataset.
- The source of the dataset is from [Maven Analytics](https://mavenanalytics.io/). Special thanks to Maven Analytics for providing this valuable resource for data analysis and insight generation.
