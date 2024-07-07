# sql-practice

**Hello everyone.This is my sql practice.
WE got here 3 type of difficulties:**
- [Easy](#easy-difficulty)
- [medium](#medium-difficulty)
- [hard](#hard-difficulty)
**Source:**https://www.sql-practice.com/

**Table Schemas:**
![image](https://github.com/Hordiychuk-Radion/sql-practice/assets/139583782/e4559030-9fc6-45d4-bdcd-2d5ccf507520)
![image](https://github.com/Hordiychuk-Radion/sql-practice/assets/139583782/1f0f5f7b-4b8d-4dc2-90d8-fca17650a9ec)
![image](https://github.com/Hordiychuk-Radion/sql-practice/assets/139583782/627db8fc-e7ed-49fc-8ff5-d98188df9813)
![image](https://github.com/Hordiychuk-Radion/sql-practice/assets/139583782/9b75e544-ecf9-4da0-a695-210c9d09d1c9)


## *EASY Difficulty:*


**Show first name, last name, and gender of patients whose gender is 'M'**

```
SELECT first_name, last_name, gender
FROM patients
Where gender = 'M'
```

**Show first name and last name of patients who does not have allergies. (null)**

```
SELECT first_name, last_name
from patients
where allergies is null
```

**Show first name of patients that start with the letter 'C'**

```
SELECT first_name
from patients
where first_name LIKE 'C%'
```

**Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)**

```
SELECT first_name, last_name
from patients
where weight between 100 AND 120
```

**Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'**

```
update patients
SET allergies = 'NKA'
WHERE allergies is null
```

**Show first name and last name concatinated into one column to show their full name.**

```
select concat(first_name,' ',last_name) AS FULL_NAME
from patients
```


**Show first name, last name, and the full province name of each patient.
Example: 'Ontario' instead of 'ON'**

```
select first_name,last_name, province_name
from patients as p
INNER join province_names as pn ON p.province_id = pn.province_id
```

**Show how many patients have a birth_date with 2010 as the birth year.**

```
select COUNT(*) as total
FROM patients
where YEAR(birth_date) = 2010
```

**Show the first_name, last_name, and height of the patient with the greatest height.**

```
select first_name,last_name, MAX(height) as height
FROM patients
```

**Show all columns for patients who have one of the following patient_ids:
1,45,534,879,1000**

```
select * 
FROM patients
WHERe patient_id IN (1, 45, 534, 879, 1000)
```




## *MEDIUM Difficulty*




**Show unique birth years from patients and order them by ascending.**

```
SELECT distinct YEAR(birth_date) as birth_year 
FROM patients
order by birth_year
```

**Show unique first names from the patients table which only occurs once in the list.
For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.**

```
SELECT first_name
FROM patients
group by 1
having count(*) = 1
```

**Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.**

```
SELECT patient_id, first_name
FROM patients
where first_name LIKE 's%s' AND LEN(first_name) >= 6
```

**Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.
Primary diagnosis is stored in the admissions table.**

```
select p.patient_id, first_name, last_name
FROM patients as p
INNER join admissions as a ON p.patient_id = a.patient_id
WHERE a.diagnosis = 'Dementia'
```


**Display every patient's first_name.
Order the list by the length of each name and then by alphabetically.**

```
select first_name
FROM patients 
order by LEN(first_name), first_name ASC
```


**Show the total amount of male patients and the total amount of female patients in the patients table.
Display the two results in the same row.**

```
SELECT 
    SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS male_count,
    SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS female_count
FROM patients;
```


**Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.**

```
SELECT first_name,last_name,allergies
from patients
where allergies = 'Penicillin' or allergies = 'Morphine'
order by allergies ASC, 1,2
```

**Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.**

```
SELECT  patient_id, diagnosis
from admissions
group by 1,2
having count(*) > 1
```


**Show the city and the total number of patients in the city.
Order from most to least patients and then by city name ascending.**

```
SELECT city, COUNT(*) AS count
FROM patients
GROUP BY city
ORDER BY count DESC, city ASC
```


**Show first name, last name and role of every person that is either patient or doctor.
The roles are either "Patient" or "Doctor"**

```
SELECT first_name, last_name, 'Patient' as role FROM patients
    union all
select first_name, last_name, 'Doctor' from doctors
```


**Show all allergies ordered by popularity. Remove NULL values from query.**

```
SELECT allergies, COUNT(*) AS total
FROM patients
WHERE allergies IS NOT NULL
GROUP BY allergies
ORDER BY total DESC;
```

**Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.**

```
SELECT
  first_name,
  last_name,
  birth_date
FROM patients
WHERE year(birth_date) LIKE '197%'
ORDER BY birth_date ASC
```


**We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order
EX: SMITH,jane**

```
SELECT
  CONCAT(UPPER(last_name), ',', LOWER(first_name)) AS new_name_format
FROM patients
ORDER BY first_name DESC
```


**Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.**

```
SELECT province_id, SUM(height) as sum
 FROM patients
 group by 1
 having SUM(height) > 7000
```

**Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'**

```
SELECT (MAX(weight) - MIN(weight)) as weight_difference
FROM patients
where last_name = 'Maroni'
```

**Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.**


```
select day(admission_date) as days, count(*) as count_admissions
FROM admissions
group by 1
order by count_admissions desc
```

**Show all columns for patient_id 542's most recent admission_date.**

```
SELECT *
FROM admissions
WHERE patient_id = 542
ORDER BY admission_date DESC
LIMIT 1
```


**Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:**
**1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.**
**2. attending_doctor_id contains a 2 and the length of patient_id is 3 characters.**

```
SELECT
  patient_id,
  attending_doctor_id,
  diagnosis
FROM admissions
WHERE
  (
    attending_doctor_id IN (1, 5, 19)
    AND patient_id % 2 != 0
  )
  OR 
  (
    attending_doctor_id LIKE '%2%'
    AND len(patient_id) = 3
  )
```

**Show first_name, last_name, and the total number of admissions attended for each doctor.
Every admission has been attended by a doctor.**


```
SELECT first_name, last_name, count(*) as admission_total
FROM doctors as d 
INNER JOIN admissions as a ON d.doctor_id = a.attending_doctor_id
group by 1,2
```

**For each doctor, display their id, full name, and the first and last admission date they attended.**
```
SELECT 
    d.doctor_id, 
    CONCAT(d.first_name, ' ', d.last_name) AS full_name, 
    MAX(a.admission_date) AS max_admission_date, 
    MIN(a.admission_date) AS min_admission_date
FROM 
    doctors AS d
INNER JOIN 
    admissions AS a 
ON 
    d.doctor_id = a.attending_doctor_id
GROUP BY 
    d.doctor_id, 
    full_name
```


**Display patient's full name,
height in the units feet rounded to 1 decimal,
weight in the unit pounds rounded to 0 decimals,
birth_date, gender non abbreviated.**

**Convert CM to feet by dividing by 30.48.
Convert KG to pounds by multiplying by 2.205.**

```select concat(first_name, ' ', last_name) as full_name, 
round(height /30.48, 1) as height, round(weight * 2.205, 0) as weight,
birth_date, CASE WhEN gender = 'M' THEN 'MALE' Else 'FEMALE' END as gender
FROM patients
```

**Show patient_id, first_name, last_name from patients whose does not have any records in the admissions table. (Their patient_id does not exist in any admissions.patient_id rows.)**

```
select p.patient_id, first_name,last_name
FROM patients as p
lEFT JOIN admissions as a ON p.patient_id =a.patient_id
where a.patient_id is NULL
```




## *HARD Difficulty*




**Show all of the patients grouped into weight groups.
Show the total amount of patients in each weight group.
Order the list by the weight group decending.
For example, if they weight 100 to 109 they are placed in the 100 weight group, 110-119 = 110 weight group, etc.**

```
SELECT (weight / 10) * 10 as correct_weight, COUNT(*) as total_amount
FROM patients
group by (weight / 10) * 10
order by correct_weight DESC
```


**Show patient_id, first_name, last_name, and attending doctor's specialty.
Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'
Check patients, admissions, and doctors tables for required information.**

```
select p.patient_id, p.first_name,p.last_name, d.specialty
FROM patients as p 
INNER JOIN admissions as a ON p.patient_id = a.patient_id
INNER JOIN doctors as d ON a.attending_doctor_id = d.doctor_id
where diagnosis = 'Epilepsy' AND d.first_name = 'Lisa'
```

**Show patient_id, weight, height, isObese from the patients table.
Display isObese as a boolean 0 or 1.
Obese is defined as weight(kg)/(height(m)2) >= 30.
weight is in units kg.
height is in units cm.**

```
SELECT patient_id, weight, height, 
    CASE WHEN (weight / (POWER(height / 100.0, 2))) >= 30 THEN 1 ELSE 0 END AS isObese
FROM  patients
```

**All patients who have gone through admissions, can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.**

**The password must be the following, in order:**
**1. patient_id
2. the numerical length of patient's last_name
3. year of patient's birth_date**

```
select DISTINCT p.patient_id, concat(p.patient_id ,LEN(last_name),YEAR(birth_date)) as tem_password
FROM patients as p
INNER JOIN admissions as a ON p.patient_id = a.patient_id
```

**Each admission costs $50 for patients without insurance, and $10 for patients with insurance. All patients with an even patient_id have insurance.
Give each patient a 'Yes' if they have insurance, and a 'No' if they don't have insurance. Add up the admission_total cost for each has_insurance group.**

```
SELECT CASE WHEN patient_id % 2 = 0 THEN 'Yes' ELSE 'No' END AS has_insurance,
       SUM(CASE WHEN patient_id % 2 = 0 THEN 10 ELSE 50 END) AS admission_total
FROM admissions
GROUP BY CASE WHEN patient_id % 2 = 0 THEN 'Yes' ELSE 'No' END
```

**We are looking for a specific patient. Pull all columns for the patient who matches the following criteria:*
**- First_name contains an 'r' after the first two letters.**
**- Identifies their gender as 'F'**
**- Born in February, May, or December**
**- Their weight would be between 60kg and 80kg**
**- Their patient_id is an odd number**
**- They are from the city 'Kingston'**



```
select  * from patients
WHERE first_name LIKE '__%r%' AND gender = 'F'AND city = 'Kingston' AND weight between 60 and 80
AND month(birth_date) IN (02,05,12)
```


**Show the percent of patients that have 'M' as their gender. Round the answer to the nearest hundreth number and in percent form.**

```
SELECT ROUND(COUNT(CASE WHEN gender = 'M' THEN 1 END) * 100.0 / COUNT(*), 2) AS percent_of_male
FROM patients
```

**Show the provinces that has more patients identified as 'M' than 'F'. Must only show full province_name**

```
WITH ctw AS (
    SELECT 
        pn.province_name, 
        COUNT(CASE WHEN p.gender = 'M' THEN 1 END) AS count_M,
        COUNT(CASE WHEN p.gender = 'F' THEN 1 END) AS count_F
    FROM 
        patients AS p
    INNER JOIN 
        province_names AS pn ON p.province_id = pn.province_id
    GROUP BY 
        pn.province_name
    HAVING 
        COUNT(CASE WHEN p.gender = 'M' THEN 1 END) > COUNT(CASE WHEN p.gender = 'F' THEN 1 END)
)

SELECT province_name 
FROM ctw
```


**Sort the province names in ascending order in such a way that the province 'Ontario' is always on top.**


```
SELECT province_name 
FROM province_names
ORDER BY CASE WHEN province_name = 'Ontario' THEN 0 ELSE 1 END,
province_name ASC;
```

**We need a breakdown for the total amount of admissions each doctor has started each year. Show the doctor_id, doctor_full_name, specialty, year, total_admissions for that year.**

```
select doctor_id, concat(first_name,' ', last_name) as doctor_full_name, specialty,
YEAR(admission_date) as year, COUNT(*) as total
FROM doctors as d 
LEFT JOIN admissions as a ON a.attending_doctor_id = d.doctor_id
GROUP BY 1,2,3,4
```

**For each day display the total amount of admissions on that day. Display the amount changed from the previous date.**


```
SELECT
 admission_date,
 count(admission_date) as admission_day,
 count(admission_date) - LAG(count(admission_date)) OVER(ORDER BY admission_date) AS admission_count_change 
FROM admissions
 group by admission_date
```
