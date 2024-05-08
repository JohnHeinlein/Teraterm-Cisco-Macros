TeraTerm Macro (.ttl) files designed to factory reset various Cisco devices.
Developed on TeraTerm 5.2 (latest as of time of writing). Older versions may not support some language features

Once in the ROMMON/bootloader, master.ttl will attempt to detect the type of device and run appropriate reset procedures automatically.

These macros DO NOT do the initial break into rommon, as this varies dramatically depending on previous configurations, models, and may 
ultimately require physical access. Some models, e.g. CAT3K/ISR units, will require a second entry into rommon to restore confreg/boot vars. These subsequent reboots ARE handled automatically, as more assumptions can be made once the model is known.

Additionally, these macros currently only wipe files from flash. The modular design makes it easy to add additional commands between the wipe and reboot, and can thus be extended to perform a more deep clean on e.g. license infomation.

Currently tested on:
- 2960 series (C2960, C2960S, C2960X) switches
  - Determined by "version" command in ROMMON
- Catalyst 3000 series (CAT3K) Switches
  - Determined by "version" command in ROMMON
- ISR4331
  - Determined if "version" command is NOT FOUND.

Incredibly WIP. Edge cases exist, and are everywhere, even for supported models.

Additionally, I've probably severely abused/misused language features, as TTL is almost as old as I am and has barely changed the entire time.
