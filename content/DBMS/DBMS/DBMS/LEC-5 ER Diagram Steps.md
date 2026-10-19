---
share: true
---

Steps :
1. Identify Entity Set 
2. Identify Attributes && their type (single, derive, single-value ,multiple-value)
3. Identify Relationship(strong,/weak)  && degree of relation , Mapping/Cardinality , Participation constraint 

ER Model of Banking System 
1. Requirement : 
![[Pasted image 20260116195024.png|Pasted image 20260116195024.png]]
2. we assume many thing as we design the ER Model 
3. Now S-1) identify Entity Set
	1. branch 
	2. Customer
	3. Employee
	4. Account
	5. Saving a/c
	6. Current a/c
	7. Loan
	8. payment (loan) (weak )
4.  Now find Attribute related to Entity
	1. Branch : name, city, asserts , liabilities
	2. customer : name, age ,  contact no , dob, customer-id , address
	3. employee : salary , employ-id, name, contact no , dependent name , year of service , start date 
	4. saving account : account no , balance , daily withdrawal limit , interest rate 
	5. current account : acc no , balance , per transaction charges , overdraft amount 
	6. Generalization entity account : acc no , balance 
	7. loan  : loan no , amount
	8. weak entity payment : payment no ,date ,amount 
5. Relationship and constraint 
	1. customer borrow loan (m:n) loan exist if a single customer is their to pay so loan total dependent 
	2. loan is given by bank (n:1) loan depend on bank so loan total 
	3. loan and loan payment (1:n) loan payment depend on loan so loan payment total dependent 
	4. customer deposit  in account (m:n)
	5. customer banker employee (n:1)

![[Pasted image 20260127192614.png|Pasted image 20260127192614.png]]