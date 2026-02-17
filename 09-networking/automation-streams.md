When running networking tools in scripts, we manage output using File Descriptors:
- 0 (stdin): Keyboard input.
- 1 (stdout): Successful command output (e.g., "64 bytes from...")
- 2 (stderr): Error messages (e.g., "Network is unreachable").

Imagine a machine where you put fruit in the top and get juice out of one nozzle and pulp (waste) out of another.
- 0 (Input): The fruit you put in.
- 1 (Output): The delicious juice (the data you want).
- 2 (Error): The pulp/scraps (the errors you want to filter out).
---
- When you write > /dev/null, Bash assumes you mean Stream 1. It’s shorthand for 1> /dev/null.
- It sends the "juice" to the trash.
- However, the "pulp" (Stream 2) is still coming out of the other nozzle and landing on your screen!

  ### Translating 2>&1
  - 2: The Error stream (stderr).
  - '>': Redirect it...
  - &: ...to the same place as...
  - 1: ...the Output stream (stdout).

So, ``` ping -c 1 $IP > /dev/null 2>&1 ``` means:
1. Send the Output (1) to the trash.
2. Then, point the Error (2) to follow Output (1) into the trash.

### Practise Questions
> Create a file called ips.txt and put 3 IP addresses in it (e.g., 8.8.8.8, 1.1.1.1, 127.0.0.1). Write a script that reads each IP and tries to ping it. If the ping is successful, print "[IP] is REACHABLE." If it fails, print "[IP] is DOWN."

```
    #!/bin/bash
    set -x
    while read -r IP; do
        if ping -c 1 -W 1 "$IP"> /dev/null 2>&1; then
                echo "$IP is REACHABLE"
        else
                echo "$IP is DOWN"
        fi
done < servers.txt
```
> By setting ``` -W 1 ``` it means to wait for 1 second for the response if the server doesn't respond within 1 second, consider it down

### Servers.txt
> 8.8.8.8
> 1.1.1.1
> 127.0.0.1
