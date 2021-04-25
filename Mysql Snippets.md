# Table

```mysql
-- Create Table
> CREATE TABLE Persons (
    PersonID INT NOT NULL AUTO_INCREMENT,
    Name VARCHAR(255),
    Address VARCHAR(255),
    Country VARCHAR(255) DEFAULT "India",
    Phone VARCHAR(12) UNIQUE,
    Age INT(5) CHECK (Age >= 18),
    PRIMARY KEY(PersonID)
);

-- Get Table Structure
SHOW CREATE TABLE Persons \G

-- Cloning an existing table
CREATE TABLE ClonedPersons LIKE Persons;

-- 2nd method cloning 
CREATE TABLE ModifiedPersons SELECT PersonID, FirstName + LastName AS FullName from Persons WHERE Coutry = "USA"

```

