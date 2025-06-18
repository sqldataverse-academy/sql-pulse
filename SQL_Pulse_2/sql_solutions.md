# SQL Server Solutions

## **Primary Problem Solution: Mentorship Program Effectiveness Analysis**

```sql
WITH first_day_analysis AS (
    -- Find each employee's first completion date and the certifications they completed on that day
    SELECT 
        employee_id,
        MIN(completion_date) as first_completion_date
    FROM employee_certifications
    GROUP BY employee_id
),
first_day_certifications AS (
    -- Get all certifications completed on the first day for each employee
    SELECT 
        fda.employee_id,
        fda.first_completion_date,
        ec.category,
        ec.certification_id
    FROM first_day_analysis fda
    INNER JOIN employee_certifications ec ON fda.employee_id = ec.employee_id 
        AND fda.first_completion_date = ec.completion_date
),
post_first_day_activity AS (
    -- Find certifications completed after the first day
    SELECT DISTINCT
        ec.employee_id,
        ec.category,
        ec.certification_id
    FROM employee_certifications ec
    INNER JOIN first_day_analysis fda ON ec.employee_id = fda.employee_id
    WHERE ec.completion_date > fda.first_completion_date
),
eligible_employees AS (
    -- Identify employees who completed different certifications after their first day
    SELECT DISTINCT
        pfda.employee_id
    FROM post_first_day_activity pfda
    WHERE NOT EXISTS (
        SELECT 1 
        FROM first_day_certifications fdc 
        WHERE fdc.employee_id = pfda.employee_id 
        AND fdc.category = pfda.category 
        AND fdc.certification_id = pfda.certification_id
    )
)
-- Count successful mentees
SELECT COUNT(*) as successful_mentees
FROM eligible_employees;
```

## **Bonus Problem Solution: Category-wise Certification Performance Analysis**

```sql
WITH category_metrics AS (
    -- Calculate metrics per category
    SELECT 
        category,
        COUNT(DISTINCT employee_id) as employee_count,
        COUNT(*) as total_certifications,
        AVG(CAST(course_fee AS FLOAT)) as avg_course_investment,
        COUNT(DISTINCT certification_id) as certification_diversity,
        CAST(COUNT(*) AS FLOAT) / COUNT(DISTINCT employee_id) as certs_per_employee
    FROM employee_certifications
    GROUP BY category
),
ranked_categories AS (
    -- Rank categories by engagement (certifications per employee)
    SELECT 
        category,
        employee_count,
        total_certifications,
        ROUND(avg_course_investment, 2) as avg_course_investment,
        certification_diversity,
        ROW_NUMBER() OVER (ORDER BY certs_per_employee DESC) as engagement_rank
    FROM category_metrics
)
-- Final output
SELECT 
    category,
    employee_count,
    total_certifications,
    avg_course_investment,
    certification_diversity,
    engagement_rank
FROM ranked_categories
ORDER BY engagement_rank;
```