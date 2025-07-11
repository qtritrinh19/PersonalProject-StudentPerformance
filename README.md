# Three-Way ANOVA on Student Performance
This project investigates the influence of gender, grade level, and attendance rate on students’ mathematics performance using a three-way ANOVA approach. The analysis is based on a dataset of 681 high school students and focuses on identifying both main effects and interaction effects among the three factors.

Key steps include:

- Data cleaning and transformation (grouping grades, categorizing attendance, filtering gender)

- Descriptive statistics and visualization of variable interactions

- Fitting both additive and interaction ANOVA models

- Validating model assumptions (normality, independence, homogeneity of variance)

- Performing post hoc Tukey tests to explore pairwise differences

The results highlight the importance of gender and attendance in predicting academic outcomes, with interaction effects suggesting deeper contextual dynamics. Despite minor deviations from normality, the ANOVA model remains robust and provides valuable insights into how demographic and behavioral factors jointly shape student performance.

## 1. Loading and Processing Data

### 1.1. Loading the Data and Selecting Variables of Interest

```r
data <- read.csv('student_info.csv')
```

|student_id |name       |gender | age| grade_level| math_score| reading_score| writing_score| attendance_rate|parent_education | study_hours|internet_access |lunch_type      |extra_activities |final_result |
|:----------|:----------|:------|---:|-----------:|----------:|-------------:|-------------:|---------------:|:----------------|-----------:|:---------------|:---------------|:----------------|:------------|
|S1         |Student_1  |Other  |  17|          10|         74|            61|            90|        94.66000|Master's         |    4.120192|Yes             |Free or reduced |Yes              |Fail         |
|S2         |Student_2  |Male   |  17|          12|         99|            70|            91|        93.17323|Bachelor's       |    2.886506|No              |Free or reduced |No               |Pass         |
|S3         |Student_3  |Other  |  17|           9|         59|            60|            99|        98.63110|PhD              |    1.909926|No              |Free or reduced |No               |Fail         |
|S4         |Student_4  |Other  |  17|          12|         70|            88|            69|        96.41962|PhD              |    1.664740|No              |Standard        |No               |Pass         |
|S5         |Student_5  |Male   |  15|           9|         85|            77|            94|        91.33211|PhD              |    2.330918|Yes             |Free or reduced |No               |Pass         |
|S6         |Student_6  |Male   |  17|          11|         61|            54|            89|        93.14061|Master's         |    4.759697|No              |Free or reduced |Yes              |Fail         |
|S7         |Student_7  |Other  |  16|           9|         76|            66|            88|        97.96773|Bachelor's       |    2.372107|Yes             |Free or reduced |No               |Pass         |
|S8         |Student_8  |Female |  16|          11|         89|            69|            98|        87.99038|PhD              |    4.952496|No              |Standard        |Yes              |Pass         |
|S9         |Student_9  |Other  |  17|          12|         51|            90|            72|        86.53589|High School      |    2.940922|No              |Standard        |No               |Pass         |
|S10        |Student_10 |Other  |  17|          11|         62|            95|            96|        80.21622|Bachelor's       |    1.716656|Yes             |Standard        |Yes              |Fail         |

In this study, I selected three independent variables: `gender`, `grade_level`, and `attendance_rate`, to analyze their effects on the dependent variable `math_score` using **ANOVA**. These variables were chosen due to their potential influence on academic performance and their practical relevance in an educational context.

```r
# Select variables relevant for analysis
data <- data %>%
  select(gender, grade_level, attendance_rate, math_score)
```
|gender | grade_level| attendance_rate| math_score|
|:------|-----------:|---------------:|----------:|
|Other  |          10|        94.66000|         74|
|Male   |          12|        93.17323|         99|
|Other  |           9|        98.63110|         59|
|Other  |          12|        96.41962|         70|
|Male   |           9|        91.33211|         85|
|Male   |          11|        93.14061|         61|
|Other  |           9|        97.96773|         76|
|Female |          11|        87.99038|         89|
|Other  |          12|        86.53589|         51|
|Other  |          11|        80.21622|         62|


### 1.2. Examine descriptive statistics and data types

```r
# Initial data inspection
summary(data)
glimpse(data)
```

**Summary**

|   gender        | grade_level  |attendance_rate |  math_score  |
|:----------------|:-------------|:---------------|:-------------|
|Length:681       |Min.   : 9.00 |Min.   :80.00   |Min.   :50.00 |
|Class :character |1st Qu.: 9.00 |1st Qu.:85.02   |1st Qu.:64.00 |
|Mode  :character |Median :10.00 |Median :89.68   |Median :76.00 |
|NA               |Mean   :10.47 |Mean   :89.87   |Mean   :75.65 |
|NA               |3rd Qu.:12.00 |3rd Qu.:94.62   |3rd Qu.:88.00 |
|NA               |Max.   :12.00 |Max.   :99.95   |Max.   :99.00 |

**Data Structure**  
Rows: 681  
Columns: 4  
$ gender          <chr> "Male", "Male", "Male", "Female", "Male", "Female", "Male", "Female", "Female", "Fem…  
$ grade_level     <int> 12, 9, 11, 11, 10, 11, 10, 9, 12, 12, 9, 11, 11, 12, 10, 11, 11, 9, 11, 11, 12, 10, …  
$ attendance_rate <dbl> 93.17323, 91.33211, 93.14061, 87.99038, 82.08909, 89.27963, 82.37418, 99.61136, 84.2…  
$ math_score      <int> 99, 85, 61, 89, 65, 81, 83, 60, 59, 64, 66, 89, 67, 58, 74, 89, 63, 74, 69, 93, 81, …  

To prepare the data for analysis, the following transformations were applied:

- Gender: Only records with valid values of "Male" or "Female" are retained, with any ambiguous or missing entries removed.

- Grade Level: The original grade levels are grouped into two broader academic categories to simplify analysis:

  - Grades 9 and 10 are combined into "Grade 9–10"    

  - Grades 11 and 12 are combined into "Grade 11–12"

- Attendance Rate: The continuous attendance_rate variable is categorized into three ordinal levels based on quartile thresholds:

  - "Low Attendance": ≤ 1st quartile (≤ 84.96)

  - "Moderate Attendance": between 1st and 3rd quartile (84.97 – 94.63)

  - "High Attendance": > 3rd quartile (> 94.63)

### 1.3. Missing Data Check

```r
# Check for missing values in each column
colSums(is.na(data))
```

|                |  x|
|:---------------|--:|
|gender          |  0|
|grade_level     |  0|
|attendance_rate |  0|
|math_score      |  0|

The dataset contains no missing or null values.

### 1.4. Data Preprocessing

**Gender**
```r
# Keep only Male and Female students for consistent gender analysis
data <- data %>%
  filter(gender %in% c("Male", "Female"))
```

**Grade Group**
```r
# Group grade levels into lower (09–10) and upper (11–12) high school
data <- data %>%
  mutate(grade_group = case_when(
    grade_level <= 10 ~ "09-10",
    grade_level >= 11 ~ "11-12"
  ))
```

**Attendace Group**

```r
qs <- quantile(data$attendance_rate, probs = c(0.25, 0.75), na.rm = TRUE)

data <- data %>%
  mutate(attendance_group = case_when(
    attendance_rate <= qs[1] ~ "1. Low Attendance",
    attendance_rate <= qs[2] ~ "2. Moderate Attendance",
    attendance_rate >  qs[2] ~ "3. High Attendance"
  ))
```

## 2. Three-Way ANOVA Model

### 2.1. Visualizing the Interaction Between Three Factors Using Plots

```r
par(mfrow = c(2, 3))

# Examines whether the effect of grade level on math scores differs between male and female students
with(data, interaction.plot(gender, grade_group, math_score, lwd = 2, col = 1:1))

# Assesses whether attendance levels influence math scores differently by gender
with(data, interaction.plot(gender, attendance_group, math_score, lwd = 2, col = 2:1))

# Equivalent to the first one but with axes swapped, offering a complementary view
with(data, interaction.plot(grade_group, gender, math_score, lwd = 2, col = 1:2))

# Investigates whether the impact of attendance varies between lower and upper grade levels
with(data, interaction.plot(grade_group, attendance_group, math_score, lwd = 2, col = 2:2))

# Explores how gender differences manifest at varying levels of attendance
with(data, interaction.plot(attendance_group, gender, math_score, lwd = 2, col = 1:3))

# Evaluates whether grade level moderates the effect of attendance on math performance
with(data, interaction.plot(attendance_group, grade_group, math_score, lwd = 2, col = 2:3))
```
