# over view
Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://youtu.be/wUSDVGivd-8?si=8OLauN7U6IfRRGb1) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.
# The Questions
Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# Tools I Used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

1. Python: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
   1. Pandas Library: This was used to analyze the data.
   2. Matplotlib Library: I visualized the data.
   3. Seaborn Library: Helped me create more advanced visuals.
2. Jupyter Notebooks: The tool I used to run my Python scripts which let me easily include my notes and analysis.
3. Visual Studio Code: My go-to for executing my Python scripts.
4. Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.
 
# Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

# Import & Clean Up Data 
``` python
import pandas as pd
from datasets import load_dataset
import matplotlib.pyplot as plt
import ast
import seaborn as sns
dataset=load_dataset('lukebarousse/data_jobs')
df=dataset['train'].to_pandas()
df['job_posted_date']=pd.to_datetime(df['job_posted_date'])
df['job_skills']=df['job_skills'].apply(lambda skill: ast.literal_eval(skill)if pd.notna(skill)else skill)
```
# Filter US Jobs
``` python 
df_US = df[df['job_country'] == 'United States']
```
# The Analysis
Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:
# 1. What are the most demanded skills for the top 3 most popular data roles?
o find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: 
[2_skills_Demand.ipynb](2_skills_Demand.ipynb)


# Visualize Data
``` python
fig,ax=plt.subplots(len(job_titles),1)
sns.set_theme(style='ticks')
for i,job_title in enumerate(job_titles):
     df_plot=df_skills_perc[df_skills_perc['job_title_short']==job_title].head(5)
     sns.barplot(data=df_plot,x='skill_perc',y='job_skills',ax=ax[i],hue='skill_count',palette="dark:b_r")
     ax[i].set_title(job_title)
     ax[i].set_ylabel('')
     ax[i].set_xlabel('')
     ax[i].get_legend().remove()
     ax[i].set_xlim(0,100) 
     if i != len(job_titles) - 1:
        ax[i].set_xticks([])
     for n,v in enumerate(df_plot['skill_perc']):
          ax[i].text(v+1,n,f'{v:.0f}%',va='center') 

fig.suptitle('likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5) 
plt.show()
```
### results

![Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.](images/likelihood%20image.png)


### Insights:
1. **Python and SQL** are key skills for all data roles.
2. **Data Analysts** focus more on **Excel** and **Tableau**.
3. **Data Engineers** need **cloud and big data tools** like **AWS, Azure, and Spark**.


# 2. How are in-demand skills trending for Data Analysts?

o find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here:[3_skills_Trend](3_skills_Trend.ipynb)

## Visualize Data
``` python
from matplotlib.ticker import PercentFormatter
df_plot=df_DA_US_percent.iloc[:,:5]
sns.lineplot(data=df_plot,dashes=False,palette='Set1')
sns.set_theme(style='ticks')
sns.despine()
plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))
for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i], color='black')

plt.show() 
```
## Result
![trend_skills_image](images\skilltrend.png)

## insights
1. SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
1. Excel experienced a significant increase in demand starting around September, surpassing both    Python and Tableau by the end of the year.
1. Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts. Power BI, while less demanded compared to the others, shows a slight upward trend towards the year's end.

# 3.How well do jobs and skills pay for Data Analysts?
o identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here:[4_salary_analysis](4_salary_anlysis.ipynb)

## Visualize Data
```python
sns.boxplot(data=df_topjobs,x='salary_year_avg',y='job_title_short',order=job_order)
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x,pos:f"${x/1000:.0f}k"))
plt.xlim(0,700000)
plt.title("salary Distributions of data jobs in the us")
plt.xlabel("median salary ")
plt.ylabel(" ")
plt.show()
```
![boxplot](images\boxplot.png)

## insghts

1. **Senior roles earn significantly more:**
   Senior Data Scientists and Senior Data Engineers have the highest median salaries among all data roles.

2. **Wide salary variation:**
   All roles show large salary ranges and many outliers, especially for Data Scientists and Data Engineers, indicating high earning potential for top performers.

3. **Analyst roles earn less:**
   Data Analysts and Senior Data Analysts have noticeably lower median salaries compared to engineering and science roles.

# Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

## Visualize Data
```python 
fig, ax = plt.subplots(2, 1)  
sns.set_style(style='ticks')
# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_top_pay, x='median', y=df_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')
ax[0].legend().remove()

ax[0].set_title('Highest Paid Skills for Data Analysts in the US')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))


# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_count, x='median', y=df_count.index, hue='median', ax=ax[1], palette='light:b')
ax[1].legend().remove()
# original code:
ax[1].set_title('Most In-Demand Skills for Data Analysts in the US')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(ax[0].get_xlim())  # Set the same x-axis limits as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

sns.set_theme(style='ticks')
plt.tight_layout()
plt.show()
```
![deman](images\most%20demans.png)
## insights
 1. Rare Skills = Higher Pay
Skills like dplyr, bitbucket, and solidity offer the highest median salaries—often above $150K. These tools aren't used by every data analyst, but companies pay more for people who master them because they’re harder to find.

2.  Common Skills = Higher Demand
Skills like SQL, Excel, and Python appear in almost every job listing. They're essential for most roles, which makes them highly in demand—but because so many people know them, the salaries are usually lower.

 3. Demand Doesn’t Equal Salary
Just because a skill is popular doesn’t mean it pays the most. For example, SQL is the most in-demand skill but doesn’t appear in the highest-paid list. Meanwhile, hugging face and mxnet offer high salaries but aren’t widely requested.

# 4. What are the most optimal skills to learn for Data Analysts?
To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

View my notebook with detailed steps here:[5_optimal_skills](5_optimal_skills.ipynb)

## Visualize Data
```python
from adjustText import adjust_text

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary ($USD)')  # Assuming this is the label you want for y-axis
plt.title('Most Optimal Skills for Data Analysts in the US')

# Get current axes, set limits, and format axes
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))  # Example formatting y-axis

# Add labels to points and collect them in a list
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], " " + txt))

# Adjust text to avoid overlap and add arrows
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

plt.show()
```
## result 
![scatter1](images\scatter.png)

## insights
1. Python Offers the Best Pay
Among all skills, Python has the highest median salary (~$97K), making it a top choice for maximizing income as a data analyst.

2. SQL Is the Most Common Skill
SQL appears in the largest percentage of job listings (~14%), showing it's the most in-demand tool—even if its salary is slightly lower than Python.

3. Excel Is Widely Used but Pays Less
Although Excel is used in many jobs ($85K), suggesting it’s essential but not highly valued financially.

# Visualizing Different Techonologies
Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

## Visualize Data

```python
sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)
```
## result
![scatter2](images\scatter2.png)

# What I Learned
Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

1. Advanced Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
1. Data Cleaning Importance: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
1. Strategic Skill Analysis: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.
# Insights

This project provided several general insights into the data job market for analysts:

1. Skill Demand and Salary Correlation: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
1. Market Trends: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
1. Economic Value of Skills: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.
# Challenges I Faced
This project was not without its challenges, but it provided good learning opportunities:

1. Data Inconsistencies: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
1. Complex Data Visualization: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
1. Balancing Breadth and Depth: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.
# Conclusion
This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.
