# Code-used-for-AWS-redshift
This was for ISM6362 Big Data and Cloud-Based Tools

### This was used to create the data warehouse
```markdown
CREATE TABLE PUBLIC.CHURNS
(
	Attrition_Flag_mapped INT,
  Customer_Age INT,
  Gender_mapped INT,
  Dependent_count INT,
  Education_Level_Doctorate_Dummy decimal,
  Education_Level_PostGraduate_Dummy decimal,
  Education_Level_College_Dummy decimal,
  Education_Level_Highschool_Dummy decimal,
  Education_Level_Graduate_Dummy decimal,
  Marital_Status_divorced_dummy decimal,
  Marital_Status_married_dummy decimal,
  Income_Category_80to120k_dummy decimal,
  Income_Category_60to80k_dummy decimal,
  Income_Category_40to60k_dummy decimal,
  Income_Category_under40k_dummy decimal,
  Card_Category_Gold_dummy decimal,
  Card_Category_Silver_Dummy decimal,
  Card_Category_Blue_dummy decimal,
  Months_on_book INT,
  Total_Relationship_Count INT,
  Months_Inactive_12_mon INT,
  Contacts_Count_12_mon INT,
  Credit_Limit decimal,
  Total_Revolving_Bal INT,
  Total_Amt_Chng_Q4_Q1 decimal,
  Total_Trans_Amt INT,
  Total_Ct_Chng_Q4_Q1 decimal
);
COMMIT;

COPY PUBLIC.CHURNS
FROM 's3://databrew-bucket7.30.21/customerchurn-data_09Aug2021_1628544885442/customerchurn-data_09Aug2021_1628544885442_part00000.csv'
IAM_ROLE 'arn:aws:iam::275927079877:role/RedshiftS3ReadOnly'
CSV
DELIMITER ','
IGNOREHEADER 1

SELECT * FROM PUBLIC.CHURNS
```

### This was the query we created showing the card category by attrition. This aggregated all the customers so we could see how many customers left per card type.
```markdown
SELECT Attrition_Flag_mapped,sum(Card_Category_Blue_dummy) as "Blue", sum(Card_Category_Silver_Dummy) as "Silver",sum(Card_Category_Gold_dummy) as "Gold"
FROM PUBLIC.CHURNS
group by Attrition_Flag_mapped
```
