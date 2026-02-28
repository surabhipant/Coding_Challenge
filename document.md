# NYC Jobs Challenge – MyDocument.md

##  Data Exploration
The source dataset (`nyc-jobs.csv`) contains job postings from NYC agencies.  
Key profiling steps performed:

- **Column Types**
  - Numerical: `Salary Range From`, `Salary Range To`, `# Of Positions`
  - Date: `Posting Date`, `Process Date`, `Posting Updated`
  - Categorical: `Agency`, `Job Category`, `Salary Frequency`, `Full-Time/Part-Time indicator`
  - Text: `Preferred Skills`, `Minimum Qual Requirements`, `To Apply`, `Recruitment Contact`

- **Missing Values**
  - Explicitly counted per column using `isNull`, `isnan`, and empty string checks.

- **Distinct Values**
  - Categorical columns profiled for unique values (e.g., agencies, job categories, salary frequency).

- **Basic Statistics**
  - Summary stats (min, max, mean, stddev) computed for numerical columns.

### KPIs to Resolve
1. Number of job postings per category (Top 10).  
2. Salary distribution per job category.  
3. Correlation between higher degree requirements and salary.  
4. Job posting with the highest salary per agency.  
5. Average salary per agency for the last 2 years.  
6. Highest paid skills in the US market.

---

## Data Processing
### Functions Implemented
- **Cleaning**
  - Cast numeric columns to appropriate types.
  - Convert date columns to `DateType`.
  - Drop rows with missing salary values.
  - Normalize categorical values (trim, uppercase).

- **Feature Engineering**
  1. **Salary Midpoint**: Average of `Salary Range From` and `Salary Range To`.
  2. **Posting Age**: Days since posting date.
  3. **DegreeRequirement Encoding**: Bachelor’s=1, Master’s=2, PhD=3, else=0.

- **Features Removal**
  - Dropped irrelevant columns: `To Apply`, `Recruitment Contact`, `Additional Information`.

- **Storage**
  - Processed dataset written safely to Parquet: `/dataset/nyc_jobs_processed`.

---

## Analysis & Visualization
- **Top Job Categories**: Horizontal bar chart of top 10 categories.  
- **Salary Distribution**: Bar chart of average salary per category.  
- **Degree vs Salary**: Scatter plot showing correlation.  
- **Highest Salary per Agency**: Vertical bar chart.  
- **Average Salary per Agency (Last 2 Years)**: Vertical bar chart.  
- **Highest Paid Skills**: Horizontal bar chart of top 20 skills.

---

## Expectations
- Challenge requirements implemented:
  - KPIs resolved.
  - Functions for cleaning, transformation, feature engineering.
  - Features removed based on profiling.
  - Processed data stored in Parquet.
- **Test Cases**: Unit tests written for each KPI function using mock data.
- **Code Comments**: Inline documentation provided for clarity.
- **Deployment Proposal**:
  - Package PySpark pipeline into a reproducible Jupyter notebook.
  - Store processed data in cloud storage (e.g., S3, HDFS).
  - Schedule pipeline execution via Airflow or Databricks jobs.
- **Triggering Approach**:
  - CLI entry point (`spark-submit job_analysis.py`) or notebook execution.
  - Optionally integrate with CI/CD pipeline for automated runs.

---

## Learnings, Challenges & Assumptions
- **Learnings**:
  - Importance of schema enforcement for consistent data types.
  - Value of modular functions for reusability and testing.
- **Challenges**:
  - Handling inconsistent text in categorical fields.
  - Skills extraction required careful trimming and deduplication.
- **Assumptions**:
  - Salary ranges are reliable indicators of compensation.
  - Degree requirements encoded from text are approximate.
  - Dropping rows with missing salary values does not bias results significantly.

---



  ### Deployment Proposal - **Packaging**: - Wrap the PySpark pipeline into a Python script (`job_analysis.py`) or Jupyter notebook. 
- Store processed data in cloud storage (e.g., AWS S3, Azure Data Lake, or HDFS). -
 Containerize with Docker for portability. - **Scheduling**: - Use Apache Airflow, 
- CI/CD integration (GitHub Actions, Azure DevOps) for automated testing and deployment. - 
**Scaling**: - Leverage Spark clusters (YARN, Kubernetes, or Databricks) for distributed execution. -
 Configure resources (executors, memory, cores) based on dataset size. 
 ### Triggering Approach - **spark-submit**: Run the pipeline as a standalone job:
 ```bash spark-submit \ 
 --master yarn \ 
 --deploy-mode cluster \ 
 --num-executors 4 \ 
 --executor-memory 4G \ 
 --executor-cores 2 \ 
 job_analysis.py
