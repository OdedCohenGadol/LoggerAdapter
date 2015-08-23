# LoggerAdapter
an accessible way to interact with the logger of log4net


instead of using the primitive logger, i've made a logger that can have a very specified details about any type log actions.

Prototype
=========

logType = log type : info, error or Fatal

actionType = the action that was running  (eg "send")

entity = the entity involved (eg user/job)

id = the entity id envovled - not mandatory

customMessage = custom message - not mandatory - especially @ info logs, because the log will take the exception message anyway

ex = the exception itself

public static void LogIt(LogType logType,ActionType actionType, SystemEntity entity, int? id = null, string customMessage = null ,Exception ex = null)


Examples
--------

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
