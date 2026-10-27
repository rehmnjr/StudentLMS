# Student Management System

A console-based Student Management System built with **C#**, **.NET** and **Microsoft SQL Server** using **ADO.NET**. It manages students, departments, courses and enrollments, and calculates grade points from marks.

## Features

- **Students:** create, view, search, update and delete student records
- **Departments:** view all departments and add new ones
- **Courses:** view all courses and add new ones
- **Enrollment:** assign courses to students, with checks that the student and course exist
- **Marks & Grade Points:** enter marks for a course and convert them to a 4.0-scale grade point
- **Search:** view a student's enrolled courses and grades
- **Security:** all queries use parameterized SQL to prevent SQL injection

## Tech Stack

| Layer        | Technology                        |
|--------------|-----------------------------------|
| Language     | C#                                |
| Framework    | .NET                              |
| Data Access  | ADO.NET (`Microsoft.Data.SqlClient`) |
| Database     | Microsoft SQL Server (SSMS)       |
| IDE          | Visual Studio / VS Code           |

## 🗄️ Database Schema

```
Department (DepartmentID PK, DepartmentName)
     │ 1
     │
     ├──────────< Student (StudentID PK, FirstName, LastName, DateOfBirth, DepartmentID FK)
     │                │ 1
     │                │
     └──────────< Course (CourseID PK, CourseName, DepartmentID FK)
                      │ 1        │
                      │          │
                      └────< Enrollment >────┘
                   (EnrollmentID PK, StudentID FK, CourseID FK, Grade)
```

- A department has many students and offers many courses.
- Students and courses have a many-to-many relationship through `Enrollment`.

## Grade Point Scale

| Marks   | Grade Point |
|---------|-------------|
| 80+     | 4.0         |
| 70–79   | 3.5         |
| 60–69   | 3.0         |
| 50–59   | 2.5         |
| 40–49   | 2.0         |
| < 40    | 0.0         |

## Getting Started

### Prerequisites

- [.NET SDK](https://dotnet.microsoft.com/download) (6.0 or later)
- Microsoft SQL Server and SQL Server Management Studio (SSMS)

### 1. Clone the repository

```bash
git clone https://github.com/rehmnjr/StudentManagementSystem.git
cd StudentManagementSystem
```

### 2. Create the database

Open SSMS and run the SQL script:

```sql
CREATE DATABASE StudentManagementSystem;
GO
USE StudentManagementSystem;
GO

CREATE TABLE Department (
    DepartmentID INT PRIMARY KEY,
    DepartmentName VARCHAR(255)
);

CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    FirstName VARCHAR(255),
    LastName VARCHAR(255),
    DateOfBirth DATE,
    DepartmentID INT FOREIGN KEY REFERENCES Department(DepartmentID)
);

CREATE TABLE Course (
    CourseID INT PRIMARY KEY,
    CourseName VARCHAR(255),
    DepartmentID INT FOREIGN KEY REFERENCES Department(DepartmentID)
);

CREATE TABLE Enrollment (
    EnrollmentID INT IDENTITY(1,1) PRIMARY KEY,
    StudentID INT FOREIGN KEY REFERENCES Student(StudentID),
    CourseID INT FOREIGN KEY REFERENCES Course(CourseID),
    Grade DECIMAL(4, 2)
);
```

### 3. Update the connection string

In `Program.cs`, set the connection string to match your SQL Server instance:

```csharp
private static readonly string connectionString =
    "Data Source=localhost;Initial Catalog=StudentManagementSystem;Integrated Security=True;Encrypt=False";
```

### 4. Run the app

```bash
dotnet add package Microsoft.Data.SqlClient
dotnet run
```

##  Usage

```
Student Management System

===============MAIN MENU===============

1. Create Student
2. Show Students
3. Search Student
4. Update Student
5. Delete Student
6. Assign course to student
7. Insert Marks and Calculate GPA
8. View courses
9. View departments
0. Exit
=============================================
Enter your choice:
```

> **Tip:** Add departments first, then courses and students, then enroll students in courses before entering marks.

## Future Improvements

- Convert to an **ASP.NET Core Web API** with a **React.js** frontend
- Store raw marks and grade points separately and calculate overall GPA per student
- Add a `CourseDepartment` table so a course can be offered by multiple departments
- Add input validation, exception handling and unique enrollment constraints
- Move the connection string to `appsettings.json`

## Author

**Fahim Rehman**
- GitHub: [@rehmnjr](https://github.com/rehmnjr)
- LinkedIn: [fahimrehman](https://linkedin.com/in/fahimrehman)
- Email: rehmnfahim@gmail.com
