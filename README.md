# Bank_DataBase
this is a project to the DataBase curse.. 

CREATE SCHEMA project;


CREATE TABLE project.bankAccount
(   accountNO           INT(10) NOT NULL ,
    bpassword			VARCHAR(20),
    balance				DECIMAL(30,2),
	CONSTRAINT bankAccount_PK PRIMARY KEY (accountNO)
);

INSERT INTO project.bankAccount
VALUES (5098 ,'st1111111', 25000.20);
INSERT INTO project.bankAccount
VALUES (3572 ,'st222222', 1014455);
INSERT INTO project.bankAccount
VALUES (1240 ,'st333333' ,99000.59 );
INSERT INTO project.bankAccount
VALUES (6174 ,'st444444',7700);
INSERT INTO project.bankAccount
VALUES (8205 ,'st555555',10582.91);


CREATE TABLE project.costumer
(   CID             INT(10) NOT NULL,
	accountNO   	INT(10),
	CONSTRAINT costumer_PK PRIMARY KEY (CID),
    CONSTRAINT costumer_FK FOREIGN KEY (accountNO) REFERENCES bankAccount(accountNO) ON DELETE SET NULL
);

INSERT INTO project.costumer
VALUE(181310 ,5098);
INSERT INTO project.costumer
VALUE(29720 ,3572);
INSERT INTO project.costumer
VALUE(31098 ,1240);
INSERT INTO project.costumer
VALUE(32077,6174 );
INSERT INTO project.costumer
VALUE(74906 ,8205 );


CREATE TABLE project.costumerInfo
(   CID             INT(10) NOT NULL,
	cname       	VARCHAR(30),
    phoneNO			VARCHAR(15),
	CONSTRAINT costumerInfo_PK PRIMARY KEY (CID)
);


INSERT INTO project.costumerInfo
VALUES(181310,'Ali Mohammad', '+9665055078');
INSERT INTO project.costumerInfo
VALUES(29720 ,'Nasser saed' , '+96655085558' );
INSERT INTO project.costumerInfo
VALUES(31098 ,'Ahmad Saif','+96658558058' );
INSERT INTO project.costumerInfo
VALUES(32077,'Hamad Khalid','+96657985505');
INSERT INTO project.costumerInfo
VALUES(74906 , 'Faisal Hasan', '+96659276393');


CREATE TABLE project.operation
(	accountNO       INT(10),
	operationNO		INT(20) NOT NULL,
    operationType   VARCHAR(20) CHECK (operationType IN ('deposit','withdraw','transaction')),
    operationAmount	DECIMAL(10,2),
	CONSTRAINT operation_PK PRIMARY KEY (operationNO),
    CONSTRAINT operation_FK FOREIGN KEY (accountNO) REFERENCES bankAccount(accountNO) ON DELETE SET NULL
);

INSERT INTO project.operation
VALUES(5098 , 273036 , 'withdraw', 300 );
INSERT INTO project.operation
VALUES(3572, 8297736, 'transaction', 7000 );
INSERT INTO project.operation
VALUES(1240, 7383197, 'deposit',500 );
INSERT INTO project.operation
VALUES(6174,30383, 'transaction',100  );
INSERT INTO project.operation
VALUES(8205,63683, 'deposit',1000);

CREATE TABLE project.accountInfo
(	accountNO       INT(10)  NOT NULL,
	balance       	DECIMAL(30,2),
   	CONSTRAINT accountInfo_PK PRIMARY KEY (accountNO)
);


INSERT INTO project.accountInfo
VALUES(5098 , 24700.20 );
INSERT INTO project.accountInfo
VALUES(3572,  1007455.00 );
INSERT INTO project.accountInfo
VALUES(1240, 99500.95);
INSERT INTO project.accountInfo
VALUES(6174 , 7600);
INSERT INTO project.accountInfo
VALUES(8205, 11682.91);


CREATE TABLE project.transactionInfo
(	operationNO		INT(20) NOT NULL,
    operationType   VARCHAR(20) CHECK (operationType IN ('deposit','withdraw','transaction')),
    operationAmount	DECIMAL(10,2),
    from_account	INT(10),
    to_account	    INT(10),
	CONSTRAINT transactionInfo_PK PRIMARY KEY (operationNO)
);

INSERT INTO project.transactionInfo
VALUES( 273036 , 'withdraw', 300 , null , null);
INSERT INTO project.transactionInfo
VALUES( 8297736, 'transaction', 7000 , 3572 , 5098);
INSERT INTO project.transactionInfo
VALUES( 7383197, 'deposit', 500 , null , null);
INSERT INTO project.transactionInfo
VALUES( 2732116 , 'withdraw', 700  , null , null);
INSERT INTO project.transactionInfo
VALUES( 30383, 'transaction', 100 , 6174,  8205 );
INSERT INTO project.transactionInfo
VALUES( 63683, 'deposit',1000, null , null);



CREATE TABLE project.chatBot
(   accountNO       INT(10),
	complaints		VARCHAR(30),
	suggestion     	VARCHAR(30),
    evaluation		DECIMAL(5,2),  
	CONSTRAINT chatBot_FK FOREIGN KEY (accountNO) REFERENCES bankAccount(accountNO) ON DELETE SET NULL
);

INSERT INTO project.chatBot
VALUES(5098, 'the process is slow','develop the device' , 4.0);
INSERT INTO project.chatBot
VALUES(3572, 'nothing', 'noting ', 5);
INSERT INTO project.chatBot
VALUES(1240, 'the withdraw was complex ', 'improve your service' ,4);
INSERT INTO project.chatBot
VALUES(6174, 'the transaction is slow ', 'update the system ' , 3);
INSERT INTO project.chatBot
VALUES(8205, 'nothing', 'noting ', 5);

UPDATE project.bankAccount
SET bpassword = 'st0000000'
WHERE bpassword='st1111111';

SELECT *
FROM project.bankAccount;

DELETE FROM project.costumer
WHERE CID = 181310;

SELECT *
FROM project.costumer;

SELECT  *	
FROM project.costumer 
ORDER BY CID DESC;

SELECT *
FROM project.bankAccount
WHERE accountNO IN ( SELECT DISTINCT accountNO    
FROM project.operation
WHERE balance  <= 10000);

SELECT balance,accountNO, COUNT(accountNO) AS count
FROM project.bankAccount
GROUP BY balance
ORDER BY balance ;

SELECT evaluation,accountNO, COUNT(accountNO) AS count
FROM project.chatBot
GROUP BY evaluation
ORDER BY evaluation;

SELECT cname,phoneNO, COUNT(CID) AS count
FROM project.costumerInfo
GROUP BY cname
ORDER BY cname;

SELECT balance,accountNO, COUNT(accountNO) AS count
FROM project.bankAccount
GROUP BY balance
HAVING (balance) <9900 ;

SELECT evaluation,accountNO, COUNT(accountNO) AS count
FROM project.chatbot
GROUP BY evaluation
HAVING (evaluation) <=3 ;

SELECT accountNO,operationNO,operationType, COUNT(accountNO) AS count
FROM project.operation
GROUP BY operationType
ORDER BY operationType;

SELECT accountNO,operationAmount,COUNT(accountNO) AS count
FROM project.operation
GROUP BY operationAmount
HAVING (operationAmount) > 600 ;

SELECT 	b.accountNO , bpassword, balance , c.accountNO , CID
FROM 	project.bankAccount b , project.costumer c 
WHERE 	b.accountNO = c.accountNO;
