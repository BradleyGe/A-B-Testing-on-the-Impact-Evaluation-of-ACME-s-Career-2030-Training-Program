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



**1. Issues related to application data**
*   ***Duplicate IDs:*** The uniqueness of IDs possibly from data input error. As this can lead to duplicates when merging with the credit status data, we will remove these duplicate IDs from the application dataset.
*   ***Missing Values:*** More than 2m+ occupation type data points were missing. This might have arised from not being entered or collected. In this case we used predictive models to impute the data.

**Step 1: Delete Duplicate IDs**  

![WhatsApp Image 2024-05-19 at 17 51 15_53f70087](https://github.com/BradleyGe/Credit-Card-Approval-Prediction-Project/assets/141160516/46e7f333-d38e-4888-b435-0eaeab95aae8)  
Since there are only 5 duplicate IDs, we decided to simply remove them.

**Step 2: Impute Missing Values**  

![WhatsApp Image 2024-05-19 at 18 05 23_eebc8a89](https://github.com/BradleyGe/Credit-Card-Approval-Prediction-Project/assets/141160516/e3fb9b6a-54bf-4a82-903f-a6c13a0d85f9)  

Since 30% of the data have missing value in occupation, we believed that deleting them would lead to a big loss of data, and also research shows that occupation is a key factor deciding whether an applicant is qualified for a credit card, therefore, we would like to impute missing values using random forest classfier.

To do so, we use data with no missing value as training data to train the model, and then use the random forest classfier to impute the missing value in occupation.  

![WhatsApp Image 2024-05-19 at 18 03 29_cf2615de](https://github.com/BradleyGe/Credit-Card-Approval-Prediction-Project/assets/141160516/36b4f8ae-51e7-4046-853e-bb2ea5811a61)  



**2. Target Variable**  

As target variable is not given, we need to define by ourselves.

The status of applicants are given as follows:  
0: 1-29 days past due  

1: 30-59 days past due  

2: 60-89 days overdue   

3: 90-119 days overdue   

4: 120-149 days overdue   

5: Overdue or bad debts, write-offs for more than 150 days   

C: paid off that month   

X: No loan for the month  

To classify, for those people who paid off that month, s/he will be calssfied as "good", and for those who were overdue, no matter how long (from 1 to 5), they will be considered "bad". For those who had no loan for that month(X), they will be exclued from the data as it doesn't help with deciding whether s/he will defafult or not.

Also, we are given the feature Month_Balance: The month of the extracted data is the starting point, backwards, 0 is the current month, -1 is the previous month, and so on.  

Therefore, the target variable is: The percentage of months a person defaulted. The value of target variable ranges from 0 to 1, where 1 means a person defaults every month, and 0 means never default in any month.  

The threshold for deciding whether a customer has good credit or not is 0.05, meaning if that customer defaults less than 5% of the months, s/he will be considered a good credit customer.  


**3. Imbalance in Target Variable**  

![WhatsApp Image 2024-05-19 at 19 01 34_4aed6894](https://github.com/BradleyGe/Credit-Card-Approval-Prediction-Project/assets/141160516/f6d99eed-bf9d-48be-97df-8aa0423cf363)  
Target Variable Distribution  


Undersampling, Oversampling, and SMOTE  

![WhatsApp Image 2024-05-19 at 19 05 29_e57b6173](https://github.com/BradleyGe/Credit-Card-Approval-Prediction-Project/assets/141160516/f2b3d254-d7f1-4a2e-bcff-9f7dbb3b189f)  
To deal with this extreme biased target variable, we used undersampling, oversampling, and SMOTE.


## *Modeling & Model Evaluation*
- Random Forest, KNN, and Logistic Regression are built
- Cross Validation with 10 folds are used
- Tuning was done with selected hyperparameters
![WhatsApp Image 2024-05-19 at 21 42 15_8f3cc247](https://github.com/BradleyGe/Credit-Card-Approval-Prediction-Project/assets/141160516/797a7974-138d-4483-9ddd-b2ebf8dab71f)

- Random Forest on SMOTE gives the highest accuracy rate
![WhatsApp Image 2024-05-19 at 21 22 28_0acfceb3](https://github.com/BradleyGe/Credit-Card-Approval-Prediction-Project/assets/141160516/b7b1a0b9-4507-4274-8a9d-d37a9c7f935c)



## *Deployment*  
Potential issues and risks
- Legal & Ethical issues: Model should be adjusted according to the laws: Fair Lending and Anti-Discrimination Laws
  - Equal Credit Opportunity Act (ECOA): 
    - The ECOA prohibits credit discrimination on the basis of race, color, religion, national origin, sex, marital status, age, receipt of income from public assistance programs, or the exercise of rights under the Consumer Credit Protection Act.
  - Age Discrimination in Credit Act (ADCA):
    - The ADCA prohibits discrimination in credit transactions based on age. Credit card issuers cannot use age as a sole reason for approving or denying credit.
- Privacy: Secure applicant’s personal information and prevent disclosure
- Transparency :The model’s decision-making process should be transparent to regulators, customers, and internal stakeholders


