# PowerShell Script for Remote Windows Update Installation

This PowerShell script connects to a remote machine, checks for and installs all available Windows updates, and logs the process (start and finish times). The log is saved to a specified file for later review.

## Prerequisites

1. **Administrative Access**: The credentials used for the remote machine must have administrative rights.
2. **PowerShell Remoting**: PowerShell Remoting must be enabled on the target machine(s).
3. **PSWindowsUpdate Module**: This script installs and uses the `PSWindowsUpdate` module to manage updates.

## Script Usage

Breakdown of the Script
1. Defining the Remote Machine

$computerName = "RemoteComputerName"

    Replace "RemoteComputerName" with the name or IP address of the remote computer to patch.

2. Getting User Credentials

$credential = Get-Credential

    Prompts the user for credentials (username and password). These credentials must have administrative privileges on the remote machine.

3. Invoke-Command Block

Invoke-Command -ComputerName $computerName -Credential $credential -ScriptBlock { ... }

    Invoke-Command: Runs the enclosed code block on the remote machine specified by $computerName.
    -ComputerName $computerName: Specifies the target remote machine.
    -Credential $credential: Passes the credentials to authenticate with the remote computer.

4. Logging the Start of the Patching Process

write-output "$(Get-Date -format 'dd-MMM-yyyy HH:mm'), Start patching, $($env:computername)"

    Logs the current date and time along with the computer’s name, indicating the start of the patching process.

5. Checking for and Importing the PSWindowsUpdate Module

if (-not (Get-Module -Name PSWindowsUpdate -ListAvailable)) {
    Install-Module -Name PSWindowsUpdate -Force -SkipPublisherCheck
}
Import-Module PSWindowsUpdate

    Checks if the PSWindowsUpdate module is installed on the remote machine. If not, it installs it using Install-Module.
    The module is then imported for use.

6. Installing Windows Updates

Get-WindowsUpdate -AcceptAll -Install

    Get-WindowsUpdate: Fetches and installs all available Windows updates.
    -AcceptAll: Automatically accepts all updates.
    -Install: Installs the updates.

7. Logging the Completion of the Patching Process

write-output "$(Get-Date -format 'dd-MMM-yyyy HH:mm'), Finished patching, $($env:computername)"

    Logs the completion of the patching process, including the time and machine name.

8. Error Handling and Logging to File

} -ErrorAction Stop | Out-File -FilePath "C:\path\to\patching-log.txt" -Append

    -ErrorAction Stop: If any error occurs, the script will stop executing.
    Out-File: Saves all output (start and finish logs) to a specified file.
    -Append: Ensures new log entries are added to the end of the file without overwriting existing entries.

Log File

    The log file is stored in the path specified in the script. Replace "C:\path\to\patching-log.txt" with the actual path where you want the log file to be saved.
    The log records the start and finish times of the patching process for later review.

Scheduling

You can schedule the execution of this script using Windows Task Scheduler. To do so:

    Open Task Scheduler.
    Create a new task and specify the trigger, e.g., on the second Thursday of every month.
    Set the action to run PowerShell with the following argument:

    bash

    -File "C:\path\to\your-script.ps1"

    Replace "C:\path\to\your-script.ps1" with the path to your PowerShell script.

Customization

    Modify the $computerName and log file path according to your requirements.
    Ensure that the remote machine has PowerShell Remoting enabled.
