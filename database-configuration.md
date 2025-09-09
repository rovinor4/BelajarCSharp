# Database Configuration

**Flow:**

* Check if `appsettings.json` exists
* If not, auto-generate with default values
* Load configuration from JSON
* Use configuration for SQL Server connection
* Update configuration via Windows Forms UI
* Save updated configuration back to JSON

---

**1. AppSettings & ConfigManager**

```csharp
using System;
using System.IO;
using System.Text.Json;

public class AppSettings
{
    public DatabaseConfig Database { get; set; } = new DatabaseConfig();
}

public class DatabaseConfig
{
    public string Server { get; set; } = "localhost";
    public string Database { get; set; } = "TestDB";
    public string User { get; set; } = "sa";
    public string Password { get; set; } = "YourPassword";
}

public static class ConfigManager
{
    private static readonly string filePath = "appsettings.json";

    public static AppSettings Load()
    {
        if (!File.Exists(filePath))
        {
            var defaultSettings = new AppSettings();
            Save(defaultSettings);
            return defaultSettings;
        }

        string json = File.ReadAllText(filePath);
        return JsonSerializer.Deserialize<AppSettings>(json) ?? new AppSettings();
    }

    public static void Save(AppSettings settings)
    {
        string json = JsonSerializer.Serialize(settings, new JsonSerializerOptions { WriteIndented = true });
        File.WriteAllText(filePath, json);
    }
}
```

**2. Database Connection Helper**

```csharp
using Microsoft.Data.SqlClient;

public static class Db
{
    private static readonly AppSettings settings = ConfigManager.Load();

    public static SqlConnection GetConnection()
    {
        string connectionString =
            $"Server={settings.Database.Server};" +
            $"Database={settings.Database.Database};" +
            $"User Id={settings.Database.User};" +
            $"Password={settings.Database.Password};" +
            "TrustServerCertificate=True;";

        return new SqlConnection(connectionString);
    }
}
```

**3. Windows Forms Usage Example**

```csharp
private void btnSave_Click(object sender, EventArgs e)
{
    var settings = ConfigManager.Load();
    settings.Database.Server = txtServer.Text;
    settings.Database.Database = txtDatabase.Text;
    settings.Database.User = txtUser.Text;
    settings.Database.Password = txtPassword.Text;
    ConfigManager.Save(settings);
    MessageBox.Show("Settings saved!");
}
```

---

Do you want me to now make a **same-format documentation** but for the **environment variable system** so your docs have both JSON and ENV methods?
