📊 Customer Usage Analysis and Segmentation — ConnectaTel
🎯 Project Objective

Analyze the usage behavior (calls and messages) of ConnectaTel customers, a telecommunications company, to answer key business questions:

Which customer segments show higher or lower usage of calls and messages?
Which users show outlier values that could indicate unusual behavior, fraud, or registration errors?
How does usage vary by age and plan type?
What patterns can help design better plans, optimize the offering, and improve customer satisfaction?

The final deliverable is an executive analysis with actionable business recommendations, based on cleaning, exploring, visualizing, and segmenting the customer data.

📁 Datasets Used
File	Description
plans.csv	Catalog of available plans (Basic, Premium) with prices and benefits.
users_latam.csv	Per-user information: personal data, city, registration date, contracted plan, and churn date.
usage.csv	Activity logged per user: event type (call or text), date, duration (calls), and length (messages).

These three datasets are combined to build a per-customer behavior profile (user_profile), which serves as the foundation for the entire analysis.

🔄 Analysis Stages

The notebook follows an 8-step flow:

Load and explore — loading the 3 datasets and initial review of structure and data types.
Data quality assessment — counting nulls, detecting sentinel values (-999, "?"), and out-of-range dates.
Basic data cleaning — replacing sentinels, fixing impossible dates, and diagnosing nulls (MCAR/MAR/MNAR) in duration and length.
Summary statistics of usage per user — aggregating usage by user_id (messages, calls, total minutes) and merging with users to build user_profile.
Distribution visualization and outliers — histograms per variable (compared by plan) and boxplots for visual outlier detection.
Customer segmentation — creating grupo_uso (Low/Medium/High usage) and grupo_edad (Young/Adult/Senior), visualized with countplots.
Executive insights for stakeholders — translating technical findings into business conclusions and recommendations.
Publication — uploading the notebook and this README to GitHub for a reproducible deliverable.
▶️ How to Run the Notebook
Option A: Google Colab (recommended, no installation required)
Upload the .ipynb file to Google Colab (File > Upload notebook).
Upload the three CSV files (plans.csv, users_latam.csv, usage.csv) to the /datasets/ folder in the Colab environment, or adjust the loading paths (pd.read_csv(...)) to match where you saved them.
Run the cells in order, from top to bottom (Runtime > Run all).
Option B: Local Jupyter Notebook
Clone this repository:
bash
   git clone <repository-url>
   cd <repository-name>
Install the required dependencies:
bash
   pip install pandas numpy matplotlib seaborn jupyter
Place the CSV files in a datasets/ folder at the project root (or adjust the paths in the notebook).
Launch Jupyter and open the notebook:
bash
   jupyter notebook
Run the cells in order.
🧪 Quick Reproduction Guide
The notebook is designed to run end-to-end without manual intervention, except for the markdown cells documenting diagnostics (those already contain the written answers).
If you swap in different source datasets, pay special attention to Step 2 (data quality), since the cleaning thresholds (e.g., the -999 sentinel in age) are defined for this project's original data and may not apply to different data.
The charts (sns.histplot, sns.boxplot, sns.countplot) are generated automatically when running the corresponding cells; they don't need to be saved separately for the analysis to be reproducible.
🛠️ Tools Used
Python 3
pandas, numpy
seaborn, matplotlib
Jupyter Notebook / Google Colab
