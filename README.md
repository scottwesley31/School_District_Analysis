# School District Analysis
Module 4

## Overview of the school district analysis
### Purpose
After the compilation of data contained in two different csv files (`schools_complete.csv` and `students_complete.csv`) and an in-depth analysis of a school district's reading and math scores by grade, the school board discovered a dishonest reporting of ninth-grade math and reading scores for one high school in particular - Thomas High School (THS). The school board is requesting an updated analysis of the school district data which excludes said math scores but keeps the rest of the data intact. A report describing how the updated analysis has affected the overall school district results has been requested.

## Results
- How is the district summary affected?

The original district summary results generated from the `district_summary_df` DataFrame in Python is as follows:
![district_summary_df_original](https://user-images.githubusercontent.com/107309793/178860750-fbf6a9ec-5031-40ef-9fbd-828efbdd65da.png)

After replacing the THS ninth-grader math and reading scores in Python with "NaN" and running the modified code, the updated district summary results look like this:
![district_summary_df_updated](https://user-images.githubusercontent.com/107309793/178860872-96381da6-6e06-491c-b064-70188178bd9e.png)

- To summarize the differences seen between these 2 dataframes:
  - **Total Schools:** unaffected
  - **Total Students:** unaffected
  - **Total Budget:** unaffected
  - **Average Math Score:** score dropped from 79.0 to 78.9 (0.1 difference)
  - **Average Reading Score:** unaffected
  - **% Passing Math:** percent dropped from 75 to 74.8 (0.2 difference)
  - **% Passing Reading:** percent dropped from 86 to 85.7 (0.3 difference)
  - **% Overall Passing:** percent dropped from 65 to 64.9 (0.1 difference)

Let's breakdown each of these points.

The **Total Schools** value is unaffected because there wasn't data from an entire school that was dropped from the analysis, only the math scores of the ninth-graders at THS.

The **Total Students** value was unaffected only because an integer value `student_count` was reported in both the original and updated DataFrames. This value is determined by counting the number of rows in the `school_data_complete_df` DataFrame specifically in the "Student ID" colum. The code used is `student_count = school_data_complete_df["Student ID"].count()`. The number of rows in the "Student ID" column is not affected by replacing math scores with "NaN".

The **Total Budget** value was also unaffected because the data from the "budget" column in the `school_data_complete_df` was not altered. The sum of the values of all the rows in this column is unchanged and calculated using this code: `total_budget = school_data_complete_df["budget"].sum()`

The **Average Math Score** value was affected slightly in the updated file (dropped from 79.0 to 78.9). This value is calculated using the following code: `average_math_score = school_data_complete_df["math_score"].mean()`. Because the "math_score" column now includes rows with "NaN" in the updated script, the calculated mean results in a slightly lower average.

The **Average Reading Score** value was not affected significantly in the updated script. This value is calcuated using the following code: `average_reading_score = school_data_complete_df["reading_score"].mean()`. Even though the ninth-grade THS reading scores were replaced with NaN, the overall average was not mathematically different (to the tenths place).

The **% Passing Math** percent dropped from 75 to 74.8 (0.2 difference). This is percentage is calculated by dividing the number of students who obtained a score of 70 or higher (`passing_math_count`) by the total number of students in the district (`student_count`) and expressing this as a percent. Both scripts use the following code to calculate `passing_math_count`: `passing_math_count = school_data_complete_df[(school_data_complete_df["math_score"] >= 70)].count()["student_name"]`. This number is affected by values dropped in the "math_score" column for the updated script. In addition, the updated script utilizes an value called `new_student_count` which accounts for a new total number of students (it subtracts the 9th grade THS students from the overall total): `new_student_count = student_count - ninth_grade_thomas_students`.

The **% Passing Reading** percent dropped from 86 to 85.7 (0.3 difference) for the same exact reason that % Passing Math dropped. This percent was calculated in both scripts in the same way detailed above (except for using the corresponding `passing_reading_count` and `student_count`/`new_student_count`).

The **% Overall Passing** dropped from 65 to 64.9 (0.1 difference). This percent is calculated in the same way between both scripts; the number of students receiving both a math and reading score of 70 or higher (`overall_passing_math_reading_count`) is divided by the total number of students in the district (`student_count`). The `overall_passing_math_reading_count` is calculated using the count() function of the following code: `passing_math_reading = school_data_complete_df[(school_data_complete_df["math_score"] >= 70) & (school_data_complete_df["reading_score"] >= 70)]`. This value again is affected in the new script by the "NaN" values replacing scores. Additionally the `new_student_count` value is used again in the updated script.

- How is the school summary affected?

The original school summary results generated from the `per_school_summary_df` DataFrame in Python is as follows:
![per_school_summary_df_original](https://user-images.githubusercontent.com/107309793/178879498-1a1b0ee6-1754-4162-b35c-acffeb0632a0.png)

After replacing the THS ninth-grader math and reading scores in Python with "NaN" and running the modified code, the updated school summary results look like this:
![per_school_summary_df_updated](https://user-images.githubusercontent.com/107309793/178879617-b7bc8223-92ca-43ef-8cda-2731912d08e1.png)

- Going over each column, the differences are as follows:
  - **School Type:** unaffected
  - **Total Students:** unaffected
  - **Total School Budget:** unaffected
  - **Per Student Budget:** unaffected
  - **Average Math Score:** score dropped from 83.418349 to 83.350937 (0.067412 difference)
  - **Average Reading Score:** score increased from 83.84893 to 83.896082 (0.047152 increase)
  - **% Passing Math:** percent dropped from 93.272171 to 93.18569 (0.086481 difference)
  - **% Passing Reading:** percent dropped from 97.308869 to 97.018739 (0.29013 difference)
  - **% Overall Passing:** percent dropped from 90.948012 to 90.630324 (0.317688 difference)

To breakdown these results further:

The **School Type**, **Total Students**, **Total School Budget**, and **Per Student Budget** values are all unaffected by the updated script because none of these values are dependent on the values listed in the "math_scores" or "reading_scores" columns of the `school_data_complete_df`. They remain the same.

The **Average Math Score**, **% Passing Math**, **% Passing Reading**, and **% Overall Passing** all decrease slightly in the updated script. This is because all of these values are calculated specifically for THS only with a newly defined DataFrame (`THS_student_data_df`) that excludes all the 9th grader rows. The code utilizes the same conditionals mentioned earlier to extract the passing scores from this DataFrame (scores that are greater than or equal to 70) and obtains a count of the rows meeting these conditions. This count is then divided by the total number of students at THS ('THS_student_count'). Here's an example of code accomplishing this task (for math scores):

```
# Step 6. Get all the students passing math from THS

# Create DataFrame of all THS students passing math.
THS_passing_math = THS_student_data_df.loc[THS_student_data_df["math_score"] >= 70]

# Step 9. Calculate the percentage of 10th-12th grade students passing math from Thomas High School. 
THS_passing_math_count = THS_passing_math["Student ID"].count()

THS_passing_math_percentage = THS_passing_math_count / THS_student_count * 100
```

The **Average Reading Score** was the only value that increased somewhat. This value was calculated in the same manner as described above but mathematically worked out as being a positive difference. This could indicate that the 9th grade THS reading scores had some lower values in them which initially worked out to a lower average overall prior to updating the script.

- How does replacing the nenth graders' math and reading scores affect Thomas High School's performance relative to the other schools?

The `top_schools` DataFrame is generated by sorting the `per_school_summary_df` based off of the values in the "% Overall Passing" column. This provides a list of the top 5 schools. This code is executed in each script:

```
# Sort and show top five schools.
top_schools = per_school_summary_df.sort_values(["% Overall Passing"], ascending=False)
top_schools.head()
```

As it turns out, THS is ranked as the 2nd best school in terms of the overall passing percentage in both cases. Here are screenshots of the `top_schools` DataFrame:

Original `top_schools` DataFrame
![top_schools_original](https://user-images.githubusercontent.com/107309793/178893047-a108672f-0c93-465c-bd81-a7e680001362.png)

Updated 'top_schools' DataFrame with newly calculated data
![top_schools_updated](https://user-images.githubusercontent.com/107309793/178893142-facc0b88-6f2a-4b99-a41f-c308ae1bd53b.png)

The differences in the "% Overall Passing" column is only slight as outlined above. A 0.317688% drop was not enough to change the ranking of this school in terms of performance.

- How does replacing the ninth-grade scores affect the following:
  - Math and reading scores by grade (The results of all other schools aside from THS are excluded because none of their scores are changed in the updated script).
    - Original script math scores by grade:
    ![math_scores_by_grade_THS_original](https://user-images.githubusercontent.com/107309793/178895254-7fb36c6c-b693-4e22-9112-e1b634af0951.png)

    - Updated script math scores by grade:
    ![math_scores_by_grade_THS_updated](https://user-images.githubusercontent.com/107309793/178895280-c2db4269-954e-4054-9134-bd2081f83717.png)

    - Original script reading scores by grade:
    ![reading_scores_by_grade_THS_original](https://user-images.githubusercontent.com/107309793/178895297-74b7676d-9af0-4762-8414-4ada3eec636f.png)

    - Updated script reading scores by grade:
    ![reading_scores_by_grade_THS_updated](https://user-images.githubusercontent.com/107309793/178895326-aecb59e9-9b67-4d68-92d2-052d969052b8.png)
  - None of the scores are different between each script except for 9th grade which reiterates that all the reading and math scores for 9th graders were replaced with "NaN" resulting in no calculated value.

  - Scores by school spending
    - Both of the original and updated script result in a `spending_summary_df` that looks like this:
    ![spending_summary_df](https://user-images.githubusercontent.com/107309793/178896354-473a039d-c1cf-4ede-81d6-400be341300e.png)

This DataFrame is obtained in the same fashion in both scripts by establishing spending bins, appending a new column to the `per_school_summary_df` called "Spending Ranges (Per Student)", utilizing the calculated `per_school_capita` for each school to sort into the bins, than calculating the averages of all the score/percentages categories using the groupby() function. This is assembled into a new DataFrame. Here's the code (the formatting code is omitted):

```
# Establish the spending bins and group names.
spending_bins = [0, 585, 630, 645, 675]
group_names = ["<$586", "$586-630", "$631-645", "$646-675"]

# Categorize spending based on the bins.
per_school_summary_df["Spending Ranges (Per Student)"] = pd.cut(per_school_capita, spending_bins, labels=group_names)

# Calculate averages for the desired columns.
spending_math_scores = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["Average Math Score"]
spending_reading_scores = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["Average Reading Score"]
spending_passing_math = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Math"]
spending_passing_reading = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Reading"]
overall_passing_spending = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Overall Passing"]

# Assemble info DataFrame.
spending_summary_df = pd.DataFrame({
    "Average Math Score": spending_math_scores,
    "Average Reading Score": spending_reading_scores,
    "% Passing Math": spending_passing_math,
    "% Passing Reading": spending_passing_reading,
    "% Overall Passing": overall_passing_spending})
```
In both the original and updated scripts, the same `per_school_capita` calculation is used: `per_school_capita = per_school_budget/per_school_counts`. This divides budget alloted to a school by the number of students in that school. The `per_school_counts` values still includes the total student count for THS, meaning the new student count which excludes the 9th graders with NaN values is NOT accounted for.

  - Scores by school size
    - Both of the original and updated script result in a `size_summary_df` that looks like this:
    ![size_summary_df](https://user-images.githubusercontent.com/107309793/178899217-521570a0-ee19-4128-8fec-d920d6144e60.png)
    
This DataFrame is generated using the same strategy as the `spending_summary_df` DataFrame. The only difference is the bins, their labels, and that the "Total Students" column of the `per_school_summary_df` is used to sort. These categories are displayed in a new column called "School Size". The DataFrame is assembled after using the groupby() function in the same way as before. Here's the necessary code to generate the "School Size" column prior to grouping:

```
# Establish the bins.
size_bins = [0, 999, 1999, 5000]
group_names = ["Small (<1000)", "Medium (1000-1999)", "Large (2000-5000)"]

# Categorize school size based on the bins.
per_school_summary_df["School Size"] = pd.cut(per_school_summary_df["Total Students"], size_bins, labels=group_names)
```
Because the "Total Students" values of the `per_school_summary_df` DataFrame are used, the 9th grade students with "NaN" scores are still accounted for numerically, leaving the data unchanged in both scripts.

  - Scores by school type
    - Both of the original and updated script result in a `type_summary_df` that looks like this:
    ![type_summary_df](https://user-images.githubusercontent.com/107309793/178900320-c3c43be7-aaea-4e96-830f-b5a98e583eb9.png)

In this case, this DataFrame does not require bins and sorting to generate because the `per_school_summary_df' DataFrame already has a column listing each high school's type. The groupby() function can be executed again in the same way prior to assembling the DataFrame:

```
# Calculate the averages for the desired columns.
type_math_scores = per_school_summary_df.groupby(["School Type"]).mean()["Average Math Score"]
type_reading_scores = per_school_summary_df.groupby(["School Type"]).mean()["Average Reading Score"]
type_passing_math = per_school_summary_df.groupby(["School Type"]).mean()["% Passing Math"]
type_passing_reading = per_school_summary_df.groupby(["School Type"]).mean()["% Passing Reading"]
type_overall_passing = per_school_summary_df.groupby(["School Type"]).mean()["% Overall Passing"]

# Assemble into DataFrame.
type_summary_df = pd.DataFrame({
    "Average Math Score": type_math_scores,
    "Average Reading Score": type_reading_scores,
    "% Passing Math": type_passing_math,
    "% Passing Reading": type_passing_reading,
    "% Overall Passing": type_overall_passing})
```
School type is not something that is dependent on 9th grade THS scores. Therefore the data in both scripts is the same.

## Summary
