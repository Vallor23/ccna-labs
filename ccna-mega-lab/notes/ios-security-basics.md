# IOS Security Basics

Reference notes for commands used across the lab series. Read this once — it won't be repeated in individual part reports.

---

## Password Hashing: Type 9 vs Type 5

Cisco IOS stores passwords as hashes, not plaintext. The hashing algorithm determines how hard it is to crack a stolen config file.

| Type | Algorithm | Speed to crack | Use when |
|------|-----------|---------------|----------|
| Type 5 | MD5 | Fast — crackable in seconds with modern hardware | PT doesn't support type 9 |
| Type 9 | scrypt | Slow — memory-hard, impractical to brute-force | IOS 15.3(3)M+ / Catalyst 15.x |

Type 9 is the current Cisco recommendation. Always prefer it. Type 7 (reversible XOR) should never be used for anything security-sensitive.

**Commands:**
```
! Type 9
enable algorithm-type scrypt secret <password>
username <name> algorithm-type scrypt secret <password>

! Type 5
enable secret <password>
username <name> secret <password>
```

---

## enable secret vs enable password

Always use `enable secret`, never `enable password`.

`enable password` stores type 7 (reversible). `enable secret` stores a real hash. If both exist on a device, `enable secret` takes precedence.

---

## login local — How it works

`login local` on a line (console or VTY) tells IOS to check the local username database when someone connects. Without it, one of two things happens depending on the line config:

- If `login` (no `local`) is set — IOS prompts for a line password (`password <pw>` on the line)
- If neither is set — the line connects with no authentication at all

This is why `login local` only works if at least one username is configured. If the local database is empty, the line will accept connections without prompting — same as having no login configured.

---

## exec-timeout

```
exec-timeout <minutes> <seconds>
```

Kills an idle EXEC session after the specified time. `exec-timeout 30 0` = 30 minutes. `exec-timeout 0 0` = never times out — acceptable in a lab, never in production.

Without a timeout, a forgotten console or VTY session stays authenticated indefinitely.

---

## logging synchronous

Without this, IOS prints syslog messages to the terminal the moment they're generated — including mid-command. The result looks like this:

```
R1(config)# interface g
*Mar 1 00:02:14: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed state to up
igabitEthernet0/0
```

The log message split your command. You didn't notice. The config is wrong.

`logging synchronous` holds log output until you press Enter, keeping your input line clean.
