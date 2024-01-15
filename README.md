![world](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/krzysztof-hepner-TH7TW20de9s-unsplash%20cropped.jpg?raw=true)
# World Wellbeing Report
The World Wellbeing Report is an in-depth analysis of global educational data, employing PostgreSQL for data management, SQL queries for insightful metrics, and a Power BI dashboard for visually presenting key findings on out-of-school rates, literacy, unemployment, and educational enrollment ratios across countries.

**Data Source**: (https://www.kaggle.com/datasets/nelgiriyewithana/world-educational-data?resource=download)

**Software Tools used**: Microsoft Excel, Microsoft Power BI, PostgreSQL-pgAdmin4

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
      
3. **Import Global_Education csv file in the table**:
Initially I ran into some problem regarding encoding of the file which I set as UTF8 and to find the correct encoding, I used a simple python code.
      ```python
      with open('Global_Education.csv') as f:
                       print(f)
      ```
      ![encoding](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/16.png?raw=true)

Here in the output we found the encoding of the file and now we have to set the encoding as WIN1252 and import the file.

4. **Top 10 Countries with High Birth Rate**
      ```sql
      SELECT COUNTRY_AREA, BIRTH_RATE
      FROM GLOBAL_EDUCATION
      ORDER BY BIRTH_RATE DESC LIMIT 10;
      ```
      ![br](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/17.png?raw=true)

5.  **Countries with 100% Female Literacy Rate in Youth**
      ```sql
      SELECT COUNTRY_AREA, Youth_15_24_Literacy_Rate_Female
      FROM GLOBAL_EDUCATION
      WHERE Youth_15_24_Literacy_Rate_Female=100
      ORDER BY Youth_15_24_Literacy_Rate_Female;
      ```
      ![](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/19.png?raw=true)
      ![](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/20.png?raw=true)
      
6. **Average Out of School Rate (OOSR) for primary age by Gender**
      ```sql
      SELECT
      AVG(OOSR_Primary_Age_Male) AS avg_oosr_male,
      AVG(OOSR_Primary_Age_Female) AS avg_oosr_female
      FROM Global_Education;
      ```
      ![Avg OOSR Primary](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/1.png?raw=true)


7. **OOSR by Education Level**
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

8. **Gender Disparities in Literacy Rate in Youth**
     ```sql
     SELECT
    AVG(Youth_15_24_Literacy_Rate_Male) AS avg_literacy_rate_male,
    AVG(Youth_15_24_Literacy_Rate_Female) AS avg_literacy_rate_female
    FROM Global_Education;
     ```
    ![lr](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/7.png?raw=true)

9. **Top 10 Countries with Highest Unemployment Rate**
     ```sql
     SELECT
    Country_Area,
    Unemployment_Rate
    FROM Global_Education
    ORDER BY Unemployment_Rate DESC
    LIMIT 10;
     ```
     ![ur10](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/8.png?raw=true)

10. **Countries with best Employment Rate**
     ```sql
     SELECT
    Country_Area,
    Unemployment_Rate
    FROM Global_Education
    ORDER BY Unemployment_Rate DESC
    LIMIT 10;
     ```
    ![er10](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/9.png?raw=true)

 11. **Countries with Low Primary Enrollment**
     ```sql
     SELECT
     Country_Area
     FROM Global_Education
     WHERE Gross_Primary_Education_Enrollment < (SELECT AVG(Gross_Primary_Education_Enrollment) FROM Global_Education)
     ORDER BY Gross_Primary_Education_Enrollment limit 10;
     ```  
![lowpe](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/10.png?raw=true)

13. **Ratio of Tertiary Enrollment to Primary Enrollment**
     ```sql
     SELECT
      Country_Area,
      NULLIF(Gross_Tertiary_Education_Enrollment,0) / NULLIF(Gross_Primary_Education_Enrollment, 0) AS tertiary_to_primary_ratio
      FROM Global_Education
      ORDER BY tertiary_to_primary_ratio DESC;
     ```
    ![ratio](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/13.png?raw=true)
    ![ratio](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/12.png?raw=true)

14. **Total Enrollment**
     ```sql
     SELECT
    Country_Area,
    (Gross_Primary_Education_Enrollment + Gross_Tertiary_Education_Enrollment) AS overall_enrollment
    FROM Global_Education
    ORDER BY overall_enrollment DESC;
     ```
    ![total enrollment](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/14.png?raw=true)

## Power BI Dashboard
![Report](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/18.png?raw=true)

### Elements of the Dashboard

**Heatmap depicting Birth Rate across countries**

![](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/21.png?raw=true)

**Line Chart showcasing Total Enrollment and Total Completion Rate across countries**
![](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/22.png?raw=true)

**Top 5 Countries with Highest Unemployment Rate**

![](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/24.png?raw=true)

**Clustered Bar Chart representing OOSR of Female and Male**
![](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/23.png?raw=true)

**Youth Literacy Rate across countries**

![](https://github.com/Shaishta-Anjum/World-Wellbeing-Report/blob/main/images/25.png?raw=true)

## Insights
- **Nigeria** has **Highest Birth Rate** across the world.
- **South Sudan** has **Highest OOSR** among both male and female.
- There are **22** Countries with **100% Female Literacy Rate in Youth**.
- **South Africa, Lesotho** & **Saint Lucia** are the Top 3 Countries with **Highest Unemployment Rate**.
 
Feel free to navigate through the dashboard and uncover more valuable insights. Should you have any questions or suggestions, please don't hesitate to reach out. 
Happy exploring!
