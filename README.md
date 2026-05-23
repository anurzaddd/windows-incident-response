<#
.SYNOPSIS
    Shows recent failed login attempts (Event ID 4625) for incident response.
.DESCRIPTION
    Useful for detecting brute-force attacks on hospital domain controllers or critical servers.
.EXAMPLE
    .\Check-FailedLogins.ps1 -Count 20
#>

param(
    [int]$Count = 10
)

Write-Host "Last $Count failed logins (Event ID 4625):" -ForegroundColor Cyan
Get-EventLog -LogName Security -InstanceId 4625 -Newest $Count | 
    Select-Object TimeGenerated, @{Name='User';Expression={$_.ReplacementStrings[5]}}, @{Name='SourceIP';Expression={$_.ReplacementStrings[18]}} |
    Format-Table -AutoSize
