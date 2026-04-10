# SQL

Zapytania robione do nauki do kolowium z baz danych

//===============Laboratorium 4
zad 4.1
CREATE TABLE ProductCategoryTest(
    ProductCategoryKey NUMBER(2),
    ProductCategoryName VARCHAR2(30)
);

CREATE TABLE ProductSubCategoryTest(
    ProductSubCategoryKey NUMBER(3),
    ProductSubCategoryName VARCHAR2(40)
);

zad 4.2
CREATE TABLE ProductTest(
    ProductKey NUMBER(5),
    ProductCode VARCHAR2(12) UNIQUE NOT NULL,
    ProductName VARCHAR2(40) NOT NULL UNIQUE,
    PRIMARY KEY(ProductKey)
);

zad 4.3
ALTER TABLE ProductSubCategoryTest
ADD ProductCategoryKey NUMBER(2) NOT NULL;

ALTER TABLE ProductTest
ADD ProductSubCategoryKey NUMBER(2) NOT NULL;

zad 4.4
ALTER TABLE ProductCategoryTest
ADD CONSTRAINT ProductCategory_PK 
PRIMARY KEY (ProductCategoryKey);

//===================================pominiete

zad 4.5
ALTER TABLE ProductSubcategory
ADD CONSTRAINT ProductSubcat_Cat_FK
FOREIGN KEY (ProductCategoryKey)
REFERENCES ProductCategory(ProductCategoryKey);

ALTER TABLE Product
ADD CONSTRAINT Product_Subcategory_FK
FOREIGN KEY (ProductSubcategoryKey)
REFERENCES ProductSubcategory(ProductSubcategoryKey);

zad 4.6
ALTER TABLE ProductCategory
DISABLE CONSTRAINT ProductCategory_PK;

ALTER TABLE ProductSubcategory
DISABLE CONSTRAINT ProductSubcategory_PK;

ALTER TABLE Product
DISABLE CONSTRAINT Product_PK;

zad 4.7
ALTER TABLE ProductSubcategory
ENABLE CONSTRAINT ProductSubcat_Cat_FK;

//dalej mi sie nie chce bo to to samo

zad 4.8
//to zle podobno
ALTER TABLE ProductCategory
DROP CONSTRAINT ProductCategory_Key_NN;


ALTER TABLE ProductCategory
MODIFY ProductCategoryKey NULL;



zad 4.9 
CREATE TABLE OrderDetailTest(
    OrderKey NUMBER(12),
    ProductKey NUMBER(5),
    Quantity NUMBER(2),
    CatalogPrice NUMBER(7,2),
    DiscountAmount NUMBER(5,2),
    DiscountPctg NUMBER(2),
    TransactionPrice NUMBER(7,2),
    FOREIGN KEY (ProductKey) REFERENCES ProductTest(ProductKey)
);
//tylko jedna z tabel

zad 4.10
ALTER TABLE OrderDetailTest
MODIFY DiscountAmount NUMBER(7,2);

zad 4.11
ALTER TABLE OrderHeader
DROP COLUMN OtherInfo;


//-------------------Laboratorium 5

zad 5.7
INSERT INTO SalesTerritory
SELECT * FROM SalesTerritoryBckp;

zad 5.8
UPDATE OrderDetailTest
SET DiscountAmount=0, DiscountPctg =0;

zad 5.9
    podpunkt 1
        UPDATE OrderDetail
        SET Quantity = 5
        WHERE OrderKey == "SO56205" and ProductKey == "217";
    podpunkt 2
        


//===============Laboratorium 6

zad 6.1

SELECT * FROM CUSTOMER;

SELECT
    CityName AS ID_City,
    CITYKEY AS City_Full_Name,
    COUNTRYKEY AS ID_Country
 FROM CITY;

 Zad 6.2

SELECT 
    City.CityName AS "City Full Name"
FROM 
    Customer
JOIN 
    City ON Customer.CityKey = City.CityKey;


SELECT 
    COUNTRY.COUNTRYNAME AS "Country Name",
    COUNTRY.COUNTRYCODE AS "Country Code"
FROM 
    Customer
JOIN 
    City ON Customer.CityKey = City.CityKey
JOIN 
    COUNTRY ON City.COUNTRYKEY = Country.COUNTRYKEY;

Zad 6.3

SELECT DISTINCT
    CUSTOMER.CITYKEY AS "ID City"
FROM CUSTOMER;

SELECT LastName, FirstName
FROM Customer;

Zad 6.4

SELECT
    CONCAT (COUNTRYNAME,' (',COUNTRYCODE,')') AS cos
FROM COUNTRY;

SELECT
    COUNTRYNAME,
    CASE
        WHEN COUNTRYNAME = 'United States' THEN 'North America'
        WHEN COUNTRYNAME = 'Canada' THEN 'North America'
        ELSE 'Other Country' 
    END AS "Continent name"
FROM COUNTRY;

Zad 6.5
SELECT DISTINCT
    EXTRACT(YEAR FROM ORDERDATE) AS "Order Year"
FROM OrderHeader;

SELECT DISTINCT
    EXTRACT(YEAR FROM ORDERDATE) AS "Order Year",
    TO_CHAR(ORDERDATE, 'Month') AS "Order Month Name"
FROM OrderHeader;

SELECT DISTINCT
    EXTRACT(YEAR FROM ORDERDATE) AS "Order Year",
    TO_CHAR(ORDERDATE, 'Day') AS "Order Month Name"
FROM OrderHeader;


zad 6.6
SELECT DISTINCT
    EXTRACT(YEAR FROM ORDERDATE) AS "Order Year"
FROM OrderHeader
ORDER BY "Order Year" DESC;

SELECT DISTINCT
    EXTRACT(YEAR FROM ORDERDATE) AS "Order Year",
    TO_CHAR(ORDERDATE, 'Month') AS "Order Month Name"
FROM OrderHeader
ORDER BY "Order Year" DESC, "Order Month Name";

zad 6.7
    podpunkt 1
        SELECT
            CUSTOMERKEY
        FROM OrderHeader
        WHERE
            TO_CHAR(ORDERDATE, 'YYYY') = 2018;

    podpunkt 2
        SELECT
            COUNTRYKEY,
            TO_CHAR(ORDERDATE, 'Day')
        FROM OrderHeader
        WHERE
            TO_CHAR(ORDERDATE, 'DY',  'NLS_DATE_LANGUAGE=ENGLISH') = 'SAT';
    
    podpunkt 3
        SELECT
            CUSTOMERKEY
        FROM ORDERHEADER
        WHERE
            EXTRACT(YEAR FROM ORDERDATE) = 2019 AND
            EXTRACT(MONTH FROM ORDERDATE) = 6;
   
    podpunkt 4
        SELECT
            ORDERKEY
        FROM ORDERDETAIL
        WHERE
            PRODUCTKEY = 217
        ORDER BY
            ORDERKEY;
    podpunkt 5
        SELECT
            PRODUCTKEY
        FROM ORDERDETAIL
        WHERE
            CATALOGPRICE = TRANSACTIONPRICE
        ORDER BY
            PRODUCTKEY ASC;
    podpunkt 6
        SELECT
            ORDERKEY
        FROM ORDERDETAIL
        WHERE
            PRODUCTKEY = 483 AND
            TRANSACTIONPRICE > 360;
    
Zad 6.8
    podpunkt 1
        SELECT DISTINCT
            CHANNELKEY
        FROM ORDERHEADER
        WHERE 
            TO_CHAR(ORDERDATE, 'DY','NLS_DATE_LANGUAGE=ENGLISH') IN ('SUN','SAT');
    podpunkt 2
        SELECT DISTINCT
            PAYMENTMETHODKEY
        FROM ORDERHEADER
        WHERE 
        COUNTRYKEY IN (6);
    podpunkt 3
        SELECT DISTINCT
            ORDERKEY
        FROM ORDERDETAIL
        WHERE 
        PRODUCTKEY IN (483,528)
        ORDER BY
            ORDERKEY;
    podpunkt 4
        SELECT DISTINCT
            PRODUCTKEY
        FROM PRODUCT
        WHERE 
        PRODUCTSUBCATEGORYKEY IN (12,1)
        ORDER BY PRODUCTKEY;

    podpunkt 5
        SELECT DISTINCT
            PRODUCTNAME
        FROM PRODUCT
        WHERE 
        PRODUCTCODE IN ('RF-9198','CH-0234');

zad 6.9
    podpunkt 1
        SELECT DISTINCT
            CUSTOMERKEY
        FROM ORDERHEADER
        WHERE 
        EXTRACT(YEAR FROM ORDERDATE) BETWEEN 2017 AND 2019
        ORDER BY
            CUSTOMERKEY;
    podpunkt 2
        SELECT DISTINCT 
            CustomerKey
        FROM OrderHeader
        WHERE 
            OrderDate BETWEEN TO_DATE('2018-07-01','YYYY-MM_DD') AND TO_DATE('2018-09-30','YYYY-MM-DD')
        ORDER BY CustomerKey;
    podpunkt 3
        SELECT DISTINCT PRODUCTKEY
        FROM ORDERDETAIL
        WHERE
            TRANSACTIONPRICE BETWEEN 1000 and 3000
        ORDER BY PRODUCTKEY;

zad 6.10
    podpunkt 1
        SELECT DISTINCT
            PRODUCTSUBCATEGORYNAME
        FROM PRODUCTSUBCATEGORY
        WHERE
            PRODUCTSUBCATEGORYNAME LIKE '%Mountain%'
        ORDER BY
            PRODUCTSUBCATEGORYNAME;
    podpunkt 2

    // reszta to to samo tak naprawde nie chce mi sie

zad 6.11
    SELECT DISTINCT
        ORDERKEY,
        ORDERDATE
    FROM ORDERHEADER
    WHERE
        DELIVERYDATE IS NULL
    order BY
    ORDERDATE DESC;

zad 6.12
    podpunkt 1
        SELECT DISTINCT
            CHANNELKEY
        FROM ORDERHEADER
        WHERE
            EXTRACT(YEAR FROM ORDERDATE) = 2019 and COUNTRYKEY = 1;
    podpunkt 2
        SELECT DISTINCT
            COUNTRYKEY,
            TO_CHAR(OrderDate, 'YYYY') AS Year,
            TO_CHAR(OrderDate, 'MM') AS Month
        FROM ORDERHEADER
        WHERE
            CHANNELKEY = 2 and EXTRACT(MONTH FROM ORDERDATE) IN (5,6) AND  EXTRACT(YEAR FROM OrderDate) = 2019
        ORDER BY
            COUNTRYKEY,Year,Month;
    podpunkt 3
        SELECT DISTINCT
            CUSTOMERKEY,
            TO_CHAR(OrderDate, 'YYYY') AS Year,
            TO_CHAR(OrderDate, 'MM') AS Month
        FROM ORDERHEADER
        WHERE
            COUNTRYKEY = 3 and 
            EXTRACT(MONTH FROM ORDERDATE) IN (7,8,9) AND  
            EXTRACT(YEAR FROM OrderDate) = 2018 AND
            DELIVERYCOST = 0 AND
            DELIVERYMETHODKEY = 2
        ORDER BY
            CUSTOMERKEY,Year,Month;

zad 6.13
    podpunkt 1
        SELECT ProductName
        FROM Product
        WHERE REGEXP_LIKE(ProductName, '[0-9]')
        ORDER BY ProductName;
    podpunkt 2
        SELECT ProductName
        FROM Product
        WHERE PRODUCTNAME LIKE '%-%' OR PRODUCTNAME LIKE '%,%'
        ORDER BY ProductName;
    podpunkt 3
        SELECT ProductName
        FROM Product
        WHERE REGEXP_LIKE(ProductName, '*[0-9]')
        ORDER BY ProductName;
    podpunkt 4
        SELECT ProductName
        FROM Product
        WHERE REGEXP_LIKE(PRODUCTNAME, '[S,M,L,XL]$')
        ORDER BY ProductName;
    podpunkt 5
        SELECT ProductName
        FROM Product
        WHERE PRODUCTNAME LIKE ('%Front Wheel%') OR 
        PRODUCTNAME LIKE ('%Rear Wheel%')
        ORDER BY ProductName;
    podpunkt 6
        SELECT ProductName
        FROM Product
        WHERE PRODUCTNAME LIKE ('%Mountain%') AND PRODUCTNAME LIKE ('%Tube%') OR 
        PRODUCTNAME LIKE ('%Road%') AND PRODUCTNAME LIKE ('%Tube%')
        ORDER BY ProductName;

//-------------------Laboratorium 7

zad 7.1
    podpunkt 1
        SELECT 
            PRODUCTCATEGORY.PRODUCTCATEGORYNAME "Category Name",
            PRODUCTSUBCATEGORY.PRODUCTSUBCATEGORYNAME "Subcategory Name"
        FROM PRODUCTCATEGORY
        INNER JOIN PRODUCTSUBCATEGORY ON PRODUCTCATEGORY.PRODUCTCATEGORYKEY = PRODUCTSUBCATEGORY.PRODUCTCATEGORYKEY
        ORDER BY "Category Name" ASC,"Subcategory Name" DESC ;
    podpunkt 2
        SELECT
            CONCAT(p.PRODUCTCODE,'-', p.PRODUCTNAME) "Product",
            psc.PRODUCTSUBCATEGORYNAME "Subcategory Name"
        FROM PRODUCT p
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTSUBCATEGORYKEY
        ORDER BY "Product";
    podpunkt 3
        SELECT 
            EXTRACT(YEAR FROM ORDERDATE) "year",
            ot.PRODUCTKEY "Product"
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL ot
            ON oh.ORDERKEY = ot.ORDERKEY
        WHERE EXTRACT(YEAR FROM ORDERDATE) IN (2018,2019)
        ORDER BY "year" DESC, "Product";
    podpunkt 4
        SELECT 
            od.TRANSACTIONPRICE,
            od.PRODUCTKEY
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            on oh.ORDERKEY = od.ORDERKEY
        WHERE ORDERDATE BETWEEN TO_DATE('2019-12-1','YYYY-MM-DD') AND TO_DATE('2019-12-24','YYYY-MM-DD') and CHANNELKEY = 2
        ORDER BY od.TRANSACTIONPRICE DESC;
zad 7.2
    podpunkt 1
        SELECT DISTINCT
            pm.PAYMENTMETHODNAME "Payment Method"
        FROM ORDERHEADER oh
        INNER JOIN ORDERCHANNEL och
            ON oh.CHANNELKEY = och.CHANNELKEY
        INNER JOIN PAYMENTMETHOD pm
            on pm.PAYMENTMETHODKEY = oh.PAYMENTMETHODKEY
        WHERE och.CHANNELNAME = 'Mobile application';
    podpunkt 2
        SELECT DISTINCT
            c.CUSTOMERKEY,
            c.FIRSTNAME,
            c.LASTNAME
        FROM ORDERHEADER oh
        INNER JOIN CUSTOMER c 
            ON oh.CUSTOMERKEY = c.CUSTOMERKEY
        INNER JOIN ORDERSTATUS os
            ON os.ORDERSTATUSKEY = oh.ORDERSTATUSKEY
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON p.PRODUCTKEY = od.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTSUBCATEGORYKEY
        WHERE
            os.ORDERSTATUSNAME = 'Canceled' AND psc.PRODUCTSUBCATEGORYNAME = 'Mountain Bikes';
    podpunkt 3 //tu nwm jak zinterpretowac "Dokonal zakupu" czy dac "Approved" czy "Finished"
        SELECT
            c.CUSTOMERKEY,
            c.FIRSTNAME,
            c.LASTNAME,
            p.PRODUCTNAME,
            p.PRODUCTKEY
        FROM ORDERHEADER oh
        INNER JOIN CUSTOMER c
            on c.CUSTOMERKEY = oh.CUSTOMERKEY
        INNER JOIN ORDERSTATUS os
            on os.ORDERSTATUSKEY = oh.ORDERSTATUSKEY
        INNER JOIN ORDERDETAIL od
            ON od.ORDERKEY = oh.ORDERKEY
        INNER JOIN PRODUCT p
            ON p.PRODUCTKEY = od.PRODUCTKEY
        WHERE
            c.LASTNAME = 'Alan' AND 
            os.ORDERSTATUSNAME = 'Finished'
        ORDER BY c.CUSTOMERKEY, p.PRODUCTKEY;
    podpunkt 4
        SELECT DISTINCT
            dm.DELIVERYMETHODNAME
        FROM ORDERHEADER oh
        INNER JOIN DELIVERYMETHOD dm
            on dm.DELIVERYMETHODKEY = oh.DELIVERYMETHODKEY
        INNER JOIN ORDERDETAIL od
            ON od.ORDERKEY = oh.ORDERKEY
        INNER JOIN PRODUCT p
            ON p.PRODUCTKEY = od.PRODUCTKEY
        WHERE
            REGEXP_LIKE(p.PRODUCTNAME,  '(.*[Ww]omen''s|[Mm]en''s).*shorts.*','i');
    podpunkt 5
        SELECT DISTINCT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            st.SALESTERRITORYNAME,
            CONCAT(p.PRODUCTCODE,'-',p.PRODUCTNAME) "Produkt"
            
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON od.ORDERKEY = oh.ORDERKEY
        INNER JOIN PRODUCT p
            ON p.PRODUCTKEY = od.PRODUCTKEY
        INNER JOIN COUNTRY c
            ON c.COUNTRYKEY = oh.COUNTRYKEY
        INNER JOIN SALESTERRITORY st
            on st.SALESTERRITORYKEY = c.SALESTERRITORYKEY
        ORDER BY "Year" DESC,st.SALESTERRITORYNAME DESC, "Produkt" DESC;
    podpunkt 6
        SELECT DISTINCT
            c.CUSTOMERKEY,
            c.LASTNAME,
            c.FIRSTNAME
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON p.PRODUCTKEY = od.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTSUBCATEGORYKEY
        INNER JOIN PRODUCTCATEGORY pc
            ON pc.PRODUCTCATEGORYKEY = psc.PRODUCTCATEGORYKEY
        INNER JOIN CUSTOMER c
            ON c.CUSTOMERKEY = oh.CUSTOMERKEY
        WHERE
            pc.PRODUCTCATEGORYNAME = 'Bikes' AND
            EXTRACT(YEAR FROM oh.ORDERDATE) = 2018
        ORDER BY c.CUSTOMERKEY;
    podpunkt 7
        SELECT DISTINCT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            CONCAT('Q',TO_CHAR(oh.ORDERDATE, 'Q')) "Quarter"
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON p.PRODUCTKEY = od.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTSUBCATEGORYKEY
        INNER JOIN PRODUCTCATEGORY pc
            ON pc.PRODUCTCATEGORYKEY = psc.PRODUCTCATEGORYKEY
        WHERE
            pc.PRODUCTCATEGORYNAME = 'Bikes'
        ORDER BY "Year" DESC, "Quarter";
zad 7.3
    podpunkt 1
        SELECT DISTINCT
            c.CUSTOMERKEY,
            c.LASTNAME,
            c.FIRSTNAME
        FROM CUSTOMER c
        LEFT JOIN ORDERHEADER oh
            ON oh.CUSTOMERKEY = c.CUSTOMERKEY
        WHERE 
            oh.CUSTOMERKEY IS NULL
        ORDER BY c.LASTNAME, c.FIRSTNAME;
    podpunkt 2
        SELECT
            pm.PAYMENTMETHODNAME
        FROM ORDERHEADER oh
        RIGHT JOIN PAYMENTMETHOD pm
            on oh.PAYMENTMETHODKEY = pm.PAYMENTMETHODKEY
        WHERE
            oh.PAYMENTMETHODKEY IS NULL;
    podpunkt 3
        SELECT
            c.COUNTRYNAME
        FROM COUNTRY c
        LEFT JOIN ORDERHEADER oh
            ON c.COUNTRYKEY = oh.COUNTRYKEY
        WHERE
            oh.COUNTRYKEY IS NULL
        ORDER BY c.COUNTRYNAME;
    podpunkt 4
        SELECT
            p.PRODUCTCODE,
            p.PRODUCTNAME
        FROM PRODUCT p
        LEFT JOIN ORDERDETAIL od
            ON p.PRODUCTKEY = od.PRODUCTKEY
        WHERE
            od.PRODUCTKEY IS NULL
        ORDER BY p.PRODUCTCODE;
zad 7.4
    podpunkt 1 //chyba zle
        SELECT DISTINCT
            c.COUNTRYNAME "Country",
            och.CHANNELNAME "Order Channer"
        FROM ORDERCHANNEL och
        INNER JOIN ORDERHEADER oh
            ON och.CHANNELKEY = oh.CHANNELKEY
        RIGHT JOIN COUNTRY c
            ON c.COUNTRYKEY = oh.COUNTRYKEY
        ORDER BY c.COUNTRYNAME;

        // to daje to samo
        SELECT DISTINCT
            c.COUNTRYNAME "Country",
            och.CHANNELNAME "Order Channer"
        FROM COUNTRY c
        LEFT JOIN ORDERHEADER oh
            ON c.COUNTRYKEY = oh.COUNTRYKEY
        INNER JOIN ORDERCHANNEL och
        ON oh.CHANNELKEY = och.CHANNELKEY
        ORDER BY c.COUNTRYNAME;
    podpunkt 2
        SELECT DISTINCT
            och.CHANNELNAME,
            pm.PAYMENTMETHODNAME
        FROM PAYMENTMETHOD pm
        INNER JOIN ORDERHEADER oh
            ON pm.PAYMENTMETHODKEY = oh.PAYMENTMETHODKEY
        RIGHT JOIN ORDERCHANNEL och
            ON oh.CHANNELKEY = och.CHANNELKEY;
    podpunkt 3
        SELECT DISTINCT
            c.CUSTOMERKEY,
            c.LASTNAME,
            c.FIRSTNAME,
            och.CHANNELNAME
        FROM ORDERCHANNEL och
        INNER JOIN ORDERHEADER oh
            on och.CHANNELKEY = oh.CHANNELKEY
        RIGHT JOIN CUSTOMER c
            ON oh.CUSTOMERKEY = c.CUSTOMERKEY
        WHERE
            c.LASTNAME = 'Alan';
zad 7.5
    podpunkt 1
        SELECT DISTINCT
            pm.PAYMENTMETHODNAME,
            dm.DELIVERYMETHODNAME
        FROM ORDERHEADER oh
        FULL OUTER JOIN DELIVERYMETHOD dm
            ON oh.DELIVERYMETHODKEY = dm.DELIVERYMETHODKEY
        FULL OUTER JOIN PAYMENTMETHOD pm
            ON oh.PAYMENTMETHODKEY = pm.PAYMENTMETHODKEY
        WHERE
            oh.ORDERKEY IS NULL
        ORDER by pm.PAYMENTMETHODNAME;
    podpunkt 2
        SELECT DISTINCT
            c.COUNTRYNAME "Country",
            p.PRODUCTNAME "Produkt"
        FROM ORDERHEADER oh
        FULL OUTER JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        FULL OUTER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        FULL OUTER JOIN PRODUCT p 
            on od.PRODUCTKEY = p.PRODUCTKEY
        WHERE
            oh.ORDERKEY IS NULL OR
            od.PRODUCTKEY IS NULL
        ORDER BY 2;
    podpunkt 3
        SELECT DISTINCT
            c.CUSTOMERKEY,
            c.LASTNAME,
            c.FIRSTNAME,
            och.CHANNELNAME
        FROM ORDERHEADER oh
        FULL JOIN ORDERCHANNEL och
            ON oh.CHANNELKEY = och.CHANNELKEY
        FULL JOIN CUSTOMER c
            ON oh.CUSTOMERKEY = c.CUSTOMERKEY
        WHERE
            oh.ORDERKEY IS NULL
        ORDER BY
            och.CHANNELNAME,
            c.LASTNAME;
zad 7.6
    podpunkt 1
        SELECT
            c.COUNTRYNAME,
            och.CHANNELNAME
        FROM COUNTRY c
        CROSS JOIN ORDERCHANNEL och
        WHERE NOT EXISTS(
            SELECT
                1
            FROM ORDERHEADER oh
            INNER JOIN COUNTRY c2
                on oh.COUNTRYKEY = c2.COUNTRYKEY
            INNER JOIN ORDERCHANNEL och2
                ON oh.CHANNELKEY = och.CHANNELKEY
        )
        ORDER BY 1;
    podpunkt 2
    
    podpunkt 3
        SELECT DISTINCT
            c.customerkey "CustomerKey",
            c.lastname "LastName",
            c.firstname "FirstName"
        FROM customer c
        cross join paymentmethod p
        left join orderheader oh 
            on c.customerkey = oh.customerkey
        WHERE
            lower(paymentmethodname) like '%paypal%' and 
            oh.orderkey is null;
    podpunkt 4
zad 7.7
    podpunkt 1 // bes sensu tu union jest wiec go nie uzylem
        SELECT
            c.CUSTOMERKEY,
            c.LASTNAME,
            c.FIRSTNAME
        FROM ORDERHEADER oh
        INNER JOIN CUSTOMER c
            ON oh.CUSTOMERKEY = c.CUSTOMERKEY
        INNER JOIN ORDERCHANNEL och
            ON oh.CHANNELKEY = och.CHANNELKEY
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON od.PRODUCTKEY = p.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTSUBCATEGORYKEY
        INNER JOIN PRODUCTCATEGORY pc
            ON psc.PRODUCTCATEGORYKEY = pc.PRODUCTCATEGORYKEY
        WHERE
            och.CHANNELNAME LIKE 'Mobile application' AND
            pc.PRODUCTCATEGORYNAME LIKE 'Bikes' AND
            EXTRACT(YEAR FROM oh.ORDERDATE) = 2019 AND
            TO_CHAR(oh.ORDERDATE,'Q') IN (1,3)
        ORDER BY
            TO_CHAR(oh.ORDERDATE,'Q'),
            c.LASTNAME;
    podpunkt 2
    
    podpunkt 3
zad 7.8// -skip
    podpunkt 1
    podpunkt 2
    podpunkt 3
    podpunkt 4
    podpunkt 5
zad 7.9// -skip
    podpunkt 1
    podpunkt 2
    podpunkt 3
    podpunkt 4
    podpunkt 5
    podpunkt 6
==================Laboratorium 8
zad 8.1 //glupio to zadanie jest zrobione bo trzeba uzywac tylko GROUP BY co za tym idzie to nie bedzie dzialac Ono jest zrobione tak jakby zeby "przygotowac do nastepnuch funkcji"
    podpunkt 1 // bez sum nie dziala
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "YEAR",
            st.SALESTERRITORYNAME "Region",
            SUM(od.TRANSACTIONPRICE)
        FROM ORDERHEADER oh
        INNER JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        INNER JOIN SALESTERRITORY st
            ON c.SALESTERRITORYKEY = st.SALESTERRITORYKEY
        INNER JOIN ORDERDETAIL od
            ON od.ORDERKEY = oh.ORDERKEY
        GROUP BY
            EXTRACT(YEAR FROM oh.ORDERDATE),
            st.SALESTERRITORYNAME

        ORDER BY
            1 DESC, 2 ASC
    podpunkt 2
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            TO_CHAR(oh.ORDERDATE, 'Month') "Month Name",
            COUNT(ORDERKEY)
        FROM ORDERHEADER oh
        GROUP BY
            "Year","Month Name",TO_CHAR(oh.ORDERDATE, 'MM')
        ORDER BY
            1,TO_CHAR(oh.ORDERDATE, 'MM');
    podpunkt 3
        SELECT
            pc.PRODUCTCATEGORYNAME "Product category",
            och.CHANNELNAME "Order Channel",
            COUNT(c.CUSTOMERKEY)
        FROM ORDERHEADER oh
        INNER JOIN ORDERCHANNEL och
            ON oh.CHANNELKEY = och.CHANNELKEY
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON od.PRODUCTKEY = p.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTCATEGORYKEY
        INNER JOIN PRODUCTCATEGORY pc
            ON psc.PRODUCTCATEGORYKEY = pc.PRODUCTCATEGORYKEY
        INNER JOIN CUSTOMER c
            ON oh.CUSTOMERKEY = c.CUSTOMERKEY
        GROUP BY
            pc.PRODUCTCATEGORYNAME,
            och.CHANNELNAME
        ORDER BY
            1,2;
    podpunkt 4
        SELECT
            st.SALESTERRITORYNAME,
            c.COUNTRYNAME,
            EXTRACT(YEAR FROM oh.ORDERDATE),
            SUM(od.TRANSACTIONPRICE)
        FROM ORDERHEADER oh
        INNER JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        INNER JOIN SALESTERRITORY st
            ON c.SALESTERRITORYKEY = st.SALESTERRITORYKEY
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        GROUP BY
            st.SALESTERRITORYNAME,
            c.COUNTRYNAME,
            EXTRACT(YEAR FROM oh.ORDERDATE)
        ORDER BY
            1,2,3;
    podpunkt 5

    podpunkt 6
zad 8.2 // tu wywala blad w ORDER BY ale dziala
    podpunkt 1
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            EXTRACT(MONTH FROM oh.ORDERDATE) "Month",
            COUNT(oh.ORDERKEY)
        FROM ORDERHEADER oh
        GROUP BY
            EXTRACT(YEAR FROM oh.ORDERDATE),
            ROLLUP(EXTRACT(MONTH FROM oh.ORDERDATE))
        ORDER BY 1,"Month" IS NULL DESC,"Month";
    podpunkt 2
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            pc.PRODUCTCATEGORYNAME "Product Category",
            SUM(od.TRANSACTIONPRICE)
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON od.PRODUCTKEY = p.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTCATEGORYKEY
        INNER JOIN PRODUCTCATEGORY pc
            ON psc.PRODUCTCATEGORYKEY = pc.PRODUCTCATEGORYKEY
        GROUP BY
                    ROLLUP(EXTRACT(YEAR FROM oh.ORDERDATE)),
                    ROLLUP(pc.PRODUCTCATEGORYNAME)
        ORDER BY
            EXTRACT(YEAR FROM oh.ORDERDATE) IS NULL DESC,"Product Category" IS NULL DESC,EXTRACT(YEAR FROM oh.ORDERDATE),"Product Category";
    podpunkt 3

    podpunkt 4 // nie chcialo mi sie tego segregowac
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            och.CHANNELNAME "Channel",
            c.COUNTRYNAME "Country",
            SUM(od.TRANSACTIONPRICE)
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN ORDERCHANNEL och
            ON oh.CHANNELKEY = och.CHANNELKEY
        INNER JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        GROUP BY
            ROLLUP(EXTRACT(YEAR FROM oh.ORDERDATE)),
            CUBE(och.CHANNELNAME),
            CUBE(c.COUNTRYNAME)
        ;
    podpunkt 5

zad 8.3 //trzeba posprawdzac czy sie wyniki nie zmieniaja jak dasz DISTINCT w COUNT
    podpunkt 1
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            COUNT(oh.ORDERKEY)
        FROM ORDERHEADER oh
        GROUP BY
            EXTRACT(YEAR FROM oh.ORDERDATE)
        ORDER BY
            1;
    podpunkt 2

    podpunkt 3 // to zadanie do wywalenia jest bo ta baza nie jest przystosowana do takiego zapytania ona zaklada ze w zamowieniu jest tylko 1 rodzaj produktu
        SELECT
            oh.ORDERKEY,
            COUNT(DISTINCT od.PRODUCTKEY) "#Products"
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        WHERE
            TO_CHAR(oh.ORDERDATE, 'Q') = 1 AND
            EXTRACT(YEAR FROM ORDERDATE) = 2018
        GROUP BY
            oh.ORDERKEY
        ORDER BY
            oh.ORDERKEY;
    podpunkt 4

    podpunkt 5

    podpunkt 6
        SELECT
            st.SALESTERRITORYNAME "Region",
            EXTRACT(YEAR FROM oh.DELIVERYDATE) "Year",
            EXTRACT(MONTH FROM oh.DELIVERYDATE) "Month",
            COUNT(oh.ORDERKEY) "#Orders"
            
        FROM ORDERHEADER oh
        INNER JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        INNER JOIN SALESTERRITORY st
            ON c.SALESTERRITORYKEY = st.SALESTERRITORYKEY
        GROUP BY
            st.SALESTERRITORYNAME,
            EXTRACT(YEAR FROM oh.DELIVERYDATE),
            EXTRACT(MONTH FROM oh.DELIVERYDATE)
        ORDER BY
            1,2,3;


    podpunkt 7

    podpunkt 8
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            pc.PRODUCTCATEGORYNAME "Product Category",
            COUNT(DISTINCT oh.ORDERKEY) "#Orders"
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON od.PRODUCTKEY = p.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTSUBCATEGORYKEY
        INNER JOIN PRODUCTCATEGORY pc
            ON psc.PRODUCTCATEGORYKEY = pc.PRODUCTCATEGORYKEY
        GROUP BY
            EXTRACT(YEAR FROM oh.ORDERDATE),
            pc.PRODUCTCATEGORYNAME
        ORDER BY
            "Year", pc.PRODUCTCATEGORYNAME;
    podpunkt 9

    podpunkt 10
        SELECT
            c.COUNTRYNAME "Country",
            och.CHANNELNAME "Channel",
            COUNT(DISTINCT oh.ORDERKEY) "#Orders"
        FROM ORDERHEADER oh
        RIGHT JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        RIGHT JOIN ORDERCHANNEL och
            ON oh.CHANNELKEY = och.CHANNELKEY
        GROUP BY
            c.COUNTRYNAME,
            och.CHANNELNAME
        ORDER BY
            c.COUNTRYNAME;
    podpunkt 11

    podpunkt 12
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            c.COUNTRYNAME "Country",
            COUNT(DISTINCT oh.CUSTOMERKEY) "#Customers"
        FROM ORDERHEADER oh
        INNER JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        GROUP BY
            c.COUNTRYNAME,
            EXTRACT(YEAR FROM oh.ORDERDATE)
        ORDER BY
            1,2;
    podpunkt 13

    podpunkt 14
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            st.SALESTERRITORYNAME "Region",
            c.COUNTRYNAME "Country",
            COUNT(DISTINCT oh.CUSTOMERKEY) "#Customer"
        FROM ORDERHEADER oh
        INNER JOIN ORDERCHANNEL och
            ON oh.CHANNELKEY = och.CHANNELKEY
        INNER JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        INNER JOIN SALESTERRITORY st
            ON c.SALESTERRITORYKEY = st.SALESTERRITORYKEY
        GROUP BY
            EXTRACT(YEAR FROM oh.ORDERDATE),
            st.SALESTERRITORYNAME,
            c.COUNTRYNAME
        ORDER BY
            EXTRACT(YEAR FROM oh.ORDERDATE),
            "Region",
            "#Customer";
zad 8.4
    podpunkt 1
        SELECT 
            SUM(TransactionPrice * Quantity) AS "Total Orders Value"
        FROM 
            OrderDetail;
    podpunkt 2
    podpunkt 3
        SELECT 
            pc.PRODUCTCATEGORYNAME "Product category",
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            EXTRACT(MONTH FROM oh.ORDERDATE) "Month",
            SUM(od.QUANTITY * od.TRANSACTIONPRICE) "Total Value"
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON od.PRODUCTKEY = p.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTSUBCATEGORYKEY
        INNER JOIN PRODUCTCATEGORY pc
            ON psc.PRODUCTCATEGORYKEY = pc.PRODUCTCATEGORYKEY
        GROUP BY
            pc.PRODUCTCATEGORYNAME,
            EXTRACT(YEAR FROM oh.ORDERDATE),
            EXTRACT(MONTH FROM oh.ORDERDATE)
        ORDER BY
            1,2,3;
    podpunkt 4
    podpunkt 5
        SELECT 
            pc.PRODUCTCATEGORYNAME "Product category",
            TO_CHAR(oh.ORDERDATE, 'Month') "Month",
            SUM(od.QUANTITY * od.TRANSACTIONPRICE) "Total Value"
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON od.PRODUCTKEY = p.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTSUBCATEGORYKEY
        INNER JOIN PRODUCTCATEGORY pc
            ON psc.PRODUCTCATEGORYKEY = pc.PRODUCTCATEGORYKEY
        INNER JOIN ORDERCHANNEL och
            ON oh.CHANNELKEY = och.CHANNELKEY
        WHERE
            EXTRACT(YEAR FROM oh.ORDERDATE) = 2019 AND
            och.CHANNELNAME = 'Mobile application'
        GROUP BY
            pc.PRODUCTCATEGORYNAME,
            TO_CHAR(oh.ORDERDATE, 'Month')
        ORDER BY
            "Total Value";
    podpunkt 6
    podpunkt 7
        SELECT
            c.COUNTRYNAME "Country",
            och.CHANNELNAME "Channel",
            SUM(od.QUANTITY * od.TRANSACTIONPRICE) "Total Value"
        FROM ORDERHEADER oh
        RIGHT JOIN ORDERCHANNEL och
            ON oh.CHANNELKEY = och.CHANNELKEY
        INNER JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        GROUP BY
            c.COUNTRYNAME,
            och.CHANNELNAME
        ORDER BY
            "Country" ASC, COUNT(od.QUANTITY * od.TRANSACTIONPRICE) DESC;
    podpunkt 8
    podpunkt 9
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            c.COUNTRYNAME "Country",
            och.CHANNELNAME "Channel",
            p.PRODUCTNAME "Product",
            SUM(od.QUANTITY) "#Items"
        FROM ORDERHEADER oh
        RIGHT JOIN ORDERCHANNEL och
            ON oh.CHANNELKEY = och.CHANNELKEY
        INNER JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON od.PRODUCTKEY = p.PRODUCTKEY
        GROUP BY
            EXTRACT(YEAR FROM oh.ORDERDATE),
            c.COUNTRYNAME,
            och.CHANNELNAME,
            p.PRODUCTNAME
        ORDER BY
            "Year","Country","Channel",SUM(od.QUANTITY);
zad 8.5
    podpunkt 1
        SELECT 
            EXTRACT(YEAR FROM OH.OrderDate) "Year",
            C.CountryName "Country",
            ROUND(AVG(OH.DeliveryDate - OH.OrderDate)) "#Avg Days"
        FROM OrderHeader OH
        INNER JOIN Country C 
            ON OH.CountryKey = C.CountryKey
        GROUP BY 
            EXTRACT(YEAR FROM OH.OrderDate), C.CountryName
        ORDER BY 
            "Year", "Country";
    podpunkt 2
    podpunkt 3
        SELECT 
            psc.PRODUCTSUBCATEGORYNAME "Product",
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            ROUND(AVG(od.DISCOUNTAMOUNT),2) "Avg Discount"
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON od.PRODUCTKEY = p.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTSUBCATEGORYKEY
        INNER JOIN PRODUCTCATEGORY pc
            ON pc.PRODUCTCATEGORYKEY = psc.PRODUCTCATEGORYKEY
        WHERE
            pc.PRODUCTCATEGORYNAME = 'Bikes'
        GROUP BY
            psc.PRODUCTSUBCATEGORYNAME,
            EXTRACT(YEAR FROM oh.ORDERDATE)
        ORDER BY
            "Product",
            "Year";
    podpunkt 4
    podpunkt 5
        SELECT 
            psc.PRODUCTSUBCATEGORYNAME "Product",
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            CONCAT('Q',TO_CHAR(oh.ORDERDATE, 'Q')) "Quarter",
            ROUND(AVG(od.TRANSACTIONPRICE),2) "Avg Trans Price"
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON od.PRODUCTKEY = p.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTSUBCATEGORYKEY
        WHERE
            psc.PRODUCTSUBCATEGORYNAME LIKE '%Bikes%'
        GROUP BY
            psc.PRODUCTSUBCATEGORYNAME,
            EXTRACT(YEAR FROM oh.ORDERDATE),
            CONCAT('Q',TO_CHAR(oh.ORDERDATE, 'Q'))
        ORDER BY
            "Year","Quarter";
zad 8.6
    podpunkt 1
        SELECT
            c.COUNTRYNAME "Country",
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            TO_CHAR(MIN(oh.ORDERDATE), 'DD-MM-RRRR') "First Order Date"
        FROM ORDERHEADER oh
        INNER JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        GROUP BY
            c.COUNTRYNAME,
            "Year"
        ORDER BY
            "Country",
            "Year" DESC;
    podpunkt 2
    podpunkt 3
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            MAX(od.DISCOUNTAMOUNT) "Max Discount Amount"
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON od.PRODUCTKEY = p.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTCATEGORYKEY
        INNER JOIN PRODUCTCATEGORY pc
            ON psc.PRODUCTCATEGORYKEY = pc.PRODUCTCATEGORYKEY
        WHERE
            pc.PRODUCTCATEGORYNAME = 'Bikes'
        GROUP BY
            "Year"
        ORDER BY
            "Year";
    podpunkt 4
    podpunkt 5
        SELECT
            EXTRACT(YEAR FROM oh.ORDERDATE) "Year",
            c.COUNTRYNAME "Country",
            MAX(od.TRANSACTIONPRICE) "Max Trans Price"
        FROM ORDERHEADER oh
        INNER JOIN ORDERDETAIL od
            ON oh.ORDERKEY = od.ORDERKEY
        INNER JOIN PRODUCT p
            ON od.PRODUCTKEY = p.PRODUCTKEY
        INNER JOIN PRODUCTSUBCATEGORY psc
            ON p.PRODUCTSUBCATEGORYKEY = psc.PRODUCTCATEGORYKEY
        INNER JOIN PRODUCTCATEGORY pc
            ON psc.PRODUCTCATEGORYKEY = pc.PRODUCTCATEGORYKEY
        INNER JOIN COUNTRY c
            ON oh.COUNTRYKEY = c.COUNTRYKEY
        WHERE
            pc.PRODUCTCATEGORYNAME = 'Bikes'
        GROUP BY
            ROLLUP("Year"),
            ROLLUP("Country")
        ORDER BY
            "Year",
            "Country";

zad 8.7
    podpunkt 1
        select 
            extract(year from orderdate) Year, 
            ProductSubcategoryName "subcategory", 
            count(distinct customerkey) "#Customers", 
            sum(transactionprice*quantity) "Total Value"
        from orderheader oh 
        inner join orderdetail od 
            on oh.orderkey=od.orderkey
        inner join product 
            on product.productkey=od.productkey
        inner join productsubcategory psc 
            on psc.productsubcategorykey=product.productsubcategorykey
        inner join productcategory pc 
            on pc.productcategorykey=psc.productcategorykey
        where productcategoryname='Bikes'
        group by 
            year, 
            "subcategory"
        order by 
            1, 2;
    podpunkt 2
    podpunkt 3
        select
            to_char(orderdate, 'yyyy') "Year",
            c.customerkey "Customer ID",
            lastname || ' ' || firstname "Customer Name",
            count(DISTINCT oh.ORDERKEY) "#Orders",
            sum(transactionprice * quantity) "Total Value"
        from orderheader oh
        inner join customer c 
            on c.customerkey = oh.customerkey
        inner join orderdetail od 
            on oh.orderkey = od.orderkey
        group by 
            to_char(orderdate, 'yyyy'), 
            c.customerkey, lastname || ' ' || firstname
        order by 1 desc, 5 desc;
    podpunkt 4
    podpunkt 5
        select
            extract(YEAR from orderdate) "Year",
            extract(MONTH from orderdate) "Month",
            salesterritoryname "Region",
            count(distinct customerkey) "#Customers",
            sum(quantity * transactionprice) "Total Value"
        from orderheader oh
        inner join orderdetail od 
            on oh.orderkey = od.orderkey
        inner join country c 
            on c.countrykey = oh.countrykey
        inner join salesterritory st 
            on st.salesterritorykey = c.SALESTERRITORYKEY 
        GROUP by extract(year from orderdate), rollup(extract(month from orderdate), salesterritoryname)
        order by 1, 2, 3;
    podpunkt 6
zad 8.8
    podpunkt 1
    podpunkt 2
    podpunkt 3
    podpunkt 4
    podpunkt 5
    podpunkt 6
    podpunkt 7
    podpunkt 8
    podpunkt 9
    podpunkt 10
    podpunkt 11

//=============Laboratorium 9
zad 9.1
    podpunkt 1
    podpunkt 2
    podpunkt 3
    podpunkt 4
    podpunkt 5
    podpunkt 6
    podpunkt 7
    podpunkt 8
    podpunkt 9
    podpunkt 10
zad 9.2
    podpunkt 1
    podpunkt 2
    podpunkt 3
    podpunkt 4
    podpunkt 5
zad 9.3
    podpunkt 1
    podpunkt 2
    podpunkt 3
    podpunkt 4
    podpunkt 5
    podpunkt 6
    podpunkt 7
    podpunkt 8
    podpunkt 9
zad 9.4
    podpunkt 1
    podpunkt 2
    podpunkt 3
    podpunkt 4
    podpunkt 5
    podpunkt 6
    podpunkt 7
    podpunkt 8
    podpunkt 9
    podpunkt 10
    podpunkt 11
    podpunkt 12
    podpunkt 13
