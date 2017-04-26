# Pokémon Encounters

Since the IV update of April 21st which makes IVs the same for players of level 25 and above, the encounter system has been reworked and now includes CP/IV scanning.

Steps for using the new encounter system:

1. Enabled encounters on your map (`-enc`).
2. Add L25 (and optionally L30) accounts for IV scanning into a CSV file (separate from your regular accounts file, e.g. 'high-level.csv'). The lines should be formatted as "25 or 30,service,user,pass":
   ```
   25,ptc,randOMusername1,P4ssw0rd!
   25,ptc,randOMusername2,P4ssw0rd!
   30,ptc,randOMusername1,P4ssw0rd!
   ```
   The config item or parameter to use this separate account file is:
   ```
   --high-lvl-accounts high-level.csv
   ```
3. Create files for your IV and CP encounter whitelists and add the Pokémon IDs which you want to encounter, one per line.
   iv-whitelist.txt:
   ```
   10
   25
   38
   168
   ```
   cp-whitelist.txt:
   ```
   35
   66
   89
   ```
4. Enable the whitelist files in your config or cli parameters (check commandline.md for usage):
   ```
   --iv-whitelist-file iv-whitelist.txt --cp-whitelist-file cp-whitelist.txt
   ```
5. Optionally set a speed limit for your high level accounts. This is separate from the usual speed limit, to allow a lower speed to keep high level accounts safer:
   ```
   --hlvl-kph 25
   ```

L25/L30 accounts are not being recycled and are not in the usual account flow. This is intentional, to allow for future reworks to handle accounts properly. This also keeps interaction with high level accounts to a minimum. We can consider handling them more automatically when the account handlers are properly fully implemented.

Some important notes:

 * The old encounter whitelists/blacklists have been removed entirely.
 * Both the IV and CP whitelists are optional.
 * When no L25 accounts are available but IV encounters are enabled, it will try to get a L30 account instead for the encounter.
 * Captcha'd L25/L30 accounts will be logged in console and disabled in the scheduler. Having `-v` enabled will show you an entry in the logs mentioning "High level account x encountered a captcha". They will not be solved automatically.
 * The encounter is a single request (1 RPM). We intentionally don't use the account for anything else besides the encounter.
 * The high level account properly uses a proxy if one is set for the scan, and properly rotates hashing keys when needed.
