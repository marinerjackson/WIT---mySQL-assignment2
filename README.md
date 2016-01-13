# WIT---mySQL-assignment2
mySQL - assignment 2

SELECT QUERIES:


1) List all clients names, addresses and phone numbers who are not in the USA.
---------------------------------------------------------------------------

SELECT ClientName, ClientAddress, ClientPhone
FROM client
WHERE ClientAddress NOT LIKE '%USA';


2) List all clients names and addresses in England. Order in ascending order.
--------------------------------------------------------------------------

SELECT ClientName, ClientAddress
FROM client
WHERE ClientAddress LIKE '%England'
ORDER BY ClientName ASC;


3) List all clients names and email addresses in ascending order. Name as 'Client Name' and 'Email Address'
--------------------------------------------------------------------------------------------------------

SELECT ClientName AS 'Client Name', ClientEmail AS 'Email Address'
FROM client
ORDER BY ClientName ASC;


4) List all sites by address and end date.
---------------------------------------

SELECT SiteAddress, EndDate
FROM Site
ORDER BY EndDate;


5) List all sites whose end date was before 31/12/2015.
----------------------------------------------------

SELECT S_ClientID, SiteAddress, EndDate
FROM Site
WHERE EndDate<'2015-12-31';


6) List all sites whose start date was between 01/07/2015 and 31/12/2015.
----------------------------------------------------------------------

SELECT S_ClientID, SiteAddress, StartDate
FROM Site
WHERE StartDate BETWEEN '2015-07-01' AND '2015-12-31';


7) List all clients who are companies.
-----------------------------------

SELECT ClientName, CompanyName
FROM client
WHERE CompanyName IS NOT NULL;


8) List all building types that are 2 storey or greater.
-----------------------------------------------------

SELECT BuildingName, BuildingType, BuildingHeight
FROM BUILDING
WHERE BUILDINGHEIGHT > 4;



JOINS/SUB QUERY STATEMENTS:


1) Join Country and Branch tables and display all Branch names and Countries they are in.
--------------------------------------------------------------------------------------

select branch.BranchName, country.COUNTRYName
FROM ecosave_construction.branch
INNER JOIN ecosave_construction.country
ON branch.B_CountryID=country.COUNTRYID;


2) Join Site, Roof and Client tables and display all clients with pitched roof.
----------------------------------------------------------------------------

SELECT clientname
FROM ecosave_construction.client
JOIN ecosave_construction.site ON S_ClientID=ClientID
Join ecosave_construction.roof ON S_roofID=RoofID
WHERE RoofType='pitched';


3) List all Client Names with sites in Ireland.
--------------------------------------------

SELECT clientname, clientemail, ClientPhone
from client
where clientid IN	(select S_clientid
                     	from site	
                     	where siteaddress like '%IRELAND'
                     	);


4) List Client Names and Sites in Dublin that have Two Storey's.
-------------------------------------------------------------

SELECT ClientName, ClientAddress, Clientemail
FROM Client
WHERE Clientid In	(SELECT S_clientid
			FROM site
			WHERE siteaddress like '%Dublin%' and S_BuildingID In		(SELECT buildingid
											FROM building
											WHERE BuildingType='Two Storey'
											));

VIEWS TO QUERY DATA:
 

1) Create view to view all clients and address in England:
--------------------------------------------------------

CREATE VIEW VUKCLIENTS
AS
SELECT ClientName, ClientAddress
FROM client
WHERE ClientAddress LIKE '%England'
ORDER BY ClientName ASC;


2) Create view to show all Two Story Sites in Dublin:
--------------------------------------------------
CREATE VIEW VTSDUBSITES
AS
SELECT clientname, SiteAddress
FROM ecosave_construction.client
JOIN ecosave_construction.site ON S_ClientID=ClientID
Join ecosave_construction.building ON S_BuildingID=BuildingID
WHERE BuildingType='Two Storey' and SiteAddress LIKE '%Dublin%';


3) Create view to show the total number of clients by branch (include all branches):
---------------------------------------------------------------------------------
CREATE VIEW VCLIENTSBRANCH
AS
SELECT branchname, count(DISTINCT clientname) as 'Client No by Branch'
FROM ecosave_construction.client
RIGHT JOIN ecosave_construction.site ON S_ClientID=ClientID
RIGHT Join ecosave_construction.branch ON S_BranchID=BranchID
Group by branchname;


MySQL COMMANDS:


Alter Table:
------------

ALTER TABLE Building
ADD COLUMN BuildingSqFoot INT(5) NOT NULL
AFTER BuildingHeight;


Delete records from table:
--------------------------

DELETE from Branch
WHERE BranchName='Madrid';


Update date in table:
---------------------

UPDATE Client
SET ClientEmail='peterhill@gmail.com'
WHERE ClientID=603;


Drop Column:
------------

ALTER TABLE Branch
DROP COLUMN BranchEmail;


Write the code that would allow you to set up 2 users and manager on your database:
-----------------------------------------------------------------------------------

CREATE USER johnsmith@localhost
IDENTIFIED BY 'welcome';


CREATE USER lucydavies@localhost
IDENTIFIED BY 'monday';


CREATE USER manager@'%'
IDENTIFIED BY 'password';


Grant the manager all privileges to all parts of the database:
--------------------------------------------------------------

GRANT ALL ON ecosave_construction.*
TO manager@'%';


Grant the users select, update and insert privileges but not delete on certain tables or views: 
-----------------------------------------------------------------------------------------------

GRANT SELECT, INSERT, UPDATE
ON ecosave_construction.building
TO johnsmith@localhost;

GRANT SELECT, INSERT, UPDATE
ON ecosave_construction.client
TO johnsmith@localhost;


GRANT SELECT, INSERT, UPDATE
ON ecosave_construction.VTSDUBSITES
TO lucydavies@localhost;

GRANT SELECT, INSERT, UPDATE
ON ecosave_construction.client
TO lucydavies@localhost;

GRANT SELECT, INSERT, UPDATE
ON ecosave_construction.site
TO lucydavies@localhost;













