# Command Line

Executive Overview

Command Line Interfaces (CLIs) and shell environments are the primary mechanisms for interaction, administration, script automation, and security analysis across modern operating systems. Mastery over native terminal interfaces—including the classic Windows Command Prompt (cmd.exe), object-oriented Windows PowerShell, and Unix-like Linux Shells (such as Bash and Zsh)—is essential for executing system diagnostics, conducting security assessments, and automating operational workflows.
1. Foundational Command Line Concepts

The command-line interface provides direct interaction with the underlying operating system kernel without the performance overhead or functional abstractions of a Graphical User Interface (GUI).
Core Operating Principles

    Shell vs. Terminal: A terminal (or terminal emulator) is the interface program that handles user input and output display. A shell is the command interpreter that reads, parses, and executes the entered instructions.

    Standard Input/Output Streams:

        stdin (Standard Input - Stream 0): Receives input commands (typically keyboard).

        stdout (Standard Output - Stream 1): Handles standard non-error output.

        stderr (Standard Error - Stream 2): Separates system error messages from standard output data.

    Environment Variables: Dynamic named values stored within the OS environment that influence process execution (e.g., PATH, USER, TEMP).

2. Windows Command Line (cmd.exe)

The traditional Windows Command Prompt is an environment rooted in the legacy MS-DOS interpreter. It relies on string-based execution and built-in system utilities for administration and network reconnaissance.
System Inspection & File Management

    ver – Displays the installed Windows operating system version.

    dir – Lists directory contents, file attributes, and available disk space (dir /a displays hidden/system files).

    cd [path] – Navigates between directory structures (cd \ redirects to the drive root).

    type [file] – Displays the raw text content of a specified file (functionally equivalent to Linux cat).

    copy / move / del – Basic file manipulation commands for copying, moving, and deleting target files.

Network Reconnaissance & Security Audit Utilities

    ipconfig /all – Displays full network interface configurations, MAC addresses, DNS servers, and active leases.

    netstat -ano – Lists active TCP/UDP connections, listening ports, and corresponding Process IDs (PIDs).

    net user – Enumerates local user accounts (net user [username] inspects group memberships and account flags).

    net localgroup administrators – Audits members assigned local administrative rights.

    tasklist / taskkill – Displays running processes (tasklist) and terminates targets by PID or executable name (taskkill /F /PID [PID]).

3. Windows PowerShell

Windows PowerShell is a task-based command-line shell and scripting framework built on top of the .NET runtime. Unlike legacy text-based shells, PowerShell works natively with .NET objects rather than raw text streams.
Core PowerShell Concepts

    Cmdlets (Command-lets): Built-in commands following a strict Verb-Noun naming convention (e.g., Get-Process, Set-ExecutionPolicy, Start-Service).

    Object Pipeline: Data passed through the pipeline (|) retains its object properties, enabling direct property access without parsing text streams.

Essential Cmdlets & Administration

    System & Service Management:

        Get-Service – Audits system services and their operational states (Running/Stopped).

        Get-Process – Returns active processes, memory usage metrics, and thread counts.

        Get-EventLog / Get-WinEvent – Queries Windows Event Logs for security monitoring and incident investigation.

    File & Network Operations:

        Get-ChildItem (Alias: dir / ls) – Lists items in specified locations (including file systems, Registry drives, and certificate stores).

        Invoke-WebRequest – Sends HTTP/HTTPS requests to REST APIs or downloads remote files.

        Test-NetConnection -ComputerName [Host] -Port [Port] – Validates TCP port connectivity and network routing paths.

Security & Script Execution Control

PowerShell includes execution controls to manage unsigned or untrusted script execution:

    Get-ExecutionPolicy – Checks the current script execution policy.

    Set-ExecutionPolicy Bypass -Scope Process – Temporarily bypasses execution restrictions within the active process context for administrative or assessment scripts.

4. Linux Shells (Bash & Zsh)

Linux shells (such as Bash - Bourne Again Shell, and Zsh - Z Shell) provide command-line environments based on POSIX standards, text-stream processing, and modular utility piping.
Environment & Shell Navigation

    echo $SHELL – Displays the active default shell binary path (e.g., /bin/bash or /bin/zsh).

    pwd – Prints the absolute path of the current working directory.

    ls -la – Returns long-format directory listings, displaying hidden dotfiles, ownership, and POSIX permission bits.

    cd [path] – Moves across directory hierarchies (cd - toggles back to the previous working directory).

Text Stream Manipulation & Processing Tools

Linux environments excel at text parsing and log analysis by chaining utility output through pipelines (|):

    grep [pattern] [file] – Filters lines matching regular expression patterns (grep -rn "password" /var/log/ performs recursive searching).

    awk – A pattern scanning and processing language used to isolate specific data fields (e.g., awk '{print $1}' extracts the first column of output).

    sed – A stream editor used for text transformation and search-and-replace operations.

    cut / sort / uniq – Utilities used to slice text strings, sort dataset entries, and eliminate duplicate lines.

Shell Scripting Primitives

Linux shells allow task automation through executable script files (.sh):
Bash

#!/bin/bash
# Example basic network ping sweep script
for ip in $(seq 1 254); do
    ping -c 1 192.168.1.$ip | grep "64 bytes" | cut -d " " -f 4 | tr -d ":" &
done

Technical Summary for Clients

    Executive Client Deliverable:
    The Command Line and Shell Environment Fundamentals module provides foundational operational mastery across major operating system CLI frameworks. It contrasts text-based administrative interfaces (cmd.exe, Bash, Zsh) with object-oriented pipeline environments (Windows PowerShell). Developing proficiency in file management, network socket auditing, text-stream filtering, process handling, and script automation enables security researchers and system administrators to conduct effective environment auditing, execute rapid incident response procedures, and perform automated infrastructure assessments.
