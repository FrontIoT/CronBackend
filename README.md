# Cron Backend

## Description

    This library create an easy way to schedule BusinessProcess in your application through a crontab-like configuration file.
    
    The idea is that you have one script that run all your backend processes when you want them to run.
    
    cCronBackendBusinessProcess is a regular BusinessProcess and work the same way appart from the added RegisterToCron procedure.
    This is where you put your code in DoProcess
    
    The cron.txt file have the familiar crontab structure that is standard in all Linux distributions.
    But instead of a Linux command to be executed this file contains the BusinessProcess object name to be executed
    # m h  dom mon dow   oBusinessProcess
    
## Usage

    Use CronBackend\CronBackendBase.pkg
    Use CronBackend\CronBackendBusinessProcess.pkg
    
    Class cCronBackend is a cCronBackendBase
        Procedure OnProcessStart
            // Here is a good place to put tests calls of your Business Processes to run directly during development
        End_Procedure
    End_Class

    
    Object oUnitTestDoNotRemove is a cCronBackendBusinessProcess
        Procedure RegisterToCron tBusinessProcessRegister ByRef tBPRegister
            Move 'oUnitTestDoNotRemove' to tBPRegister.sName
            Move (RefProc(OnProcess)) to tBPRegister.hoDoProcessFunction
            Move (Self) to tBPRegister.hoSourceBPObject
        End_Procedure
        
        Procedure OnProcess
            
            // HERE GOES YOUR CODE THAT YOU WANT TO SCHEDULE
            
        End_Procedure
    
    End_Object
    
    Object oCronBackend is a cCronBackend

        Procedure OnProcessStart
            Integer iSize
            tBusinessProcessRegister[] atBusinessProcessRegister
            
            Move (SizeOfArray(atBusinessProcessRegister)) to iSize
            // Register Business Processes here so they can be called by the script later
            Send RegisterToCron of oUnitTestDoNotRemove (&atBusinessProcessRegister[iSize])
            // ...
            
            // Store the register
            Set paBPRegister to atBusinessProcessRegister
        End_Procedure
    End_Object
    
    Send StartCronProcess of oCronBackend
    
    
## Properties<br>

    Boolean pbTestState False // Primarilly used for UnitTest. Only run through the loop once
    Integer piIntervalFrequency 5 // Seconds between cycles
    Integer piRestartAfterCycles (12 * 60) // 12 * 5 seconds * 60 => 1 hour
    Boolean pbExitAfterRestartAfterCycles False // Use this if you have issues with memory leaks and configure the TaskManager in Windows to restart if it is closed
    
    String psCronFilePath 'Programs\cron.txt' // Default path to cron.txt file where the schedule is configured