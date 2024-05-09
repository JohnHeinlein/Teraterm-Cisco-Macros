TeraTerm Macro (.ttl) files designed to factory reset various Cisco devices.
Developed on TeraTerm 5.2 (latest as of time of writing). Older versions may not support some language features

Incredibly WIP. Edge cases exist, and are everywhere, even for supported models.

Additionally, I've probably severely abused/misused language features, as TTL is almost as old as I am and has barely changed.

> [!NOTE]
> Currently only wipes files from flash. May be extended for license info, etc, in the future

# Usage
- Break into ROMMON / Bootloader
  - Varies depending on model: hold MODE, send break, etc
- Run "master.ttl" from TeraTerm's Control menu
  - Model is detected automatically. See Tested Compatible section
- Look cool while it does all of the work

> [!NOTE]
> Debugging can be set with a flag at the top of master.ttl, where individual files can alo be run while preserving the relative paths to shared subroutines.

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
- Clean up syncrhonization
