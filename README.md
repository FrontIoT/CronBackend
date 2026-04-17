# Cron Backend

## Description
<br>
    This library create an easy way to schedule BusinessProcess in your application through a crontab-like configuration file.<br>
    <br>
    The idea is that you have one script that run all your backend processes when you want them to run.<br>
    <br>
    cCronBackendBusinessProcess is a regular BusinessProcess and work the same way appart from the added RegisterToCron procedure.<br>
    This is where you put your code in DoProcess<br>
    <br>
    The cron.txt file have the familiar crontab structure that is standard in all Linux distributions.<br>
    But instead of a Linux command to be executed this file contains the BusinessProcess object name to be executed<br>
    # m h  dom mon dow   oBusinessProcess<br>
    <br>
<b>## Usage</b><br>
<br>
    Use CronBackend\CronBackendBase.pkg<br>
    Use CronBackend\CronBackendBusinessProcess.pkg<br>
    <br>
    Class cCronBackend is a cCronBackendBase<br>
        Procedure OnProcessStart<br>
            // Here is a good place to put tests calls of your Business Processes to run directly during development<br>
        End_Procedure<br>
    End_Class<br>
    <br>
    Object oUnitTestDoNotRemove is a cCronBackendBusinessProcess<br>
        Procedure RegisterToCron tBusinessProcessRegister ByRef tBPRegister<br>
            Move 'oUnitTestDoNotRemove' to tBPRegister.sName<br>
            Move (RefProc(DoProcess)) to tBPRegister.hoDoProcessFunction<br>
            Move (Self) to tBPRegister.hoSourceBPObject<br>
        End_Procedure<br>
        
        Procedure OnProcess<br>
            <br>
            // HERE GOES YOUR CODE THAT YOU WANT TO SCHEDULE<br>
            <br>
        End_Procedure<br>
    End_Object<br>
    <br>
    Object oCronBackend is a cCronBackend<br>
        Procedure OnProcessStart<br>
            Integer iSize<br>
            tBusinessProcessRegister[] atBusinessProcessRegister<br>
            <br>
            Move (SizeOfArray(atBusinessProcessRegister)) to iSize<br>
            // Register Business Processes here so they can be called by the script later<br>
            Send RegisterToCron of oUnitTestDoNotRemove (&atBusinessProcessRegister[iSize])<br>
            // ...<br>
            <br>
            // Store the register<br>
            Set paBPRegister to atBusinessProcessRegister<br>
        End_Procedure<br>
    End_Object<br>
    <br>
    Send StartCronProcess of oCronBackend<br>
    <br>
<b>## Properties</b><br>
<br>
    Boolean <b>pbTestState</b> False // Primarilly used for UnitTest. Only run through the loop once<br>
    Integer <b>piIntervalFrequency</b> 5 // Seconds between cycles<br>
    Integer <b>piRestartAfterCycles</b> (12 * 60) // 12 * 5 seconds * 60 => 1 hour<br>
    Boolean <b>pbExitAfterRestartAfterCycles</b> False // Use this if you have issues with memory leaks and configure the TaskManager in Windows to restart if it is closed<br>
    <br>
    String psCronFilePath 'Programs\cron.txt' // Default path to cron.txt file where the schedule is configured<br>
<br>
<b>## TODO</b><br>
<br>
    [ ] There are some features in a regular crontab that has been left out here like the */x syntax to run every x:th minute or hours<br>
    [ ] OnProcessStart in cCronBackendBase might be clearer if we rename or add a separate procedure like RegisterBusinessProcesses<br>
