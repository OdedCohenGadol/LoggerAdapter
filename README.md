# LoggerAdapter
an accessible way to interact with the logger of log4net


instead of using the primitive logger, i've made a logger that can have a very specified details about any type log actions.

Prototype explanation:

/// <param name="logType">log type : info, error or Fatal</param>
/// <param name="actionType">the action that was running  (eg "send")</param>
/// <param name="entity">the entity involved (eg user/job)</param>
/// <param name="id">the entity id envovled - not mandatory</param>
/// <param name="customMessage">custom message - not mandatory - especially @ info logs, because the log will take the exception message anyway</param>
/// <param name="ex">the exception itself</param>
public static void LogIt(LogType logType,ActionType actionType, SystemEntity entity, int? id = null, string customMessage = null ,Exception ex = null)


examples of using:

1.) 

catch (Exception exp)
{
  Logger.LogIt(Logger.LogType.Fatal, Logger.ActionType.Upload, Logger.SystemEntity.CVFile, null, null, exp);
}

2.)

catch (Exception exp)
{
    Logger.LogIt(Logger.LogType.Fatal, Logger.ActionType.Submit, Logger.SystemEntity.Form, null, "trying to send contact form", exp);
}


3.)
 Logger.LogIt(Logger.LogType.Info, Logger.ActionType.Download, Logger.SystemEntity.User, null, "START Download Important file Process", null);
