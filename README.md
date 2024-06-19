TeraTerm Macro (.ttl) files designed to factory reset various Cisco devices.
Developed on TeraTerm 5.2 (latest as of time of writing). Older versions may not support some language features.

Currently pretty solid, especially for 2960 series switches. Some edge cases absolutely exist due to the [unreliable](https://en.wikipedia.org/wiki/Reliability_(computer_networking)) nature of RS-232 as these devices use it.
I've attempted to get around some issues by looping checks, sending extra characters, etc, but this will always be fundamentally flawed.

Additionally, I've probably abused some language features. TTL is almost as old as I am, and the docs seem to be (sometimes poorly) translated from Japanese.

> [!WARNING]
> Currently only wipes non-essential files from flash. May be extended for license info, etc, in the future. Always double-check!
> 
> 'Essential file' is determined by matching the regular expression found in `util/flash_wipe.ttl`:
> 
> `((SPA)|(PTW)|(SE[0-9])|(E(X?)[0-9])|(\.bin)|(\.conf)|(\.lic)|(\.pkg)|(\.pack)$)`
> 
> (Essentially a list of file extensions)

> [!NOTE]
> I recommend notepad++ with [TTL Language support](https://github.com/notepad-plus-plus/userDefinedLanguages/blob/master/UDLs/TeraTermLanguage_allCmdsV4.xml) from NPP's official repo to edit files.

# Usage
- Break into ROMMON / Bootloader
  - Varies depending on model: hold MODE, send break, etc
- Run `master.ttl` from TeraTerm's Control menu
  - `Control -> Macro -> master.ttl`
  - Model is detected automatically based on information obtainable in ROMMON.
- Look cool while it does all of the work

> [!NOTE]
> Debugging can be set with a flag at the top of master.ttl, where individual files can alo be run while preserving the relative paths to shared subroutines.
> Prints additional information to status box

# Tested compatible
- 2960 series switches (C2960, C2960S, C2960X)
  - WS-C2960-24PC-L / WS-C2960+24PC-L
  - WS-C2960-48TT-L
  - WS-C2960-48PST-L
  - WS-C2960X-24PS-L
  - WS-C2960X-48TD-L
  - WS-C2960X-48FPS-L
  - WS-C2960X-48LPS-L
- 3000 series switches (C3750E, CAT3K)
  - WS-C3750X-24P-S
  - WS-C3750X-48PF-S
  - WS-C3650-24TS-E
  - WS-C3650-24PS-S
- Integrated service routers
  - ISR4331 (See [FN64253](https://www.cisco.com/c/en/us/support/docs/field-notices/642/fn64253.html))
  - 2901
- Gateways
  - VG310 (Detected as a router)
- Adaptive Security Appliances
  - ASA 5516-X (Only one unit so far)

> [!NOTE]
> More models than listed may work, given a ROMMON & OS structure similar to existing models.
> For example, 3750's are essentially identical to 2960's, so we just call those subroutines.

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
- Reboot breaks sometimes don't register. Sending a bunch of them seems to make it fail less. That's pretty cool.
