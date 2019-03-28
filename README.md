Hello

Despite my search on community I haven’t been able yet to find any “advanced” Section Access automation script. (But let me know If you found one).

In my opinion, building and keeping up to date a Section Access table tends to be laborious, quite complex to identify all the appropriate combinations and time consuming.

Therefore I decided to create my own generic script which automatically builds the section access table without having a headache.

I decided to focus on a single reduction field (and use composite security key in the datamodel for more complex scenarios). See this very informative topic for more information : https://community.qlik.com/t5/Qlik-Design-Blog/Data-Reduction-Using-Multiple-Fields/ba-p/1474917

 

Key points :

    For now, users and data reduction rules are specified in an Excel file (but it could be modified to load it from another source).

    Security strategy based on ACCESS, USERID, OMIT and [YourReductionField] properties.
    Two distinct user profiles for the rules (see corresponding sheets in the Excel file) :
        “Administrators”, these users are always included in the section access table of every apps, automatically gets “ADMIN” value for “ACCESS” property and the ability to view all values.
        “Users”, all the other users with specific reduction rules for multiple applications.

    Two different approaches to write reduction rules:
        By listing all the authorized values (default)
        Or on the contrary by giving access to all values except some of them. In this case, you must add a prefix in the value list wich means “all except” (by default : [*-] )

    Possible input values for reduction field:
    “*” for all values. In this case, the script will automatically search for the reduction field in your datamodel and use all corresponding values if it exists.
    “Value” for a single value
    “A;B;C” for multiple values, using a specific separator

    In addition you can use generic characters (?/*) to write “like” rules which is very handy.

    You can hide (omit) one or many fields per user and app.
    Optionally creates a visible Section Access table in datamodel for audit purposes (see script config section).

 

Note : You can easily change the reduction field name, ‘ALL’ alias, multiple values separator, and exclusion prefix in the script config section if you need to.

 

How to setup ?

    Identify the reduction field name in your datamodel. Duplicate / rename it in order to match with F1 cell value (mine default is “SECURITY_KEY”). If you want to use a different one, you’ll have to update F1 cell and the following config variable: SET SA_NomChampSecurite = 'SECURITY_KEY';
    Paste or include my script file at the end of your app script. Why ? Because it needs to loop and search for values through already loaded tables to work.
    Control and update config values if necessary, the most important to care about is those which refers to the location of the Excel rights file (folder LIB name) : SET SA_CheminExcelDroits = 'Security files';
    Fill, the excel file with all the rules you need. (Any incomplete line will be automatically ignored; qvf extension is not required in the appname column.)
    If for some reason you want to disable a rule without deleting it from the Excel file, simply put ‘0’ value in the ENABLED column.
    If you want to add an optionnal section access control table in your model, ensure the following config variable is set 1. Otherwise set it to 0. SET SA_CreerTableVerification = 1;

    Reload and relax ??

 

You'll find examples of reduction rules in the Excel file.

Script comments and variable names are in French (because I am), but I’ll take the time to translate it in english if you ask for it.

Enjoy !