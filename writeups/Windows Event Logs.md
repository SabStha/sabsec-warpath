📄 Windows Event Logs
Stored in .evtx format → located in C:\Windows\System32\winevt\Logs.

Can't open with Notepad — must use specific tools.

3 ways to view:

Event Viewer (GUI)

wevtutil (Command line)

Get-WinEvent (PowerShell)

These are your go-to logs for things like:

Logins/logoffs

Application crashes

Service starts/stops

Driver failures

🧑‍💻 Osquery
Like having SQL for your endpoint.

Run: osqueryi in CMD or PowerShell.

Query anything: processes, open ports, users, autoruns, etc.

sql
Copy code
SELECT pid, name, path FROM processes WHERE name = 'lsass.exe';
✅ Use Kolide Fleet to run Osquery across many endpoints at once via web UI.
