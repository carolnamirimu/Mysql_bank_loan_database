 -- this is a database for loans extended to a number of customers in 
 -- 3 bank branches in the city of Kampala
 CREATE DATABASE `loans`;
 USE loans;
-- creating a table for the 3 branches of the bank in Kampala City
CREATE TABLE `bank_branches` (
  `branch_id` int(4) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  PRIMARY KEY (`branch_id`)
);
INSERT INTO `bank_branches` VALUES (1,'entebbe');
INSERT INTO `bank_branches` VALUES (2,'bukoto');
INSERT INTO `bank_branches` VALUES (3,'ntida');

-- creating a customers table for the customers that are current servicing loans in the bank
CREATE TABLE `customer` (
  `customer_id` int(11) NOT NULL,
  `first_name` varchar(50) NOT NULL,
  `last_name` varchar(50) NOT NULL,
  `address` varchar(50) NOT NULL,
  `city` varchar(50) NOT NULL,
  `email` varchar(50) NOT NULL,
  `phone` int(50) DEFAULT NULL,
  PRIMARY KEY (`customer_id`)
); 
INSERT INTO `customer` VALUES (1,'cherrie','namita','bukoto','kampala','namita@gmail.com',773820511);
INSERT INTO `customer` VALUES (2,'enock','wyatta','entebbe','kampala','wyatta@gmail.com',754561994);
INSERT INTO `customer` VALUES (3,'james','balisanyka','nakawa','kampala','balisanyka@yahoo.com',769837119);
INSERT INTO `customer` VALUES (4,'mary','atengo','entebbe','kampala','atengo@hotmail.com',701370128);
INSERT INTO `customer` VALUES (5,'eve','najja','ntinda','kampala','najja@gmail.com',781238910);

-- creating a table for the loans taken from the bank in Kampala city
CREATE TABLE `loan` (
  `loan_id` int(11) NOT NULL,
  `customer_id` int(11) NOT NULL,
  `branch_id` int(11) NOT NULL,
  `total_loan`  int(11) NOT NULL,
  `amount_paid` int(11),
  `date_of_issuance` date NOT NULL,
  `due_date` date NOT NULL,
  PRIMARY KEY (`loan_id`));

INSERT INTO `loan` VALUES (1,2,1,5000000,4000000,'2023-03-09','2024-04-30');
INSERT INTO `loan` VALUES (2,1,1,3000000,250000,'2024-01-02','2024-12-29');
INSERT INTO `loan` VALUES (3,1,2,5000000,10000000,'2023-09-23','2024-06-20');
INSERT INTO `loan` VALUES (4,3,1,2000000,200000,'2024-02-29','2025-01-20');
INSERT INTO `loan` VALUES (5,4,3,4000000,null,'2023-12-22','2027-12-01');
INSERT INTO `loan` VALUES (6,4,2,5000000,1000000,'2022-12-09','2023-12-31');


-- creating a table for the different payment made on loans, 
-- includeing the payment method id
CREATE TABLE `payment` (
  `payment_id` int(11) NOT NULL AUTO_INCREMENT,
  `loan_id` int(11) NOT NULL,
  `payment_method_id` int(11) NOT NULL,
  `amount_per_payment` int(11) NOT NULL,
   PRIMARY KEY (`payment_id`));
INSERT INTO `payment` VALUES (1,1,2,50000);
INSERT INTO `payment` VALUES (2,2,1,1000000);
INSERT INTO `payment` VALUES (3,3,3,1000000);
INSERT INTO `payment` VALUES (4,5,1,200000);
INSERT INTO `payment` VALUES (5,5,1,2000000);
INSERT INTO `payment` VALUES (6,6,2,50000);


-- creating a table to specify the different payment methods available to the custmers to 
-- pay their loans
CREATE TABLE `payment_methods` (
  `payment_methods_id` int(4) NOT NULL AUTO_INCREMENT,
  `payment_methods_name` varchar(50) NOT NULL,
  PRIMARY KEY (`payment_methods_id`)
);
INSERT INTO `payment_methods` VALUES (1,'cash');
INSERT INTO `payment_methods` VALUES (2,'money tranfer');
INSERT INTO `payment_methods` VALUES (3,'credit');

-- how much money has not yet been paid by each customer (outstanding amount)
SELECT c.customer_id,first_name, last_name,
 (total_loan-amount_paid) as outstanding_amount, due_date 
 FROM customer c
JOIN 
loan l
 on
c.customer_id= l.customer_id;

-- the bank wants to carry out a survey 
-- to acertain the reasons why customer chose the specific payment methods 
-- this will be done over phone call and in order of the different methods
-- therefore the survey group will need a table containing
-- the names of the customer, phone number and payment method and 
-- arranged according to the payment methods(order by payment methods)
SELECT last_name, first_name,phone, payment_methods_name FROM payment p
join
payment_methods pm
on p.payment_method_id=pm.payment_methods_id
join loan l
on l.loan_id=p.loan_id
join customer c
on c.customer_id=l.customer_id
ORDER BY P.payment_method_id;


-- the city bank manager wants to know  the number of customers in each branch
-- creating a table with the different branch ids,
-- and grouping all the customers by the branch id

SELECT  bb.branch_id, COUNT(bb.branch_id) as number_or_cutomers, name FROM customer c
join loan l
on c.customer_id=l.customer_id
join bank_branches bb
on l.branch_id= bb.branch_id
group BY bb.branch_id;
