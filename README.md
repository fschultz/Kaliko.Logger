Kaliko.Logger
=============

A light weight yet competent logger for .NET with configurable output modules, such as log file, e-mail and debug log. Easy to add your own output module if needed.

##Usage

In your configuration file (web.config for web applications or app.config for applications) register a new section called *loggers*:

```
  <configSections>
    <section name="loggers" type="Kaliko.Configuration.LoggersSection, Kaliko.Logger" />
  </configSections>
```

Add a new section in your configuration and add one or more loggers to it:

```
  <loggers>
    <fileLogger filename="c:\temp\log%yyyy%mm%dd.txt" treshold="Warning" />
    <mailLogger from="website@example.com" to="recipeint@example.com" subject="An error has occured" treshold="Major" />
    <debugLogger treshold="Info" />
  </loggers>
```

Each logger has a treshold (should have been "threshold") value that sets the lower bound of which items that should be logged through that logger.

In the example above all the different loggers are set up (**fileLogger**, **mailLogger** and **debugLogger**) but with different thresholds. That means that a log item of the severity **"Info"** only will be written to the **debugLogger** but an item of the severity **"Critical"** will be written to all.

The severity levels are (in ascending order): **NotSet**, **Info**, **Warning**, **Minor**, **Major** and **Critical**

To write a string to the log:
```
using Kaliko;

// ...

Logger.Write("Just some information", Logger.Severity.Info);
```

To write an exception to the log:
```
using Kaliko;

// ...
try {
  // ...
}
catch (Exception exception) {
  Logger.Write(exception , Logger.Severity.Major);
}
```

### Loggers

#### FileLogger

The FileLogger requires a **filename** to be specified that points out which file the logger should write to. It's either specified with an absolute path like *"c:\temp\mylog.txt"* or a relative path based on from where the application is executed. In order to use the App_data folder in a website project, the shortcut **|datadirectory|** could be used (such as *"|datadirectory|mylog.txt"*).

To use rolling log files the following variables can be used:
**%yyyy** = Year
**%mm** = Month
**%dd** = Day of month

The following will write to a log file for each month in the App_data folder: *"|datadirectory|mylog-%yyyy-%mm.txt"*

#### MailLogger

Sends the log item as an email to the specified recipient. Needs both a **from** and a **to** address to be specified as well as a subject to be used for the mails.

#### DebugLogger

Writes the log item to the debug view. Usable together with a tool like https://technet.microsoft.com/en-us/sysinternals/bb896647.aspx
