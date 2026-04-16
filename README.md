# Cron Backend

## Description

    This library create an easy way to schedule BusinessProcess in your application through a crontab-like configuration file.
    
## Usage

    Use CronBackend\CronBackendBase.pkg
    
    Class cCronBackend is a cCronBackendBase
        Procedure OnProcessStart
            // Here is a good place to put tests calls of your Business Processes to run directly during development
        End_Procedure
    End_Class
    
    Object oCronBackend is a cCronBackend
    End_Object
    
    Send StartCronProcess of oCronBackend
    
    
## Properties<br>

    Boolean pbTestState False // Primarilly used for UnitTest. Only run through the loop once
    Integer piIntervalFrequency 5 // Seconds between cycles
    Integer piRestartAfterCycles (12 * 60) // 12 * 5 seconds * 60 => 1 hour
    Boolean pbExitAfterRestartAfterCycles False // Use this if you have issues with memory leaks and configure the TaskManager in Windows to restart if it is closed
    
    String psCronFilePath 'Programs\cron.txt' // Default path to cron.txt file where the schedule is configured