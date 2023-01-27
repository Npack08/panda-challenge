# pandas-challenge
My analysis of the the PyCitySchool module is as follows:

Small to medium size schools had greater overall passing % vs large schools as seen in the data chart in output 360. The data demostartes that small and medium size schools had overall passing perecentages greater than large schools passing percentages. For example the data displays that small to medium size schools had an overall passing perecnt of 90 vs large schools that had an overall passing percentage of 58.

Taking a more in depth analysis of the data on why charter schools vs district schools perform better or worse, we see in output 186 and 185 that charter schools with math and reading scores greater than 70 were predominatley female students opposed to district schools that contained a mix of male and female students with scores higher than 70. Charter schools may be producing better overall grade scores based on the gender population, but data focused on the ratio between the number of male and female students at charter vs district schools is not presented in this data set.

# Dependencies and Setup
import pandas as pd

# File to Load (Remember to Change These)
school_data_to_load = "Resources/schools_complete.csv"
student_data_to_load = "Resources/students_complete.csv"

# Read School and Student Data File and store into Pandas DataFrames
school_data = pd.read_csv(school_data_to_load)
school_data_df= school_data
student_data = pd.read_csv(student_data_to_load)
student_data_df = student_data

# Combine the data into a single dataset.  
school_data_complete = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])
school_data_complete.head()
Student ID	student_name	gender	grade	school_name	reading_score	math_score	School ID	type	size	budget
0	0	Paul Bradley	M	9th	Huang High School	66	79	0	District	2917	1910635
1	1	Victor Smith	M	12th	Huang High School	94	61	0	District	2917	1910635
2	2	Kevin Rodriguez	M	12th	Huang High School	90	60	0	District	2917	1910635
3	3	Dr. Richard Scott	M	12th	Huang High School	67	58	0	District	2917	1910635
4	4	Bonnie Ray	F	9th	Huang High School	97	84	0	District	2917	1910635
District Summary
# Calculate the total number of unique schools
school_count = school_data_complete["school_name"].value_counts()
number_of_schools = len(school_count)
print(number_of_schools)
15
# Calculate the total number of students
student_count = school_data_complete["student_name"].count()
print(student_count)
39170
# Calculate the total budget
total_budget = school_data_df["budget"].sum()
print(total_budget)
24649428
# Calculate the average (mean) math score
average_math_score = student_data_df["math_score"].mean()
print(average_math_score)
78.98537145774827
# Calculate the average (mean) reading score
average_reading_score = student_data_df["reading_score"].mean()
print(average_reading_score)
81.87784018381414
# Use the following to calculate the percentage of students who passed math (math scores greather than or equal to 70)
passing_math_count = school_data_complete[(school_data_complete["math_score"] >= 70)].count()["student_name"]
passing_math_percentage = passing_math_count / float(student_count) * 100
passing_math_percentage
74.9808526933878
# Calculate the percentage of students who passeed reading (hint: look at how the math percentage was calculated)  
passing_reading_count = school_data_complete[(school_data_complete["reading_score"] >= 70)].count()["student_name"]
passing_reading_percentage = passing_reading_count / float(student_count) * 100
passing_reading_percentage
85.80546336482001
# Use the following to calculate the percentage of students that passed math and reading
passing_math_reading_count = school_data_complete[
    (school_data_complete["math_score"] >= 70) & (school_data_complete["reading_score"] >= 70)
].count()["student_name"]
overall_passing_rate = passing_math_reading_count /  float(student_count) * 100
overall_passing_rate
65.17232575950983
# Create a high-level snapshot of 
district_summary = pd.DataFrame([{"Total Schools":number_of_schools,
                                  "Total Students":student_count,
                                  "Total Budget":total_budget,
                                  "Average Math Score":average_math_score,
                                  "Average Reading Score":average_reading_score,
                                  "% Passing Math":passing_math_percentage,
                                  "% Passing Reading":passing_reading_percentage,
                                  "% Overall Passing":overall_passing_rate}
                                ])

# Formatting
district_summary["Total Schools"] = district_summary["Total Schools"].map("{:,.1f}".format)
district_summary["Total Students"] = district_summary["Total Students"].map("{:,.1f}".format)
district_summary["Total Budget"] = district_summary["Total Budget"].map("${:,.1f}".format)
district_summary["Average Math Score"] = district_summary["Average Math Score"].map("${:,.1f}".format)
district_summary["Average Reading score"] = district_summary["Average Reading Score"].map("${:,.1f}".format)
district_summary["% Passing Math"] = district_summary["% Passing Math"].map("${:,.1f}".format)
district_summary["% Passing Reading"] = district_summary["% Passing Reading"].map("${:,.1f}".format)
district_summary["% Overall Passing"] = district_summary["% Overall Passing"].map("${:,.1f}".format)

# Display the DataFrame
district_summary
Total Schools	Total Students	Total Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing	Average Reading score
0	15.0	39,170.0	$24,649,428.0	$79.0	81.87784	$75.0	$85.8	$65.2	$81.9
School Summary
# Use the code provided to select the school type
school_types = school_data.set_index(["school_name"])["type"]
school_types
school_name
Huang High School        District
Figueroa High School     District
Shelton High School       Charter
Hernandez High School    District
Griffin High School       Charter
Wilson High School        Charter
Cabrera High School       Charter
Bailey High School       District
Holden High School        Charter
Pena High School          Charter
Wright High School        Charter
Rodriguez High School    District
Johnson High School      District
Ford High School         District
Thomas High School        Charter
Name: type, dtype: object
# Calculate the total student count
per_school_counts = school_data_complete.groupby("school_name")["student_name"].count()
print(per_school_counts)
school_name
Bailey High School       4976
Cabrera High School      1858
Figueroa High School     2949
Ford High School         2739
Griffin High School      1468
Hernandez High School    4635
Holden High School        427
Huang High School        2917
Johnson High School      4761
Pena High School          962
Rodriguez High School    3999
Shelton High School      1761
Thomas High School       1635
Wilson High School       2283
Wright High School       1800
Name: student_name, dtype: int64
# Calculate the total school budget and per capita spending
per_school_budget = school_data_complete.groupby(["school_name"]).mean()["budget"]
per_school_capita = per_school_budget / per_school_counts
per_school_capita.head()
school_name
Bailey High School      628.0
Cabrera High School     582.0
Figueroa High School    639.0
Ford High School        644.0
Griffin High School     625.0
dtype: float64
# Calculate the average test scores
per_school_math = school_data_complete.groupby(["school_name"]).mean()["math_score"]
per_school_reading = school_data_complete.groupby(["school_name"]).mean()["reading_score"]
per_school_math.head()
per_school_reading.head()
school_name
Bailey High School      81.033963
Cabrera High School     83.975780
Figueroa High School    81.158020
Ford High School        80.746258
Griffin High School     83.816757
Name: reading_score, dtype: float64
# Calculate the number of schools with math scores of 70 or higher
school_passing_math = school_data_complete.loc[school_data_complete["math_score"] >70]
school_passing_math
Student ID	student_name	gender	grade	school_name	reading_score	math_score	School ID	type	size	budget
0	0	Paul Bradley	M	9th	Huang High School	66	79	0	District	2917	1910635
4	4	Bonnie Ray	F	9th	Huang High School	97	84	0	District	2917	1910635
5	5	Bryan Miranda	M	9th	Huang High School	94	94	0	District	2917	1910635
6	6	Sheena Carter	F	11th	Huang High School	82	80	0	District	2917	1910635
8	8	Michael Roth	M	10th	Huang High School	95	87	0	District	2917	1910635
...	...	...	...	...	...	...	...	...	...	...	...
39164	39164	Joseph Anthony	M	9th	Thomas High School	97	76	14	Charter	1635	1043130
39165	39165	Donna Howard	F	12th	Thomas High School	99	90	14	Charter	1635	1043130
39167	39167	Rebecca Tanner	F	9th	Thomas High School	73	84	14	Charter	1635	1043130
39168	39168	Desiree Kidd	F	10th	Thomas High School	99	90	14	Charter	1635	1043130
39169	39169	Carolyn Jackson	F	11th	Thomas High School	95	75	14	Charter	1635	1043130
28356 rows × 11 columns

# Calculate the number of schools with reading scores of 70 or higher
school_passing_reading = school_data_complete.loc[school_data_complete["reading_score"] >70]
school_passing_reading
Student ID	student_name	gender	grade	school_name	reading_score	math_score	School ID	type	size	budget
1	1	Victor Smith	M	12th	Huang High School	94	61	0	District	2917	1910635
2	2	Kevin Rodriguez	M	12th	Huang High School	90	60	0	District	2917	1910635
4	4	Bonnie Ray	F	9th	Huang High School	97	84	0	District	2917	1910635
5	5	Bryan Miranda	M	9th	Huang High School	94	94	0	District	2917	1910635
6	6	Sheena Carter	F	11th	Huang High School	82	80	0	District	2917	1910635
...	...	...	...	...	...	...	...	...	...	...	...
39165	39165	Donna Howard	F	12th	Thomas High School	99	90	14	Charter	1635	1043130
39166	39166	Dawn Bell	F	10th	Thomas High School	95	70	14	Charter	1635	1043130
39167	39167	Rebecca Tanner	F	9th	Thomas High School	73	84	14	Charter	1635	1043130
39168	39168	Desiree Kidd	F	10th	Thomas High School	99	90	14	Charter	1635	1043130
39169	39169	Carolyn Jackson	F	11th	Thomas High School	95	75	14	Charter	1635	1043130
32500 rows × 11 columns

# Use the provided code to calculate the schools that passed both math and reading with scores of 70 or higher
passing_math_and_reading = school_data_complete[
    (school_data_complete["reading_score"] >= 70) & (school_data_complete["math_score"] >= 70)
]
passing_math_and_reading
Student ID	student_name	gender	grade	school_name	reading_score	math_score	School ID	type	size	budget
4	4	Bonnie Ray	F	9th	Huang High School	97	84	0	District	2917	1910635
5	5	Bryan Miranda	M	9th	Huang High School	94	94	0	District	2917	1910635
6	6	Sheena Carter	F	11th	Huang High School	82	80	0	District	2917	1910635
8	8	Michael Roth	M	10th	Huang High School	95	87	0	District	2917	1910635
9	9	Matthew Greene	M	10th	Huang High School	96	84	0	District	2917	1910635
...	...	...	...	...	...	...	...	...	...	...	...
39165	39165	Donna Howard	F	12th	Thomas High School	99	90	14	Charter	1635	1043130
39166	39166	Dawn Bell	F	10th	Thomas High School	95	70	14	Charter	1635	1043130
39167	39167	Rebecca Tanner	F	9th	Thomas High School	73	84	14	Charter	1635	1043130
39168	39168	Desiree Kidd	F	10th	Thomas High School	99	90	14	Charter	1635	1043130
39169	39169	Carolyn Jackson	F	11th	Thomas High School	95	75	14	Charter	1635	1043130
25528 rows × 11 columns

# Use the provided code to calculate the passing rates
per_school_passing_math = school_passing_math.groupby(["school_name"]).count()["student_name"] / per_school_counts * 100
per_school_passing_reading = school_passing_reading.groupby(["school_name"]).count()["student_name"] / per_school_counts * 100
overall_passing_rate = passing_math_and_reading.groupby(["school_name"]).count()["student_name"] / per_school_counts * 100
overall_passing_rate
school_name
Bailey High School       54.642283
Cabrera High School      91.334769
Figueroa High School     53.204476
Ford High School         54.289887
Griffin High School      90.599455
Hernandez High School    53.527508
Holden High School       89.227166
Huang High School        53.513884
Johnson High School      53.539172
Pena High School         90.540541
Rodriguez High School    52.988247
Shelton High School      89.892107
Thomas High School       90.948012
Wilson High School       90.582567
Wright High School       90.333333
Name: student_name, dtype: float64
# Create a DataFrame called `per_school_summary` with columns for the calculations above.
per_school_summary = pd.DataFrame({"School Type":school_types,
                                  "Total Students":per_school_counts,
                                  "Total School Budget":per_school_budget,
                                  "Per Student Budget":per_school_capita,
                                  "Average Math Score":per_school_math,
                                  "Average Reading Score":per_school_reading,
                                  "% Passing Math":per_school_passing_math,
                                  "% Passing Reading":per_school_passing_reading,
                                  "% Overall Passing":overall_passing_rate}
                                )

per_school_summary_copy= per_school_summary.copy()

# Formatting
per_school_summary["School Type"] = per_school_summary["School Type"].map("{:}".format)
per_school_summary["Total Students"] = per_school_summary["Total Students"].map("{:,}".format)
per_school_summary["Total School Budget"] = per_school_summary["Total School Budget"].map("${:,}".format)
per_school_summary["Per Student Budget"] = per_school_summary["Per Student Budget"].map("${:,.1f}".format)
per_school_summary["Average Math Score"] = per_school_summary["Average Math Score"].map("${:,.1f}".format)
per_school_summary["Average Reading Score"] = per_school_summary["Average Reading Score"].map("${:,.1f}".format)
per_school_summary[ "% Passing Math"] = per_school_summary[ "% Passing Math"].map("{:,.1f}".format)
per_school_summary["% Passing Reading"] = per_school_summary["% Passing Reading"].map("${:,.1f}".format)
per_school_summary["% Overall Passing"] = per_school_summary["% Overall Passing"].map("${:,.1f}".format)

# Display the DataFrame
per_school_summary
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
school_name									
Bailey High School	District	4,976	$3,124,928.0	$628.0	$77.0	$81.0	64.6	$79.3	$54.6
Cabrera High School	Charter	1,858	$1,081,356.0	$582.0	$83.1	$84.0	89.6	$93.9	$91.3
Figueroa High School	District	2,949	$1,884,411.0	$639.0	$76.7	$81.2	63.8	$78.4	$53.2
Ford High School	District	2,739	$1,763,916.0	$644.0	$77.1	$80.7	65.8	$77.5	$54.3
Griffin High School	Charter	1,468	$917,500.0	$625.0	$83.4	$83.8	89.7	$93.4	$90.6
Hernandez High School	District	4,635	$3,022,020.0	$652.0	$77.3	$80.9	64.7	$78.2	$53.5
Holden High School	Charter	427	$248,087.0	$581.0	$83.8	$83.8	90.6	$92.7	$89.2
Huang High School	District	2,917	$1,910,635.0	$655.0	$76.6	$81.2	63.3	$78.8	$53.5
Johnson High School	District	4,761	$3,094,650.0	$650.0	$77.1	$81.0	63.9	$78.3	$53.5
Pena High School	Charter	962	$585,858.0	$609.0	$83.8	$84.0	91.7	$92.2	$90.5
Rodriguez High School	District	3,999	$2,547,363.0	$637.0	$76.8	$80.7	64.1	$77.7	$53.0
Shelton High School	Charter	1,761	$1,056,600.0	$600.0	$83.4	$83.7	89.9	$92.6	$89.9
Thomas High School	Charter	1,635	$1,043,130.0	$638.0	$83.4	$83.8	90.2	$92.9	$90.9
Wilson High School	Charter	2,283	$1,319,574.0	$578.0	$83.3	$84.0	90.9	$93.3	$90.6
Wright High School	Charter	1,800	$1,049,400.0	$583.0	$83.7	$84.0	90.3	$93.4	$90.3
Highest-Performing Schools (by % Overall Passing)
# Sort the schools by `% Overall Passing` in descending order and display the top 5 rows
school_summary_df = per_school_summary.sort_values(["% Overall Passing"], ascending = False)
school_summary_df.head()
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
school_name									
Cabrera High School	Charter	1,858	$1,081,356.0	$582.0	$83.1	$84.0	89.6	$93.9	$91.3
Thomas High School	Charter	1,635	$1,043,130.0	$638.0	$83.4	$83.8	90.2	$92.9	$90.9
Griffin High School	Charter	1,468	$917,500.0	$625.0	$83.4	$83.8	89.7	$93.4	$90.6
Wilson High School	Charter	2,283	$1,319,574.0	$578.0	$83.3	$84.0	90.9	$93.3	$90.6
Pena High School	Charter	962	$585,858.0	$609.0	$83.8	$84.0	91.7	$92.2	$90.5
Bottom Performing Schools (By % Overall Passing)
# Sort the schools by `% Overall Passing` in ascending order and display the top 5 rows.
school_df = per_school_summary.sort_values(["% Overall Passing"], ascending = True)
school_df.head()
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
school_name									
Rodriguez High School	District	3,999	$2,547,363.0	$637.0	$76.8	$80.7	64.1	$77.7	$53.0
Figueroa High School	District	2,949	$1,884,411.0	$639.0	$76.7	$81.2	63.8	$78.4	$53.2
Hernandez High School	District	4,635	$3,022,020.0	$652.0	$77.3	$80.9	64.7	$78.2	$53.5
Huang High School	District	2,917	$1,910,635.0	$655.0	$76.6	$81.2	63.3	$78.8	$53.5
Johnson High School	District	4,761	$3,094,650.0	$650.0	$77.1	$81.0	63.9	$78.3	$53.5
Math Scores by Grade
# Use the code provided to separate the data by grade
ninth_graders = school_data_complete[(school_data_complete["grade"] == "9th")]
tenth_graders = school_data_complete[(school_data_complete["grade"] == "10th")]
eleventh_graders = school_data_complete[(school_data_complete["grade"] == "11th")]
twelfth_graders = school_data_complete[(school_data_complete["grade"] == "12th")]

# Group by "school_name" and take the mean of each.
ninth_graders_scores = ninth_graders.groupby(["school_name"]).mean()
tenth_graders_scores = tenth_graders.groupby(["school_name"]).mean()
eleventh_graders_scores = eleventh_graders.groupby(["school_name"]).mean()
twelfth_graders_scores = twelfth_graders.groupby(["school_name"]).mean()

# Use the code to select only the `math_score`.
ninth_grader_math_scores = ninth_graders_scores["math_score"]
tenth_grader_math_scores = tenth_graders_scores["math_score"]
eleventh_grader_math_scores = eleventh_graders_scores["math_score"]
twelfth_grader_math_scores = twelfth_graders_scores["math_score"]

# Combine each of the scores above into single DataFrame called `math_scores_by_grade`
math_scores_by_grade=pd.DataFrame({"9th":ninth_grader_math_scores,"10th": tenth_grader_math_scores,"11th": eleventh_grader_math_scores,"12th": twelfth_grader_math_scores})

# Minor data wrangling
math_scores_by_grade = math_scores_by_grade[["9th", "10th", "11th", "12th"]]
math_scores_by_grade.index.name = None

# Display the DataFrame
math_scores_by_grade
9th	10th	11th	12th
Bailey High School	77.083676	76.996772	77.515588	76.492218
Cabrera High School	83.094697	83.154506	82.765560	83.277487
Figueroa High School	76.403037	76.539974	76.884344	77.151369
Ford High School	77.361345	77.672316	76.918058	76.179963
Griffin High School	82.044010	84.229064	83.842105	83.356164
Hernandez High School	77.438495	77.337408	77.136029	77.186567
Holden High School	83.787402	83.429825	85.000000	82.855422
Huang High School	77.027251	75.908735	76.446602	77.225641
Johnson High School	77.187857	76.691117	77.491653	76.863248
Pena High School	83.625455	83.372000	84.328125	84.121547
Rodriguez High School	76.859966	76.612500	76.395626	77.690748
Shelton High School	83.420755	82.917411	83.383495	83.778976
Thomas High School	83.590022	83.087886	83.498795	83.497041
Wilson High School	83.085578	83.724422	83.195326	83.035794
Wright High School	83.264706	84.010288	83.836782	83.644986
 
Reading Score by Grade
# Use the code provided to separate the data by grade
ninth_graders = school_data_complete[(school_data_complete["grade"] == "9th")]
tenth_graders = school_data_complete[(school_data_complete["grade"] == "10th")]
eleventh_graders = school_data_complete[(school_data_complete["grade"] == "11th")]
twelfth_graders = school_data_complete[(school_data_complete["grade"] == "12th")]

# Group by "school_name" and take the mean of each.
ninth_graders_scores = ninth_graders.groupby(["school_name"]).mean()
tenth_graders_scores = tenth_graders.groupby(["school_name"]).mean()
eleventh_graders_scores = eleventh_graders.groupby(["school_name"]).mean()
twelfth_graders_scores = twelfth_graders.groupby(["school_name"]).mean()

# Use the code to select only the `reading_score`.
ninth_grader_reading_scores = ninth_graders_scores["reading_score"]
tenth_grader_reading_scores = tenth_graders_scores["reading_score"]
eleventh_grader_reading_scores = eleventh_graders_scores["reading_score"]
twelfth_grader_reading_scores = twelfth_graders_scores["reading_score"]

# Combine each of the scores above into single DataFrame called `reading_scores_by_grade`
reading_scores_by_grade=pd.DataFrame({"9th":ninth_grader_reading_scores,"10th": tenth_grader_reading_scores,"11th": eleventh_grader_reading_scores,"12th": twelfth_grader_reading_scores})


# Minor data wrangling
reading_scores_by_grade = reading_scores_by_grade[["9th", "10th", "11th", "12th"]]
reading_scores_by_grade.index.name = None

# Display the DataFrame
reading_scores_by_grade
9th	10th	11th	12th
Bailey High School	81.303155	80.907183	80.945643	80.912451
Cabrera High School	83.676136	84.253219	83.788382	84.287958
Figueroa High School	81.198598	81.408912	80.640339	81.384863
Ford High School	80.632653	81.262712	80.403642	80.662338
Griffin High School	83.369193	83.706897	84.288089	84.013699
Hernandez High School	80.866860	80.660147	81.396140	80.857143
Holden High School	83.677165	83.324561	83.815534	84.698795
Huang High School	81.290284	81.512386	81.417476	80.305983
Johnson High School	81.260714	80.773431	80.616027	81.227564
Pena High School	83.807273	83.612000	84.335938	84.591160
Rodriguez High School	80.993127	80.629808	80.864811	80.376426
Shelton High School	84.122642	83.441964	84.373786	82.781671
Thomas High School	83.728850	84.254157	83.585542	83.831361
Wilson High School	83.939778	84.021452	83.764608	84.317673
Wright High School	83.833333	83.812757	84.156322	84.073171
Scores by School Spending
# Establish the bins 
spending_bins = [0, 585, 630, 645, 680]
labels = ["<$585", "$585-630", "$630-645", "$645-680"]
# Create a copy of the school summary since it has the "Per Student Budget" 
school_spending_df = per_school_summary_copy.copy()
school_spending_df.head()
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
school_name									
Bailey High School	District	4976	3124928.0	628.0	77.048432	81.033963	64.630225	79.300643	54.642283
Cabrera High School	Charter	1858	1081356.0	582.0	83.061895	83.975780	89.558665	93.864370	91.334769
Figueroa High School	District	2949	1884411.0	639.0	76.711767	81.158020	63.750424	78.433367	53.204476
Ford High School	District	2739	1763916.0	644.0	77.102592	80.746258	65.753925	77.510040	54.289887
Griffin High School	Charter	1468	917500.0	625.0	83.351499	83.816757	89.713896	93.392371	90.599455
# Use `pd.cut` to categorize spending based on the bins.
school_spending_df["Spending Ranges (Per Student)"] = pd.cut(per_school_capita,spending_bins, labels=labels, right=False)
school_spending_df
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing	Spending Ranges (Per Student)
school_name										
Bailey High School	District	4976	3124928.0	628.0	77.048432	81.033963	64.630225	79.300643	54.642283	$585-630
Cabrera High School	Charter	1858	1081356.0	582.0	83.061895	83.975780	89.558665	93.864370	91.334769	<$585
Figueroa High School	District	2949	1884411.0	639.0	76.711767	81.158020	63.750424	78.433367	53.204476	$630-645
Ford High School	District	2739	1763916.0	644.0	77.102592	80.746258	65.753925	77.510040	54.289887	$630-645
Griffin High School	Charter	1468	917500.0	625.0	83.351499	83.816757	89.713896	93.392371	90.599455	$585-630
Hernandez High School	District	4635	3022020.0	652.0	77.289752	80.934412	64.746494	78.187702	53.527508	$645-680
Holden High School	Charter	427	248087.0	581.0	83.803279	83.814988	90.632319	92.740047	89.227166	<$585
Huang High School	District	2917	1910635.0	655.0	76.629414	81.182722	63.318478	78.813850	53.513884	$645-680
Johnson High School	District	4761	3094650.0	650.0	77.072464	80.966394	63.852132	78.281874	53.539172	$645-680
Pena High School	Charter	962	585858.0	609.0	83.839917	84.044699	91.683992	92.203742	90.540541	$585-630
Rodriguez High School	District	3999	2547363.0	637.0	76.842711	80.744686	64.066017	77.744436	52.988247	$630-645
Shelton High School	Charter	1761	1056600.0	600.0	83.359455	83.725724	89.892107	92.617831	89.892107	$585-630
Thomas High School	Charter	1635	1043130.0	638.0	83.418349	83.848930	90.214067	92.905199	90.948012	$630-645
Wilson High School	Charter	2283	1319574.0	578.0	83.274201	83.989488	90.932983	93.254490	90.582567	<$585
Wright High School	Charter	1800	1049400.0	583.0	83.682222	83.955000	90.277778	93.444444	90.333333	<$585
#  Calculate averages for the desired columns. 
spending_math_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"]).mean()["Average Math Score"]
spending_reading_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"]).mean()["Average Reading Score"]
spending_passing_math = school_spending_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Math"]
spending_passing_reading = school_spending_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Reading"]
overall_passing_spending = school_spending_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Overall Passing"]
# Assemble into DataFrame
spending_summary = pd.DataFrame({"Average Math Score":spending_math_scores,
                                "Average Reading Score":spending_reading_scores,
                                "% Passing Math":spending_passing_math,
                                "% Passing Reading":spending_passing_reading,
                                "% Overall Passing":overall_passing_spending})

# Display results
spending_summary.style.format({"Average Math Score":"{:.1f}",
                                "Average Reading Score":"{:.1f}",
                                "% Passing Math":"{:.1f}",
                                "% Passing Reading":"{:.1f}",
                                "% Overall Passing":"{:.1f}"})
 	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
Spending Ranges (Per Student)	 	 	 	 	 
<$585	83.5	83.9	90.4	93.3	90.4
$585-630	81.9	83.2	84.0	89.4	81.4
$630-645	78.5	81.6	70.9	81.6	62.9
$645-680	77.0	81.0	64.0	78.4	53.5
Scores by School Size
# Establish the bins.
size_bins = [0, 1000, 2000, 5000]
labels = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
# Categorize the spending based on the bins
# Use `pd.cut` on the "Total Students" column of the `per_school_summary` DataFrame.

per_school_summary_copy["School Size"] = pd.cut(per_school_counts, size_bins, labels=labels, right=False)
per_school_summary_copy
School Type	Total Students	Total School Budget	Per Student Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing	School Size
school_name										
Bailey High School	District	4976	3124928.0	628.0	77.048432	81.033963	64.630225	79.300643	54.642283	Large (2000-5000)
Cabrera High School	Charter	1858	1081356.0	582.0	83.061895	83.975780	89.558665	93.864370	91.334769	Medium (1000-2000)
Figueroa High School	District	2949	1884411.0	639.0	76.711767	81.158020	63.750424	78.433367	53.204476	Large (2000-5000)
Ford High School	District	2739	1763916.0	644.0	77.102592	80.746258	65.753925	77.510040	54.289887	Large (2000-5000)
Griffin High School	Charter	1468	917500.0	625.0	83.351499	83.816757	89.713896	93.392371	90.599455	Medium (1000-2000)
Hernandez High School	District	4635	3022020.0	652.0	77.289752	80.934412	64.746494	78.187702	53.527508	Large (2000-5000)
Holden High School	Charter	427	248087.0	581.0	83.803279	83.814988	90.632319	92.740047	89.227166	Small (<1000)
Huang High School	District	2917	1910635.0	655.0	76.629414	81.182722	63.318478	78.813850	53.513884	Large (2000-5000)
Johnson High School	District	4761	3094650.0	650.0	77.072464	80.966394	63.852132	78.281874	53.539172	Large (2000-5000)
Pena High School	Charter	962	585858.0	609.0	83.839917	84.044699	91.683992	92.203742	90.540541	Small (<1000)
Rodriguez High School	District	3999	2547363.0	637.0	76.842711	80.744686	64.066017	77.744436	52.988247	Large (2000-5000)
Shelton High School	Charter	1761	1056600.0	600.0	83.359455	83.725724	89.892107	92.617831	89.892107	Medium (1000-2000)
Thomas High School	Charter	1635	1043130.0	638.0	83.418349	83.848930	90.214067	92.905199	90.948012	Medium (1000-2000)
Wilson High School	Charter	2283	1319574.0	578.0	83.274201	83.989488	90.932983	93.254490	90.582567	Large (2000-5000)
Wright High School	Charter	1800	1049400.0	583.0	83.682222	83.955000	90.277778	93.444444	90.333333	Medium (1000-2000)
# Calculate averages for the desired columns. 
size_math_scores = per_school_summary_copy.groupby(["School Size"]).mean()["Average Math Score"]
size_reading_scores = per_school_summary_copy.groupby(["School Size"]).mean()["Average Reading Score"]
size_passing_math = per_school_summary_copy.groupby(["School Size"]).mean()["% Passing Math"]
size_passing_reading = per_school_summary_copy.groupby(["School Size"]).mean()["% Passing Reading"]
size_overall_passing = per_school_summary_copy.groupby(["School Size"]).mean()["% Overall Passing"]
# Create a DataFrame called `size_summary` that breaks down school performance based on school size (small, medium, or large).
# Use the scores above to create a new DataFrame called `size_summary`
size_summary = pd.DataFrame({"Average Math Score":size_math_scores,
                                "Average Reading Score":size_reading_scores,
                                "% Passing Math":size_passing_math,
                                "% Passing Reading":size_passing_reading,
                                "% Overall Passing":size_overall_passing})

# Display results
size_summary.style.format({"Average Math Score":"{:.6f}",
                                "Average Reading Score":"{:.6f}",
                                "% Passing Math":"{:.6f}",
                                "% Passing Reading":"{:.6f}",
                                "% Overall Passing":"{:.6f}"})
 	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
School Size	 	 	 	 	 
Small (<1000)	83.821598	83.929843	91.158155	92.471895	89.883853
Medium (1000-2000)	83.374684	83.864438	89.931303	93.244843	90.621535
Large (2000-5000)	77.746417	81.344493	67.631335	80.190800	58.286003
Scores by School Type
# Group the per_school_summary DataFrame by "School Type" and average the results.
type_math_scores = per_school_summary_copy.groupby(["School Type"]).mean()
type_reading_scores = per_school_summary_copy.groupby(["School Type"]).mean()
type_passing_math = per_school_summary_copy.groupby(["School Type"]).mean()
type_passing_reading = per_school_summary_copy.groupby(["School Type"]).mean()
type_overall_passing = per_school_summary_copy.groupby(["School Type"]).mean()

# Use the code provided to select new column data
average_math_score_by_type = type_math_scores["Average Math Score"]
average_reading_score_by_type = type_reading_scores["Average Reading Score"]
average_percent_passing_math_by_type = type_passing_math["% Passing Math"]
average_percent_passing_reading_by_type = type_passing_reading["% Passing Reading"]
average_percent_overall_passing_by_type = type_overall_passing["% Overall Passing"]
# Assemble the new data by type into a DataFrame called `type_summary`
type_summary=pd.DataFrame({"Average Math Score":average_math_score_by_type,
                                "Average Reading Score":average_reading_score_by_type,
                                "% Passing Math":average_percent_passing_math_by_type,
                                "% Passing Reading":average_percent_passing_reading_by_type,
                                "% Overall Passing":average_percent_overall_passing_by_type})

# Display results
type_summary.style.format({"Average Math Score":"{:.6f}",
                                "Average Reading Score":"{:.6f}",
                                "% Passing Math":"{:.6f}",
                                "% Passing Reading":"{:.6f}",
                                "% Overall Passing":"{:.6f}"})
 	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing
School Type	 	 	 	 	 
Charter	83.473852	83.896421	90.363226	93.052812	90.432244
District	76.956733	80.966636	64.302528	78.324559	53.672208
 
