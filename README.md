TeraTerm Macro (.ttl) files designed to factory reset various Cisco devices.
Developed on TeraTerm 5.2 (latest as of time of writing). Older versions may not support some language features

Incredibly WIP. Edge cases exist, and are everywhere, even for supported models.

Additionally, I've probably severely abused/misused language features, as TTL is almost as old as I am, and the docs are translated from Japanese.

> [!WARNING]
> Currently only wipes files from flash. May be extended for license info, etc, in the future

# Usage
- Break into ROMMON / Bootloader
  - Varies depending on model: hold MODE, send break, etc
- Run `master.ttl` from TeraTerm's Control menu
  - `Control -> Macro -> master.ttl`
  - Model is detected automatically based on information obtainable in ROMMON. See Tested Compatible section
- Look cool while it does all of the work

> [!NOTE]
> Debugging can be set with a flag at the top of master.ttl, where individual files can alo be run while preserving the relative paths to shared subroutines.
> Prints additional information to status box

> [!NOTE]
> I recommend notepad++ with [TTL Language support](https://github.com/notepad-plus-plus/userDefinedLanguages/blob/master/UDLs/TeraTermLanguage_allCmdsV4.xml) from NPP's official repo to edit files.

# Tested compatible
- 2960 series switches
  - C2960
  - C2960S
  - C2960X
- 3000 series Switches
  - C3750E
  - 3650 (CAT3K)
- Integrated Service Routers
  - ISR4331

# To do
- ISR
  - Record serial number to check against [FN64253](https://www.cisco.com/c/en/us/support/docs/field-notices/642/fn64253.html)
  - Check must be performed manually. In theory a helper program could grab the login cookie and check cisco's API, but that ain't me.
- Clean up syncrhonization
- Documentation for subroutines
  - Many are re-used as functions to list the flash directory, build a file array, or for logging
- Proper logging
  - Currently only uses `statusbox` with a bunch of custom logic in `stat.ttl` and `db.ttl`.

# Known issues
- After boot, many systems will continue to spam console with diagnostic information as stdout
  - Potentially messes up anything that requires newlines (`waitln`, `recvln`, etc.)
  - Requires looping timeouts, careful regex checks on `dir flash:` output, etc.
  - CAT3K units have `configure terminal` options to disable logging
    - Only requires `wait` commands (no newlines), so is resistant to the spam   
- `files.ttl`, which deletes common files from rommon, rarely only sends 16 characters at a time
  - I genuinely have no idea how or why. I've just split 'send' commands into 16 char chunks whenever it happens
- `wipe_flash.ttl` only has room for 200 files at a time. Arrays cap at 65536 (2^16) elements, but I haven't tested if larger values have any noticeable impact on memory or speed. They shouldn't, since those values are only initialized and not iterated over, but this language does weird stuff all the time.
