# MySQL-Connection-Example
Examples of MySQL connection

DataBaseManager.cs

```csharp
using MySql.Data.MySqlClient;
using System;
using System.Collections.Generic;
using System.Data;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace kursach
{
    public class DataBaseManager
    {
        private static MySqlConnection _conn;
        private static MySqlDataAdapter _adapter;
        private static DataTable _data;

        public static void OpenConnection(string user, string password)
        {
            _conn f= new MySqlConnection("server=localhost;userid="+user+";password="+password+";database=data");
        }

        public static void CloseConnection()
        {
            _conn = null;
        }

        public static void Update()
        {
            if (_conn is null)
                return;

            _conn.Open();
            _adapter.Update(_data);
            _conn.Close();
        }

        public static void Select(DataGridView ui, string query)
        {
            if (_conn is null)
                return;

            _conn.Open();
            _data = new DataTable();
            _data.Locale = System.Globalization.CultureInfo.InvariantCulture;
            _adapter = new MySqlDataAdapter(query, _conn);
            MySqlCommandBuilder cb = new MySqlCommandBuilder(_adapter);
            _adapter.Fill(_data);
            ui.DataSource = _data;
            _conn.Close();
        }

        public static void DoQuery(string query)
        {
            if (_conn is null)
                return;

            _conn.Open();
            _data = new DataTable();
            _data.Locale = System.Globalization.CultureInfo.InvariantCulture;
            _adapter = new MySqlDataAdapter(query, _conn);
            MySqlCommandBuilder cb = new MySqlCommandBuilder(_adapter);
            _adapter.Fill(_data);
            _conn.Close();
        }
    }
}
```

FormManager.cs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace kursach
{
    public class FormManager
    {
        public static void OpenForm(Form original, Form opened)
        {
            opened.FormClosed += delegate (object s, FormClosedEventArgs ev)
            {
                original.Show();
                opened.Hide();
            };
            original.Hide();
            opened.Show();
        }
    }
}
```
