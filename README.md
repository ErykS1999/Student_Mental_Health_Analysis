# The step by step process of creating my Student Mental Health Analysis project:

1- First step is to import the libraries such as Pandas, Numpy, Seaborn, Matplotlib in this case as well as read both of the csv files. 
  - I have also used the .describe() in order to check for any inconsistent values that may occur in the dataset.
  - It was also important for me to create a copy of my file in order to track back to the original file if some mistake was made. 

  ```
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
import numpy as np

data.describe()

  ```
2- The second step wasto rename inappropriate columns. 
  ```
data = data.rename(columns={'SymptomFrequency_Last7Days':'Symptom Frequency','StudyHoursPerWeek':'Study Hours','StudyStressLevel':'Stress Level','HasMentalHealthSupport':'Support','PanicAttack':'Panic Attack','SpecialistTreatment':'Treatment','SleepQuality':'Sleep Quality','YearOfStudy':'Study Year','Stress_level':'Stress','Study_hours':'Study Hours'})
  ```

3- The next step was to find out any missing values that exist in dataset. Luckily in this case, there were no missing values


  ```
data.isnull().sum()


  ```

<img width="212" alt="Screenshot 2025-05-18 at 13 27 40" src="https://github.com/user-attachments/assets/3d4a946a-c192-4873-b877-3996441cb432" />


4- After finding out the amount of null values, I went on to find out the percentage of males and females within the dataset. The difference was quite major. 

  ```
men = data.loc[data.Gender == 'Male']['Gender'].count()
female = data.loc[data.Gender == 'Female']['Gender'].count()

plt.pie([men,female],labels=['Male','Female'],colors=['green','pink'],autopct='%1.1f%%')
plt.title('Percentage of Males/ Females')
plt.show()


  ```
![download](https://github.com/user-attachments/assets/0821f23a-8047-4871-a05f-6ea48c36e5c7)


5- It was important for me to also find out the age distribution of individuals within our dataset: 
 ```
sns.countplot(x='Age',data=data,palette=['c','tomato'])
plt.title('Age Distribution')
plt.show()

  ```

![image](https://github.com/user-attachments/assets/c63784be-14f1-4761-b147-de9aa1429b80)



6- Moving on, I wanted to research the amount of 'No' values within our dataset, to see if there are any values that stand out as a whole:


 ```
no_values = data.applymap(lambda x: 1 if str(x).strip().lower() == '0' else 0)

plt.figure(figsize=(12, 6))
sns.heatmap(no_values, cmap='Reds', cbar=True)
plt.title("'No' Responses Heatmap")
plt.xlabel("Columns")
plt.ylabel("Rows")
plt.show()

  ```


![image](https://github.com/user-attachments/assets/e60a3759-6e12-4a67-9d8c-d74980939d59)


7 - To prepare for the preparation of the next graph, I wanted to find out the top 3 popular courses in this dataset. To do this, I did the following:
 ```
data['Course'].value_counts().head(3)
  ```


8 - After finding that out, the graph was created:

 ```
males_in_engineering = data.loc[(data.Gender == 'Male') & (data.Course == 'Engineering')]['Course'].count()
females_in_engineering = data.loc[(data.Gender == 'Female') & (data.Course == 'Engineering')]['Course'].count()

males_in_BCS = data.loc[(data.Gender == 'Male') & (data.Course == 'BCS')]['Course'].count()
females_in_BCS = data.loc[(data.Gender == 'Female') & (data.Course == 'BCS')]['Course'].count()

males_in_BIT = data.loc[(data.Gender == 'Male') & (data.Course == 'BIT')]['Course'].count()
females_in_BIT = data.loc[(data.Gender == 'Female') & (data.Course == 'BIT')]['Course'].count()

value_labels = ['Male', 'Female']
engineering_values = [males_in_engineering, females_in_engineering]
bcs_values = [males_in_BCS, females_in_BCS]
bit_values = [males_in_BIT, females_in_BIT]

x = range(len(value_labels))
width = 0.20

plt.bar([i - width for i in x], engineering_values, width=width, label='Engineering', color='steelblue')
plt.bar(x, bcs_values, width=width, label='BCS', color='mediumseagreen')
plt.bar([i + width for i in x], bit_values, width=width, label='BIT', color='salmon')

# Axis and labels
plt.xticks(x, value_labels)
plt.ylabel('Count')
plt.title('Enrollment by Gender and Course')
plt.legend()
plt.tight_layout()
plt.show()


  ```
![image](https://github.com/user-attachments/assets/031c684a-8b75-470f-a083-043d16beac7d)


9 - To make the variety more interesting I have created a bar graph showing the amount of males and females and their age groups. 


 ```
sns.countplot(data, x='Age', hue='Gender').set(title='Students of different ages and genders in data')

  ```

![image](https://github.com/user-attachments/assets/bcbd0252-21e7-4232-a495-27e4b6907019)


 



10 - The following step which I took was to create a bar chart showing the top courses which students study the most: 


 ```
popular_courses = data['Course'].value_counts()
popular_courses = popular_courses[popular_courses > 16].sort_values(ascending=False)

# Plot
plt.figure(figsize=(10, 6))
sns.barplot(x=popular_courses.index, y=popular_courses.values, palette='viridis')
plt.title('Most Popular Courses (More than 16 Students)')
plt.xlabel('Course')
plt.ylabel('Number of Students')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
  ```


![image](https://github.com/user-attachments/assets/5eac26b3-5627-42da-a3f7-b5bfd5dfc47f)


11 - CGPA graph has been created to analyse what is the mean grade that the students within the dataset recieve: 

 ```
sns.histplot(data,x='CGPA',color='darkcyan',kde=True)
plt.title('CGPA Distribution')
plt.xlabel('CGPA')
  ```
![image](https://github.com/user-attachments/assets/9027381a-f835-4ba9-98a1-47f302a02681)


12 - Once we started to compare CGPA, I wanted to see if the amount of sleep hours has an impact on the grades:



 ```
plt.figure(figsize=(10, 6))
sns.boxplot(x=data['Sleep Quality'],y=data['CGPA'], palette =['royalblue','lightpink'])
data.head()
plt.title('Sleep Quality vs CGPA')
plt.xlabel('Sleep Quality')
plt.ylabel('CGPA')
plt.show()
plt.ylabel('CGPA')
plt.show()
  ```

![image](https://github.com/user-attachments/assets/de0651b2-a6a3-4f5b-bc2c-5d7a682ef664)

13 - Furthermore, stress level has also been measured. In a boxplot:


 ```
plt.figure(figsize=(10, 6))
sns.boxplot(x=data['Stress Level'],y=data['CGPA'], palette =['peru','khaki'])
plt.title('Stress Level vs CGPA')
plt.xlabel('Stress Level')
plt.ylabel('CGPA')
plt.show()
  ```
![image](https://github.com/user-attachments/assets/0522a67f-b121-432a-9e20-2c629dc080bf)

14 - Next, a stacked bar chart was created to find out the percentage amount of anxiety for each study year

 ```
data['Study Year'] = data['Study Year'].str.strip().str.title()

# Group and calculate percentages
group = data.groupby(['Study Year', 'Depression']).size().unstack(fill_value=0)
percentages = group.div(group.sum(axis=1), axis=0) * 100

# Create a bar chart
percentages.plot(kind='bar', stacked=True,color = ['royalblue','orange'])
plt.title('Anxiety Percentage by Study Year')
plt.ylabel('Percentage')
plt.xlabel('Study Year')
plt.legend(title='Anxiety')
plt.xticks(rotation=0)
plt.tight_layout()
plt.show()


  ```
![image](https://github.com/user-attachments/assets/de678a69-68c0-49dc-bb6a-deddbf3ee4e4)



15 - Continuing, one of my most proud graphs which I have developed was the scatterplot showing the Study hours vs CGPA. It clearly showed the results:

 ```
sns.scatterplot(data=data, x='Study Hours', y='CGPA', hue='Study Hours', palette='coolwarm')
plt.title('Study Hours vs CGPA')
plt.xlabel('Study Hours Per Week')
plt.ylabel('CGPA')
plt.legend(title='Study Hours')
plt.show()

  ```
![image](https://github.com/user-attachments/assets/732dcdcf-6f08-416e-9364-3d29e143042e)



16 - A lineplot was created to show the academic engagement over time:
 ```
data['Timestamp'] = pd.to_datetime(data['Timestamp'], format='mixed')

# Time trend for Academic Engagement
data.groupby(data['Timestamp'].dt.date)['AcademicEngagement'].mean().plot(kind='line', figsize=(10, 6))
plt.title('Academic Engagement Over Time')
plt.xlabel('Date')
plt.ylabel('Average Academic Engagement')
plt.grid(True)
plt.show()

  ```
![image](https://github.com/user-attachments/assets/74e1ae1b-3b0b-477d-92f2-982c4131fb10)

17 - In the next step, I wanted to challenge myself and create a side by side graph showing the depression per age. This took a lot of organising and planning but the result came out well: 
 ```
depression_18 = data.loc[(data.Age == 18) & (data.Depression == 1), 'Depression'].count()

no_depression_18 = data.loc[(data.Age == 18) & (data.Depression == 0), 'Depression'].count()

depression_24 = data.loc[(data.Age == 24) & (data.Depression == 1), 'Depression'].count()

no_depression_24 = data.loc[(data.Age == 24) & (data.Depression == 0), 'Depression'].count()

depression_23 = data.loc[(data.Age == 23) & (data.Depression == 1), 'Depression'].count()

no_depression_23 = data.loc[(data.Age == 23) & (data.Depression == 0), 'Depression'].count()

depression_20 = data.loc[(data.Age == 20) & (data.Depression == 1), 'Depression'].count()

no_depression_20 = data.loc[(data.Age == 20) & (data.Depression == 0), 'Depression'].count()

depression_19 = data.loc[(data.Age == 19) & (data.Depression == 1), 'Depression'].count()

no_depression_19 = data.loc[(data.Age == 19) & (data.Depression == 0), 'Depression'].count()

depression_25 = data.loc[(data.Age == 25) & (data.Depression == 1), 'Depression'].count()

no_depression_25 = data.loc[(data.Age == 25) & (data.Depression == 0), 'Depression'].count()

depression_21 = data.loc[(data.Age == 21) & (data.Depression == 1), 'Depression'].count()

no_depression_21 = data.loc[(data.Age == 21) & (data.Depression == 0), 'Depression'].count()

depression_22 = data.loc[(data.Age == 22) & (data.Depression == 1), 'Depression'].count()

no_depression_22 = data.loc[(data.Age == 22) & (data.Depression == 0), 'Depression'].count()


visuals = [18,19,20,21,22,23,24,25]

depress = [depression_18,depression_19,depression_20,depression_21,depression_22,depression_23,depression_24,depression_25]
non_depress = [no_depression_18,no_depression_19,no_depression_20,no_depression_21,no_depression_22,no_depression_23,no_depression_24,no_depression_25]

x = range(len(visuals))
width = 0.20

plt.bar(x, depress, width=width, label='Depression', color='tomato')
plt.bar([i + width for i in x], non_depress, width=width, label='No Depression', color='c')

# Axis and labels
plt.xticks(x, visuals)
plt.ylabel('Count')
plt.xlabel('Age')
plt.title('Depression per Age')
plt.legend()
plt.tight_layout()
plt.show()


  ```
![image](https://github.com/user-attachments/assets/2b6c4408-9cd5-4947-ad69-f9c63104acb4)

18 - To dive more deeper, I have create the percentage amount of support that students recieve per year group. I have create a stacked graph and with different colours to make the variety more acceptable. 

 ```
support_year1 = data.loc[(data['Study Year'] == 'Year 1') & (data['Support'] == 1), 'Support'].count()


no_support_year1 = data.loc[(data['Study Year'] == 'Year 1') & (data['Support'] == 0), 'Support'].count()

support_year2 = data.loc[(data['Study Year'] == 'Year 2') & (data['Support'] == 1), 'Support'].count()


no_support_year2 = data.loc[(data['Study Year'] == 'Year 2') & (data['Support'] == 0), 'Support'].count()

support_year3 = data.loc[(data['Study Year'] == 'Year 3') & (data['Support'] == 1), 'Support'].count()

no_support_year3 = data.loc[(data['Study Year'] == 'Year 3') & (data['Support'] == 0), 'Support'].count()

support_year4 = data.loc[(data['Study Year'] == 'Year 4') & (data['Support'] == 1), 'Support'].count()

no_support_year4 = data.loc[(data['Study Year'] == 'Year 4') & (data['Support'] == 0), 'Support'].count()



values = ['Year 1', 'Year 2', 'Year 3', 'Year 4']
support = [support_year1, support_year2, support_year3, support_year4]
no_support = [no_support_year1, no_support_year2, no_support_year3, no_support_year4]


support_prc = [d / (d + n) * 100 for d, n in zip(support, no_support)]
non_support_prc = [100 - p for p in support_prc]

x = np.arange(len(values))

plt.bar(x, support_prc, label='Support', color='lightskyblue')
plt.bar(x, non_support_prc, bottom=support_prc, label='No Support', color='plum')

plt.xticks(x, values)
plt.ylabel('Percentage')
plt.xlabel('Year Group')
plt.title('Support Percentage by Year Group')
plt.legend()
plt.tight_layout()
plt.show()

  ```
![image](https://github.com/user-attachments/assets/6e69caca-47fb-478a-91fe-2b67cd2de303)

19 - A violinplot was created in order to find out the sleep quality vs specialist treatment. I think this type of plot is acceptable because we can see a clear pattern in this type of graph:
 ```
sns.violinplot(data,x='Treatment',y='Sleep Quality',hue='Treatment',palette = ['mediumorchid','lightgreen'])
plt.title('Sleep Quality vs Specialist Treatment')
plt.xlabel('Specialist Treatment')
plt.ylabel('Sleep Quality')
plt.show()
  ```
![image](https://github.com/user-attachments/assets/fab3897d-94c6-47a9-ad10-a975e88ed431)



20 - Another violinplot was made to find out the stress level by age:
 ```
sns.violinplot(data,x='Age',y='Stress Level',palette = ['darkolivegreen','crimson'])
plt.title('Stress Level vs Age')
plt.xlabel('Age')
plt.ylabel('Stress Level')
plt.show()
  ```
![image](https://github.com/user-attachments/assets/10d70c7f-bb96-455e-b345-2c99276d4319)


21 - As we didn't yet find out, I have created an easy to read pie chart showing us the percentages of most popular cases: 
 ```
depression_amount = data.loc[data['Depression'] == 1]['Depression'].count()
anxiety_amount = data.loc[data['Anxiety'] == 1]['Anxiety'].count()
panic_attack_amount = data.loc[data['Panic Attack'] == 1]['Panic Attack'].count()


all_in_one = [depression_amount,anxiety_amount,panic_attack_amount]
labels = ['Depression', 'Anxiety', 'Panic Attack']


plt.pie(all_in_one, labels=labels, autopct='%1.1f%%', startangle=200)
plt.axis('equal')
plt.title('Most Popular Case')
plt.show()
  ```
![image](https://github.com/user-attachments/assets/95f8480c-fda4-4871-bb8e-1b997ae80ec9)



22  - The next step was to find out the top 3 courses with the most amount of study hours. The pattern is very similar to the previous graph, the top 3 courses are the same:
 ```
top_3_courses = data.groupby('Course')['Study Hours'].sum().nlargest(3)

plt.figure(figsize=(10, 6))

bars1 = plt.bar(top_3_courses.index, top_3_courses.values, color = ['khaki','powderblue','lavender'])


for bar in bars1:
    height = bar.get_height()
    plt.text(bar.get_x() + bar.get_width() / 2, height + 10, str(height), ha='center', fontsize=9)


plt.xlabel('Course')
plt.ylabel('Total Study Hours')
plt.title('Top 3 Courses with the Biggest Study Amount')
plt.show()
  ```
![image](https://github.com/user-attachments/assets/e20fe101-d0ff-4b76-887a-2177e99d6849)



23  - The final violinplot was made the calculate whether the academic engagement has a big impact on the overall CGPA score: 
 ```
sns.violinplot(data=data,x='AcademicEngagement',y='CGPA',palette = ['palegreen','darkturquoise'])
plt.title('Academic Engagement vs CGPA')
plt.xlabel('Academic Engagement')
plt.ylabel('CGPA')
plt.show()
  ```
![image](https://github.com/user-attachments/assets/96c5ee50-165e-4417-86ec-75c623de6c6c)



24  - The last graph that was created was a pie chart showing the percentage of study hours that each year does, the results in my opinion are surprising: 
 ```
year1 = data.loc[data['Study Year'] == 'Year 1']['Study Hours'].count()

year2 = data.loc[data['Study Year'] == 'Year 2']['Study Hours'].count()

year3 = data.loc[data['Study Year'] == 'Year 3']['Study Hours'].count()

year4 = data.loc[data['Study Year'] == 'Year 4']['Study Hours'].count()



all_years = [year1,year2,year3,year4]
labels = ['Year 1', 'Year 2', 'Year 3', 'Year 4']


plt.pie(all_years, labels=labels, autopct='%1.1f%%', startangle=200,colors=['lightcoral','bisque','palegreen','lavender'])
plt.axis('equal')
plt.title('Study Hours per Year')
plt.show()
  ```
![image](https://github.com/user-attachments/assets/3016c43a-70b7-4434-badc-b746a6dd42a7)


## 25 - Thank you for reading my Student Mental Health Analysis project. Your support and comments help me to increase the quality of my work and it allows me to develop my analysis skills in the meantime. If you wish to add anything to this piece of work, please fork it and let me know!
