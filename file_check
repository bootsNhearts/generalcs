#region Namespaces
using System;
using System.Data;
using Microsoft.SqlServer.Dts.Runtime;
using System.Windows.Forms;
using System.IO;
using System.Data.SqlClient;
using System.Text.RegularExpressions;
#endregion

namespace ST_06e16ab20f7c4437825f285c425dad29
{
    /// <summary>
    /// ScriptMain is the entry point class of the script.  Do not change the name, attributes,
    /// or parent of this class.
    /// </summary>
	[Microsoft.SqlServer.Dts.Tasks.ScriptTask.SSISScriptTaskEntryPointAttribute]
	public partial class ScriptMain : Microsoft.SqlServer.Dts.Tasks.ScriptTask.VSTARTScriptObjectModelBase
	{
		public void Main()
        {
            ConnectionManager cm;
            System.Data.SqlClient.SqlConnection conn;
            System.Data.SqlClient.SqlCommand com;

            cm = Dts.Connections["voxsql01.ClientData"];
            conn = (System.Data.SqlClient.SqlConnection)cm.AcquireConnection(null);

            string filepath = @"G:\HRUploads\CIBC\Completed";
            DirectoryInfo d = new DirectoryInfo(filepath);
            FileInfo[] Files = d.GetFiles("*.csv");
            
            foreach (FileInfo file in Files)
            {
                string fileName = file.Name;
                string runDate = "";
                long fileSize = file.Length;
                
                try
                {
                    //runDate = fileName.Substring(file.Name.Length - 14, 10);
                    Regex r = new Regex(@"\d{2}-\d{2}-\d{4}");
                    Match m = r.Match(fileName);
                    if (m.Success)
                    {
                        runDate = m.Value;
                        Convert.ToDateTime(runDate).ToString("yyyy-mm-dd");
                    }
                    com = new System.Data.SqlClient.SqlCommand(
                        $"INSERT INTO ClientData.dbo.file_monitor " +
                        $"(filesize, client, filename, filedate, file_path) VALUES('{fileSize}', 'CIBC', '{fileName}', '{runDate}', '{filepath}')", conn);
                    com.ExecuteNonQuery();
                }
                catch (Exception e)
                {
                    MessageBox.Show(e.Message);
                }
                //finally
                //{
                //    cm.ReleaseConnection(conn);
                //}
            }

            cm.ReleaseConnection(conn);
            //Dts.TaskResult = (int)ScriptResults.Success;
		}

        #region ScriptResults declaration
        /// <summary>
        /// This enum provides a convenient shorthand within the scope of this class for setting the
        /// result of the script.
        /// 
        /// This code was generated automatically.
        /// </summary>
        enum ScriptResults
        {
            Success = Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Success,
            Failure = Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Failure
        };
        #endregion

	}
}
