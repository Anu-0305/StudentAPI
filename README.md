

# Student Web API

A simple **ASP.NET Core Web API** built using **C#**, **Entity Framework Core**, and **SQL Server**. The API connects to an existing SQL Server database and provides CRUD operations for student records.

## Technologies Used

* C#
* ASP.NET Core Web API
* .NET
* Entity Framework Core
* SQL Server
* SQL Server Management Studio (SSMS)
* Swagger / OpenAPI
* Visual Studio

---

## Project Structure

```text
StudentAPI
│
├── Controllers
│   └── StudentsController.cs
│
├── Data
│   └── StudentDbContext.cs
│
├── Models
│   └── Student.cs
│
├── Properties
│
├── appsettings.json
├── Program.cs
└── StudentAPI.csproj
```

---

## Database Configuration

The project uses an existing SQL Server database.

### Database Structure

```text
University
│
└── Student
    │
    └── Students
```

Where:

* **Database:** `University`
* **Schema:** `Student`
* **Table:** `Students`

The table contains the student information used by the API.

---

## Prerequisites

Before running the project, make sure you have:

1. Visual Studio 2022
2. .NET SDK installed
3. SQL Server
4. SQL Server Management Studio (SSMS)
5. An existing `University` database
6. `Student.Students` table inside the database

---

## Entity Framework Core Packages

The following NuGet packages are required:

```powershell
Microsoft.EntityFrameworkCore.SqlServer 8.0.0
Microsoft.EntityFrameworkCore.Tools 8.0.0
Microsoft.EntityFrameworkCore.Design 8.0.0
```

They can be installed from:

```text
Visual Studio
→ Tools
→ NuGet Package Manager
→ Package Manager Console
```

Using:

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer -Version 8.0.0
Install-Package Microsoft.EntityFrameworkCore.Tools -Version 8.0.0
Install-Package Microsoft.EntityFrameworkCore.Design -Version 8.0.0
```

---

## Database Connection

The SQL Server connection string is configured in `appsettings.json`.

Example:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=YOUR_SERVER_NAME;Database=University;Trusted_Connection=True;TrustServerCertificate=True;"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

Replace:

```text
YOUR_SERVER_NAME
```

with the SQL Server instance used in SSMS.

For example:

```text
DESKTOP-ABC123\SQLEXPRESS
```

The connection string would be:

```json
"DefaultConnection": "Server=DESKTOP-ABC123\\SQLEXPRESS;Database=University;Trusted_Connection=True;TrustServerCertificate=True;"
```

---

## Entity Framework Configuration

The `StudentDbContext` connects the C# model to the SQL Server table.

```csharp
using Microsoft.EntityFrameworkCore;
using StudentAPI.Models;

namespace StudentAPI.Data
{
    public class StudentDbContext : DbContext
    {
        public StudentDbContext(DbContextOptions<StudentDbContext> options)
            : base(options)
        {
        }

        public DbSet<Student> Students { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Student>()
                .ToTable("Students", "Student");
        }
    }
}
```

The following mapping:

```csharp
.ToTable("Students", "Student");
```

maps the C# `Student` entity to:

```text
Student.Students
```

in the `University` database.

---

## API Endpoints

The API provides standard CRUD operations.

| Method | Endpoint             | Description                |
| ------ | -------------------- | -------------------------- |
| GET    | `/api/Students`      | Get all students           |
| GET    | `/api/Students/{id}` | Get a student by ID        |
| POST   | `/api/Students`      | Add a new student          |
| PUT    | `/api/Students/{id}` | Update an existing student |
| DELETE | `/api/Students/{id}` | Delete a student           |

---

## Running the Project

### Step 1: Open the Project

Open the `StudentAPI` project in Visual Studio.

### Step 2: Verify SQL Server

Make sure SQL Server is running and the database is available.

You can verify the database using:

```sql
USE University;

SELECT *
FROM Student.Students;
```

### Step 3: Check the Connection String

Open:

```text
appsettings.json
```

and verify the SQL Server name and database name.

### Step 4: Build the Project

In Visual Studio:

```text
Build → Build Solution
```

or press:

```text
Ctrl + Shift + B
```

### Step 5: Run the API

Press:

```text
Ctrl + F5
```

or click the green **Run** button.

---

## Testing with Swagger

When the application starts, Swagger/OpenAPI can be used to test the API.

The Swagger page will normally be available at a URL similar to:

```text
https://localhost:7265/swagger
```

The port number may be different on your computer.

Swagger allows you to test:

```text
GET
POST
PUT
DELETE
```

without needing a separate frontend application.

---

## How the Application Works

The overall architecture is:

```text
                    Client
                      │
                      ▼
                  Swagger
                      │
                      ▼
              StudentsController
                      │
                      ▼
               StudentDbContext
                      │
                      ▼
             Entity Framework Core
                      │
                      ▼
                  SQL Server
                      │
                      ▼
             University Database
                      │
                      ▼
              Student.Students
```

### Controller

`StudentsController.cs` handles HTTP requests.

For example:

```text
GET /api/Students
```

is handled by the `GetStudents()` method.

### Model

`Student.cs` represents a student record in C#.

### DbContext

`StudentDbContext.cs` manages communication between the C# application and SQL Server.

### Entity Framework Core

Entity Framework Core converts C# operations into SQL queries.

For example:

```csharp
_context.Students.ToListAsync();
```

is translated into a SQL query that retrieves records from:

```text
Student.Students
```

---

## Common Error

### Invalid object name 'student'

If you receive:

```text
Microsoft.Data.SqlClient.SqlException:
Invalid object name 'student'
```

check the following:

1. The database name is `University`.
2. The schema is `Student`.
3. The table is `Students`.
4. The connection string uses:

```text
Database=University
```

5. The DbContext contains:

```csharp
.ToTable("Students", "Student");
```

The correct table reference is:

```text
Student.Students
```

not:

```text
student
```

---

## Future Improvements

Possible improvements to the project include:

* Add input validation
* Add DTOs
* Add authentication and authorization
* Add pagination
* Add search and filtering
* Add exception handling middleware
* Add logging
* Add unit tests
* Add repository/service layers
* Connect the API to a frontend application
* Deploy the API to Azure or another cloud platform

---

## Author

**Student Web API Project**

Built using ASP.NET Core Web API, C#, Entity Framework Core, and SQL Server.
