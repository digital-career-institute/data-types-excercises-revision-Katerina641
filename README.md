# java-DB-datatypes

#### 1. Enum data type usage

- Create enum that will contain statuses 'NEW', "PAID", "WAITING FOR PAYMENT".
- Create table "Invoices" with columns: buyer, seller, value, account number, status. Chose the right column types.
- Insert a few rows into this table with different statuses.
- Select only those invoices with status = "NEW"

#### 2. UUID data type usage

- Modify table "Invoices" - add column that will keep "internal_id". 
- Update each row with a value in a column "internal_id". You can use this generator: https://www.uuidgenerator.net/

#### 3. JSON data type usage

- Modify table "Invoices" - add 2 columns that will keep json view of an entity. One column should be with type json and other with type jsonb.
- Try to fill newly created columns with below json:

'{
  "buyer": "company a",
  "seller": "company b",
  "status": "NEW",
  "account_number": 123321123
  "value": 23.43
}



#### 4. Array data type usage

- Modify table "Invoices". Add a column where phone numbers of buyer will be kept. Maximum number of those number is 3.
- Insert a list of phone numbers for an invoice with id = 3: 123211233, 125433221, 127643454
- Insert a list of phone numbers for an invoice with id = 2: 432323112, 123344311
- Select buyer name and his LAST phone number for invoice with id = 3;
- Select an invoice which contains phone: 432323112 

#### 5. Binary data type usage

- Create a file, can be an image or just a text file
- Create a new column in Invoices table that will keep this file
- Insert this file into database for invoice with id = 1


#### 6. Date/time data types usage

- Modify table "Invoices", add 4 new columns: payment deadline, transaction time, transaction hour, cyclic payment. Choose the right column types. 
- Update each column with values
- Update timezone in transaction type, set it to be Melbourne, Australia. 
- Update timezone in transaction time column, set it to be Melbourne, Australia. 
- Select transaction time column, make sure that 10 hours has been added to the time that previously was set.

####1


CREATE type statuses as enum('NEW', 'PAID', 'WAITING FOR PAYMENT');
CREATE TABLE Invoices (buyer INT, seller VARCHAR(30), value INT, account_number INT, status statuses)
INSERT INTO Invoices VALUES (2,'ggg',10,5555,'WAITING FOR PAYMENT');
INSERT INTO Invoices VALUES (3,'ppp',12,2222,'NEW');
SELECT buyer FROM Invoices WHERE status = 'NEW';

####2

ALTER TABLE Invoices ADD COLUMN internal_id uuid
UPDATE Invoices SET internal_id = '595b2ecd-359f-455f-92e8-d14fac06ced8' WHERE buyer = 2;
UPDATE Invoices SET internal_id = '605b2ecd-359f-455f-92e8-d14fac06ced8' WHERE buyer = 3;
select * from Invoices

####3


ALTER TABLE Invoices ADD COLUMN view JSON;
ALTER TABLE Invoices ADD COLUMN entity JSONB;
UPDATE Invoices SET view ='{ "buyer": "company a", "seller": "company b", "status": "NEW", "account_number": 123321123, "value": 23.43 }'::JSON
WHERE buyer = 2;
UPDATE Invoices SET view ='{ "buyer": "company a", "seller": "company b", "status": "NEW", "account_number": 123321123, "value": 23.43 }'::JSONB
WHERE buyer = 3;

####4


ALTER TABLE Invoices ADD COLUMN phone_number INTEGER[3]
UPDATE Invoices SET phone_number =ARRAY[123211233, 125433221, 127643454] WHERE buyer = 2;
UPDATE Invoices SET phone_number =ARRAY[432323112, 123344311] WHERE buyer = 3;
SELECT phone_number[2] FROM Invoices WHERE buyer = 3;

####5

ALTER TABLE Invoices ADD column image_file bytea;
UPDATE Invoices SET image_file =pg_read_binary_file('/home/dci-student/Desktop/test2.txt')::bytea WHERE buyer = 3


####6

ALTER TABLE Invoices
ADD COLUMN payment_deadline DATE,
ADD COLUMN transaction_time TIMESTAMP,
ADD COLUMN transaction_hour TIME,
ADD COLUMN cyclic_payment INTERVAL;

UPDATE Invoices SET
    payment_deadline = '2024-02-01',
    transaction_time = '2024-01-30 12:00:00',
    transaction_hour = '12:00:00',
    cyclic_payment = '1 day';
	
UPDATE Invoices SET
    transaction_time = transaction_time + interval '10 hours';
	
SELECT * FROM Invoices;
