# project_manage_database
The program is about ships and they have dates
start code:
------
--sql 
drop table captain cascade constraint;
drop table ship cascade constraints;
drop table BoardingPass cascade constraints;
drop table voyage cascade constraints;
drop table Passenger cascade constraint; 

-- Tables 
create table captain 
(captain_id number(10) primary key,
 name varchar2(10),
 license number(10),
 phone number(10));
 
 
create table ship(
ship_id char(6)PRIMARY key ,
model varchar2(10),
manufacturer varchar2 (30),
captain_id number(5),
constraint ship_fk foreign key(captain_id) REFERENCES captain(captain_id));

CREATE TABLE voyage(
V_ID NUMBER(5)  PRIMARY KEY,
departure_date DATE,
departure_time TIMESTAMP,
arrival_date DATE,
arrival_time TIMESTAMP,
arrival_from VARCHAR2(12),
arriavl_to VARCHAR2(12),
captain_id NUMBER(5),
ship_id char(6),
CONSTRAINT fk_voyage_id FOREIGN KEY (captain_id) REFERENCES captain (captain_id),
CONSTRAINT fk_ship_id FOREIGN KEY (ship_id) REFERENCES ship (ship_id));


CREATE TABLE Passenger (
 passenger_id number (10) Primary key , 
 Fname varchar2 (20) , 
 Lname varchar2  (20) , 
 email varchar (40) , 
 phone number (10) , 
 DOB varchar2 (15) );
 
 CREATE TABLE BoardingPass (
  ID number (10),
  hascheckedin VARCHAR(25),
  hasboarded VARCHAR(25),
  no_baggage number (4),
  passenger_id number (10),
  voyage_id number (5),
  CONSTRAINT fk_passenger_id1 FOREIGN KEY (passenger_id) REFERENCES Passenger (passenger_id),
  CONSTRAINT fk_voyage_id1 FOREIGN KEY (voyage_id) REFERENCES voyage (v_id));



--(a)

 CREATE USER user_group3 IDENTIFIED BY 12341234;
GRANT CREATE TABLE, CREATE TRIGGER, CREATE PROCEDURE, CREATE FUNCTION, ALTER USER TO user_group3;

CREATE USER Client IDENTIFIED BY 43214321;
GRANT CREATE TABLE, ALTER USER TO Client

--(b)
 CREATE USER user_group3 IDENTIFIED BY 12341234;
GRANT CREATE TABLE, CREATE TRIGGER, CREATE PROCEDURE, CREATE FUNCTION, ALTER USER TO user_group3;

CREATE USER Client IDENTIFIED BY 43214321;
GRANT CREATE TABLE, ALTER USER TO Client;

ALTER USER Client IDENTIFIED BY new1234;


create tablespace project_table datafile 'c:\users\default.dbf' size 100m;
create TABLESPACE project_Index datafile'c:\users\index.dbf' size 100m;
Grant unlimited tablespace to user_group3;
alter user client default tablespace  project_table;


--(f)

ALTER USER Client IDENTIFIED BY new1234;

--(e)

Create index Passenger_NDX1
On Passenger( Passenger ID) 
Tablespace Project_INDX;
Create index captain_NDX1
On captain(name) 
tablespace Project_INDX;

--(g)
GRANT select,insert on captain  to Client;

delete from captain  where captain_id=110305;

insert into captain  values(178901,'maha',11025891,0598731764);






--insert taples

 INSERT INTO Captain VALUES(110305,'noura',11204598,0556698347); 
INSERT INTO Captain VALUES(110786,'sara',113007634,0593599985); 
INSERT INTO Captain VALUES(112407,'reem',112816300,0553488246); 
 INSERT INTO Captain VALUES(113345,'mohammad',11334567,0555599487);  
INSERT INTO Captain VALUES(114456,'ahmed',11556789,0573663647);  
INSERT INTO Captain VALUES(115567,'fahad',11457890,0583653663);


INSERT INTO ship VALUES(433993,'Adventure','Royal caribbean cruises',110305);
INSERT INTO ship VALUES(115566,'Harmoney','OASIS-class cruise',110786);
INSERT INTO ship VALUES(964443,'ICON','MEYER turku',112407);
INSERT INTO ship VALUES(223443,'JEWEL','Meyer worft of germany',113345);
INSERT INTO ship VALUES(998883,'STX europe turku ship','MEYER turku ',114456);
INSERT INTO ship VALUES(220003,'VOYAGER','REGENT seven seas cruises',115567);


insert into voyage VALUES(11234,TO_DATE('2023-06-12', 'YYYY-MM-DD'),'18:24:23',TO_DATE('2023-06-30', 'YYYY-MM-DD'),'20:24:23','Riyadh','MOROCCO',1234567890,'12345A');
insert into voyage VALUES(11235,TO_DATE('2023-07-10', 'YYYY-MM-DD'),'18:24:23',TO_DATE('2023-07-20', 'YYYY-MM-DD'),'20:24:23','Riyadh','ITALY',1234567891,'12345B');
insert into voyage VALUES(11236,TO_DATE('2023-07-20', 'YYYY-MM-DD'),'18:24:23',TO_DATE('2023-07-30', 'YYYY-MM-DD'),'20:24:23','Riyadh','PARIS',1234567892,'123458C');
insert into voyage VALUES(11237,TO_DATE('2023-08-01', 'YYYY-MM-DD'),'18:24:23',TO_DATE('2023-08-10', 'YYYY-MM-DD'),'20:24:23','Riyadh','JEDDAH',1234567893,'12345D');
insert into voyage VALUES(11238,TO_DATE('2023-08-10', 'YYYY-MM-DD'),'18:24:23',TO_DATE('2023-08-20', 'YYYY-MM-DD'),'20:24:23','Riyadh','BAHRAIN',1234567894,'12345E');
insert into voyage VALUES(11239,TO_DATE('2023-08-20', 'YYYY-MM-DD'),'18:24:23',TO_DATE('2023-08-40', 'YYYY-MM-DD'),'20:24:23','Riyadh','LEBANON',1234567895,'12345F');

insert into Passenger values ( 1122334455 , 'anfal' , 'saleh' , 'anf_1@hotmail.com', 0553232065, '11/11/2000');
insert into Passenger values (1234567126 , ' lama' , 'omar ' , 'lam2@hotmail.com ' , 0555432896 , '20/1/1995');
insert into Passenger values (1156439807 , 'sara' , ' zaid ' , 'seo@gmail.com', 0551295288 , '28/8/1990');
insert into Passenger values (1199885577, ' roaa ','mohamed' , 'roaa_de@hotmail.com' , 0553246054 , '15/4/1989');
insert into Passenger values (1928373645 , 'ghalal' , 'yasser ' ,' gh_05@hotmail.com' ,0545192851, '12/12/1999');
insert into Passenger values (116642853, 'alaa' , ' naif ', 'alaa_389@gmail.com' , 0543232065, '23/7/1988');


--Raghad samran (trigger and function)

SET serveroutput ON;
CREATE OR REPLACE FUNCTION totalPass

RETURN number IS
 total number(5):=0;
 BEGIN 
 
   SELECT count(*) into total 
   FROM Passenger; 
    RETURN total; 
END; 
/ 

DECLARE 
   p number(5); 
BEGIN 
   p := totalPass(); 
   dbms_output.put_line('Total number of  Passengers:' || p); 
END; 
/

SET serveroutput ON;

CREATE OR REPLACE TRIGGER baggage
BEFORE
INSERT on BoardingPass
FOR EACH ROW 
BEGIN
if :NEW.no_baggage>3 THEN
raise_application_error(-20001, 'number of baggage should not be more than 3'); 
     END IF; 
END;
/    

insert into BoardingPass (no_baggage) values(5);

insert into BoardingPass (no_baggage) values(2);


 --afnan alghamdi (TRIGGER and function) 
 
--(trigger)give any ship pressenger at most 50
CREATE OR REPLACE TRIGGER check_passenger_count
BEFORE
INSERT ON BoardingPass
FOR EACH ROW
DECLARE
  passenger_count NUMBER;
BEGIN
  SELECT COUNT(*) INTO passenger_count
  FROM BoardingPass
  WHERE voyage_id = :NEW.voyage_id;
 
  IF passenger_count >= 50 THEN
    RAISE_APPLICATION_ERROR(-20001, 'Cannot insert more than 50 passengers for this voyage.');
  END IF;
END;
/

 CREATE OR REPLACE FUNCTION get_max_arrival_date( p_ship_id IN char)
RETURN DATE
IS
    g_max_arrival_date DATE;
BEGIN
    SELECT MAX(arrival_date)
    INTO v_max_arrival_date
    FROM voyage
    WHERE ship_id = p_ship_id;

    RETURN g_max_arrival_date;
END;
/

DECLARE
BEGIN
    DBMS_OUTPUT.PUT_LINE('Max Arrival Date for Ship '  || get_max_arrival_date('433993'));
END;
/


--Layan (trigger and function)

CREATE OR REPLACE TRIGGER check_passenger_age
BEFORE INSERT OR UPDATE ON Passenger
FOR EACH ROW
DECLARE
    v_age NUMBER;
BEGIN
    -- Calculate the age based on the DOB
    v_age := trunc((SYSDATE-:NEW.DOB) / 365);

    IF v_age < 18 THEN
        RAISE_APPLICATION_ERROR(-20001, 'Passenger is under 18 years old. He/she needs an accompanying person.');
    END IF;
END;
/

 INSERT INTO Passenger (passenger_ID, fname, lname, email, phone, DOB)
    VALUES (1, 'John', 'Doe', 'john@example.com', 1234567890, TO_DATE('2008-05-19', 'YYYY-MM-DD'));
 UPDATE Passenger
    SET DOB = TO_DATE('2010-03-10', 'YYYY-MM-DD')
    WHERE passenger_ID = 1;


CREATE OR REPLACE FUNCTION Numder_Passenger 
RETURN NUMBER 
IS 
 total NUMBER(8,2) := 0; 
BEGIN 
 SELECT COUNT(passenger_ID) INTO total FROM Passenger; 
 RETURN total; 
END; 
/
DECLARE
BEGIN
dbms_output.put_line('total number :'||Numder_Passenger());
end;
/
