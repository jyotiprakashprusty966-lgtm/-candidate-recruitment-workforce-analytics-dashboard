# Candidate Recruitment & Workforce Analytics Dashboard

An end-to-end recruitment analytics project using Python, Pandas, NumPy, Matplotlib, and Power BI to clean, transform, analyze, and visualize a large recruitment dataset containing nearly **100,000 candidate records**.

---

## Project Structure

```
Candidate-Recruitment-Workforce-Analytics-Dashboard/
│
├── Datasets/
│   ├── cleaned_candidate_dataset.xls
│   ├── final_candidate_dataset.xls
│   └── skills_df.xls
│
├── Notebook/
│   └── Python_For_Excel.ipynb
│
├── Power BI/
│   └── First_PowerBi_DashBoard.pbix
│
├── screenshots/
│   └── dashboard.png
│
└── README.md
```

---

## Project Overview

This project works with a synthetic candidate recruitment dataset containing rich, nested JSON-like candidate information. The dataset covers candidate profiles, career history, education, skills, certifications, languages, salary expectations, activity signals, recruiter response metrics, interview outcomes, and work preferences.

The project transforms this complex raw structure into clean, analysis-ready tables and builds an interactive Power BI dashboard that provides actionable recruitment insights.

---

## Objectives

1. Parse and extract nested JSON-like candidate data from Excel
2. Identify and fix data-quality issues (reversed salaries, reversed dates, duplicates)
3. Validate important fields and business rules
4. Engineer analytical features for deeper insights
5. Perform exploratory data analysis (EDA)
6. Identify patterns in candidate activity, skills, salary, and recruitment outcomes
7. Build an interactive Power BI dashboard
8. Deliver a recruiter-focused analytical view of the candidate pool

---

## Dataset

| Property | Details |
|---|---|
| Total Records | 99,999 candidates |
| Final Columns | 100 analytical columns |
| Original Format | Excel with JSON-like nested data |
| Final Format | XLS (cleaned & structured) |

### Data Coverage

| Area | Key Fields |
|---|---|
| Profile | Country, title, company, industry, experience |
| Career | Company, title, dates, duration |
| Education | Degree, field of study, institution, tier |
| Skills | Skill name, proficiency, endorsements |
| Recruitment | Applications, recruiter response, interviews |
| Activity | Profile views, searches, recruiter saves |
| Salary | Expected minimum, maximum, midpoint (INR LPA) |
| Verification | Email, phone, LinkedIn |
| Work Preferences | Work mode, relocation, notice period |

---

## Data Cleaning

### 1. Duplicate Validation
- Full-row duplicates: **0**
- Duplicate candidate IDs: **0**

### 2. Reversed Salary Ranges
Some records had `min salary > max salary`. A boolean mask was used to detect and swap these values:

```python
mask = df["expected_salary_range_inr_lpa.min"] > \
       df["expected_salary_range_inr_lpa.max"]
```

**18,865 reversed salary ranges** were detected and corrected by swapping the values.

### 3. Reversed Dates
Some records had `last_active_date < signup_date`, which is logically inconsistent:

```python
mask = df["last_active_date"] < df["signup_date"]
```

**7,496 reversed date records** were detected and corrected.

### 4. Expected Nulls
Not all null values represent bad data. For example:
- `career_history.end_date` → null for currently employed candidates
- Certification fields → null when no certification exists
- Skill assessment fields → null when assessment not taken

These were intentionally left as null rather than replaced with zero.

### 5. Placeholder Values
`-1` was used as a placeholder in fields like `github_activity_score` and `offer_acceptance_rate`. These were treated as missing data, not valid scores, and excluded from aggregations.

---

## JSON Data Transformation

The original dataset contained deeply nested JSON-like objects. These were parsed and normalized into separate relational tables to avoid data duplication:

| Table | Contents |
|---|---|
| `main_df` | Candidate-level profile and recruitment signals |
| `career_df` | Career history records |
| `education_df` | Education records |
| `skills_df` | Skill assessments and proficiency |
| `certifications_df` | Certification records |
| `languages_df` | Language proficiency records |

---

## Feature Engineering

| Feature | Description |
|---|---|
| `experience_level` | Groups candidates into Entry, Junior, Mid, Senior |
| `expected_salary_mid_lpa` | Midpoint of expected salary range |
| `activity_score` | Combined candidate activity measure |
| `verification_score` | Count of completed verification methods |
| `activity_level` | Categorizes candidates as Low, Medium, High |
| `skill_count` | Count of available skill assessments |
| `engagement_score` | Combined recruitment engagement indicator |
| `highest_education_label` | Bachelor, Master, or Ph.D. label |

Example:
```python
df["expected_salary_mid_lpa"] = (
    df["expected_salary_range_inr_lpa.min"] +
    df["expected_salary_range_inr_lpa.max"]
) / 2
```

---

## Exploratory Data Analysis

Key findings from the dataset:

- **~75%** of candidates are based in India
- Average experience: **7.17 years**
- Average expected salary midpoint: **16.01 LPA**
- **35.34%** of candidates are open to work
- Average recruiter response rate: **43.66%**
- Higher skill count is associated with higher recruiter response and salary
- Higher activity level is associated with higher recruiter response and offer acceptance

> Note: These are descriptive associations within the dataset and should not be interpreted as causal relationships.

---

## Power BI Dashboard

The cleaned dataset was imported into Power BI to build an interactive **Candidate Analytics & Recruitment Dashboard**.

### KPI Cards
- Total Candidates
- Average Salary (LPA)
- Average Experience (Years)
- Open to Work %

### Slicers / Filters
- Country
- Experience Level
- Current Industry
- Activity Level

### Visualizations
- Candidate Distribution by Country
- Top 10 Job Titles
- Candidate Distribution by Experience Level
- Average Salary by Experience Level
- Recruiter Response Rate by Activity Level
- Recruiter Response Rate by Skill Count
- Average Salary by Skill Count
- Recruiter Response Rate by Verification Score
- Top 10 Candidate Skills
- Candidate Distribution by Education Level
- Average Salary by Education Level
- Offer Acceptance Rate by Activity Level

### DAX Measure Example

A custom DAX measure was used to exclude `-1` placeholder values when calculating offer acceptance rate:

```DAX
Average Valid Offer Acceptance =
CALCULATE(
    AVERAGE('final_candidate_dataset'[offer_acceptance_rate]),
    'final_candidate_dataset'[offer_acceptance_rate] >= 0
)
```

---

## Dashboard Preview

![Candidate Analytics Dashboard](screenshots/dashboard.png)

---

## Tech Stack

| Tool / Library | Purpose |
|---|---|
| Python 3.x | Core programming language |
| Pandas | Data cleaning, transformation, EDA |
| NumPy | Numerical operations |
| Matplotlib | Data visualization during EDA |
| Jupyter Notebook | Interactive development environment |
| Microsoft Excel | Original data source format |
| Microsoft Power BI | Interactive dashboard |
| DAX | Custom Power BI measures |

---

## Project Workflow

```
Raw Excel Dataset
       ↓
JSON Parsing & Extraction
       ↓
Data Normalization
       ↓
Data Quality Validation
       ↓
Data Cleaning
       ↓
Feature Engineering
       ↓
Exploratory Data Analysis
       ↓
Final Candidate Dataset
       ↓
Power BI
       ↓
Interactive Dashboard
       ↓
Recruitment Insights
```

---

## Success Criteria

| Metric | Target |
|---|---|
| Duplicate candidate records | 0 |
| Reversed salary ranges after fix | 0 |
| Reversed date records after fix | 0 |
| Final analytical columns | 100 |
| Dashboard filters functional | Yes |
| DAX placeholder exclusion working | Yes |

---

## Key Learning Outcomes

Through this project, I practised:

- Handling large datasets (~100K records) with Pandas
- Parsing and normalizing nested JSON structures from Excel
- Data cleaning and multi-rule validation
- Identifying logical data-quality issues (reversed values, placeholders)
- Feature engineering for analytical purposes
- Exploratory data analysis and visualization
- Power BI dashboard development
- Writing DAX measures for custom aggregations
- Translating raw data into recruiter-focused insights

---

## Disclaimer

This project uses a synthetic/anonymized recruitment dataset created for learning and portfolio purposes. The relationships identified in the analysis are descriptive associations within the dataset and should not be interpreted as causal conclusions.

---
