# 🔍 LinkedIn Job Search 
![WTMproject-MadewithClipchamp-ezgif com-video-to-gif-converter-min](https://github.com/user-attachments/assets/c4151a8a-5d87-40c6-8c96-20f13c669fea)
 Job Market Dashboard Project

## 🎯 Objectives

This project aims to analyze job market demand using scraped data from LinkedIn job posts, with a specific focus on data-related roles. The goal is to provide an interactive dashboard that helps job seekers explore opportunities by:

- **Job Title**
- **Country**
- **Experience Level**
- **Skills in Demand**

By understanding the current job trends, users can identify which roles and skills are most valuable in the market.

---

## 🧪 Dataset

- **Source**: Scraped from LinkedIn over one week
- **Focus**: Data-related jobs (e.g., Data Analyst, Data Scientist, Data Engineer, etc.)
- **Challenges**:
  - Unstructured and noisy text data
  - Inconsistent formatting
  - Missing or vague fields

---

## ⚙️ Data Cleaning & Preparation
![applied steps in final tablepng](https://github.com/user-attachments/assets/15ae341b-d9dc-4076-8990-241eaad87c57)

### 🧹 Title & Location Cleaning (Power Query M)
Standardized `title` and `location` fields using Power Query:

```m
Text.Replace(Text.Replace(Text.Lower(Text.Trim([title])), ".", ""), ",", "")
Text.Replace(Text.Replace(Text.Lower(Text.Trim([location])), ".", ""), ",", ",")
```


### 🧠 Title & Country Normalization
Used ChatGPT to map messy job titles and country names into standardized, generic labels (e.g., "Data Analyst", "USA").

![country look like](https://github.com/user-attachments/assets/69bee2e4-3f76-4317-a8d3-63c553a839bc)
---

## 🧠 Skill Extraction from Description

Used custom M logic to assign relevant skills based on job title and then matched them in the job description:

```m
let
    title = Text.Lower([Title]),
    desc = if [description] = null then "" else [description],

    skillsList =
        if Text.Contains(title, "data analyst") then
            {"SQL", "Excel", "Tableau", "Power BI", "Python", "Statistics"}
        else if Text.Contains(title, "data scientist") then
            {"Python", "R", "SQL", "Machine Learning", "NLP", "Statistics"}
        else if Text.Contains(title, "data engineer") then
            {"SQL", "Python", "ETL", "Spark", "AWS", "BigQuery"}
        else if Text.Contains(title, "frontend") then
            {"HTML", "CSS", "JavaScript", "React", "Angular"}
        else if Text.Contains(title, "machine learning") then
            {"Python", "TensorFlow", "PyTorch", "ML", "NLP"}
        else if Text.Contains(title, "software engineer") then
            {"Python", "Java", "C++", "Git", "Algorithms", "SQL"}
        else
            {},

    matchedSkills =
        List.Select(
            skillsList,
            each Text.Contains(desc, _, Comparer.OrdinalIgnoreCase)
        )
in
    Text.Combine(matchedSkills, ", ")
```

- **Post-Processing**:
  - Split skills by delimiter
  - Unpivoted skill columns for better aggregation
    
![applied steps in skills](https://github.com/user-attachments/assets/19dd63dc-5e20-46e0-bec8-33a340e47279)

![selection layer](https://github.com/user-attachments/assets/2d998d5c-5bf9-41ed-b262-3cf13c12f9d7)

---

## 🧭 Experience Level Classification

Extracted seniority from the title using this logic:

```m
let
    titleLower = Text.Lower([Title])
in
    if Text.Contains(titleLower, "senior") then
        "Senior"
    else if Text.Contains(titleLower, "intern") then
        "Intern"
    else
        "Junior"
```

---

## 🔗 Data Modeling

- Merged:
  - Title table
  - Country table
  - Skills table
- Built relationships with the main fact table

![the data](https://github.com/user-attachments/assets/9b87cf90-dfe3-4de1-b900-59813df79cf0)

---

## 📊 Power BI Report

- Designed layout using **Canva** for consistency and user-centric visualization
- Key Features:
  - Filters: Job Title, Country, Company, Experience Level
  - Bookmarks for interaction design
  - Drill-through pages by Job Title
  - Tooltips for contextual info
  - Selection Layer cleanup for a polished look

![selection layer](https://github.com/user-attachments/assets/409c0003-15c5-4eec-8c83-07e4e99c8cd1)

---

## 📐 Measures & KPIs

Created core measures such as:

- Count of jobs by:
  - Country
  - Title
  - Skill
  - Experience Level
- % of roles requiring each skill
- Skill distribution per role

---

## ✅ Outcome

The final dashboard provides an insightful overview of job market trends for data professionals. It helps users explore:
![full dashboard](https://github.com/user-attachments/assets/1adb29cd-532d-4fc8-9924-532ba9676446)

- What skills are in demand
- Which countries are hiring
- What levels companies are looking for

---

## 📎 Project Access



You can find the full project files and dashboard visuals [here on GitHub](./) *(insert your link)*.
