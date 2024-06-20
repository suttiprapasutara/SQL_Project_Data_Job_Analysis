# Introduction

📊 Explore the data job market! This project delves into data scientist roles, highlighting 💰 the highest-paying jobs, 🔥 the most sought-after skills, and where 📈 high demand intersects with high salaries for data scientists.

🔍 SQL queries? Check them out here: [project_sql folder](/project_sql/)

# Background
Motivated by the need to better navigate the data scientist job market, this project aims to identify top-paid and in-demand skills, simplifying the job search process for others to find optimal positions.

### The questions I wanted to answer through my SQL queries were:

1. What are the top-paying data scientist jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data scientist?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

# Tools I Used
For my in-depth exploration of the data scientist job market, I utilized several key tools:

- **SQL:** The backbone of my analysis, enabling me to query the database and extract critical insights.
- **PostgreSQL:** The selected database management system, perfect for handling job posting data.
- **Visual Studio Code:** My preferred tool for database management and executing SQL queries.
- **Git & GitHub:** Crucial for version control and sharing SQL scripts and analyses, ensuring collaboration and project tracking.


# The Analysis
Each query for this project was designed to explore specific aspects of the data scientist job market. Here’s my approach for each question:

### 1. Top Paying Data Scientist Jobs
To identify the highest-paying roles, I filtered data scientist positions by average yearly salary and location, with a focus on remote jobs. This query highlights the lucrative opportunities available in the field.

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
To determine the skills required for top-paying jobs, I joined job postings with skills data, revealing insights into what employers value for high-compensation roles.

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
)

SELECT 
    top_paying_jobs.*,
    skills 
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC
LIMIT 10; 
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
    COUNT(skills_job_dim.job_id) > 100
ORDER BY
    demand_count DESC,
    avg_salary DESC
LIMIT 10;
```

### Key Trends
**1.	High Demand for Core Programming Languages:**
- **Python** and **SQL** are the most in-demand skills, with demand counts of 763 and 591 respectively.
- These skills also command competitive average salaries, with Python at $143,828 and SQL at $142,833.

**2.	Emerging Importance of Cloud and Big Data Technologies:**
- **AWS**, **Azure**, and **Spark** are notable skills with significant demand and high salaries.
- Spark has an average salary of $150,188, AWS offers $149,630, and Azure stands at $142,306.

**3.	AI and Machine Learning Frameworks Offer Highest Salaries:**
- **TensorFlow** and **PyTorch**, key machine learning frameworks, have the highest average salaries at $151,536 and $152,603 respectively.
- Despite their lower demand counts compared to Python and SQL, these skills are highly lucrative.


### Summary
In the field of data science, Python and SQL remain essential skills due to their high demand and solid average salaries. However, there is a growing importance of cloud services (AWS, Azure) and big data technologies (Spark), which also offer competitive salaries. Machine learning frameworks such as TensorFlow and PyTorch stand out by offering the highest average salaries, reflecting their critical role and specialized nature in the industry. As such, aspiring data scientists should consider balancing foundational skills with advanced, niche technologies to maximize their career prospects and earning potential.



| Skill ID | Skills     | Demand Count | Average Salary ($) |
|----------|------------|--------------|--------------------|
| 1        | Python     | 763          | 143,828            |
| 0        | SQL        | 591          | 142,833            |
| 5        | R          | 394          | 137,885            |
| 182      | Tableau    | 219          | 146,970            |
| 76       | AWS        | 217          | 149,630            |
| 92       | Spark      | 149          | 150,188            |
| 99       | TensorFlow | 126          | 151,536            |
| 74       | Azure      | 122          | 142,306            |
| 101      | PyTorch    | 115          | 152,603            |
| 93       | Pandas     | 113          | 144,816            |

*Table of the most optimal skills for data scientist sorted by salary*




# What I Learned
Throughout this adventure, I've significantly boosted my SQL skills with some powerful tools:

- 🧩 **Complex Query Crafting:** Mastery of advanced SQL techniques includes seamlessly merging tables and using WITH clauses for sophisticated temporary table operations.
- 📈 **Data Aggregation:** Proficiency in GROUP BY operations and utilizing aggregate functions like COUNT() and AVG() to effectively summarize data.
- 🌟 **Analytical Wizardry:** Enhanced skills in transforming complex questions into actionable insights through SQL queries.

# Conclusions

### Insights

From the analysis, several overarching insights became apparent:

**1. Top-Paying Data Scientist Jobs:** The analysis reveals that remote data scientist roles offer a diverse range of salaries, with the highest reaching an impressive $550,000!

**2. Skills for Top-Paying Jobs:** High-paying data scientist positions emphasize advanced proficiency in SQL and Python, highlighting these as critical skills for achieving top salaries.

**3. Most In-Demand Skills:** Python emerges as the most sought-after skill in the data scientist job market, underscoring its essential importance for job seekers.

**4. Skills with Higher Salaries:** Specialized skills like Asana and Airtable are linked to the highest average salaries, emphasizing the premium placed on niche expertise.

**5. Optimal Skills for Job Market Value:** Python and SQL lead in demand and offer high average salaries, making them among the most optimal skills for data scientists to learn in order to maximize their market value.


### Closing Thoughts

This project has not only boosted my SQL skills but also yielded valuable insights into the data scientist job market. The analysis findings serve as a roadmap for prioritizing skill development and optimizing job search efforts. Aspiring data scientists can strengthen their position in a competitive job market by focusing on skills that are both in high demand and command high salaries. This exploration underscores the significance of continuous learning and adapting to evolving trends in the dynamic field of data science. 


