You have to write db schema for school management systems
 
My kid John was studied in School A which is located in Centre A on 01/01/2021 to 31/12/2021 in grade 1 class 1
 
My kid John was studied in School A which is located in Centre B on 01/01/2022 to 31/12/2022 in grade 2 class 3
 
My kid John was studied in School A which is located in Centre B on 01/01/2023 to 31/12/2023 in grade 3 class 1
 
My kid John was studied in School A which is located in Centre B on 01/01/2024 to till in grade 1 class 2

### Database Schema for School Management System

Here’s a proposed schema that reflects the requirements:

#### 1. **School Table**
Stores information about each school.

```sql
CREATE TABLE School (
    SchoolID INT PRIMARY KEY,
    SchoolName VARCHAR(255),
    Address VARCHAR(255),
    Centre VARCHAR(255)
);
```

#### 2. **Student Table**
Stores information about students.

```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    FirstName VARCHAR(100),
    LastName VARCHAR(100),
    DateOfBirth DATE,
    Gender VARCHAR(10)
);
```

#### 3. **Grade Table**
Stores information about grades and classes.

```sql
CREATE TABLE Grade (
    GradeID INT PRIMARY KEY,
    GradeName VARCHAR(50),
    ClassName VARCHAR(50)
);
```

#### 4. **Enrollment Table**
Captures the enrollment of students in schools, including the duration and class details.

```sql
CREATE TABLE Enrollment (
    EnrollmentID INT PRIMARY KEY,
    StudentID INT,
    SchoolID INT,
    GradeID INT,
    StartDate DATE,
    EndDate DATE,
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (SchoolID) REFERENCES School(SchoolID),
    FOREIGN KEY (GradeID) REFERENCES Grade(GradeID)
);
```

### Explanation of Tables

- **School Table**: This table holds details about each school, including its name and location (Centre A or Centre B).
  
- **Student Table**: Contains personal information about students, such as their names and date of birth.

- **Grade Table**: Represents different grades and classes available in the school system. For example, it can include entries like "Grade 1, Class 1" or "Grade 2, Class 3".

- **Enrollment Table**: This crucial table links students to schools and grades while capturing the enrollment period (start and end dates). It allows tracking of which student is enrolled in which school during specific time frames.

### Example Data Insertion

Based on John’s educational history, here’s how you might insert data into these tables:

```sql
-- Insert schools
INSERT INTO School (SchoolID, SchoolName, Address, Centre) VALUES 
(1, 'School A', '123 Main St', 'Centre A'),
(2, 'School A', '123 Main St', 'Centre B');

-- Insert grades
INSERT INTO Grade (GradeID, GradeName, ClassName) VALUES 
(1, 'Grade 1', 'Class 1'),
(2, 'Grade 2', 'Class 3'),
(3, 'Grade 3', 'Class 1'),
(4, 'Grade 1', 'Class 2');

-- Insert student
INSERT INTO Student (StudentID, FirstName, LastName, DateOfBirth, Gender) VALUES 
(1, 'John', 'Doe', '2015-01-01', 'Male');

-- Insert enrollments
INSERT INTO Enrollment (EnrollmentID, StudentID, SchoolID, GradeID, StartDate, EndDate) VALUES 
(1, 1, 1, 1, '2021-01-01', '2021-12-31'),
(2, 1, 2, 2, '2022-01-01', '2022-12-31'),
(3, 1, 2, 3, '2023-01-01', '2023-12-31'),
(4, 1, 2, 4, '2024-01-01', NULL); -- Current enrollment with no end date
```

### Conclusion

This database schema effectively captures the necessary details for managing student enrollments in a school management system. It allows for tracking students’ educational paths across different schools and grades over time. Depending on further requirements—such as attendance tracking or performance evaluation—additional tables can be added to enhance the system's capabilities.

Citations:
[1] https://www.surfsidemedia.in/post/database-schema-for-school-management-system
[2] https://apps-gate.net/design-school-management-system-database/
[3] https://www.techprofree.com/school-management-system-project-database-design/
[4] https://dba.stackexchange.com/questions/278459/school-database-schema
[5] https://www.studentprojects.live/studentprojectreport/projectreport/database-tables-for-school-management-system/
[6] https://vertabelo.com/blog/database-design-management-system/