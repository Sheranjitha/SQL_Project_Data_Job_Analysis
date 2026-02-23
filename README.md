##SQL Portfolio Project: Data Analyst Job Market Analysis
Introduction

Welcome to my SQL Portfolio Project! In this project, I explore the data analyst job market, focusing on:

Top-paying jobs

In-demand skills

Skills that offer both high demand and high salary

This project showcases my SQL skills and analytical capabilities by deriving actionable insights from real-world job posting data.
Background

I created this project to better understand the data analyst job market and identify:

Skills in high demand

Skills linked to higher salaries

A strategic path for skill development

Data Source: Luke Barousse’s SQL Course
Key Questions:

What are the top-paying data analyst jobs?

Which skills do these high-paying jobs require?

What skills are most in-demand?

Which skills are associated with higher salaries?

Which skills provide the best balance of demand and salary?
Tools Used
| Tool                   | Purpose                             |
| ---------------------- | ----------------------------------- |
| **SQL**                | Data querying and analysis          |
| **PostgreSQL**         | Database management                 |
| **Visual Studio Code** | Code execution & project management |
Analysis & Queries
1. Top-Paying Data Analyst Jobs
 SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date
FROM
    job_postings_fact
WHERE
    job_title = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_location = 'Anywhere'
ORDER BY
    salary_year_avg DESC
LIMIT 10;
2. Skills for Top-Paying Jobs
   WITH top_paying_jobs AS (
    SELECT
        job_id,
        job_title,
        salary_year_avg
    FROM
        job_postings_fact
    WHERE
        job_title_short = 'Data Analyst'
        AND salary_year_avg IS NOT NULL
        AND job_location = 'Anywhere'
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)
SELECT
    tp.job_id,
    tp.job_title,
    tp.salary_year_avg,
    s.skills
FROM
    top_paying_jobs tp
INNER JOIN skills_job_dim sj ON tp.job_id = sj.job_id
INNER JOIN skills_dim s ON sj.skill_id = s.skill_id
ORDER BY
    tp.salary_year_avg DESC;
   Shows skills required for top-paying Data Analyst roles.
3. Most In-Demand Skills
   SELECT
    s.skills,
    COUNT(sj.job_id) AS demand_count
FROM
    job_postings_fact j
INNER JOIN skills_job_dim sj ON j.job_id = sj.job_id
INNER JOIN skills_dim s ON sj.skill_id = s.skill_id
WHERE
    j.job_title_short = 'Data Analyst'
GROUP BY
    s.skills
ORDER BY
    demand_count DESC
LIMIT 5;
Highlights the most frequently requested skills.
4. Skills Based on Salary
SELECT
    s.skills AS skill,
    ROUND(AVG(j.salary_year_avg), 2) AS avg_salary
FROM
    job_postings_fact j
INNER JOIN skills_job_dim sj ON j.job_id = sj.job_id
INNER JOIN skills_dim s ON sj.skill_id = s.skill_id
WHERE
    j.job_title_short = 'Data Analyst'
    AND j.salary_year_avg IS NOT NULL
GROUP BY
    s.skills
ORDER BY
    avg_salary DESC;
   Reveals which skills are associated with the highest average salaries.
5. Optimal Skills to Learn
   WITH skills_demand AS (
    SELECT
        s.skill_id,
        s.skills,
        COUNT(sj.job_id) AS demand_count
    FROM
        job_postings_fact j
    INNER JOIN skills_job_dim sj ON j.job_id = sj.job_id
    INNER JOIN skills_dim s ON sj.skill_id = s.skill_id
    WHERE
        j.job_title_short = 'Data Analyst'
        AND j.salary_year_avg IS NOT NULL
        AND j.job_location = 'Anywhere'
    GROUP BY
        s.skill_id
),
average_salary AS (
    SELECT
        sj.skill_id,
        AVG(j.salary_year_avg) AS avg_salary
    FROM
        job_postings_fact j
    INNER JOIN skills_job_dim sj ON j.job_id = sj.job_id
    WHERE
        j.job_title_short = 'Data Analyst'
        AND j.salary_year_avg IS NOT NULL
        AND j.job_location = 'Anywhere'
    GROUP BY
        sj.skill_id
)
SELECT
    sd.skills,
    sd.demand_count,
    ROUND(as.avg_salary, 2) AS avg_salary
FROM
    skills_demand sd
INNER JOIN average_salary as ON sd.skill_id = as.skill_id
ORDER BY
    demand_count DESC,
    avg_salary DESC
LIMIT 10;
Combines demand and salary to highlight the most strategic skills for career growth.
Key Learnings

Complex Queries: Use of WITH clauses, joins, and subqueries.

Data Aggregation: Effective use of GROUP BY, COUNT(), and AVG().

Analytical Thinking: Translating real-world questions into actionable SQL queries.
Insights

Top-Paying Jobs: Remote roles can reach $650,000.

Skills for High Salaries: Advanced SQL proficiency is essential.

Most In-Demand Skills: SQL consistently ranks highest.

Specialized Skills Pay More: Skills like SVN and Solidity command top salaries.

Optimal Skills: SQL offers both high demand and high salary potential.

Conclusion

This project helped me enhance my SQL skills while uncovering actionable insights into the data analyst job market. By focusing on high-demand, high-paying skills, aspiring data analysts can strategically improve their career prospects.

Certification: Completed as part of Luke Barousse’s SQL Course
