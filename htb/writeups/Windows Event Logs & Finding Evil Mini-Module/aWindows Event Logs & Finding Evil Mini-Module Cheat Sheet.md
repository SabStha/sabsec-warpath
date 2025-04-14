🧠 Sysmon + Event Log + ETW Detection Cheat Sheet
🔍 Detection Use Case	📄 Event Source & ID(s)	⚙️ Key Commands / Indicators
Process Creation (Initial Foothold)	Sysmon Event ID 1	Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; ID=1}
Network Connections	Sysmon Event ID 3	Look for suspicious DestinationIp, use Where-Object or XML parsing
DLL Hijacking	Sysmon Event ID 7	Monitor ImageLoaded ≠ System32 path + unsigned DLLs
Unmanaged PowerShell Injection	Sysmon Event ID 7 + clr.dll/clrjit.dll in weird processes	Use SilkETW + Microsoft-Windows-DotNETRuntime
Credential Dumping (LSASS Access)	Sysmon Event ID 10	TargetImage = lsass.exe + user ≠ SYSTEM
Suspicious Parent-Child Process	Sysmon Event ID 1	Use ProcessGuid, ParentImage, ParentCommandLine
Encoded PowerShell Commands	Sysmon Event ID 1	.Properties[21].Value -like "*-enc*"
.NET Assembly Load (BYOL attacks)	Sysmon Event ID 7 + ETW: DotNETRuntime	Look for clr.dll, mscoree.dll, and method names with SilkETW
New Share Created (Lateral Movement)	Security Log Event ID 5142	Look for Message with "\\*\PRINT" share name
🧰 Tool-Specific Command Cheats
🔧 Tool	🧪 Purpose / Usage	✅ Sample Command
Get-WinEvent	Query event logs (live or .evtx)	Get-WinEvent -LogName 'System'
FilterHashtable	Filter by LogName, ID, Date, Path	@{LogName='Microsoft-Windows-Sysmon/Operational'; ID=1}
FilterXPath	Advanced filtering with XPath	-FilterXPath "*[EventData[Data='mscoree.dll']]"
FilterXml	Custom XML queries	See: -FilterXml $Query
SilkETW	Capture ETW in JSON (real-time)	SilkETW.exe -t user -pn Microsoft-Windows-DotNETRuntime -uk 0x2038 -ot file -p log.json
logman	View ETW sessions and providers	logman query -ets, `logman query providers
🧠 Important Sysmon Event IDs
📑 Event ID	🗂️ Description
1	Process Create
3	Network Connection
7	Image (DLL) Load
10	Process Access (e.g., LSASS)
11	File Created
13	Registry Value Set
23	File Delete
📎 Useful ETW Providers to Monitor
ETW Provider Name	Purpose / Coverage
Microsoft-Windows-Kernel-Process	All process-related activity (parent-child tracing)
Microsoft-Windows-Kernel-File	File creation/deletion events
Microsoft-Windows-Kernel-Network	Low-level network events
Microsoft-Windows-DotNETRuntime	Managed code assembly loading (Seatbelt, CobaltStrike, etc.)
Microsoft-Windows-PowerShell	Script block execution, logging PowerShell misuse
Microsoft-Windows-Sysmon	Sysmon operational events
Microsoft-Windows-Threat-Intelligence	Highly restricted, advanced APT-level telemetry
