## Part 1 – Initial Setup

**Objective:** Baseline config on all 13 devices before any feature work.

**Devices:** R1, CSW1/2, DSW-A1/A2, DSW-B1/B2, ASW-A1/A2/A3, ASW-B1/B2/B3

### Config (apply on every device — change hostname only)

    enable
    configure terminal
    hostname <DEVICE-NAME>
    enable algorithm-type scrypt secret jeremysitlab
    username cisco algorithm-type scrypt secret ccna
    line console 0
     login local
     exec-timeout 30 0
     logging synchronous
    exit
    end
    write memory

> Fallback if PT rejects scrypt: use `enable secret` and `username cisco secret` (type 5)

### Verification

    show running-config | section line con
    show running-config | include username
    show running-config | include enable secret
