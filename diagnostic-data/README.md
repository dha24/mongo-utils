Retrieve Diagnostic Data
=====================

mdiag.sh
--------

### Description

mdiag.sh is a utility to gather a wide variety of system and hardware diagnostic information.

### Usage

```
sudo bash mdiag.sh _referenceno_
```

- Running without sudo (ie. as a regular user) is possible, but less information will be collected.
- It is not necessary to `chmod` the script.
- Bash version 4.0 or later is required.
- The program will generate an output file in `/tmp/mdiag-$HOSTNAME.txt` with the information.

