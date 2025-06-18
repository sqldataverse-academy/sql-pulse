# CloudTech Solutions - Employee Certification Analysis

## Enhanced Business Case Scenario: Employee Training Program Effectiveness at CloudTech Solutions

**Business Context:**
CloudTech Solutions (a hypothetical subsidiary of Amazon focused on cloud infrastructure training) runs a comprehensive employee development program called "CloudMastery Academy." When employees complete their first technical certification course, they automatically enter an advanced mentorship program designed to encourage continuous skill development in cloud technologies. The mentorship program begins one day after completing the first course, where senior cloud architects provide personalized learning paths and motivational support to encourage additional certifications.

**The Challenge:**
The Learning & Development team at CloudTech Solutions wants to measure the effectiveness of their mentorship program by identifying how many employees pursued additional certification opportunities as a direct result of the mentorship intervention. This data will help justify the program's ROI and potential expansion across other Amazon subsidiaries.

**Key Business Rules:**
- Employees who complete only one certification or multiple certifications on their first day of training don't qualify for analysis (they haven't experienced mentorship influence yet)
- Employees who later complete only the same certifications they took on their first day also don't count (no skill diversification achieved)
- Only employees who complete different certifications after their first day demonstrate successful mentorship program impact

**Objective:**
Calculate the number of employees who successfully diversified their cloud certification portfolio through additional course completions, indicating the mentorship program's effectiveness in driving continuous professional development within Amazon's cloud ecosystem.

---

<img src="./assets/pulse2_image.png" alt="CloudTech Solutions!" width="600"/>

## Table Structure: employee_certifications

**Table Name:** `employee_certifications`

**Schema:**

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `employee_id` | BIGINT | Unique identifier for each employee |
| `completion_date` | DATE | The date when the employee completed the certification course |
| `category` | VARCHAR(50) | Certification category (AI, Cloud, DevOps, Security, Data) |
| `certification_id` | BIGINT | Unique identifier for each certification within a category |
| `department_name` | VARCHAR(100) | Level/specialization name within the category |
| `credits_earned` | BIGINT | Number of professional development credits earned |
| `course_fee` | BIGINT | The cost/fee associated with the certification course completion |

**Sample Table Structure:**
```sql
CREATE TABLE employee_certifications (
    employee_id BIGINT,
    completion_date DATE,
    category VARCHAR(50),
    certification_id BIGINT,
    department_name VARCHAR(100),
    credits_earned BIGINT,
    course_fee BIGINT
);
```

---

## Detailed Requirements

### **Primary Problem: Mentorship Program Effectiveness Analysis**

**Business Requirement:**
Identify the number of employees who successfully completed additional certifications after entering the mentorship program, demonstrating the program's effectiveness in encouraging continuous learning.

**Expected Output Columns:**
- `successful_mentees`: Total count of employees who completed additional different certifications after their first day

---

### **Bonus Problem: Category-wise Certification Performance Analysis**

**Business Requirement:**
CloudTech Solutions wants to analyze certification performance across different certification categories to identify learning trends, top-performing categories, and skill development patterns for strategic workforce planning.

**Expected Output Columns:**
- `category`: Certification category (AI, Cloud, DevOps, Security, Data)
- `employee_count`: Number of employees in this category who completed certifications
- `total_certifications`: Total number of certification completions in this category
- `avg_course_investment`: Average course fee investment per employee in this category
- `certification_diversity`: Number of unique certification types completed in this category
- `engagement_rank`: Rank of category based on certifications per employee (1 being highest)

**Engagement Rank Explanation with Example:**

The `engagement_rank` measures which category has the highest learning engagement by calculating the average number of certifications completed per employee in each category.

**Formula:** `engagement_rank = RANK based on (total_certifications / employee_count) DESC`

**Example Calculation:**
```
Category: AI
- total_certifications = 20
- employee_count = 8  
- certifications_per_employee = 20/8 = 2.5

Category: Cloud  
- total_certifications = 25
- employee_count = 12
- certifications_per_employee = 25/12 = 2.08

Category: DevOps
- total_certifications = 30
- employee_count = 10
- certifications_per_employee = 30/10 = 3.0

Ranking Result:
1. DevOps (3.0 certs per employee) → engagement_rank = 1
2. AI (2.5 certs per employee) → engagement_rank = 2  
3. Cloud (2.08 certs per employee) → engagement_rank = 3
```

**Business Interpretation:**
- **Rank 1**: Highest engagement - employees in this category complete the most certifications on average
- **Higher ranks**: Lower engagement - employees complete fewer certifications per person
- This metric helps identify which technology areas drive the most continuous learning behavior among employees

---

## Sample Input Data: employee_certifications

```sql
INSERT INTO employee_certifications VALUES
(10, '2019-01-01', 'Cloud', 1, 'Basic AWS', 3, 550),
(10, '2019-01-02', 'AI', 1, 'Basic Machine Learning', 5, 290),
(10, '2019-03-31', 'Data', 2, 'Advanced Analytics', 2, 1490),
(11, '2019-01-02', 'DevOps', 1, 'Basic CI/CD', 3, 2340),
(11, '2019-03-31', 'Security', 1, 'Basic Cybersecurity', 3, 990),
(12, '2019-01-02', 'Data', 1, 'Basic SQL', 2, 2000),
(12, '2019-03-31', 'Cloud', 2, 'Advanced Azure', 2, 2990),
(13, '2019-01-05', 'Security', 2, 'Advanced Penetration Testing', 1, 670),
(13, '2019-03-31', 'AI', 2, 'Advanced Deep Learning', 3, 350),
(14, '2019-01-06', 'DevOps', 2, 'Advanced Kubernetes', 5, 1990),
(14, '2019-01-06', 'Cloud', 3, 'Expert Multi-Cloud', 2, 270),
(14, '2019-03-31', 'Data', 1, 'Basic SQL', 3, 2000),
(15, '2019-01-08', 'DevOps', 1, 'Basic CI/CD', 4, 2340),
(15, '2019-01-09', 'Cloud', 2, 'Advanced Azure', 4, 2990),
(15, '2019-03-31', 'AI', 3, 'Expert Neural Networks', 2, 4990),
(16, '2019-01-10', 'Security', 2, 'Advanced Penetration Testing', 2, 670),
(16, '2019-03-31', 'Cloud', 3, 'Expert Multi-Cloud', 4, 270),
(17, '2019-01-11', 'AI', 3, 'Expert Neural Networks', 2, 4990),
(17, '2019-03-31', 'Cloud', 1, 'Basic AWS', 1, 550),
(18, '2019-01-12', 'Data', 3, 'Expert Big Data', 2, 2480),
(18, '2019-01-12', 'Security', 2, 'Advanced Penetration Testing', 4, 670),
(19, '2019-01-12', 'Data', 3, 'Expert Big Data', 3, 2480),
(20, '2019-01-15', 'AI', 4, 'Expert Computer Vision', 2, 9990),
(21, '2019-01-16', 'DevOps', 1, 'Basic CI/CD', 3, 2340),
(21, '2019-01-17', 'Data', 3, 'Expert Big Data', 4, 2480),
(22, '2019-01-18', 'Security', 2, 'Advanced Penetration Testing', 3, 670),
(22, '2019-01-19', 'AI', 2, 'Advanced Deep Learning', 4, 350),
(23, '2019-01-20', 'AI', 1, 'Basic Machine Learning', 3, 290),
(24, '2019-01-21', 'Data', 3, 'Expert Big Data', 2, 2480),
(25, '2019-01-22', 'Data', 3, 'Expert Big Data', 2, 2480),
(25, '2019-01-22', 'Security', 3, 'Expert Ethical Hacking', 2, 720),
(25, '2019-01-24', 'Data', 3, 'Expert Big Data', 5, 2480),
(25, '2019-01-27', 'Security', 3, 'Expert Ethical Hacking', 1, 720),
(26, '2019-01-25', 'Security', 3, 'Expert Ethical Hacking', 1, 720),
(27, '2019-01-26', 'Cloud', 1, 'Basic AWS', 3, 550),
(28, '2019-01-27', 'Cloud', 1, 'Basic AWS', 4, 550),
(29, '2019-01-27', 'Data', 2, 'Advanced Analytics', 3, 1490),
(30, '2019-01-29', 'Data', 2, 'Advanced Analytics', 1, 1490),
(31, '2019-01-30', 'Cloud', 1, 'Basic AWS', 3, 550),
(32, '2019-01-31', 'AI', 4, 'Expert Computer Vision', 1, 9990),
(33, '2019-01-31', 'AI', 4, 'Expert Computer Vision', 2, 9990),
(34, '2019-01-31', 'Cloud', 2, 'Advanced Azure', 3, 2990),
(35, '2019-02-03', 'AI', 4, 'Expert Computer Vision', 2, 9990),
(36, '2019-02-04', 'Cloud', 4, 'Expert GCP', 4, 820),
(37, '2019-02-05', 'Cloud', 4, 'Expert GCP', 2, 820),
(38, '2019-02-06', 'Security', 2, 'Advanced Penetration Testing', 2, 670),
(39, '2019-02-07', 'Security', 1, 'Basic Cybersecurity', 5, 990),
(40, '2019-02-08', 'Security', 3, 'Expert Ethical Hacking', 2, 720),
(41, '2019-02-08', 'Data', 3, 'Expert Big Data', 1, 2480),
(42, '2019-02-10', 'DevOps', 1, 'Basic CI/CD', 5, 2340),
(43, '2019-02-11', 'Cloud', 4, 'Expert GCP', 1, 820),
(43, '2019-03-05', 'Cloud', 1, 'Basic AWS', 3, 550),
(44, '2019-02-12', 'DevOps', 1, 'Basic CI/CD', 3, 2340),
(44, '2019-03-05', 'Cloud', 4, 'Expert GCP', 4, 820),
(45, '2019-02-13', 'AI', 1, 'Basic Machine Learning', 5, 290),
(45, '2019-03-05', 'DevOps', 1, 'Basic CI/CD', 3, 2340),
(46, '2019-02-14', 'Cloud', 4, 'Expert GCP', 4, 820),
(46, '2019-02-14', 'AI', 1, 'Basic Machine Learning', 5, 290),
(46, '2019-03-09', 'Cloud', 4, 'Expert GCP', 2, 820),
(46, '2019-03-10', 'DevOps', 3, 'Expert Container Orchestration', 1, 1990),
(46, '2019-03-11', 'DevOps', 3, 'Expert Container Orchestration', 1, 1990),
(47, '2019-02-14', 'Cloud', 2, 'Advanced Azure', 2, 2990),
(47, '2019-03-11', 'DevOps', 1, 'Basic CI/CD', 5, 2340),
(48, '2019-02-14', 'Security', 3, 'Expert Ethical Hacking', 4, 720),
(48, '2019-03-12', 'DevOps', 1, 'Basic CI/CD', 3, 2340),
(49, '2019-02-18', 'DevOps', 4, 'Expert Site Reliability', 2, 12300),
(49, '2019-02-18', 'Data', 3, 'Expert Big Data', 1, 2480),
(49, '2019-02-18', 'Data', 1, 'Basic SQL', 4, 2000),
(49, '2019-02-18', 'AI', 3, 'Expert Neural Networks', 1, 4990),
(50, '2019-02-20', 'AI', 2, 'Advanced Deep Learning', 4, 350),
(50, '2019-02-21', 'AI', 1, 'Basic Machine Learning', 4, 290),
(50, '2019-03-13', 'Cloud', 2, 'Advanced Azure', 5, 2990),
(50, '2019-03-14', 'DevOps', 2, 'Advanced Kubernetes', 2, 1990),
(51, '2019-02-21', 'Security', 1, 'Basic Cybersecurity', 2, 990),
(51, '2019-03-13', 'DevOps', 5, 'Expert Infrastructure as Code', 4, 1200),
(52, '2019-02-23', 'AI', 4, 'Expert Computer Vision', 2, 9990),
(52, '2019-03-18', 'Data', 1, 'Basic SQL', 5, 2000),
(53, '2019-02-24', 'Security', 1, 'Basic Cybersecurity', 4, 990),
(53, '2019-03-19', 'DevOps', 1, 'Basic CI/CD', 5, 2340),
(54, '2019-02-25', 'AI', 1, 'Basic Machine Learning', 4, 290),
(54, '2019-03-20', 'Cloud', 2, 'Advanced Azure', 1, 2990),
(55, '2019-02-26', 'AI', 4, 'Expert Computer Vision', 2, 9990),
(55, '2019-03-20', 'AI', 4, 'Expert Computer Vision', 5, 9990),
(56, '2019-02-27', 'Security', 3, 'Expert Ethical Hacking', 2, 720),
(56, '2019-03-20', 'AI', 3, 'Expert Neural Networks', 2, 4990),
(57, '2019-02-28', 'DevOps', 1, 'Basic CI/CD', 4, 2340),
(57, '2019-02-28', 'DevOps', 4, 'Expert Site Reliability', 1, 12300),
(57, '2019-03-20', 'DevOps', 5, 'Expert Infrastructure as Code', 1, 1200),
(57, '2019-03-20', 'DevOps', 3, 'Expert Container Orchestration', 1, 1990),
(58, '2019-02-28', 'Cloud', 1, 'Basic AWS', 1, 550),
(58, '2019-03-01', 'Cloud', 1, 'Basic AWS', 3, 550),
(58, '2019-03-02', 'AI', 1, 'Basic Machine Learning', 2, 290),
(58, '2019-03-25', 'Cloud', 4, 'Expert GCP', 2, 820),
(59, '2019-03-04', 'AI', 4, 'Expert Computer Vision', 4, 9990),
(60, '2019-03-05', 'Data', 3, 'Expert Big Data', 3, 2480),
(61, '2019-03-26', 'Security', 1, 'Basic Cybersecurity', 2, 990),
(62, '2019-03-27', 'DevOps', 4, 'Expert Site Reliability', 1, 12300),
(63, '2019-03-27', 'Security', 1, 'Basic Cybersecurity', 5, 990),
(64, '2019-03-27', 'DevOps', 1, 'Basic CI/CD', 3, 2340),
(65, '2019-03-27', 'DevOps', 3, 'Expert Container Orchestration', 4, 1990),
(66, '2019-03-31', 'Cloud', 3, 'Expert Multi-Cloud', 2, 270),
(67, '2019-03-31', 'Cloud', 4, 'Expert GCP', 5, 820);
```

## Solutions

For the solutions to these problems, please refer to the [CloudTech Solutions - Solutions](sql_solutions.md) file.