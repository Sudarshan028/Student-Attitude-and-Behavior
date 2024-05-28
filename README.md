# Student Attitude and Behavior Analysis

This project analyzes a dataset of student attitudes and behaviors to uncover patterns and trends that impact their academic performance. The dataset contains various attributes such as gender, department, academic marks, hobbies, daily study time, salary expectations, and more. The analysis aims to provide insights into factors affecting student performance and well-being.

## Table of Contents

- [Introduction](#introduction)
- [Dataset Overview](#dataset-overview)
- [Analysis and Insights](#analysis-and-insights)
  - [Gender Distribution](#gender-distribution)
  - [Correlation Analysis](#correlation-analysis)
    - [Academic Marks Correlation](#academic-marks-correlation)
    - [Study Time and Performance](#study-time-and-performance)
    - [Degree Satisfaction and Performance](#degree-satisfaction-and-performance)
  - [Outliers in Salary Expectations](#outliers-in-salary-expectations)
  - [Daily Study Time and Performance](#daily-study-time-and-performance)
  - [Salary Expectations and Career Pursuit](#salary-expectations-and-career-pursuit)
  - [Stress Levels, Financial Status, and Performance](#stress-levels-financial-status-and-performance)
- [Conclusion](#conclusion)
- [How to Use](#how-to-use)
- [Dependencies](#dependencies)

## Introduction

This project focuses on analyzing a dataset of student attitudes and behaviors to identify factors that influence their academic performance. The dataset includes information about students' academic marks, hobbies, study habits, salary expectations, and more.

## Dataset Overview

The dataset contains 235 entries and 19 columns. Each column represents a different aspect of student life and behavior. Key columns include:
Certification Course: Whether the student has taken a certification course (Yes/No).

Gender: Gender of the student (Male/Female).

Department: Department of the student (BCA/BBA/Engineering/Science).

Height(CM): Height of the student in centimeters.

Weight(KG): Weight of the student in kilograms.

10th Mark: Percentage marks obtained in 10th grade.

12th Mark: Percentage marks obtained in 12th grade.

college mark: Percentage marks obtained in college.

hobbies: Hobbies of the student.

daily studing time: Daily studying time of the student.

prefer to study in: Preferred time of day to study.

salary expectation: Salary expectation of the student after graduation.

Do you like your degree?: Whether the student likes their degree (Yes/No).

willingness to pursue a career based on their degree: Willingness to pursue a career based on their degree.

social medai & video: Time spent on social media and video platforms.

Travelling Time: Daily travelling time of the student.

Stress Level: Stress level of the student.

Financial Status: Financial status of the student.

part-time job: Whether the student has a part-time job (Yes/No).

## Analysis and Insights

### Gender Distribution

The gender distribution in the dataset shows a significant disparity, with nearly twice as many males (156) as females (79).

```python
gen = df.groupby("Gender")["Gender"].count().plot.bar(color=("lightgreen", "skyblue"))
gen.set_title("Gender Distribution")
gen.set_xlabel("Gender")
gen.set_ylabel("Count")
for p in gen.patches:
    height = p.get_height()
    gen.text(p.get_x() + p.get_width() / 2., height + 3, '{:1.2f}'.format(height), ha="center")
plt.show()
```

### Correlation Analysis

#### Academic Marks Correlation

The correlation heatmap reveals moderate positive relationships between college marks and 10th/12th-grade marks.

```python
correlation_marks = df[['college mark', '10th Mark', '12th Mark']].corr()
sns.heatmap(correlation_marks, annot=True, cmap='coolwarm', linewidths=0.5)
plt.title("Correlation Heatmap: College vs. 10th vs. 12th Marks")
plt.show()
```

#### Study Time and Performance

Students who study anytime tend to achieve higher college marks.

```python
plt.scatter(df['prefer to study in'], df['college mark'], color='r', label='Marks')
plt.xlabel('Study Time')
plt.ylabel('College Marks')
plt.title('Impact of Study Time on College Marks')
plt.grid(True)
plt.legend()
plt.show()
```

#### Degree Satisfaction and Performance

A weak positive correlation suggests that students who like their degree tend to have slightly higher college marks.

```python
df['Degree_like'] = df['Do you like your degree?'].map({'Yes': 1, 'No': 0})
correlation_coefficient = df['Degree_like'].corr(df['college mark'])
plt.scatter(df['Degree_like'], df['college mark'], color='g', label='Marks')
plt.xlabel('Degree_Like(1 = Yes, 0 = No)')
plt.ylabel('College Marks')
plt.title('Correlation: Degree Like vs. College Marks')
plt.grid(True)
plt.legend()
plt.show()
print(f"Correlation coefficient: {correlation_coefficient:.2f}")
```

### Outliers in Salary Expectations

The box plot reveals a significant outlier—a salary expectation of ₹1,500,000 (1.5 million).

```python
salary_expectations = df["salary expectation"]
plt.boxplot(salary_expectations, vert=False, notch=True, patch_artist=True)
plt.xlabel('Salary Expectations (₹)')
plt.title('Box Plot: Salary Expectations')
plt.grid(axis='x', linestyle='--', alpha=0.7)
plt.show()
```

### Daily Study Time and Performance

Students who study for 3 to 4 hours daily tend to achieve the highest college marks.

```python
y = df.groupby("daily studing time")["college mark"].mean()
plt.plot(y.index, y.values, marker='o', color='g')
plt.xlabel('Daily Studying Time (minutes)')
plt.xticks(rotation=45)
plt.ylabel('College Marks')
plt.title('Impact of Daily Studying Time on College Marks')
plt.grid(axis='both', linestyle='--', alpha=0.7)
plt.show()
```

### Salary Expectations and Career Pursuit

Financial status influences students' willingness to pursue a career based on their degree.

```python
salarywill = df.groupby('willingness to pursue a career based on their degree')["salary expectation"].mean()
plt.bar(salarywill.index, salarywill.values, color=['skyblue', 'orange', 'green', 'purple'])
plt.xlabel('Proportion Willing to Pursue Career')
plt.ylabel('Salary Expectations')
plt.title('Salary expectations vs. Career Pursuit')
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()
```

### Stress Levels, Financial Status, and Performance

Students with good financial status tend to have the highest average college marks across all stress levels.

```python
grouped_data = df.groupby(['Stress Level', 'Financial Status'])['college mark'].mean().unstack()
x = np.arange(len(grouped_data))
width = 0.2

fig, ax = plt.subplots(figsize=(10, 6))
ax.bar(x - width, grouped_data['Bad'], width, label='Bad Stress', color='plum')
ax.bar(x, grouped_data['Awful'], width, label='Awful Stress', color='mediumorchid')
ax.bar(x + width, grouped_data['good'], width, label='Good Stress', color='indigo')

ax.set_xticks(x)
ax.set_xticklabels(grouped_data.index)
ax.set_xlabel('Financial Status')
ax.set_ylabel('Average College Marks')
ax.set_title('Average College Marks by Stress Level and Financial Status')
ax.legend()
plt.show()
```

## Conclusion

This analysis provides insights into the factors influencing student performance and well-being. Key findings include:
- Significant gender disparity in the dataset.
- Moderate positive correlations between academic marks.
- Flexible study schedules positively impact academic performance.
- Students who like their degree tend to have slightly higher marks.
- Outliers in salary expectations.
- Optimal daily study time for best performance is 3-4 hours.
- Financial status and stress levels significantly affect academic performance.

## How to Use

1. Clone the repository.
2. Ensure you have the necessary dependencies installed.
3. Load the dataset and run the analysis scripts to generate insights and visualizations.

## Dependencies

- pandas
- numpy
- seaborn
- matplotlib

Install dependencies using:

```bash
pip install pandas numpy seaborn matplotlib
```

## Author

Sudarshan

## License

This project is licensed under the MIT License.
