# sqlclient CRUD Example

**Flow:**

* Install `Microsoft.Data.SqlClient` NuGet package
* Define connection string
* Implement CRUD operations: Create, Read, Update, Delete
* Prepare SQL table before running the program

---

## 1. Install NuGet Package

```powershell
dotnet add package Microsoft.Data.SqlClient
```

---

## 2. Full CRUD Example

```csharp
using System;
using Microsoft.Data.SqlClient;

class Program
{
    static string connectionString = "Server=localhost;Database=TestDB;User Id=sa;Password=YourPassword;TrustServerCertificate=True;";

    static void Main()
    {
        CreateData("John Doe", "john@example.com");
        ReadData();
        UpdateData(1, "John Updated", "johnupdated@example.com");
        DeleteData(1);
    }

    // CREATE
    static void CreateData(string name, string email)
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            string sql = "INSERT INTO Users (Name, Email) VALUES (@Name, @Email)";
            using (SqlCommand cmd = new SqlCommand(sql, conn))
            {
                cmd.Parameters.AddWithValue("@Name", name);
                cmd.Parameters.AddWithValue("@Email", email);
                int rows = cmd.ExecuteNonQuery();
                Console.WriteLine($"{rows} row(s) inserted.");
            }
        }
    }

    // READ
    static void ReadData()
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            string sql = "SELECT Id, Name, Email FROM Users";
            using (SqlCommand cmd = new SqlCommand(sql, conn))
            using (SqlDataReader reader = cmd.ExecuteReader())
            {
                while (reader.Read())
                {
                    Console.WriteLine($"{reader["Id"]} - {reader["Name"]} - {reader["Email"]}");
                }
            }
        }
    }

    // UPDATE
    static void UpdateData(int id, string newName, string newEmail)
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            string sql = "UPDATE Users SET Name=@Name, Email=@Email WHERE Id=@Id";
            using (SqlCommand cmd = new SqlCommand(sql, conn))
            {
                cmd.Parameters.AddWithValue("@Id", id);
                cmd.Parameters.AddWithValue("@Name", newName);
                cmd.Parameters.AddWithValue("@Email", newEmail);
                int rows = cmd.ExecuteNonQuery();
                Console.WriteLine($"{rows} row(s) updated.");
            }
        }
    }

    // DELETE
    static void DeleteData(int id)
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            string sql = "DELETE FROM Users WHERE Id=@Id";
            using (SqlCommand cmd = new SqlCommand(sql, conn))
            {
                cmd.Parameters.AddWithValue("@Id", id);
                int rows = cmd.ExecuteNonQuery();
                Console.WriteLine($"{rows} row(s) deleted.");
            }
        }
    }
}
```

---

## 3. Important Notes

* Ensure that the `Users` table exists before running the application:

```sql
CREATE TABLE Users (
    Id INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(100),
    Email NVARCHAR(100)
);
```

* Replace `Server`, `Database`, `User Id`, and `Password` in the connection string with your own configuration.
* `TrustServerCertificate=True;` is used to bypass SSL certificate warnings in a local environment.
