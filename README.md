# INTERACTIVE DATA JOBS DASHBOARD (MS EXCEL PROJECT) 
<img width="1348" height="532" alt="Data Jobs Dashboard" src="https://github.com/user-attachments/assets/54343542-75b3-4e62-990f-c583f2251804" />  
This is an interactive Dashboard I made while learning data analysis. The data used is for the year 2023, taken from various job recruitment websites. The analysis/dashboard is supposed to help a hypothetical job seeker. The dashboard shows the Median salary, Job count and Top job platform based on the Job Title, Country and Schedule type of their choice. 

### Skills used in this project include ###
1. Data Validation(For the drop down lists the job seeker selects from in the dasbhboard)
2. Charts, Chart design/formatting
3. Inserted and formatted shapes for the "Median Salary", "Job Count", and "Top Job Platform" Cards.
4. Functions: =SUBSTITUTE, =SORT, =FILTER, =UNIQUE, =MEDIAN, =ISNUMBER, =IF, =XLOOKUP =SEARCH, =COUNT, =NOT =COUNTIFS
5. Cell naming for easy referencing in the "Median Salary", "Job Count", and "Job Platform" cards.

### Step By step process of making the dashboard ###  
1. First thing I did was write the title of the dashboard ***"Data Jobs Dashboard"***, go to the Home tab, select "Cell Styles" in the formatting group and chose "Heading 1"
2. I created the 'Job Medians' sheet in the workbook, used *=UNIQUE* function to get unique job titles from the job_title_short column in the data table.`=UNIQUE(jobs[job_title_short])`
The list would be used for data validation; making a drop down list for the **Job Title** part of the dashboard. For data validation, you select the cell you want the drop down list to appear, then go to the Data tab, click Data tools, then Data validation. Under settings, I selected ***'List'*** as the validation criteria, selected the cell range that contained the list of unique job titles in the 'Job Medians' sheet and then clicked okay. Bam!, the drop down list appears.
<img width="469" height="383" alt="Data Validation" src="https://github.com/user-attachments/assets/43eba7d5-5f33-4f8c-9884-d34a58f2ef7f" />


I then clicked on the cell containing the drop down list, renamed it as ***'Title'*** in the formular bar for easy referencing in my formulas.
<img width="410" height="197" alt="Title" src="https://github.com/user-attachments/assets/f792f0c1-b263-4b7f-b3be-96275c105f74" />  

I repeated the same process for the **Country** part of the dashboard: I Created 'Country Medians' sheet, Used *=UNIQUE* function to get unique countries from the job_country column. I then enclosed the *=UNIQUE* function with the *=SORT* function to arrange the countries in alphabetical order.  
`=SORT(UNIQUE(jobs[job_country]))`  
In the dashboard sheet, I clicked the cell where I wanted the drop down list for countries to appear, did the data validation process and the list appeared. I renamed the cell as ***'Country'*** for easy referencing in my formulas.  

For the **Schedule Type** part of the dashboard, I created 'Schedule Type Medians' sheet and got unique values from the job_schedule_type column. A number of the values had combined schedule types like; 'Full-time and Part-time', 'Full-time and tempwork'. I needed stand-alone schedule type values(not combined) which were luckily available. They were: Full-time, Contractor, Part-time, Internship, and Tempwork. There were other schedule types like 'volunteer' contained in the combined values but the aforementioned 5 unique schedule type values would suffice(They were the major schedule types in all values anyway).  
<img width="268" height="433" alt="Schedule" src="https://github.com/user-attachments/assets/3ca7c8f1-aa7a-4e54-b5b6-df7c710ecd14" />  

All I had to do next was filter out the combined shcedule type values and also the '0' value appearing in the middle of the list so I could remain with the 5 main schedule type values I needed.  
`=FILTER(A3#,NOT(ISNUMBER(SEARCH("and",A3#)))*(A3#<>0))`  
I began by searching the values had "and" in them(the combined schedule types). I used this formula: *=SEARCH("and",A3#)* The formula returns numbers. I enclosed the formula with *=ISNUMBER(SEARCH("and",A3#))* and it returned 'TRUE' or 'FALSE' all down the list. 'TRUE' for the values that contained 'and', 'FALSE' for those that didn't. In order to return the values that did not have 'and' in them using the *=FILTER* function, I had to turn the FALSEs into TRUEs. *FILTER* function only returns values for which the condition required evaluated to 'TRUE'. SO I enclosed the formula further with: *=NOT(ISNUMBER(SEARCH("and",A3#)))*. The FALSEs changed to TRUEs and vice versa. I added one more condition for the *=FILTER* function: `NOT(ISNUMBER(SEARCH("and",A3#)))*(A3#<>0)`. A3#<>0 means which of the values in the list does not equal to '0'. The aformentioned conditioned would be included in our formula to return all values that don't have 'and' & '0' in them. Final formula was: `=FILTER(A3#,NOT(ISNUMBER(SEARCH("and",A3#)))*(A3#<>0))`. The result was the list below:  

<img width="196" height="215" alt="Capture" src="https://github.com/user-attachments/assets/04fae735-b249-4ce9-8e46-73619cacd484" />  

I then used data validation to make the drop down list for the **Schedule Type** part of the Dashboard. I renamed the cell ***'schedule'*** for easy reference in my formulas.  

3. Next step was to calculate the Median salaries.
   
   What is the median salary for this job title based on the **Country** and **Schedule type** selected? (Job Medians Sheet).
   What is the median salary for this country based on **Job Title** and **Schedule type** selected? (Country Medians Sheet).
   What is the median salary for this schedule type based on **Job Title** and **Country** selected? (Schedule type Medians sheet).

Formula used in the **'Job Medians'** sheet: `=MEDIAN(IF((jobs[job_title_short]=B2)*(jobs[salary_year_avg]<>"")*(jobs[job_country]=Country)*(ISNUMBER(SEARCH(schedule,jobs[job_schedule_type]))),jobs[salary_year_avg]))`

The *=IF* function was used to return the salaries for which 4 conditions were met: Job Title is the one selected, the salary is not empty, job country is the one selected in the dashboard, values in the schedule_type column contain whatever is selected in the schedule type column. The function was then enclosed with the *=MEDIAN* function. I autofilled down for each job title to return all the medians.

Based on the country or schedule type selected, sometimes the formulas would return errors next to the Job titles because maybe some countries didn't have certain job titles or schedule types, so I had to use the filter function to display only the jobs that met all the selected conditions. I then sorted the jobs in ascending order based on the median salaries. `=SORT(FILTER(B2:C11,ISNUMBER(C2:C11)),2,1)`
<img width="693" height="298" alt="sort" src="https://github.com/user-attachments/assets/e950a793-8c81-420a-a51f-11b119f40542" />  

Next was inserting a chart to visualize the Job title medians.
Before inserting the chart, I used the following formulas to return values for a 2 in 1 bar chart:  
`=IF(D2=Title,E2,NA()) and =IF(D2<>Title,E2,NA())`  
<img width="407" height="222" alt="chart" src="https://github.com/user-attachments/assets/30750867-9169-46db-8a0c-909366716527" />  
IF the job title in this list is the one selected in the Job Title drop down list in the dashboard sheet, return the median salary, otherwise return NA()/#NA. If the job title in this list is not the one selected in the drop down list, return the median salary, otherwise return NA()/#NA. I then selected all the job titles, and both median salary columns evaluated from the the *=IF* functions, went to the Insert tab, recommended charts, then selected a bar chart. Everytime I selected a job title, it would have its own unique color bar in the chart.  
<img width="460" height="398" alt="2 IN 1" src="https://github.com/user-attachments/assets/b8f9e730-1a71-4f03-bf86-36ab45ad5fb6" />  

For the **Country Medians** sheet, these were the formulas used:  
`=MEDIAN(IF((jobs[job_title_short]=Title)*(jobs[salary_year_avg]<>"")*(jobs[job_country]=A2)*(ISNUMBER(SEARCH(schedule,jobs[job_schedule_type]))),jobs[salary_year_avg]))`  
<img width="308" height="270" alt="country" src="https://github.com/user-attachments/assets/02d68286-89c3-47e7-94ca-10d1fe3755e4" />  

`=FILTER(A2:B112,ISNUMBER(B2:B112))` In a new column, I used this formula to filter out the countries with *#NUM!* errors for median salary.  
I then selected the country column and the median column and inserted a map chart. For this chart, I did not do the 2 in 1 chart for unique highlighting everytime I selected a country. Instead I needed the chart to show colors proportional to highness or lowness of the median salaries.
 
<img width="491" height="335" alt="map" src="https://github.com/user-attachments/assets/96100250-899e-4652-a515-4b56323bf666" />  

For the **Schedule type medians** sheet, these were the formulas used:
`=MEDIAN(IF((jobs[job_title_short]=Title)*(jobs[salary_year_avg]<>"")*(jobs[job_country]=Country)*(ISNUMBER(SEARCH(E3,jobs[job_schedule_type]))),jobs[salary_year_avg]))`
I used the formula below to filter out the schedule types with NUM errors for median salary, and then *SORT* the salaries in ascending order.  
`=SORT(FILTER(E3:F7,ISNUMBER(F3:F7)),2,1)`
For a 2 in 1 bar chart, I used the following formulas to return the values:  
`=IF(G3=schedule,H3,NA())` & `=IF(G3<>schedule,H3,NA())`  
I then inserted a bar chart, and a unique color bar would show for each schedule type selected in the dashboard.
<img width="465" height="331" alt="type" src="https://github.com/user-attachments/assets/81aa9a87-9bfc-4603-ae97-624356f275f2" />  

4. For the **Median Salary** card in the dashboard, I inserted a text box first. In the **Job Medians** sheet, in a new cell, I used *=XLOOKUP* to return the median salary based on the job title selected in the drop down list. This is the formula used:
`=XLOOKUP(Title,D2:D11,E2:E11,"No Result",0)`
I then renamed the cell with the value returned by *XLOOKUP* to ***JobMedian***. In the text box already inserted in the dashboard sheet, I referenced ***JobMedian*** `=JobMedian`. I then inserted a rectangular shape from the insert tab, placed it on top of the textbox, sent it behind the textbox, typed the title "Median Salary" in the shape, centered it. I then removed the outline around the textbox to blend in with the shape behind it.

For the **Job Count** card, same process was used. In the **Job Medians** sheet, Formulas used were:  
`=COUNT(IF((jobs[job_title_short]=A13)*(jobs[job_country]=Country)*(ISNUMBER(SEARCH(schedule,jobs[job_schedule_type]))),jobs[salary_year_avg]))`  
`=XLOOKUP(Title,A13:A22,B13:B22,"No Result",0)` The function would return the count based on the job title selected. I renamed that cell with the XLOOKUP result to ***Count***  
<img width="971" height="515" alt="cards" src="https://github.com/user-attachments/assets/8dcb80ff-0a64-4a93-9c18-5b99bf24c03c" />  

For the **Top Job Platform** card, this was the process:
In the **Top Job Platform** sheet I used `=UNIQUE(jobs[job_via])` to extract unique job platforms from the data table.  
I used `=COUNTIFS(jobs[job_via],A2,jobs[job_title_short],Title,jobs[job_country],Country,jobs[job_schedule_type],schedule)` to count the number of jobs posted in each platform.  
I then sorted the platforms in descending order based on the counts: `=SORT(A2:B594,2,-1)`  
In a new cell, I used `=SUBSTITUTE(D2,"via","")` to remove the word 'Via' from the platform names. I renamed the cell to ***Platform***. The cell would be used as reference in the textbox for the **Top job Platform** card in the dashboard.  
<img width="786" height="529" alt="Platform" src="https://github.com/user-attachments/assets/7ddd3c48-cb22-4a03-bab5-91d911c4dbfb" />  

For all the dashboard cards, each value is displayed based on the Job title, Country and Schedule type selected.  
<img width="1339" height="152" alt="Cards2" src="https://github.com/user-attachments/assets/deccdcf7-4ef7-4e92-8731-a60bba4cb992" />  

5. The final step was polishing up the chart/card formatting and protecting the sheet. To protect the sheet, I select the cells that contained the drop down lists, right clicked and clicked on 'format cells', unchecked 'locked' box. Went to the review tab and selected 'Protect sheet', then 'select unlocked cells', input the password then 'okay'

[Median Salary Dashboard](https://github.com/Datageek254/Median-Salary-Dashboard/blob/main/Median%20Salary%20Dashboard.xlsx)











   



