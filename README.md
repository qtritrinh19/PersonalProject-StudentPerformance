# 📊 Three-Way ANOVA on Student Performance

## 1. Project Overview

This project analyzes the impact of **gender**, **grade level**, and **attendance rate** on the mathematics performance of high school students. A three-way Analysis of Variance (ANOVA) is employed to identify significant main effects and interaction effects among these factors. The goal is to understand how demographic and behavioral variables jointly shape academic outcomes.

The analysis is based on a dataset of 681 students.

---

## 2. Analytical Workflow 📈

The project followed these key steps:

1.  **Data Preprocessing**:
    * Filtered the `gender` variable to include only "Male" and "Female".
    * Grouped `grade_level` into two categories: "Grade 9–10" and "Grade 11–12".
    * Categorized the continuous `attendance_rate` into three levels ("Low", "Moderate", "High") based on quartiles.

2.  **Exploratory Analysis**:
    * Generated interaction plots to visually inspect the relationships between the independent variables and students' `math_score`.

3.  **ANOVA Modeling**:
    * Developed two separate models:
        * An **additive model** (`math_score ~ gender + grade_group + attendance_group`) focusing only on main effects.
        * An **interaction model** (`math_score ~ gender * grade_group * attendance_group`) to test for combined effects.

4.  **Model Comparison & Selection**:
    * Compared the additive and interaction models using a nested F-test to determine if the complex interaction terms provided a significantly better fit.

5.  **Post-Hoc Analysis**:
    * Performed a **Tukey HSD (Honestly Significant Difference)** test on the selected model to identify which specific group pairs showed statistically significant differences in mean math scores.

6.  **Assumption Diagnostics**:
    * Validated the ANOVA model's core assumptions:
        * Independence of residuals (Durbin-Watson test).
        * Normality of residuals (Shapiro-Wilk test, Q-Q plot).
        * Homogeneity of variance (Levene's test, Scale-Location plot).

---

## 3. Key Findings 💡

* **Significant Main Effects**: Both **gender** ($p = 0.030$) and **attendance rate** ($p = 0.040$) have a statistically significant effect on math scores. Grade level showed a marginal effect ($p = 0.065$).

* **Significant Three-Way Interaction**: The interaction model revealed a significant three-way interaction between `gender`, `grade_group`, and `attendance_group` ($p = 0.009$). This indicates that the effect of any one factor on math scores depends on the levels of the other two.

* **Model Selection**: Despite the significant interaction, the nested F-test ($p = 0.1497$) showed that the more complex interaction model did not provide a statistically significant improvement over the simpler **additive model**. Therefore, the additive model was selected for its parsimony and ease of interpretation.

* **Group Differences (Tukey HSD)**:
    * On average, male students scored **2.35 points higher** in math than female students, a statistically significant difference.
    * No significant pairwise differences were found between attendance groups or grade groups after accounting for other factors in the model.

* **Model Validity**: The model assumptions for **independence of residuals** and **homogeneity of variance** were met. The assumption of **normality of residuals was violated**, but given the large sample size (N=681), the ANOVA results are considered robust enough to be reliable.

---

## 4. Conclusion

This analysis demonstrates that gender and attendance are significant predictors of students' math performance. The discovery of a complex three-way interaction highlights that these factors do not operate in isolation. While the simpler additive model is preferred for general interpretation, the findings underscore the importance of considering multiple demographic and behavioral factors simultaneously to understand the nuances of academic achievement.

---

## 5. Technologies Used 💻

* **Language**: R
* **Key Packages**: `dplyr`, `car`

---

## 6. How to Reproduce

1.  Clone this repository to your local machine.
2.  Ensure you have R and RStudio installed.
3.  Install the necessary R packages by running the following command in your R console:
    ```r
    install.packages(c("dplyr", "car"))
    ```
4.  Make sure the dataset `student_info.csv` is in the project's root directory.
5.  Open the R script or R Markdown file and run the code.
