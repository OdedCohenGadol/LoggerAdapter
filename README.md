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
Output @ fatal log file: 
2015-08-06 16:44:39,137 [49] FATAL LoggerAdapter.Logger [(null)] - FATAL:  in Upload Action for CV File  :
System.NullReferenceException: Object reference not set to an instance of an object.
   at XXXXXXXXXXXXXXXXXXXXXXXXX(HttpPostedFileBase file, FormCollection form) in 
   d:\Projects\XXXXXXXX.cs:line 485

2.)

catch (Exception exp)

{

    Logger.LogIt(Logger.LogType.Fatal, Logger.ActionType.Submit, Logger.SystemEntity.Form, null, "trying to send contact form", exp);
    
}


3.)

 Logger.LogIt(Logger.LogType.Info, Logger.ActionType.Download, Logger.SystemEntity.User, null, "START Download Important file Process", null);
 
 Output @ info log file : 
 2015-08-06 15:48:51,609 [19] INFO  LoggerAdapter.Logger [(null)] - Information:  in Create Action for User  START Download Important file Process
