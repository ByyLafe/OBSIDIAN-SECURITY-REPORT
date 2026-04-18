# Obsidian — Security Audit Report

Black-box penetration testing and reverse engineering of the Obsidian binary a simulated nuclear power plant control system.

Authors : @ByyLafe & @tom-larconnier
Reference: G-SEC-210
Date: April 16, 2026
Provider: The Stone Corporation — Purple Team Division

## Context
This project is a security audit carried out as part of a CTF-style exercise. The target, obsidian, is a binary simulating a nuclear reactor control interface. The audit was conducted using a black-box methodology: no access to source code, only the compiled binary and its associated shared library (obsidian_lib.so).
The objective was to identify vulnerabilities before a hypothetical attack by the G.O.L.E.M. organization on the infrastructure of the Republic of Obsidian.

## Tools Used

Ghidra — static analysis, decompilation, hardcoded string discovery
Binary Ninja — control flow graph visualization, dispatch table mapping
GDB — dynamic analysis, stack monitoring, buffer overflow confirmation
Linux utilities — strings, ltrace, strace for initial reconnaissance
Scrapy + Browser DevTools (F12) — JavaScript de-obfuscation of the web portal

## Key Findings

Hardcoded credentials appear in multiple locations, both in the binary (admin123, plain strcmp) and in the obfuscated JavaScript of the web portal (Jordan / cherry), recoverable via browser console.
Undocumented commands such as unlock_secret_mode and trigger_emergency_shutdown are present in a dispatch table omitted from the help menu, discoverable through reverse engineering.
Unsafe C functions (gets, strncpy without null-termination) enable stack-based buffer overflows, allowing function pointer overwrites via raw byte injection through pipe redirection.
Debug artifacts were left in the production binary: hidden branches, plaintext flags, diagnostic keys, and a 5%-probability meltdown simulation path.
Race condition: the turbine_remote_access function writes a secret to a predictable temporary file in Data/, then sleeps 5 seconds before deletion trivially exploitable.

## Methodology

All tests followed the rules of engagement agreed between the client and The Stone Corporation. No production systems were targeted. The audit represents a point-in-time assessment; it does not guarantee the absence of vulnerabilities outside the defined scope.

## Disclaimer
### This report is the exclusive property of The Stone Corporation and is intended solely for the government of the Republic of Obsidian. Redistribution or malicious use of its contents is strictly prohibited.

© 2026 Stone Corporation — All rights reserved.
