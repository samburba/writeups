# Key Mission
Its a `pcap` file that is all usb transfer type. This is a USB keyboard, which I confirmed with  [this post](https://book.hacktricks.xyz/forensics/pcaps-analysis/usb-keyboard-pcap-analysis) on https://book.hacktricks.xyz/.
I used this [tool and guide](https://github.com/TeamRocketIst/ctf-usb-keyboard-parser) by TeamRocketlst.

```bash=
$ tshark -r ./key_mission.pcap -Y 'usb.capdata && usb.data_len == 8' -T fields -e usb.capdata | sed 's/../:&/g2' > usbPcapData

$ python usbkeyboard.py usbPcapData                                                                                                                                                                                                 
I am sendg secretary's location over this totally encrypted channel to make sure no one else will be able to read it except of us. This information is confidential and must not be shared with anyone else. The secretary's hidden location is CHTB{a_plac3_fAr_fAr_away_fr0m_earth}
```

The flag is `CHTB{a_plac3_fAr_fAr_away_fr0m_earth}`