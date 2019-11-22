# loading of vietCoffee Visual Basic

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;

using System.Data.SqlClient;
using System.Data.OleDb;

using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace VietCoffee
{
    public partial class Form1 : Form
    {
        //Global variables declared for database
        DataSet dSet = new DataSet();
        DataTable dTable1;
        SqlCommand sqlCmd;
        SqlConnection sqlConn;
        SqlDataAdapter sqlDapter;

        public Form1()
        {
            InitializeComponent();
        }

        private void notifyIcon1_MouseDoubleClick(object sender, MouseEventArgs e)
        {
            //Double clicking of icon in task bar will reopen form
            Show();
            this.WindowState = FormWindowState.Normal;
            notifyIcon.Visible = false;
        }

        private void Form1_Resize_1(object sender, EventArgs e)
        {
            //if the form is minimized  
            //hide it from the task bar  
            //and show the system tray icon (represented by the NotifyIcon control)  
            if (this.WindowState == FormWindowState.Minimized)
            {
                Hide();
                
                notifyIcon.Visible = true;
            }
        }

        private void logActivityBtn_Click(object sender, EventArgs e)
        {
            //Logs all the information as a string for storage
            //string adds all information for placement in the dataset

            String activityStr = "loggedOn: " + DateTime.Now + "; employeeID: " + employeeIDTxtbox.Text + "; activity: " + activityTxtbox.Text + "; startedOn: " + startedOnTxtbox.Text + "; endedOn: " + endedOnTxtbox.Text;

            dSet = new DataSet();
            sqlConn = new SqlConnection(@"Data Source=aurnt145;Initial Catalog=p_npi-paperless_enclosures;User Id=npiweb;Password=np1w3b;");
            sqlDapter = new SqlDataAdapter();
            sqlCmd = new SqlCommand("insert into ___vietCoffee (activity) values (@activity)", sqlConn);
            sqlDapter.SelectCommand = sqlCmd;
            sqlCmd.Parameters.Add("@activity", SqlDbType.VarChar).Value = activityStr;

            sqlDapter.Fill(dSet, "MyTable");

            dTable1 = new DataTable();

            dTable1 = dSet.Tables["MyTable"];

        }

        private void Form1_Load(object sender, EventArgs e)
        {
            loadDefaultEmployee();
        }

        private void loadDefaultEmployee()
        {
            string machineName = Environment.MachineName;
        }
    }
}
