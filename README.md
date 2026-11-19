# MIS 311 – Introduction to Business Analytics

| **Student Name** | Huỳnh Ngọc Kim Ngân                          |
|------------------|----------------------------------------------|
| **Subject**      | MIS 311 – Introduction to Business Analytics |
| **Assessment**   | Assignment \#1: Personal Portfolio Site      |
| **Dataset**      | Employee Performance                         |
| **Tools Used**   | Python – pandas, matplotlib, seaborn         |

# 1. Data Overview

## 1.1 Dataset Background

This dataset holds HR records for a company with three departments (HR, IT and Sales) spread over three locations: Los Angeles, Chicago and New York. Each row is one employee. For every person we have basic details such as age and gender, job information (department, salary, joining date, years of experience and working shift) and a performance score from 1 to 5. I used it to look at how pay, performance and employment status relate to each other. The analysis was done in Python, using pandas for data handling (McKinney, 2022; Pandas Development Team, 2024) and matplotlib (Hunter, 2007) and seaborn (Waskom, 2021) for the charts.

| **Rows (raw)**  | 203                                                           |
|-----------------|---------------------------------------------------------------|
| **Columns**     | 12                                                            |
| **Source**      | Employee Performance data file (02_Employee Performance.xlsx) |
| **Departments** | HR, IT, Sales                                                 |
| **Locations**   | Los Angeles, Chicago, New York                                |

<img src="./media/image1.png" width="492">

## 1.2 Column Descriptions

| **Column**            | **Type**                    | **Description**                                                                                   |
|-----------------------|-----------------------------|---------------------------------------------------------------------------------------------------|
| **ID**                | int64 (identifier)          | Unique number that tells employees apart. It is a label, so it is not used in any calculation.    |
| **Name**              | object (text)               | Full name of the employee.                                                                        |
| **Age**               | int64 (numerical)           | Age of the employee, in years.                                                                    |
| **Gender**            | object (nominal)            | Female, Male or Other.                                                                            |
| **Department**        | object (nominal)            | Department the employee works in: HR, IT or Sales.                                                |
| **Salary**            | int64 (numerical)           | Compensation paid to the employee.                                                                |
| **Joining Date**      | datetime64                  | Date the employee started working at the company.                                                 |
| **Performance Score** | float64 (ordinal)           | Performance rating from 1 to 5. A higher score means better performance. Many values are missing. |
| **Experience**        | int64 (numerical, discrete) | Number of years of working experience.                                                            |
| **Status**            | object (nominal)            | Working status of the employee: Active or Inactive.                                               |
| **Location**          | object (nominal)            | Office location: Los Angeles, Chicago or New York.                                                |
| **Session**           | object (nominal)            | Working shift: Morning, Evening or Night.                                                         |

<img src="./media/image2.png" width="624">

# 2. Data Cleaning

## 2.1 Missing Values

Only one column has missing values: Performance Score, with 108 blanks out of 203 rows (53.2%). Every other column is complete.

With more than half of the scores missing, filling them in with the mean or median would mean inventing most of the ratings, and everything would get pulled towards the middle. Dropping the rows was not a good option either, since those employees still have valid age, salary and department data. So I left the missing scores as blanks and let pandas skip them whenever a performance figure is calculated. The trade-off is that every result about performance is based on a smaller group, 94 employees after cleaning.

<img src="./media/image3.png" width="290"><img src="./media/image4.png" width="323">

## 2.2 Duplicate Rows

There were 4 exact duplicate rows. The employees with ID 7, 8, 27 and 176 each appeared twice, with identical values in every column, which points to data entry errors rather than real records. I kept the first copy of each and removed the extra ones, so the dataset went from 203 to 199 rows.

<img src="./media/image5.png" width="624">

<img src="./media/image6.png" width="624">

## 2.3 Consistency Check

I also checked whether Age and Experience make sense together. For 48 employees (24.1%), the years of experience are higher than their age minus 18, which means they would have started working before turning 18. That is a fairly large share. Some of it may be part-time or early work, so I flagged these rows but kept them. It is worth remembering when reading anything about experience later on.

<img src="./media/image7.png" width="624">

| **Issue**                           | **Rows Affected**     | **Action**                             |
|-------------------------------------|-----------------------|----------------------------------------|
| **Missing Performance Score**       | 108 rows (53.2%)      | Kept as blank, skipped in calculations |
| **Duplicate rows**                  | 4 rows                | Removed                                |
| **Age vs Experience inconsistency** | 48 rows (24.1%)       | Flagged, kept in the data              |
| **Final clean dataset**             | 199 rows x 12 columns | Used for all analysis below            |

# 3. Descriptive Statistics

## 3.1 Numerical Variables

| **Statistic**    | **Age** | **Salary** | **Experience** | **Performance Score** |
|------------------|---------|------------|----------------|-----------------------|
| **Count**        | 199     | 199        | 199            | 94                    |
| **Mean**         | 39.40   | 5,621.85   | 10.14          | 2.96                  |
| **Std Dev**      | 14.53   | 2,280.33   | 5.66           | 1.38                  |
| **Min**          | 18      | 2,025      | 1              | 1                     |
| **25%**          | 27      | 3,694.50   | 6              | 2                     |
| **50% (Median)** | 37      | 5,432      | 10             | 3                     |
| **75%**          | 51.50   | 7,595.50   | 15             | 4                     |
| **Max**          | 65      | 9,973      | 20             | 5                     |

*Table 1: Descriptive statistics for the numerical variables*

<img src="./media/image8.png" width="487">

Employees are between 18 and 65 years old, with a mean of 39.4 and a median of 37, so the workforce leans a bit young. Salary runs from 2,025 to 9,973 and averages about 5,622. The standard deviation is large (2,280), so pay differs a lot from person to person. Experience goes from 1 to 20 years with an average of about 10. The histograms are fairly flat with a slight right skew for Age and Salary, and the boxplots show no extreme outliers.

| <img src="./media/image9.png" width="624"> |
|-----------------------------------------------------------------------|

*Figure 1: Distribution of Age, Salary and Experience*

Performance Score is rated from 1 to 5 with a mean of 2.96 and a median of 3, based on the 94 employees who have a score. The most common scores are 2 (24 employees) and 4 (22 employees), which together cover about half of the rated staff. A score of 3 is the least common (15 employees), so the ratings are spread out and there is no clear typical score in the middle.

| <img src="./media/image10.png" width="624"> |
|------------------------------------------------------------------------|

*Figure 2: Performance Score distribution*

## 3.2 Categorical Variables

| **Gender** | **Department** | **Status**    | **Location**    | **Session** |
|------------|----------------|---------------|-----------------|-------------|
| Other: 72  | Sales: 70      | Inactive: 101 | Los Angeles: 70 | Evening: 73 |
| Female: 64 | IT: 66         | Active: 98    | Chicago: 66     | Night: 69   |
| Male: 63   | HR: 63         |               | New York: 63    | Morning: 57 |

*Table 2: Number of employees in each category*

<img src="./media/image11.png" width="624">

Employees are spread quite evenly across the categorical variables. Each department, location and gender group holds roughly a third of the staff (63 to 72 people), and Status is almost a perfect split with 101 inactive and 98 active employees. The only slightly uneven variable is Session, where Morning has the fewest employees (57) compared with Evening (73). Since the groups are similar in size, comparing them in the next sections is fair.

| <img src="./media/image12.png" width="624"> |
|------------------------------------------------------------------------|

*Figure 3: Employees by categorical variable*

## 3.3 Relationships Between Variables

The correlations between the numerical variables are all very weak. The strongest one is between Experience and Performance Score, and it is only 0.13. Salary is almost unrelated to everything else: −0.02 with Age, 0.08 with Experience and 0.00 with Performance Score. In other words, older or more experienced employees are not paid noticeably more, and neither are the employees with higher scores.

| <img src="./media/image13.png" width="624"> |
|------------------------------------------------------------------------|

*Figure 4: Correlation matrix of the numerical variables*

| **Department** | **Avg Salary** | **Avg Performance Score** | **Avg Experience** |
|----------------|----------------|---------------------------|--------------------|
| **HR**         | 5,556.81       | 2.85                      | 10.81              |
| **IT**         | 5,838.17       | 3.22                      | 9.12               |
| **Sales**      | 5,476.44       | 2.85                      | 10.50              |

*Table 3: Average salary, performance and experience by department*

<img src="./media/image14.png" width="624">

| **Status**   | **Avg Salary** | **Avg Performance Score** | **Avg Experience** | **Avg Age** |
|--------------|----------------|---------------------------|--------------------|-------------|
| **Active**   | 5,351.37       | 2.98                      | 9.60               | 38.38       |
| **Inactive** | 5,884.31       | 2.94                      | 10.66              | 40.39       |

*Table 4: Average values by status*

<img src="./media/image15.png" width="624">

IT has the highest average salary (5,838) and also the highest average performance score (3.22), while HR and Sales are identical on performance (2.85). The salary gap between departments is small though, under 7% between IT and Sales. By status, inactive employees earn about 10% more on average than active ones (5,884 against 5,351) and have around a year more experience, yet their performance scores are almost the same (2.94 against 2.98).

| <img src="./media/image16.png" width="624"> |
|------------------------------------------------------------------------|

*Figure 5: Average salary and performance by department, and salary by status*

## 3.4 Age Structure

| **Age Group** | **Employees** | **Share (%)** |
|---------------|---------------|---------------|
| **18-29**     | 62            | 31.2          |
| **30-39**     | 52            | 26.1          |
| **40-49**     | 29            | 14.6          |
| **50-59**     | 30            | 15.1          |
| **60-65**     | 26            | 13.1          |

*Table 5: Employees by age group*

<img src="./media/image17.png" width="624">

The company is young. About 57% of employees are under 40 (31.2% + 26.1%), and the 18-29 group is the largest on its own. Average salary is similar across most age groups, roughly 5,650 to 5,900, except the 40-49 group which sits lower at around 4,750. The share of inactive employees also grows with age: it stays around half up to the 40-49 group, then rises to roughly 53% for 50-59 and about 62% for 60-65.

| <img src="./media/image18.png" width="624"> |
|------------------------------------------------------------------------|

*Figure 6: Age structure of employees*

## 3.5 Salary

| **Salary Band** | **Employees** | **Share (%)** |
|-----------------|---------------|---------------|
| **2-4k**        | 63            | 31.7          |
| **4-6k**        | 51            | 25.6          |
| **6-8k**        | 43            | 21.6          |
| **8-10k**       | 42            | 21.1          |

*Table 6: Employees by salary band (the shares are the counts divided by 199)*

Salaries are concentrated at the lower end. 63 employees (about 32%) earn between 2,000 and 4,000, and the number of employees drops with each higher band, down to 42 in the 8,000-10,000 band. Average salary is nearly the same whatever the performance score, between about 5,200 and 5,600, and employees with a score of 5 are actually among the lowest paid on average. Across departments the salary ranges overlap a lot. IT has a higher upper quartile (roughly 8,100 compared with about 7,200 for HR and Sales), but the medians are all close.

| <img src="./media/image19.png" width="624"> |
|------------------------------------------------------------------------|

*Figure 7: Salary distribution and its link with performance and department*

# 4. Key Insights

## Insight 1: Pay Does Not Follow Performance, Age or Experience

| <img src="./media/image20.png" width="400"><img src="./media/image21.png" width="440"> |
|------------------------------------------------------------------------------------------------------------------------------------------------------|

*Figure 8: Average salary by performance score and correlation matrix*

Salary has almost no relationship with any other numerical variable. The correlation between Salary and Performance Score is 0.00, and the average salary stays between about 5,200 and 5,600 across all five score levels. Employees rated 5 earn slightly less on average than those rated 4 (around 5,150 against 5,600). The same is true for Age (−0.02) and Experience (0.08), so getting older or staying longer at the company barely moves pay. One caution: performance scores exist for only 94 of the 199 employees, so this finding rests on a limited sample.

**Business Implication:** The current pay structure does not seem to reward good performance or experience. That can demotivate strong performers and make them easier to lose. Management could look at clearer salary bands or performance-linked bonuses, and check whether pay is mostly decided by something outside this data, for example negotiation at the time of hiring. It would also help to complete the missing performance ratings, because without them the link cannot be tested properly.

## Insight 2: A Young Workforce, but Inactive Status Is Highest in the Oldest Group

| <img src="./media/image22.png" width="624"> |
|------------------------------------------------------------------------|

*Figure 9: Employees by age group and status by age group*

Most employees are young: about 57% are under 40, and the 18-29 group is the biggest. But the share of inactive employees rises with age and reaches roughly 62% in the 60-65 group, compared with around half in the younger groups. Inactive employees are also the better paid and more experienced ones, earning about 10% more on average than active employees (5,884 against 5,351) with an average of 10.66 years of experience against 9.60.

**Business Implication:** If "Inactive" means people who are leaving or disengaged, the company is losing its more experienced and higher-paid staff first, along with the knowledge they carry. Succession planning and mentoring that pairs younger employees with senior ones would help, and so would retention programmes aimed at employees over 50. As a next step, the company should find out why employees become inactive, for example through exit interviews or a short survey.

# 5. References

Hunter, J. D. (2007). Matplotlib: A 2D graphics environment. Computing in Science & Engineering, 9(3), 90-95. https://doi.org/10.1109/MCSE.2007.55

McKinney, W. (2022). Python for data analysis (3rd ed.). O'Reilly Media.

Pandas Development Team. (2024). pandas documentation (v2.2). https://pandas.pydata.org/docs/

Waskom, M. (2021). Seaborn: Statistical data visualization. Journal of Open Source Software, 6(60), 3021. https://doi.org/10.21105/joss.03021
