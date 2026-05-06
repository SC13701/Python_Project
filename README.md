# Introduction
The study focuses on analysing the data job market in India, with a primary emphasis on Data Analyst roles. Using datasets containing job postings, skills, salaries, and trends, the objective is to:

 - Understand demand for different job roles
 - Identify the most important and trending skills
 - Analyse salary distributions across roles
 - Determine optimal skill combinations for career growth

This kind of analysis is especially useful for students and professionals aiming to enter or grow in the data domain.

The data for the study is sourced from [Luke Barousse's Dataset on Top Data Jobs](https://huggingface.co/datasets/lukebarousse/data_jobs), which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills.

Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics (analysis conducted only for India).

###### All analysis is conducted from the above given dataset, which may not accurately reflect the current job scenario.

## Objective

The dataset includes data on the various types of jobs in the data industry across multiple nations and the various skills necessary for each job.
The main objective of the study is to identify the jobs in India, specifically, along with the various skills prioritised by the various companies for the corresponding job profile.
The study thoroughly identifies the salary with respect to the job profile, and a rough estimation of the skills in demand and the salary attained with respect to the skills in question.

# Analysis

Given that raw data was taken from the Hugging Face web servers, the data needed to be cleaned and processed before any analysis could be conducted.

### Initial Clean-up and processing

Before cleaning, all the important libraries along with the dataset are imported, followed by a basic cleanup of the date-time of the dataset, given that the date-time values of the database are in string format.
The job_skills column in the dataset is also converted into a list in order to access each individual item in the list (a single job can have multiple skills).

```python
import pandas as pd
import numpy as np
from datasets import load_dataset
import matplotlib.pyplot as plt
from matplotlib.ticker import PercentFormatter
import ast
import seaborn as sns

# Load the dataset and convert it to a pandas DataFrame
dataset = load_dataset("lukebarousse/data_jobs")
df = dataset["train"].to_pandas()

# Clean the date and time columns
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_posted_date'] = df['job_posted_date'].dt.tz_localize('UTC')
df['job_skills'] = df['job_skills'].apply(lambda skill_list: ast.literal_eval(skill_list) if pd.notna(skill_list) else [skill_list])
```

### Data segregation based on country and job

In this project, the data is filtered for jobs in India and data analyst jobs. Hence, new dataframes are generated in order to study the data for jobs in India and data analyst jobs in India.

```python
# Filter the DataFrame for Data Analyst jobs in India
df_DA_India = df[df['job_title'].str.contains('Data Analyst', case=False, na=False) & (df['job_country'] == 'India')]

# Filter the DataFrame for jobs in India
df_India = df[df['job_country'] == 'India'].copy()
```
Using these dataframes, the following trends were plotted and studied.

### Exploratory Analysis

Before diving into analysing the scenario of data analyst jobs in India, a basic representation of the various jobs in the field of data study is provided in the graphs below:
![Popular Jobs and Work Format](Project_Images\Popular_Jobs_and_Work_Format.png)
Based on the above given graph, Data Analyst jobs are the most popular, with a majority of the workforce being full-time employees, with very few contractors and interns.

![Top Job Locatioins](Project_Images\Top_Job_Destinations.png)
The above graph tries to filter the distribution of the majority of the Data Analyst jobs in India; hence, the further studies conducted on this field specifically.

![Job Requirements and Benefits](Project_Images\Job_Requirements_and_Benefits.png)
Further study into the graph displays that 17.2% of the jobs are remote, with almost 36% not requiring a degree to be an employee. However, a concerning display of no health insurance based on the provided dataset.

![Top Companies](Project_Images\Top_Companies_Hiring_in_India.png)
Displaying the proportion of job offers from the top hiring companies.

Analysis for the above study can be viewed here: [1_Exploratory_Data_Analysis](1_Exploratory_Data_Analysis.ipynb)

### Skill Analysis
Although Data Analysis is the top-most sought-after job in India.

![Top skills for Data-based jobs](Project_Images\Top_Skills_Data_Analyst_Scientist_Engineer.png)
The study on the most important skills for careers in Data Analysis, Engineering, and Science displays that SQL and Python are among the most common skills requested from all three job portfolios.

Analysis for the above study can be viewed here: [2_Skill_Count](2_Skill_Count.ipynb)

### Trend of Skill Requirement
A study on the relevance of the skill over the year is conducted to understand the requirements of the companies, helping better streamline the approach for future endeavours.

![Skill trend over the year](Project_Images\Skill_Trend_year.png)
Based on the study conducted, it can be understood that SQL is a core skill that is sought by most companies, while Python and Excel are just as important.

Analysis for the above study can be viewed here: [3_Skill_Trends](3_Skill_Trends.ipynb)

### Salary Analysis
The main objective behind any job is to get good pay. This study dives into the salary (in USD) of the various job profiles in India and how a skill matters in regards the pay.

![Average Salary](Project_Images\Average_Salary_Data_Jobs.png)
The above graph displays the distribution of salary based on the different data-based jobs, with the highest average salary obtained by senior data engineers.

![Average salary based on skill](Project_Images\Salary_Skills.png)
The above study also suggests that the skills an employee has may affect their overall salary, and that the most popular skills may not be the highest-paying skills.

Analysis for the above study can be viewed here: [4_Salary_Analysis](4_Salary_Analysis.ipynb)

### Optimal Skill Analysis
This study determines the most optimal approach to learning skills, considering the job market provided in the study.

![Skill comparison based on Job availibility and Skill requirement](Project_Images\Skill_Requirement_Salary_vs_Job_Ratio.png)

Analysis for the above study can be viewed here: [5_Optimal_Skills](5_Optimal_Skills.ipynb)

# Challenges
During the course of this study, several challenges were encountered that impacted data processing, analysis, and interpretation:

### A. Data Quality Issues
Some job postings contained missing or null values, especially in salary and skill fields
Inconsistent formatting of data (e.g., skills stored as strings instead of lists) required preprocessing

#### Impact:
Additional data cleaning steps were necessary, increasing the overall complexity of the analysis.

### B. Unstructured Skill Data
Skills were often stored as string representations of lists
Required conversion using parsing techniques (e.g., ast.literal_eval)
Variations in naming (e.g., “Power BI” vs “PowerBI”) caused duplication issues

#### Impact:
Extra effort was needed to standardise and normalise skill data before analysis.

### C. Temporal Analysis Limitations
Skill trends depend heavily on time-stamped data availability
Gaps or inconsistencies in posting dates affected trend accuracy

#### Impact:
Trend analysis may not fully capture real-time market dynamics.

### D. Tool and Environment Challenges
Issues with Python libraries (e.g., warnings, deprecations in Matplotlib/Seaborn)
IDE-related problems (e.g., VS Code configuration, kernel delays)

#### Impact:
Slowed down workflow and required troubleshooting during development.

### E. Interpretation Complexity
High demand for a skill does not always equate to a high salary
Multiple factors (experience, company, domain) influence outcomes

####  Impact:
Required careful interpretation to avoid misleading conclusions.

### Summary of Challenges

Despite these challenges, appropriate preprocessing, filtering, and analytical techniques were applied to ensure the study remains reliable and insightful. Addressing these limitations also highlights areas for improvement in future analyses.

# Inference and Conclusion
From the analysis, several key conclusions can be drawn:

 - Data Analyst roles dominate the Indian data job market
 - Skill-based hiring is the primary driver
 - Python and SQL are foundational skills
 - Emerging tools and technologies are gaining importance
 - Salary is strongly correlated with specialisation
 - Learning isolated skills is less effective than mastering ecosystems

The study provides a clear roadmap for aspiring data professionals:

 - Focus on core skills (Python, SQL, visualisation tools)
 - Build expertise in complete technology stacks
 - Stay updated with emerging trends
 - Aim for skill combinations that maximise both demand and salary potential

Overall, the analysis highlights that success in the data field is driven by a combination of:

 - Technical proficiency
 - Market awareness
 - Continuous learning

Overall, the above study conducted displays my understanding of the various Python libraries and how to navigate the dataset to narrow the field of study and filter data for the specifications required.
