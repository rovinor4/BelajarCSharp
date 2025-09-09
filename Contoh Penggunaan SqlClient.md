## 1. Install NuGet Package

```powershell
dotnet add package Microsoft.Data.SqlClient
```

---

## 2. Contoh Lengkap CRUD

```csharp
using System;
using Microsoft.Data.SqlClient;

class Program
{
    static string connectionString = "Server=localhost;Database=TestDB;User Id=sa;Password=YourPassword;TrustServerCertificate=True;";

    static void Main()
    {
        CreateData("Budi", "budi@example.com");
        ReadData();
        UpdateData(1, "Budi Update", "budiupdate@example.com");
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

## 3. Catatan Penting

* Pastikan **database & tabel** `Users` sudah dibuat:

```sql
CREATE TABLE Users (
    Id INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(100),
    Email NVARCHAR(100)
);
```

* Ganti `Server`, `Database`, `User Id`, dan `Password` sesuai konfigurasi lo.
* `TrustServerCertificate=True;` dipakai buat ngilangin SSL warning di lokal.

---

Kalau mau, gue bisa bikinin **versi async** biar performanya lebih mantap dan cocok untuk Windows Forms atau WPF lo nanti.
Mau gue buatin sekalian bro?
