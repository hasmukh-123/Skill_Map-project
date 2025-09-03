# Skill_Map-project
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("college_survey.csv")
df

df.fillna("None", inplace=True)

rating_columns = ['Python Rating', 'SQL Rating', 'Excel Rating', 'Power BI Rating', 'Communication Rating']
for col in rating_columns:
    df[col] = pd.to_numeric(df[col], errors='coerce').fillna(0)
ef detect_skill_gap(row):
    if row['Career Interest'] == 'Data Science' and row['Python Rating'] < 3:
        return "YES"
    return "NO"

df['Python Gap'] = df.apply(detect_skill_gap, axis=1)

plt.figure(figsize=(10, 6))
sns.boxplot(data=df[rating_columns])
plt.title("Skill Rating Distribution")
plt.ylabel("Rating (1-5)")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

tool_counts = df['Tools Known'].str.get_dummies(sep=",").sum().sort_values(ascending=False)
tool_counts.plot(kind='bar', figsize=(10, 5), title='Tool Usage Count')
plt.ylabel("Number of Students")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

def suggest_career(row):
    if row['Career Interest'] == 'Data Science' and row['Python Rating'] < 3:
        return "Try a basic Python + Pandas course (4 weeks)."
    elif row['Career Interest'] == 'Web Dev' and 'HTML' not in row['Tools Known']:
        return "Start learning HTML & CSS basics on freeCodeCamp."
    else:
        return "You're on track. Keep building projects!"

df['Suggestion'] = df.apply(suggest_career, axis=1)

utput_file = "skill_analysis_output.csv"
df.to_csv(output_file, index=False)
print(f"Output saved to {output_file}")

import pandas as pd

# Load your file
df = pd.read_csv("C:/Users/hp/Desktop/college_survey.csv")

# Print all column names
print("🔍 Column Names:\n", df.columns.tolist())

# Show Alice's full data row
alice_data = df[df['Name'].str.strip().str.lower() == "alice"]
print("\n🧾 Alice's Row:\n")
print(alice_data.T)  # transpose to read better

import pandas as pd
import matplotlib.pyplot as plt
from IPython.display import Markdown, display

# Load the dataset
file_path = "college_survey.csv"
df = pd.read_csv(file_path)

# Normalize the name column to handle case & spacing
df['Name'] = df['Name'].str.strip().str.lower()

# Filter for Alice
person_name = "alice"
person_df = df[df['Name'] == person_name]

# Check if Alice's data is available
if person_df.empty:
    print(f" No data found for {person_name}")
else:
    # Display basic info
    display(Markdown(f"Career Skill Analysis for {person_name.capitalize()}"))

    person_row = person_df.iloc[0]

    # Display basic info
    display(Markdown("Basic Information"))
    print("Name:", person_row['Name'].capitalize())
    print("Career Interest:", person_row['Career Interest'])
    print("Tools Known:", person_row['Tools Known'])

    # Select numeric skill ratings
    skill_columns = ['Python Rating', 'SQL Rating', 'Excel Rating', 'Power BI Rating', 'Communication Rating']
    skill_data = person_row[skill_columns]

    # Drop NaNs and convert to numeric (if needed)
    skill_series = pd.to_numeric(skill_data, errors='coerce').dropna()

    if skill_series.empty:
        print("No skill ratings available.")
    else:
        # Plot the skill ratings as horizontal bar chart
        plt.figure(figsize=(8, 5))
        skill_series.sort_values().plot(kind='barh', color='mediumseagreen')
        plt.title(f"{person_name.capitalize()}'s Skill Ratings")
        plt.xlabel("Rating (Out of 10)")
        plt.grid(axis='x', linestyle='--', alpha=0.7)
        plt.tight_layout()
        plt.show()

        # Suggest career based on interest + skills
        career = person_row['Career Interest']
        print(f"\n Based on {person_name.capitalize()}'s interest in **{career}** and current skills, this career is suitable.")


