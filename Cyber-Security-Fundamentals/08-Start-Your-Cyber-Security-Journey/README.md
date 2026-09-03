# 08 - Start Your Cyber Security Journey 

A Journey into Cybersecurity Fundamentals: From Offensive to Defensive
Introduction

In today's digital world, cybersecurity is no longer a luxury—it's a necessity. But to understand how to protect systems, you must first understand how to break into them. This is the core philosophy behind the "Start Your Cyber Security Journey" pathway, which takes beginners on an integrated journey from the attacker's mindset (Offensive Security) to the defender's mindset (Defensive Security), passing through essential research skills that any cybersecurity professional cannot do without.

In this article, we will explore the key milestones of this journey and explain the concepts learned through practical, hands-on applications.
Station One: Offensive Security – Think Like an Attacker

What is Offensive Security?

Offensive Security is an approach that simulates the actions of hackers (attackers) to discover vulnerabilities and weaknesses in systems before real attackers do. Simply put, it's "thinking like a thief to secure the house."

Hands-On Practice: Hack Your First Website (Legally!)

In this lab, a completely safe environment was provided, simulating a fake banking application called "FakeBank." The task was to find and exploit a vulnerability—but in an ethical and legal manner.

Steps Taken:

    Using the "Dirb" Tool: This tool acts like a "master key" for discovering hidden pages and files on websites. Developers sometimes leave unprotected admin pages or backup files, and this tool helps uncover them.

        The command executed was: dirb http://fakebank.thm

        The tool tests thousands of common words to find any hidden content.

    Discovering the Vulnerability: The scan results revealed a hidden page: /bank-transfer. This page was an admin panel that allowed money transfers and was not adequately protected.

    Exploiting the Vulnerability: Using the account number (8881), a deposit of $2000 was made into the account. The result? The balance went from negative to positive, proving that the vulnerability was real and could be exploited.

Key Takeaway:
Offensive Security isn't just about hacking—it's the art of finding simple weaknesses that could lead to major disasters. A hidden page is a classic example of a simple human error that could cost organizations millions.
Station Two: Defensive Security – Protecting Systems

What is Defensive Security?

While Offensive Security focuses on "breaking in," Defensive Security focuses on "protection, monitoring, and response." The goal is to detect attacks as they happen, analyze them, and stop them before they cause significant damage.

Hands-On Practice: Be a Security Analyst in a SOC (Security Operations Center)

This lab simulated the role of a security analyst in a Security Operations Center. The trainee faced a "typical workday" scenario where suspicious alerts appeared on the monitoring dashboard.

Steps Taken:

    Detection Phase:

        The monitoring dashboard was opened, and alerts were reviewed.

        A suspicious IP address (32.122.195.63) was identified, repeatedly attempting to discover hidden pages on the website (URL Discovery Attempts). This behavior is the beginning of any attack.

    Analysis Phase:

        It wasn't enough to just see the activity; the type of attack had to be identified.

        By reviewing the "URL Discovery Attempts" list, it was determined that the attacker was using a tool similar to "Dirb" to discover website pages—a preparatory attack before the main assault.

    Response Phase:

        In Defensive Security, time is the most critical factor. Immediate "containment" action was taken to prevent the attacker from continuing.

        The attacker's IP address was added to the block list, instantly stopping the attack and protecting the system.

Key Takeaway:
Defensive Security is a race against time. The essential skills here are: vigilant monitoring, rapid analysis, and decisive action to stop threats. Notice that the defender didn't need to "attack" the attacker—just "prevent" them from accessing the system.
Station Three: Search Skills – Your Secret Weapon in Cybersecurity

In cybersecurity, "knowing where to look" is half the battle. This station teaches you how to use specialized search tools that professionals rely on daily.

1. Shodan Search Engine:

    What is it? Often described as a "search engine for internet-connected devices." While Google searches for websites, Shodan searches for devices (cameras, servers, industrial control systems) and reveals what's running on them.

    Practical Example: Searching for apache 2.4.1 will display all servers worldwide using this specific version, helping attackers (or defenders) identify potential targets vulnerable to a known exploit.

2. VirusTotal Platform:

    What is it? A free service that aggregates results from over 70 antivirus engines. It analyzes suspicious files and URLs.

    Practical Example: When uploading a file named invoice_payment.exe (a fictional malicious file), the results showed a large number of antivirus engines flagging it as malware. This tool is indispensable for verifying any file before opening it.

3. CVE Database (Common Vulnerabilities and Exposures):

    What is it? CVE is a global indexing system for every known security vulnerability. Each vulnerability has a unique identifier like CVE-2026-1337.

    Importance: It allows professionals to search for vulnerability details, severity scores (CVSS Score), and exploitation methods. This helps organizations prioritize patching (which vulnerabilities to fix first).

4. Man Pages and GitHub:

    Man Pages: These are the official built-in documentation in Linux systems. For any unfamiliar command, typing man <command> displays everything about its usage and options. This is a golden habit for every Linux user.

    GitHub: A platform for sharing code. In cybersecurity, researchers publish "Proof of Concept" (PoC) code there, demonstrating how vulnerabilities can be exploited. Learning how to read these repositories keeps you up to date with the latest threats.

Conclusion: Why This Pathway Matters?

Completing these three labs puts you on the right starting path to understanding cybersecurity. You have learned:

    How to attack? (to understand vulnerabilities).

    How to defend? (to protect systems).

    Where to search? (to leverage the collective knowledge of the security community).

Cybersecurity is not just a collection of tools—it's a mindset that combines curiosity, vigilance, and continuous learning. This journey is just the beginning, but it's the solid foundation that will enable you to build your skills step by step toward professionalism.
