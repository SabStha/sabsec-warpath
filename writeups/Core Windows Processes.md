These are legitimate processes that run every time Windows boots. Some have parent-child relationships, which matter during threat hunting.

Legit Process Tree:

markdown
Copy code
System  
└── smss.exe  
    ├── csrss.exe  
    ├── wininit.exe  
        ├── services.exe  
        │   └── svchost.exe  
    ├── lsass.exe  
    └── winlogon.exe  
        └── explorer.exe
      
Notes:
System should only have PID 4 and parent System Idle Process (0).

wininit.exe, services.exe, and svchost.exe follow a strict parent-child chain.

If svchost.exe is running but wasn’t started by services.exe, 🚨 that's suspicious.

