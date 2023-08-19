- 👋 Hi, I’m @daNomTK
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
daNomTK/daNomTK is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import pandas as pd
from datetime import datetime

# Sample DataFrame (replace this with your actual DataFrame)
data = pd.DataFrame({
    "column1": ["A", "A", "A", "A", "B", "B"],
    "column2": ["X", "X", "Y", "Y", "X", "X"],
    "date_column": ["2023-08-01", "2023-08-03", "2023-08-05", "2023-08-07", "2023-08-05", "2023-08-08"]
})

# Convert date_column to datetime type
data["date_column"] = pd.to_datetime(data["date_column"])
print(data)
# Initialize variables to keep track of the current group data
current_group = None
current_dates = []
output_data = []

for index, row in data.iterrows():
    if current_group is None:
        current_group = (row["column1"], row["column2"])
        current_dates.append(row["date_column"])
    elif (row["column1"], row["column2"]) == current_group:
        current_dates.append(row["date_column"])
    else:
        # Calculate and store the date differences
        if len(current_dates) > 1:
            date_diffs = [(current_dates[i + 1] - current_dates[i]).days for i in range(len(current_dates) - 1)]
            output_data.append({"column1": current_group[0], "column2": current_group[1], "date_diff": date_diffs})
        
        # Reset variables for the new current group
        current_group = (row["column1"], row["column2"])
        current_dates = [row["date_column"]]

# Calculate and store the date differences for the last group
if current_group is not None and len(current_dates) > 1:
    date_diffs = [(current_dates[i + 1] - current_dates[i]).days for i in range(len(current_dates) - 1)]
    output_data.append({"column1": current_group[0], "column2": current_group[1], "date_diff": date_diffs})

# Create a new DataFrame with the date differences
output_df = pd.DataFrame(output_data)

# Print the updated DataFrame with date differences
print(output_df)
