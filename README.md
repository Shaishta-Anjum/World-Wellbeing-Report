![world](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/krzysztof-hepner-TH7TW20de9s-unsplash%20cropped.jpg?raw=true)
# World Wellbeing Report
The World Wellbeing Report is a comprehensive analysis of global educational data, derived from the Kaggle dataset "World Educational Data."

**Data Source**: (https://www.kaggle.com/datasets/nelgiriyewithana/world-educational-data?resource=download)

**Software Tools used**: Microsoft Excel, Microsoft Power BI, PostgreSQL-pgAdmin4

### Step 1: Writing Queries in PostgreSQL
1. **Create a Database**

2. **Create Table Global_Education**
      ```sql
      CREATE TABLE Global_Education (
      Country_Area VARCHAR(55),
      Latitude DOUBLE PRECISION,
      Longitude DOUBLE PRECISION,
      OOSR_Pre0Primary_Age_Male INT,
      OOSR_Pre0Primary_Age_Female INT,
      OOSR_Primary_Age_Male INT,
      OOSR_Primary_Age_Female INT,
      OOSR_Lower_Secondary_Age_Male INT,
      OOSR_Lower_Secondary_Age_Female INT,
      OOSR_Upper_Secondary_Age_Male INT,
      OOSR_Upper_Secondary_Age_Female INT,
      Completion_Rate_Primary_Male INT,
      Completion_Rate_Primary_Female INT,
      Completion_Rate_Lower_Secondary_Male INT,
      Completion_Rate_Lower_Secondary_Female INT,
      Completion_Rate_Upper_Secondary_Male INT,
      Completion_Rate_Upper_Secondary_Female INT,
      Grade_2_3_Proficiency_Reading INT,
      Grade_2_3_Proficiency_Math INT,
      Primary_End_Proficiency_Reading INT,
      Primary_End_Proficiency_Math INT,
      Lower_Secondary_End_Proficiency_Reading INT,
      Lower_Secondary_End_Proficiency_Math INT,
      Youth_15_24_Literacy_Rate_Male INT,
      Youth_15_24_Literacy_Rate_Female INT,
      Birth_Rate NUMERIC(5, 2),
      Gross_Primary_Education_Enrollment NUMERIC(5,2),
      Gross_Tertiary_Education_Enrollment NUMERIC(5,2),
      Unemployment_Rate NUMERIC(5, 2)
      );
      ```
      
3. **Import Global_Education csv file in the table**
Initially I ran into some problem regarding encoding of the file which I set as UTF8 and to find the correct encoding, I used a simple python code.
      ```python
      with open('Global_Education.csv') as f:
                       print(f)
      ```
      ![encoding](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/16.png?raw=true)

Here in the output we found the encoding of the file and now we have to set the encoding as WIN1252 and import the file.

4. **Average Out of School Rate (OOSR) for primary age by Gender**
      ```sql
      SELECT
      AVG(OOSR_Primary_Age_Male) AS avg_oosr_male,
      AVG(OOSR_Primary_Age_Female) AS avg_oosr_female
      FROM Global_Education;
      ```
      ![Avg OOSR Primary](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/1.png?raw=true)


6. **OOSR by Education Level**
      ```sql
      SELECT
      'Pre-Primary' AS education_level,
      AVG((OOSR_Pre0Primary_Age_Male + OOSR_Pre0Primary_Age_Female) / 2) AS avg_oosr_combined
      FROM Global_Education
      UNION ALL
      SELECT
      'Primary' AS education_level,
      AVG((OOSR_Primary_Age_Male + OOSR_Primary_Age_Female) / 2) AS avg_oosr_combined
      FROM Global_Education
      UNION ALL
      SELECT
      'Lower Secondary' AS education_level,
      AVG((OOSR_Lower_Secondary_Age_Male + OOSR_Lower_Secondary_Age_Female) / 2) AS avg_oosr_combined
      FROM Global_Education
      UNION ALL
      SELECT
      'Upper Secondary' AS education_level,
      AVG((OOSR_Upper_Secondary_Age_Male + OOSR_Upper_Secondary_Age_Female) / 2) AS avg_oosr_combined
      FROM Global_Education;
      ```
      ![OOSR](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/6.png?raw=true)

7. **Gender Disparities in Literacy Rate in Youth**
     ```sql
     SELECT
    AVG(Youth_15_24_Literacy_Rate_Male) AS avg_literacy_rate_male,
    AVG(Youth_15_24_Literacy_Rate_Female) AS avg_literacy_rate_female
    FROM Global_Education;
     ```
    ![lr](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/7.png?raw=true)

8. **Top 10 Countries with Highest Unemployment Rate**
     ```sql
     SELECT
    Country_Area,
    Unemployment_Rate
    FROM Global_Education
    ORDER BY Unemployment_Rate DESC
    LIMIT 10;
     ```
     ![ur10](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/8.png?raw=true)

9. **Countries with best Employment Rate**
     ```sql
     SELECT
    Country_Area,
    Unemployment_Rate
    FROM Global_Education
    ORDER BY Unemployment_Rate DESC
    LIMIT 10;
     ```
    ![er10](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/9.png?raw=true)

 
4. **Countries with Low Primary Enrollment**
     ```sql
     SELECT
    Country_Area
    FROM Global_Education
    WHERE Gross_Primary_Education_Enrollment < (SELECT AVG(Gross_Primary_Education_Enrollment) FROM Global_Education)
    ORDER BY Gross_Primary_Education_Enrollment limit 10;
     ```
    ![lowpe](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/10.png?raw=true)

5. **Bottom 5 Pizzas by Total Quantity**
     ```sql
     SELECT
      Country_Area,
      NULLIF(Gross_Tertiary_Education_Enrollment,0) / NULLIF(Gross_Primary_Education_Enrollment, 0) AS tertiary_to_primary_ratio
      FROM Global_Education
      ORDER BY tertiary_to_primary_ratio DESC;
     ```
    ![ratio](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/13.png?raw=true)
    ![ratio](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/12.png?raw=true)

7. **Total Enrollment**
     ```sql
     SELECT
    Country_Area,
    (Gross_Primary_Education_Enrollment + Gross_Tertiary_Education_Enrollment) AS overall_enrollment
    FROM Global_Education
    ORDER BY overall_enrollment DESC;
     ```
    ![total enrollment](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/14.png?raw=true)

## Power BI Dashboard
![Report](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/15.png?raw=true)

## Insights
- 
 
Feel free to navigate through the dashboard and uncover more valuable insights. Should you have any questions or suggestions, please don't hesitate to reach out. 
Happy exploring!
