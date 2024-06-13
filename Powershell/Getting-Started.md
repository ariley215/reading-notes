# Powershell Overview and Basics

Management execution engine that includes

- command-line shell
- scripting language based on .Net Core
- automation platform
- Powershell Desired State Configuration
    - management framework in Powershell
    - using configuration as code


 I need an Excel spreadsheet that has the list of all of the services for a specific system on your network that are stopped:
```powershell

get-service |
where-Object Status -eq 'Stoppped' |
select-object Name, Status
# repsonse is held in variable 
$data = get-service | where-Object Satus -eq 'Stopped' | select-object Name, Status
$data | out-file .\services.csv
notepad .\services.csv
# opens in the notepad app