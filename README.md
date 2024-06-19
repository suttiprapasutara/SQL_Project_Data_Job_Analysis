# Introduction
📊 Dive into the data job market! Focusing on data scientist roles, this project explores 💰 top-paying jobs, 🔥 in-demand skills, and where 📈 high demand meets high salary in data scientist.

🔍 SQL queries? Check them out here: [project_sql folder](/project_sql/)

# Background
Driven by a quest to navigate the data scientist job market more effectively, this project was born from a desire to pinpoint top-paid and in-demand skills, streamlining others work to find optimal jobs.

### The questions I wanted to answer through my SQL queries were:

1. What are the top-paying data scientist jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data scientist?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

# Tools I Used
For my deep dive into the data scientist job market, I harnesses the power of several key tools:

- **SQL:** The backbone of my analysis, allowing me to query the database and unearth critical insights.
- **PostgreSQL:** The chosen database management system, ideal for handling the job posting data.
- **Visual Studio Code:** My go-to for database management and executing SQL queries.
- **Git & GitHub:** Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.


# The Analysis
Each query for this project aimed at investigating specific aspects of the data scientist job market.
Here's how I approached each question:

### 1. Top Paying Data Scientist Jobs
To identify the highest-paying roles, I filtered data scientist positions by average yearly salary and location, focusing on remote jobs. This query highlights the high paying opportunities in the field.

```sql
SELECT 
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Scientist' 
    AND job_location = 'Anywhere'
    AND salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10;
```

### Key Trends
**1.	High Compensation for Senior Roles:**
- The highest salaries are associated with senior positions such as **Staff Data Scientist/Quant Researcher**($550,000) and **Staff Data Scientist - Business Analytics** ($525,000), indicating that seniority and specialised expertise significantly boost earning potential.

**2.	Leadership and Management Focus:**
- Positions like **Head of Data Science** ($351,500 and $324,000), **Director Level - Product Management - Data Science** ($320,000), and **Director of Data Science & Analytics** ($313,000) show that leadership roles in data science, which combine technical skills with management responsibilities, are highly valued.

**3.	Industry and Company Influence:**
- Companies like **Selby Jennings**, **Demandbase**, and **Reddit** are willing to offer substantial salaries, suggesting that top firms in finance, marketing technology, and social media are particularly competitive in attracting top data science talent.

### Summary
The top-paying data scientist roles are characterised by seniority, leadership responsibilities, and affiliation with high-profile companies. These positions demand not only advanced technical skills but also strategic vision and management capabilities, reflecting the critical role of data science in driving business decisions and innovation. 



### 2. Skills for Top Paying Jobs
To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.

```sql
WITH top_paying_jobs AS (

    SELECT 
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Scientist' 
        AND job_location = 'Anywhere'
        AND salary_year_avg IS NOT NULL
    ORDER BY salary_year_avg DESC
    LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills 
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC; 
```

### Key Trends
**1.	Core Programming and Data Manipulation:**
- **Python** and **SQL** are essential skills across multiple high-paying roles, highlighting the necessity of proficiency in these foundational tools for data analysis and manipulation.
- Other programming languages like **Java** and **Scala** are also valued, particularly for their use in large-scale data processing and backend development.
**2.	Advanced Machine Learning and Data Science Tools:**
- Tools and frameworks such as **TensorFlow**, **PyTorch**, **scikit-learn**, and **Keras** are frequently required, indicating that expertise in machine learning and deep learning is crucial for top-paying data science positions.
- **Spark** and **Hadoop** are significant for big data processing, reflecting the importance of skills in handling and analyzing large datasets.
**3.	Cloud and Data Engineering Platforms:**
- Knowledge of cloud platforms like **AWS**, **Azure**, and **GCP** is essential, showing the demand for skills in cloud computing and data engineering.
- Additional tools like **Kubernetes** and **Cassandra** point to the necessity of skills in container orchestration and NoSQL databases.

### Summary
The top-paying data scientist roles emphasize a strong foundation in core programming languages (Python, SQL), advanced machine learning frameworks (TensorFlow, PyTorch), and cloud computing platforms (AWS, Azure). Additionally, expertise in big data tools (Spark, Hadoop) and data engineering technologies (Kubernetes, Cassandra) is highly valued. This reflects the multifaceted nature of data science, requiring a blend of coding proficiency, machine learning expertise, and cloud infrastructure knowledge.


## 3. In-Demand Skills for Data Scientists
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.

```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM 
    job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Scientist'
GROUP BY 
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```

### Key Trends

**1. Programming Languages:**

- **Python** leads with a demand count of 114,016, highlighting its dominance and versatility in data science for tasks ranging from data analysis and machine learning to web development and automation.
- **R** is also in high demand (59,754), particularly valued in academia and specialized statistical analysis, reinforcing its importance alongside Python.

**2. Data Management and Querying:**

- **SQL** is highly demanded (79,174), emphasizing the necessity for data scientists to efficiently manage and query databases. This skill is critical for data retrieval and manipulation, foundational to any data science workflow.

**3. Statistical Analysis and Business Intelligence:**

- **SAS** (29,642) and **Tableau** (29,513) indicate a strong need for expertise in statistical analysis and data visualization. SAS is known for its robust statistical capabilities, while Tableau is essential for creating impactful data visualizations and business intelligence reports.

### Summary
The top-paying data scientist roles prioritize a combination of programming proficiency (Python, R), strong database querying skills (SQL), and expertise in statistical analysis and visualization tools (SAS, Tableau). This mix of skills underscores the importance of a well-rounded technical background that can handle various aspects of data science, from data manipulation and analysis to visualization and reporting.


| **Skill**  | **Demand Count** |
|------------|------------------|
| Python     | 114,016          |
| SQL        | 79,174           |
| R          | 59,754           |
| SAS        | 29,642           |
| Tableau    | 29,513           |

*Table of the demand for the top 5 skills in data scientist job postings*


## 4. Skills Based on Salary
Exploring the average salaries associated with different skills revealed which skills are the highest paying.

```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM 
    job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Scientist'
    AND salary_year_avg IS NOT NULL
GROUP BY 
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
```


### Key Trends
**1.	Project Management and Collaboration Tools:**
- **Asana** ($215,477) and **Airtable** ($201,143) command the highest average salaries, indicating that expertise in project management and collaborative tools is highly valued.
- Other collaboration tools like **Slack** ($168,219) and **Notion** ($165,636) also feature prominently, reinforcing the importance of these skills.

**2.	Specialized and Emerging Technologies:**
- High-paying skills include specialized software and programming languages such as **Redhat** ($189,500), **Watson** ($187,417), **Elixir** ($170,824), and **Solidity** ($166,980). These skills reflect the demand for expertise in niche areas and emerging technologies.
- Tools like **Hugging Face** ($160,868) and **Theano** ($153,133) highlight the need for advanced machine learning and deep learning frameworks.

**3.	Data Management and Visualization:**
- Skills in data management and visualization tools, such as **Neo4j** ($163,971) and **Dplyr** ($163,111), are well-compensated, showcasing the value of handling complex data structures and effective data presentation.
- **Rshiny** ($166,436) and **Tableau** (from previous context) indicate the importance of creating interactive data visualizations and dashboards.


### Summary
The top-paying data scientist roles are highly influenced by skills in project management and collaboration tools (Asana, Airtable), specialized and emerging technologies (Redhat, Watson, Solidity), and data management and visualization tools (Neo4j, Rshiny). This reflects the diverse range of expertise required to excel in high-paying data science positions, combining technical proficiency, project management, and effective data presentation.



| **Skill** | **Average Salary ($)** |
|-----------|------------------------|
| Asana     | 215,477                |
| Airtable  | 201,143                |
| Redhat    | 189,500                |
| Watson    | 187,417                |
| Elixir    | 170,824                |
| Lua       | 170,500                |
| Slack     | 168,219                |
| Solidity  | 166,980                |
| Ruby on Rails | 166,500            |
| Rshiny    | 166,436                |

*Table of the average salary for the top 10 paying skills for data scientists*


## 5. Most Optimal Skills to Learn
Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic focus for skill development.

```sql
SELECT 
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM
    job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_dim.skill_id = skills_job_dim.skill_id
WHERE
    job_title_short = 'Data Scientist'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = TRUE
GROUP BY
    skills_dim.skill_id
HAVING 
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```

### Key Trends
**1. Emerging and Specialized Skills Command Higher Salaries:**
- Skills such as **C,** **Go,** and **Qlik** offer some of the highest average salaries, indicating a premium on less common but highly specialized technical skills.
- **C** has an average salary of $164,865, **Go** offers $164,691, and **Qlik** commands $164,485.

**2. Demand for Certain Skills Does Not Always Align with Salary:**
- While some skills like **Airflow** and **BigQuery** have moderate demand counts (23 and 36, respectively), they still offer high average salaries ($157,414 and $157,142).
- This trend suggests that specific skills are highly valued despite not being the most in-demand.

**3. Advanced Analytical Tools and Platforms Are Highly Valued:**
- Skills in advanced data platforms and tools such as **Airflow,** **BigQuery,** **Looker,** and **Snowflake** are associated with higher salaries, reflecting their importance in sophisticated data science workflows.

### Summary
The trends indicate that specialization in cloud platforms, proficiency in leading machine learning frameworks, and mastery of advanced data visualization tools are key factors influencing high-paying salaries in the data science field. As companies increasingly rely on data-driven insights, these skills continue to be in high demand, reflecting their critical role in leveraging data for business success.


| **Skill**   | **Demand Count** | **Average Salary ($)** |
|-------------|------------------|------------------------|
| C           | 48               | 164,865                |
| Go          | 57               | 164,691                |
| Qlik        | 15               | 164,485                |
| Looker      | 57               | 158,715                |
| Airflow     | 23               | 157,414                |
| BigQuery    | 36               | 157,142                |
| Scala       | 56               | 156,702                |
| GCP         | 59               | 155,811                |
| Snowflake   | 72               | 152,687                |
| PyTorch     | 115              | 152,603                |

*Table of the most optimal skills for data scientist sorted by salary*




# What I Learned
Throughout this adventure, I've turbocharged my SQL toolkit with some serious firepower:

- 🧩 **Complex Query Crafting:** Mastered the art of advanced SQL, merging tables like a pro and wielding WITH clauses for ninja-level temp table maneuvers.
- 📊 **Data Aggregation:** Got cozy with GROUP BY and turned aggregate functions like COUNT() and AVG() into my data-summarizing sidekicks.
- 💡 **Analytical Wizardry:** Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.

# Conclusions

### Insights

From the analysis, several general insights emerged:

**1. Top-Paying Data Scientist Jobs:** The highest-paying jobs for data scientists that allow remote work offer a wide range of salaries, the highest at $550,000!

**2. Skills for Top-Paying Jobs:** High-paying data scientist jobs require advanced proficiency in SQL, suggesting it’s a critical skill for earning a top salary.

**3. Most In-Demand Skills:** Python is the most demanded skill in the data scientist job market, thus making it essential for job seekers.

**4. Skills with Higher Salaries:** Specialized skills, such as Asana and Airtable, are associated with the highest average salaries, indicating a premium on niche expertise.

**5. Optimal Skills for Job Market Value:** 


### Closing Thoughts

This project enhanced my SQL skills and provided valuable insights into the data scientist job market. The findings from the analysis serve as a guide to prioritising skill development and job search efforts. Aspiring data scientists can better position themselves in a competitive job market by focusing on high-demand, high-salary skills. This exploration highlights the importance of continuous learning and adaptation to emerging trends in the field of data science. 


