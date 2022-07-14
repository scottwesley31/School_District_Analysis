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
- How does replacing the ninth-grade scores affect the following:
  - Math and reading scores by grade
  - Scores by school spending
  - Scores by school size
  - Scores by school type

## Summary
