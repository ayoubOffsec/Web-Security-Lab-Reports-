# Windows and AD Fundamentals

Executive Overview

Microsoft Windows and Active Directory (AD) form the core identity and access management (IAM) framework for enterprise networks worldwide. Understanding the security mechanisms, administrative tools, access control lists (ACLs), authentication protocols, and directory structures across Windows operating systems is critical for vulnerability assessments, privilege escalation audits, and enterprise domain hardening.
1. Windows Fundamentals - Part 1: Architecture, GUI & Core Tools

Part 1 establishes familiarity with the Windows operating system architecture, essential administrative utilities, and basic file management.
System Architecture & File System

    Architecture Overview: Windows utilizes a dual-mode architecture comprising User Mode (where applications and subsystems execute with restricted privileges) and Kernel Mode (where core OS components, hardware drivers, and system services run with unrestricted access).

    NTFS File System: The standard file system offering features such as File System Encryption (EFS), compression, transactional logging, and fine-grained access control via NTFS Access Control Lists (ACLs).

    Core File Locations:

        C:\Windows\System32\ – Contains critical OS binaries, DLLs, and administrative executables.

        C:\Users\ – Stores user profiles, application configurations, and desktop environments.

        C:\Program Files & C:\Program Files (x86) – Standard installation directories for 64-bit and 32-bit software.

Primary Administrative Consoles

    Task Manager (taskmgr.exe): Monitors real-time resource utilization, active applications, background processes, and startup programs.

    System Information (msinfo32.exe): Displays detailed summaries of hardware, system components, and software environments.

    Control Panel & Settings App: Provide Graphical User Interfaces (GUI) for system-wide adjustments, network configuration, and account management.

2. Windows Fundamentals - Part 2: Security Features, Firewall & Management Utilities

Part 2 explores built-in security features, perimeter filtering, system auditing, and administrative utilities.
Operating System Hardening & Security Controls

    User Account Control (UAC): Prevents unauthorized administrative changes by requiring administrative consent or credentials before executing programs with elevated privileges.

    Windows Defender Antivirus & SmartScreen: Provides real-time signature and heuristic protection against malicious executables, scripts, and network threats.

    Windows Host Firewall: Filters inbound and outbound traffic based on rules defined by IP address, protocol, port, or specific application.

Advanced Management Tools

    Computer Management (compmgmt.msc): An integrated management console providing access to:

        Local Users and Groups: Administers local system accounts and security groups.

        Device Manager: Controls connected hardware components and driver installations.

        Disk Management: Configures disk partitions, volumes, and file systems.

    Event Viewer (eventvwr.msc): Centralizes system, application, and security logging.

        Security Application: Auditing Event ID 4624 (Successful Logon), 4625 (Failed Logon), and 4672 (Special Privileges Assigned) during security monitoring and incident response.

3. Windows Fundamentals - Part 3: Command Line, PowerShell & Registry Operations

Part 3 covers command-line administration, automation using PowerShell, system management via the Windows Registry, and service handling.
Command Line (CMD) & Network Reconnaissance

    whoami /priv – Displays the security privileges assigned to the current user token (e.g., SeDebugPrivilege, SeImpersonatePrivilege).

    net user [username] – Queries or modifies local user account properties.

    net localgroup administrators – Lists members holding local administrative rights.

    ipconfig /all – Displays network interface configurations, DNS servers, and DHCP leased addresses.

    netstat -ano – Displays active network connections, listening ports, and corresponding Process IDs (PIDs).

Windows PowerShell & Scripting

PowerShell is an object-oriented shell and scripting language built on the .NET framework:

    Get-Service – Lists all installed services and their operational status.

    Get-Process – Enumerates running processes and memory allocation.

    Get-ExecutionPolicy – Displays the local PowerShell script execution policy (e.g., Restricted, RemoteSigned, Bypass).

    Invoke-WebRequest -Uri [URL] -OutFile [Path] – Downloads files over HTTP/HTTPS.

Windows Registry (regedit.exe)

A centralized hierarchical database used to store operational configuration settings for the OS, software, and hardware drivers:

    HKEY_LOCAL_MACHINE (HKLM): Stores system-wide configuration settings (applies to all users).

    HKEY_CURRENT_USER (HKCU): Stores environment preferences specific to the currently logged-in user.

    Persistence Vector: Persistence mechanisms often target Registry run keys, such as:
    HKLM\Software\Microsoft\Windows\CurrentVersion\Run

4. Active Directory Basics: Enterprise Identity & Domain Architecture

Active Directory Domain Services (AD DS) is Microsoft's centralized identity management and directory service designed for enterprise environments.
Core Active Directory Structures

    Domain: A logical administrative boundary containing shared network objects (users, computers, groups, printers).

    Domain Controller (DC): A server running AD DS that authenticates users, enforces security policies, and stores the directory database (NTDS.dit).

    Organizational Units (OUs): Subdivision containers within a domain used to group objects logically for administrative delegation and policy application.

    Trees & Forests: A Tree is a collection of contiguous domain namespaces; a Forest is the highest organizational boundary containing one or more trees that share a common schema, global catalog, and trust relationships.

Identity Management & Group Policy

    Security Groups:

        Domain Admins: High-privilege group granting administrative control across all domain controllers and domain-joined assets.

        Enterprise Admins: Highest privilege tier within a forest structure.

    Group Policy Objects (GPOs): Virtual collections of policy settings configured by administrators to enforce security baselines, deploy software, or restrict local user rights across domain-joined systems.

Authentication Protocols & Security

    Kerberos Authentication: The default ticket-based authentication protocol in Active Directory domains using a trusted third party—the Key Distribution Center (KDC)—to issue Ticket Granting Tickets (TGTs) and Service Tickets (TGSs).

    NTLM Authentication: A legacy challenge-response authentication protocol used when Kerberos is unavailable or for local authentication.

    Active Directory Administrative Tools:

        dsa.msc – Active Directory Users and Computers (ADUC).

        gpmc.msc – Group Policy Management Console.

Technical Summary for Clients

    Executive Client Deliverable:
    The Windows and Active Directory Fundamentals series provides a comprehensive foundation for enterprise identity management, systems administration, and security architecture. It details the operational controls governing local Windows environments—including POSIX-equivalent NTFS ACLs, user rights, and PowerShell management—and extends these concepts to centralized Active Directory domain environments. Understanding kerberized domain authentication, directory structures, Group Policy Object (GPO) enforcement, and privilege hierarchies is vital for conducting security assessments, auditing domain trust boundaries, and hardening corporate IT infrastructure.
