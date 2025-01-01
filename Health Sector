# Hospital Management SQL Queries

This document contains advanced SQL queries designed for hospital management systems, addressing patient records, appointments, diagnoses, and operational analysis.

---

## 1. Retrieve the Count of Patients Who Visited a Specific Hospital in the Last Month

**Description**: Count the number of patients who visited hospital ID `1` in the last month.

```sql
SELECT COUNT(*) AS patient_count
FROM visits
WHERE hospital_id = 1
  AND visit_date >= DATEADD(MONTH, -1, GETDATE());
```

---

## 2. List All Patients Who Have Not Booked an Appointment in the Past Year

**Description**: Identify patients who have not booked an appointment in the past year.

```sql
SELECT patient_id, name
FROM patients
WHERE patient_id NOT IN (
  SELECT DISTINCT patient_id
  FROM appointments
  WHERE appointment_date >= DATEADD(YEAR, -1, GETDATE())
);
```

---

## 3. Find the Average Length of Stay for Patients Admitted to a Hospital

**Description**: Calculate the average length of stay for patients in each hospital.

```sql
SELECT hospital_id, AVG(DATEDIFF(day, admission_date, discharge_date)) AS avg_length_of_stay
FROM admissions
GROUP BY hospital_id;
```

---

## 4. Identify Duplicate Patient Records in the Database

**Description**: Detect duplicate patient records based on their name and date of birth.

```sql
SELECT first_name, last_name, date_of_birth, COUNT(*)
FROM patients
GROUP BY first_name, last_name, date_of_birth
HAVING COUNT(*) > 1;
```

---

## 5. Retrieve the Total Number of Appointments by Department

**Description**: Count the number of appointments scheduled for each department.

```sql
SELECT department_id, COUNT(*) AS total_appointments
FROM appointments
GROUP BY department_id;
```

---

## 6. Find Doctors Who Have Treated More Than 100 Unique Patients

**Description**: Identify doctors with a patient count exceeding 100.

```sql
SELECT doctor_id, COUNT(DISTINCT patient_id) AS unique_patients
FROM treatments
GROUP BY doctor_id
HAVING COUNT(DISTINCT patient_id) > 100;
```

---

## 7. Detect Gaps in Bed Occupancy for a Specific Hospital

**Description**: Find gaps in bed occupancy by comparing discharge and admission dates.

```sql
WITH cte AS (
  SELECT bed_id, discharge_date, 
         LEAD(admission_date) OVER (PARTITION BY bed_id ORDER BY discharge_date) AS next_admission_date
  FROM admissions
  WHERE hospital_id = 'hospital_id_here'
)
SELECT bed_id, discharge_date, next_admission_date
FROM cte
WHERE next_admission_date > discharge_date;
```

---

## 8. Retrieve the Top 3 Most Prescribed Medications in the Last Year

**Description**: Find the most frequently prescribed medications in the past year.

```sql
SELECT medication_id, COUNT(*) AS prescriptions_count
FROM prescriptions
WHERE prescription_date >= DATEADD(YEAR, -1, GETDATE())
GROUP BY medication_id
ORDER BY prescriptions_count DESC
LIMIT 3;
```

---

## 9. Find Patients with Multiple Diagnoses for the Same Condition

**Description**: Identify patients diagnosed multiple times for the same condition.

```sql
SELECT patient_id, condition_id, COUNT(*) AS diagnosis_count
FROM diagnoses
GROUP BY patient_id, condition_id
HAVING COUNT(*) > 1;
```

---

## 10. Calculate the Readmission Rate of Patients Within 30 Days of Discharge

**Description**: Determine the percentage of patients readmitted within 30 days of discharge.

```sql
WITH readmissions AS (
  SELECT a.patient_id, a.discharge_date, b.admission_date
  FROM admissions a
  JOIN admissions b ON a.patient_id = b.patient_id
  WHERE DATEDIFF(day, a.discharge_date, b.admission_date) <= 30
    AND a.discharge_date < b.admission_date
)
SELECT COUNT(DISTINCT patient_id) * 100.0 / (SELECT COUNT(*) FROM admissions) AS readmission_rate
FROM readmissions;
```

---

## 11. Find the Percentage of Appointments That Were Canceled

**Description**: Calculate the cancellation rate for all appointments.

```sql
SELECT ROUND(
  (SELECT COUNT(*) FROM appointments WHERE status = 'Canceled') * 100.0 / COUNT(*), 2
) AS cancellation_rate
FROM appointments;
```

---

## 12. Identify High-Risk Patients Based on Their Medical History

**Description**: Find patients with more than 3 hospital admissions in the past year.

```sql
SELECT patient_id, COUNT(*) AS admission_count
FROM admissions
WHERE admission_date >= DATEADD(YEAR, -1, GETDATE())
GROUP BY patient_id
HAVING COUNT(*) > 3;
```

---

## 13. Retrieve the Hospital with the Highest Patient Satisfaction Score

**Description**: Identify the hospital with the best average satisfaction score.

```sql
SELECT hospital_id, AVG(satisfaction_score) AS avg_score
FROM feedback
GROUP BY hospital_id
ORDER BY avg_score DESC
LIMIT 1;
```

---

## 14. List Patients Who Have Not Received Any Medication After Diagnosis

**Description**: Find patients who were diagnosed but not prescribed any medication.

```sql
SELECT d.patient_id, d.condition_id
FROM diagnoses d
LEFT JOIN prescriptions p ON d.patient_id = p.patient_id AND d.condition_id = p.condition_id
WHERE p.prescription_id IS NULL;
```

---

## 15. Calculate the Average Cost Per Treatment Type

**Description**: Determine the average cost for each type of treatment.

```sql
SELECT treatment_type, AVG(cost) AS avg_cost
FROM treatments
GROUP BY treatment_type;
```

---

## 16. Detect Overlapping Shifts for Doctors in a Hospital

**Description**: Identify overlapping shifts for the same doctor.

```sql
SELECT d1.doctor_id, d1.shift_start, d1.shift_end, d2.shift_start AS overlap_start, d2.shift_end AS overlap_end
FROM doctor_shifts d1
JOIN doctor_shifts d2 ON d1.doctor_id = d2.doctor_id
  AND d1.shift_start < d2.shift_end
  AND d1.shift_end > d2.shift_start
  AND d1.shift_id <> d2.shift_id;
```

---

## 17. Find the Total Cost of Treatments Per Patient, Excluding Unpaid Bills

**Description**: Calculate the total paid cost of treatments for each patient.

```sql
SELECT patient_id, SUM(cost) AS total_paid_treatment_cost
FROM treatments
WHERE payment_status = 'Paid'
GROUP BY patient_id;
```

---

## 18. Identify Trends in Patient Visits by Month Over the Past Year

**Description**: Analyze patient visit trends by month for the last year.

```sql
SELECT MONTH(visit_date) AS visit_month, COUNT(*) AS visit_count
FROM visits
WHERE visit_date >= DATEADD(YEAR, -1, GETDATE())
GROUP BY MONTH(visit_date)
ORDER BY visit_month;
```

---

## 19. Calculate the Average Waiting Time for Appointments Per Department

**Description**: Determine the average waiting time for appointments by department.

```sql
SELECT department_id, AVG(DATEDIFF(MINUTE, appointment_scheduled_time, appointment_start_time)) AS avg_waiting_time
FROM appointments
WHERE appointment_start_time IS NOT NULL
GROUP BY department_id;
```

---

## 20. Find the Ratio of Inpatient to Outpatient Visits for Each Hospital

**Description**: Calculate the ratio of inpatient to outpatient visits for each hospital.

```sql
SELECT hospital_id,
       SUM(CASE WHEN visit_type = 'Inpatient' THEN 1 ELSE 0 END) AS inpatient_visits,
       SUM(CASE WHEN visit_type = 'Outpatient' THEN 1 ELSE 0 END) AS outpatient_visits,
       ROUND(SUM(CASE WHEN visit_type = 'Inpatient' THEN 1 ELSE 0 END) * 1.0 /
             SUM(CASE WHEN visit_type = 'Outpatient' THEN 1 ELSE 0 END), 2) AS inpatient_outpatient_ratio
FROM visits
GROUP BY hospital_id;
```

---


