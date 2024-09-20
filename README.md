# hcx-scripts

[![Python 3.12.5](https://img.shields.io/badge/Python-3.12.5-yellow.svg)](http://www.python.org/download/)

Useful python scripts for cracking/processing WPA-PBKDF2-PMKID+EAPOL hashes and passwords.

## Installation

### Requirements
* python
* hashcat
* hcxtools
* hcxdumptool
* [hcx-fastgenlst](https://github.com/0000xFFFF/hcx-fastgenlst)

### Requirements - pip
* colorama
* tabulate
* scapy
* getkey

### Run before usage
```
./install.sh
```
This will just `ln -sfr <scripts> /usr/local/bin/.`, some scripts depend on each other...

## Processing hashes
```
hcx-info hashes.txt    - display a nice table for hashes in file
                         (MACs, BSSIDs, ESSIDs, passwords, vendor info, ...)
                         fetches passwords from hashcat if any cracked hashes detected,
                         display vendor info with -v for all macs...
hcx-cracker hashes.txt - crack wifi passwords by their essids
hcx-potfile            - display a nice table for all hashcat passwords in potfile
```

#### Examples:
###### ./hcx-info hashes.txt
```
    #  TYPE    HASH                              MAC AP        MAC CLIENT    ESSID                            PASSWORD               VENDOR AP    VENDOR CLIENT
-----  ------  --------------------------------  ------------  ------------  -------------------------------  ---------------------  -----------  ---------------
    1  EAPOL   4hj32jkh5jk34h5klj324hk5jh4kjkh5  efefefefefef  dededededeed  testwifi                         test1234
    2  PMKID   9214h2314kjh132kj4h321h4lkj23hhj  edededdedede  fefefefefefe  anothertestwifi                  testing123
```

##### crack wifi passwords by essids
```
./hcx-cracker hashes.txt -ab    # generates gen and run scripts
./gen.sh                        # generates wordlists by network ESSID for each network
./run.sh                        # runs hashcat with generated wordlists
```

## Capturing hashes with raspberry pi and hcxdumptool
```
hcx-rpidump       - small script that starts hcxdumptool when wlan1
                    device is connected to raspberry pi
hcx-rpidump-wmenu - rasberry pi waveshare menu for starting hcxdumptool
```

## Generate password wordlists for cracking
Use newer version: [hcx-fastgenlst](https://github.com/0000xFFFF/hcx-fastgenlst)

```
hcx-genlst           - name + numer, number + name, number + name + number
hcx-genlst-num8      - numbers from 00000000 to 99999999
hcx-genlst-numcommon - common numbers (dates, etc.)
hcx-genlst-upper8    - generate upper ascii with length 8
```

#### Examples:
```
hcx-genlst -lut123 -s steve
# will generate a wordlist that has passwords like: steve66, 123Steve, 69STEVE69, ...
# -l -- lower word variation
# -u -- UPPER word variation
# -t -- Title word variation
# -1 -- word + int
# -2 -- int + word
# -3 -- int + word + int
# ..... use -h to show other options...
```

## Reacon after cracking
```
hcx-wifi            - airodump-ng clone written in python that shows you passwords of
                      nearby networks you have cracked with hashcat
hcx-wifi-genpasslst - generate password csv list for hcx-wifi
```

#### Examples:
##### ./hcx-wifi-genpasslst hashes.txt > passlst.csv
##### ./hcx-wifi wlan1mon passlst.csv
```
CH  4 | 2024-09-07 22:46:13.812907 | COUNT: 21 | PASS: 10 (3) | SORT BY: ↓ PWR
> RESUMED CHANNEL HOPPER

BSSID              ESSID             PASSWORD      PWR  LAST SEEN              #    CH  CRYPTO                       TIMESTAMP    PACKETS
-----------------  ----------------  ----------  -----  -------------------  ---  ----  -----------------------  -------------  ---------
48:8E:EF:E6:55:22  My Home Network   password1     -37  2024-09-07 22:46:13    6     1  {'WPA2/PSK'}              211487642022          3
96:9A:4A:7E:7E:7E  Network Test 1    123456789     -51  2024-09-07 22:46:13   18     4  {'WPA2/PSK'}             6445574041984          5
90:9A:4A:97:77:66  Super Fast AP     ...           -63  2024-09-07 22:46:13   20     4  {'WPA2/PSK', 'WPA/PSK'}  6445573529984          2
```

## Misc scripts that should be manually modified
```
hcx-cap   - extract info from newly captured cap/pcapng files
hcx-new   - get newly captured hashes that are not in main hashes db
hcx-fetch - grep hcx-info for main hashes db
```

## Disclaimer
The hcx-scripts are intended for educational purposes only.
The author is not responsible or liable for any misuse, illegal activity, or damage caused by the use of these scripts.
Users are solely responsible for ensuring compliance with applicable laws and regulations.
