using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace LoggerAdapter
{
    public static class Logger
    {

        internal static readonly log4net.ILog _logger = log4net.LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

       
        public static void LogIt(LogType logType,ActionType actionType, SystemEntity entity, int? id = null, string customMessage = null ,Exception ex = null)
        {
            string entityType = GetDescription((int)entity, typeof(SystemEntity));
            string customerString = id > 0 ? string.Format(entityType + " ", id) : string.Empty;
            string actionTypestring = GetDescription((int)actionType, typeof(ActionType));

            string message = " in " + actionTypestring + " for " + entityType + " " + (id != null ? id.Value.ToString() : "")  + " " + (!string.IsNullOrWhiteSpace(customMessage)? customMessage : "") + ":";

            switch (logType)
            {
                case LogType.Info:
                    _logger.Info("Information: " + message);
                    break;
                case LogType.Error:
                    if (ex != null)
                    {
                        if (ex.InnerException != null)
                        {
                            message += " Inner Exception: " + ex.InnerException.Message;
                        }
                        _logger.Error("ERROR: " + message, ex);
                    }
                    else
                    {
                        _logger.Error("ERROR: " + message);
                    }
                    break;
                case LogType.Fatal:
                    if (ex != null)
                    {
                        if (ex.InnerException != null)
                        {
                            message += " Inner Exception: " + ex.InnerException.Message;
                        }
                        _logger.Fatal("FATAL: " + message, ex);
                    }
                    else
                    {
                        _logger.Fatal("FATAL: " + message);
                    }
                    break;
                default:
                    break;
            }

        }



       



        #region X10tions
        public static string GetDescription(int number, Type type)
        {
            var dictionary = EnumsToDictionary(type);
            foreach (var item in dictionary)
            {
                if (item.Key == number)
                    return item.Value;
            }

            return "";
        }

        public static Dictionary<int, string> EnumsToDictionary(this Type EnumerationType)
        {
            Dictionary<int, string> RetVal = new Dictionary<int, string>();
            var AllItems = Enum.GetValues(EnumerationType);
            foreach (var CurrentItem in AllItems)
            {
                DescriptionAttribute[] DescAttribute = (DescriptionAttribute[])EnumerationType.GetField(CurrentItem.ToString()).GetCustomAttributes(typeof(DescriptionAttribute), false);
                string Description = DescAttribute.Length > 0 ? DescAttribute[0].Description : CurrentItem.ToString();
                RetVal.Add((int)CurrentItem, Description);
            }
            return RetVal;
        }


        public enum SystemEntity
        {
            [Description("User")]
            User,
            [Description("Form")]
            Form,
            [Description("Comment")]
            Comment,
            [Description("Modes")]
            Modes,
            [Description("Job")]
            Job
            
        }

        public enum ActionType
        {
            [Description("Create Action")]
            Create,
            [Description("Delete Action")]
            Delete,
            [Description("Upload Action")]
            Upload,
            [Description("Send Action")]
            Send,
            [Description("Update Action")]
            Update,
            [Description("Get Action")]
            Get,
            [Description("Download Action")]
            Download,
            [Description("Submit Action")]
            Submit
        }

        public enum LogType
        {
            Info,
            Error,
            Fatal
        }
        #endregion
    }
}

