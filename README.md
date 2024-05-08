TeraTerm Macro (.ttl) files designed to factory reset various Cisco devices.
Developed on TeraTerm 5.2 (latest as of time of writing). Older versions may not support some language features

Once in the ROMMON/bootloader, master.ttl will attempt to detect the type of device and run appropriate reset procedures automatically.

These macros DO NOT do the initial break into rommon, as this varies dramatically depending on previous configurations, models, and may 
ultimately require physical access. Some models, e.g. CAT3K/ISR units, will require a second entry into rommon to restore confreg/boot vars. These subsequent reboots ARE handled automatically, as more assumptions can be made once the model is known.

Incredibly WIP. Edge cases exist, and are everywhere, even for supported models.

Additionally, I've probably severely abused/misused language features, as TTL is almost as old as I am and has barely changed the entire time.

> [!NOTE]
> Currently only wipes files from flash. May be extended for license info, etc, in the future

# Tested compatible
- 2960 series switches
  - C2960
  - C2960S
  - C2960X
- Catalyst 3000 series (CAT3K) Switches
  - 3650
- Integrated Service Routers
  - ISR4331

# To do
- ISR
  - Record serial number to check against [FN64253](https://www.cisco.com/c/en/us/support/docs/field-notices/642/fn64253.html)
  - Check must be performed manually. In theory a helper program could grab the login cookie and check cisco's API, but that ain't me.
