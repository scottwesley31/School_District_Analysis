# School District Analysis
Module 4

## Overview of the school district analysis
### Purpose
After the compilation of data contained in two different csv files (`schools_complete.csv` and `students_complete.csv`) and an in-depth analysis of a school district's reading and math scores by grade, the school board discovered a dishonest reporting of ninth-grade math scores for one high school in particular - Thomas High School (THS). The school board is requesting an updated analysis of the school district data which excludes said math scores but keeps the rest of the data intact. A report describing how the updated analysis has affected the overall school district results has been requested.

## Results
- How is the district summary affected?
The original district summary results generated from the `district_summary_df` DataFrame in Python is as follows:
![district_summary_df_original](https://user-images.githubusercontent.com/107309793/178860750-fbf6a9ec-5031-40ef-9fbd-828efbdd65da.png)

After removing the THS ninth-grader math scores in Python and running the modified code, the updated district summary results look like this:
![district_summary_df_updated](https://user-images.githubusercontent.com/107309793/178860872-96381da6-6e06-491c-b064-70188178bd9e.png)

To summarize the differences seen between these 2 dataframes:
- **Total Schools:** unaffected
- **Total Students:** unaffected
- **Total Budget:** unaffected
- **Average Math Score:** score dropped from 79.0 to 78.9 (0.1 difference)
- **Average Reading Score:** unaffected
- **% Passing Math:** percent dropped from 75 to 74.8 (0.2 difference)
- **% Passing Reading:** percent dropped from 86 to 85.7 (0.3 difference)
- **% Overall Passing:** percent dropped from 65 to 64.9 (0.1 difference)

- How is the school summary affected?
- How does replacing the nenth graders' math and reading scores affect Thomas High School's performance relative to the other schools?
- How does replacing the ninth-grade scores affect the following:
  - Math and reading scores by grade
  - Scores by school spending
  - Scores by school size
  - Scores by school type

## Summary
