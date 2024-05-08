TeraTerm Macro (.ttl) files designed to factory reset various Cisco devices.

Once in ROMMON, master.ttl will attempt to detect the type of device and run appropriate reset procedures.

Currently tested on:
- 2960 series (C2960, C2960S, C2960X) switches
  - Determined by "version" command in ROMMON
- ISR4331
  - Determined if "version" command is NOT FOUND.

Incredibly WIP. Edge cases exist, and are everywhere, even for tested compatible models.

For one, the scripts will fail if the flash contains more than 511 characters of combined file names.
Additionally, I've probably severely abused/misused synchronization, and subsequently the recvln buffer. This language is as old as I am 
