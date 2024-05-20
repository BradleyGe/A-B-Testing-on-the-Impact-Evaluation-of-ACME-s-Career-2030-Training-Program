# A/B Testing on the Impact of ACME's Career 203 Training program

•⁠ Project:      A/B Testing + Observational Study  
•⁠ Duration:     Feb.2024 - Mar.2024    
•⁠ Team Members: Bradley Ge, , Valerie Chan, Jim Tiao, Irene Wang, Ella Lee  

## *Background*

ACME Manufacturing, a company with over 60,000 employees, has launched the Career 2030 training program aimed at fostering the career development of its workforce. Despite its ongoing nature, the initial data from a randomly selected cohort of employees who participated in the training a year earlier has recently become available. ACME's Chief People Officer prioritizes data-informed decision-making and seeks insights into the program's impact on employee promotion and retention.

## *Problem*
Your analytics consulting group, specializing in causal inference, is one of six firms being considered by ACME to continuously monitor the effects of various employee programs, including Career 2030. As a proof of concept, the client desires a demonstration of your analysis approach, insights derived from the data, and recommendations for program optimization. The dataset provided by the client originates from a randomized controlled trial, where 5% of employees were randomly selected for training while another 5% were not. However, external factors such as interventions by managers and individual motivations may have influenced participation. There were also anecdotal evidence that some managers intervened for direct reports to be part of the program. It is also a fact that some motivated employees may be more interested in the training program as well as upward mobility in their careers. Your task is to analyze this dataset within three weeks, extract insights, and provide actionable recommendations to the client, demonstrating your competence in supporting data-informed decision-making.

## *Data*

### Raw Data
Available Data
The dataset comprises 6,000 employee records with the following attributes:

-empid: Employee ID  
-promoted: Whether the employee was promoted within a year of training  
-training: Participation in the Career 2030 program  
-manager: Employee managerial status  
-raise: Merit increase in the last review cycle  
-salary: Employee's salary bracket  
-children: Number of children  
-mstatus: Marital status  
-age: Age at promotion  
-sex: Gender  
-edu: Years of education at promotion  
-vacation: Vacation days taken in the year prior to promotion  
-weight: Weight at the last physical examination  
-height: Height at the last physical examination  
-hrfriend: Friend within the Human Resources department  
-cxofriend: C-level friend within the organization  
-insurance: Type of insurance coverage  
-flexspend: Participation in the Flexible Spending Account program  
-retcont: Participation in the 401k retirement saving program  
-race: Race  
-disthome: Distance from training facility to employee's home  
-testscore: Score in a standard test during the recruitment process  

### **Data Processing**  
Due to the skewness of features, we applied log transformation to satisfy the normality assumption that statistical tests require.
![WhatsApp Image 2024-05-19 at 23 39 34_3823a68d](https://github.com/BradleyGe/A-B-Testing-on-the-Impact-Evaluation-of-ACME-s-Career-2030-Training-Program/assets/141160516/f348a86b-9e76-45eb-a7f7-4a230885c689)  

The following graphs show the effect of log transformation on the distribution of features.  
![WhatsApp Image 2024-05-19 at 23 44 22_0e216f66](https://github.com/BradleyGe/A-B-Testing-on-the-Impact-Evaluation-of-ACME-s-Career-2030-Training-Program/assets/141160516/2fdc309f-2289-4364-87ec-858175107e6a)


## **Exploratory Data Analysis**

- Approximately 38.2% (N = 2,291) participated in the Career 2030 training program, while 61.8% (N = 3,709) did not participate.
- Managers represent 14.4% (N = 864) of the workforce, with non-managers making up 85.6% (N = 5,136).
- Around 53.6% (N = 3,216) of the employees have a friend in HR, while 55.05% (N = 3,303) have a friend at the C-level.
- For benefits participation, 66.43% (N = 3,986) are enrolled in the flexible spending account.
- Around 87.92% (N = 5,275) contribute to a retirement savings plan.

![WhatsApp Image 2024-05-19 at 22 00 21_5cca2e84](https://github.com/BradleyGe/A-B-Testing-on-the-Impact-Evaluation-of-ACME-s-Career-2030-Training-Program/assets/141160516/4672d1cb-de75-4759-8bdb-b28984ef0fa3)

## **Standardized Mean Difference**
Compute standardized mean difference to assess if each covariate 𝑗 has similar means between matched treatment 𝑇 and control 𝐶 groups 

- Balanced: 𝑆𝑀𝐷𝑗 < 0.1
- Investigate: 0.1 ≤ 𝑆𝑀𝐷𝑗 ≤ 0.2
- Imbalanced: 0.2 < 𝑆𝑀𝐷𝑗

![WhatsApp Image 2024-05-19 at 23 31 48_f8f69b05](https://github.com/BradleyGe/A-B-Testing-on-the-Impact-Evaluation-of-ACME-s-Career-2030-Training-Program/assets/141160516/c7bad05d-48f7-447a-87ca-a2ad58c16482)

When SMD is larger than 0.2(indicated by the red dashed line), there is a large discrepency on that feature for control and treatment groups. Even though this is an observational experiment where treatment group and control group were randomly chosen, there are 9 covariates that have SMD larger than 0.2, indicating very imbalanced groups.  For example, for the feature distance from home to the training facility, the mean and standard deviation is very different for employees in control and treatment groups (mean: 15.69 miles V.S. 25.61 miles).This happened due to the compliance issue where people that were chosen for the training did not choose to participate. This discrepency will lead to inaccurate reflection of the effect of training program on the promotion rate as you cannot decide whether it is the training program or the distance from home that leads to difference in promotion rate. 

Therefore, **Matching** is used to solve this problem.
 
## **Matching**
Three different matching methods are used: **1:1 Matching, Propensity Score Matching(PSM), and Inverse Probability of Treatment Weighting(IPTW)**  

After tuning the caliper, these three matching methods give the following results:  

![WhatsApp Image 2024-05-20 at 00 05 03_f2b78629](https://github.com/BradleyGe/A-B-Testing-on-the-Impact-Evaluation-of-ACME-s-Career-2030-Training-Program/assets/141160516/1b7fb23c-f826-4b12-b6a5-ad73451c54a7)  

According to the results, PSM were chosen because:
1. It doesn't violate the positivity assumption
2. only two features have SMD slightly higher than 0.1, while 1:1 Matching has 2 over 0.2
3. PSM has more matched pairs

## **Instrumental Variable Analysis**  
- The experiment suggests that external factors such as **manager intervention** and **individual motivation** may influence the training attendance rate. Anecdotal evidence suggests that managers might intervene on behalf of their direct reports to ensure their inclusion in the program.
- However, as these factors were not directly observed in the experiment, it is crucial for the team to employ **Instrumental Variable Analysis** to address potential confounding effects.

![WhatsApp Image 2024-05-20 at 00 22 24_19f8ad18](https://github.com/BradleyGe/A-B-Testing-on-the-Impact-Evaluation-of-ACME-s-Career-2030-Training-Program/assets/141160516/da45a712-de8e-4842-87bc-6bd5e4b4f396)


## **Instrumental Variable**  
An instrumental variable needs to satisfy the following three assumptions: **Relevance, Exclusion, Exogeneity**  

"Disthome", which is distance from home to the training facility, satisfies those three assumptions:  

a. **Relevance**: The traveling distance and time is potentially associated with employees’ willingness to attend the training program.   

b. **Exclusion**: There is no obvious relationship between the distance from employees’ home to facility and their promotional opportunities.   

c. **Exogeneity**: The distance variable is independent of the two unmeasured covariates, individual’s motivation and manager intervention. 

## **Two Stage Least Square Regression**
**First Stage**: Use Participation in the Training Program as Target Variable  

![image](https://github.com/BradleyGe/A-B-Testing-on-the-Impact-Evaluation-of-ACME-s-Career-2030-Training-Program/assets/141160516/2b5414b0-cbcb-4b87-a2c7-4b14d0a432f0)

**Second Stage**: Use Promotion as Target Variable  

![image](https://github.com/BradleyGe/A-B-Testing-on-the-Impact-Evaluation-of-ACME-s-Career-2030-Training-Program/assets/141160516/ea435184-cfd7-484f-adb1-3ee7ecb0fe18)

## **Results & Conclusion**
- Attending training program is **positively & causally** related to getting promotion 
- **Increases** the chance of promotion by **87.6%**

- Employees with higher chance to get promoted may have the following characteristic:
 
1. Took fewer vacation days last year

2. Have HR & C-executive connections
  
3. Achieved a higher test score

## **Recommendations** 

- **Training Program**:
  - Conduct experiments on reimbursing transportation costs for employees who participate in the training program
 
  - Focus on developing soft skills, particularly networking and communication, to boost promotion prospects
  
  - Customize training to engage top performers for better career advancement opportunities
  

- **Instrumental Variable Analysis**:
  - Improve the randomization in selecting treatment and control group to avoid imbalance in covariates that could cause confounding issues

