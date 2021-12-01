# SQL-Project-P2

# CREATION OF DATABASE (AND SCHEMA AS "public")

CREATE DATABASE Project;


# BEFORE EDITING TABLES YOU MUST ACCOUNT FOR DISORGANIZED .CSV FILES!!!


# CREATION OF TABLES

CREATE TABLE Albums (
	AlbumID SERIAL NOT NULL,
    Title VARCHAR(160) NOT NULL,
    ArtistID INTEGER  NOT NULL,    
    PRIMARY KEY (AlbumID));
    
CREATE TABLE Employee (
	EmployeeID SERIAL NOT NULL , 
    FirstName VARCHAR(30) NOT NULL, 
    LastName VARCHAR(30) NOT NULL, 
    SSN VARCHAR(11), 
    Title VARCHAR(40), 
    ReportsToID int, 
    Store VARCHAR(30), 
    BirthDate date, 
    HireDate date, 
    Address VARCHAR(30), 
    City VARCHAR(30),
    State VARCHAR(5), 
    Country VARCHAR(40),
    Zipcode VARCHAR(15), 
    Email VARCHAR(60),
    Phone VARCHAR(20),
    PRIMARY KEY (EmployeeID));
  
CREATE TABLE Invoices (
	InvoiceID SERIAL NOT NULL , 
    CustomerID INT NOT NULL, 
    InvoiceDate TIMESTAMP NOT NULL, 
    BillingAddress VARCHAR(70), 
    BillingCity VARCHAR(40), 
    BillingState VARCHAR(40), 
    BillingCountry VARCHAR(40), 
    BillingPostalCode VARCHAR(10), 
    Total numeric(10,2), 
    PRIMARY KEY (InvoiceID));
  
CREATE TABLE Playlists (
	PlaylistID SERIAL NOT NULL , 
    Name VARCHAR(120),
    PRIMARY KEY (PlaylistID));
  
CREATE TABLE Artists (
	ArtistID SERIAL NOT NULL , 
    Name VARCHAR(120),
    PRIMARY KEY (ArtistID));
  
CREATE TABLE Genres (
	GenreID SERIAL NOT NULL , 
    Name VARCHAR(120),
    PRIMARY KEY (GenreID));
  
CREATE TABLE Media_Types (
	MediaTypeID SERIAL NOT NULL , 
    Name VARCHAR(120),
    PRIMARY KEY (MediaTypeID));
  
CREATE TABLE Tracks (
	TrackID SERIAL NOT NULL , 
    Name VARCHAR(200) NOT NULL, 
    AlbumID int, 
    MediaTypeID int NOT NULL,
    GenreID int,
    Composer VARCHAR(220), 
    Milliseconds int NOT NULL,
    Bytes int,
    UnitPrice numeric(10,2),
    PRIMARY KEY (TrackID));
  
CREATE TABLE Customers (
	CustomerID SERIAL NOT NULL , 
    FirstName VARCHAR(30), 
    LastName VARCHAR(30), 
    Company VARCHAR(80),
	Address VARCHAR(70),
	City VARCHAR(30),
    State VARCHAR(6),
    Country VARCHAR(40),
    Zipcode VARCHAR(6),
    Phone VARCHAR(13),
	SecPhone VARCHAR(13),
	Email VARCHAR(60), 
	SupportRepID int, 
    PRIMARY KEY(CustomerID));

CREATE TABLE Invoice_Items (
	InvoiceLineID SERIAL NOT NULL , 
    InvoiceID int NOT NULL, 
    TrackID int NOT NULL, 
    UnitPrice numeric(10,2) NOT NULL,
    Quantity int NOT NULL,
    PRIMARY KEY (InvoiceLineID));

CREATE TABLE Playlist_Track (
	PlaylistID SERIAL NOT NULL , 
    TrackID int NOT NULL, 
    PRIMARY KEY (PlaylistID, TrackID));





# CREATION OF SEQUENCES FOR SERIALIZED INTEGERS (AUTO-INCREMENT ID'S IN POSTGRESQL)


CREATE SEQUENCE album_sequence
	START 348
	INCREMENT 1;

CREATE SEQUENCE artists_sequence
	START 276
	INCREMENT 1;

CREATE SEQUENCE customer_sequence
	START 60
	INCREMENT 1;

CREATE SEQUENCE employee_sequence
	START 9
	INCREMENT 1;

CREATE SEQUENCE genre_sequence
	START 25
	INCREMENT 1;

CREATE SEQUENCE invoice_item_sequence
	START 2241
	INCREMENT 1;

CREATE SEQUENCE invoice_sequence
	START 413
	INCREMENT 1;

CREATE SEQUENCE media_type_sequence
	START 6
	INCREMENT 1;

CREATE SEQUENCE playlist_sequence
	START 19
	INCREMENT 1;

CREATE SEQUENCE playlist_track_sequence
	START 8716
	INCREMENT 1;

CREATE SEQUENCE track_sequence
	START 3504
	INCREMENT 1;

CREATE SEQUENCE digital_media_artistid_seq
	START 1
	INCREMENT 1;





# 1)

INSERT INTO Employee (employeeID, firstname, lastname, ssn, title, reportstoid, birthdate, hiredate, address, city, state, country, zipcode, phone, email) 
VALUES (nextval('employee_sequence'), 'Jason', 'Johnson', 133-59-2011, 'Director of Technology', 
	(SELECT employeeid FROM employee WHERE firstname = 'Mitchell' AND lastname = 'Michael'),
	'10-25-1967', '11-10-21' , '15554 Hardee St', 'Oklahoma City', 'OK', 'USA', 73107, 991-451-4458, 'jjohnson@hotmail.com');																						

INSERT INTO Employee (employeeID, firstname, lastname, ssn, title, reportstoid, birthdate, hiredate, address, city, state, country, zipcode, phone, email) 
VALUES (nextval('employee_sequence'), 'Karen', 'Tiffany', 996-56-2554, 'Sales Council', 
	(SELECT employeeid FROM employee WHERE firstname = 'Edwards' AND lastname = 'Nancy'),
	'11-5-1972' , '11-15-21' , '123 S Main', 'Omaha', 'NE', 'USA', 68114, 441-758-6985, 'karen.tiff72@gmail.com');																					
	
INSERT INTO Artist (artistid, name) VALUES (nextval('artist_sequence'), 'Twenty-One Pilots'));

INSERT INTO Artist (artistid, name) VALUES (nextval('artist_sequence'), 'Adele'));

INSERT INTO Genre (genreid, name) VALUES (nextval('genre_sequence'), 'Country'));

INSERT INTO Tracks (trackid, name, mediatypeid, composer, milliseconds, bytes, unitprice)
VALUES (nextval('track_sequence), 'Ride', 2, 'Twenty-One Pilots', 214000, 9254879, 1.99);

INSERT INTO Tracks (trackid, name, mediatypeid, composer, milliseconds, bytes, unitprice)
VALUES (nextval('track_sequence), 'Stressed Out', 2, 'Twenty-One Pilots', 202000, 9458963, 1.99);

INSERT INTO Tracks (trackid, name, mediatypeid, composer, milliseconds, bytes, unitprice)
VALUES (nextval('track_sequence), 'Hello', 2, 'Ed Sheeran', 299000, 16658421, 2.99);

INSERT INTO Tracks (trackid, name, mediatypeid, composer, milliseconds, bytes, unitprice)
VALUES (nextval('track_sequence), 'Send My Love', 2, 'Ed Sheeran', 223000, 6658421, 1.99);





# 2)

ALTER TABLE albums
ADD CONSTRAINT fk_artistid
FOREIGN KEY (artistid)
REFERENCES artists (artistid);

ALTER TABLE customers
ADD CONSTRAINT fk_employeeid
FOREIGN KEY (supportrepid)
REFERENCES employee (employeeid);

ALTER TABLE invoice_items
ADD CONSTRAINT fk_trackid
FOREIGN KEY (trackid)
REFERENCES tracks (trackid);

ALTER TABLE invoices
ADD CONSTRAINT fk_customerid
FOREIGN KEY (customerid)
REFERENCES customers (customerid);

ALTER TABLE playlist_track
ADD CONSTRAINT fk_trackid
FOREIGN KEY (trackid)
REFERENCES tracks (trackid);

ALTER TABLE playlist_track
ADD CONSTRAINT fk_playlistid
FOREIGN KEY (playlistid)
REFERENCES playlists (playlistid);

ALTER TABLE tracks
ADD CONSTRAINT fk_albumid
FOREIGN KEY (albumid)
REFERENCES albums (albumid);

ALTER TABLE tracks
ADD CONSTRAINT fk_genreid
FOREIGN KEY (genreid)
REFERENCES genres (genreid);

ALTER TABLE tracks
ADD CONSTRAINT fk_mediatype
FOREIGN KEY (mediatype)
REFERENCES media_types (mediatype);





# 3)

CREATE TABLE Digital_Media (
	digimid INT NOT NULL,
artistid SERIAL NOT NULL,
release_date TIMESTAMP,
downloaded INT,
song_length INT NOT NULL,
file_size INT NOT NULL,
    PRIMARY KEY (digimid));


ALTER TABLE digital_media
ADD CONSTRAINT fk_digital_media_artistid
FOREIGN KEY (artistid)
REFERENCES artists (artistid);

ALTER TABLE digital_media
ADD CONSTRAINT fk_digital_media_digimid
FOREIGN KEY (digimid)
REFERENCES tracks (trackid);





# 4) PROBABLY AN EASIER WAY TO DO THIS THAN FULL QUERY

ALTER TABLE employee
DROP COLUMN ssn;

SELECT invoiceid FROM invoices
INNER JOIN customers
ON customers.customerid = invoices.customerid
WHERE customers.firstname = 'Mark' AND customers.lastname = 'Taylor';

DELETE FROM invoices
WHERE invoicesid IN ('21', '44', '66', '118', '239', '250', '305');





# 5)

ALTER TABLE invoices
ALTER COLUMN billingpostalcode VARCHAR(10);

ALTER TABLE customers
ALTER COLUMN state VARCHAR (6);




# OPTIONAL SQL (customers phone lengths too long for db constraints in PostgreSQL)


# OPTION 1: ALTER CUSTOMERS ORIGINAL .CSV, THEN SQL, THEN CREATE A NEW TABLE FOR MULTIVALUED ATTRIBUTES (PHONE NUMBERS)

CREATE TABLE Customers (
	CustomerID SERIAL NOT NULL , 
    FirstName VARCHAR(30), 
    LastName VARCHAR(30), 
    Company VARCHAR(80),
	Address VARCHAR(70),
	City VARCHAR(30),
    State VARCHAR(6),
    Country VARCHAR(40),
    Zipcode VARCHAR(6),
	Email VARCHAR(60), 
	SupportRepID int, 
    PRIMARY KEY(CustomerID));

CREATE TABLE cust_phone (
	customerid SERIAL NOT NULL,
phone VARCHAR (19) NOT NULL UNIQUE,
CONSTRAINT fk_customerid
	FOREIGN KEY (customerid)
		REFERENCES customers (customerid));



/* OPTION 2: ALTER CUSTOMERS ORIGINAL SQL BY ADDING EXTRA SECONDARY PHONE NUMBER COLUMN (AS SHOWN ABOVE),
THEN ALTER PHONE VARCHAR LENGTHS TO HANDLE INTERNATIONAL NUMBERS */

CREATE TABLE Customers (
	CustomerID SERIAL NOT NULL , 
    FirstName VARCHAR(30), 
    LastName VARCHAR(30), 
    Company VARCHAR(80),
	Address VARCHAR(70),
	City VARCHAR(30),
    State VARCHAR(6),
    Country VARCHAR(40),
    Zipcode VARCHAR(6),
    Phone VARCHAR(13),
	SecPhone VARCHAR(13),
	Email VARCHAR(60), 
	SupportRepID int, 
    PRIMARY KEY(CustomerID));

ALTER TABLE customers
ALTER COLUMN phone VARCHAR (19);

ALTER TABLE customers
ALTER COLUMN secphone VARCHAR (19);
