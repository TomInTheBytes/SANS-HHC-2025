# Mail Detective

Difficulty: :material-star::material-star::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Help Mo in City Hall solve a curly email caper and crack the IMAP case. What is the URL of the pastebin service the gnomes are using?

??? quote "Maurice Wilson"

    Hey there! I'm Mo, on loan from the Air Force, and let me tell you - Counter Hack is the best job I have ever had!

    There is something in the works. Come see me later and we will talk. Can't say much right now, but check back later. You might be interested.

    So here's our situation: those gnomes have been sending JavaScript-enabled emails to everyone in the neighborhood, and it's causing chaos.

    We had to shut down all the email clients because they weren't blocking the malicious scripts - kind of like how we'd ground aircraft until we clear a security threat.

    The only safe way to access the email server now is through curl - yes, the HTTP tool!

    Think you can help me use curl to connect to the IMAP server and hunt down one of these gnome emails?

## Hints

??? tip "Did You Say Curl?"

    If I heard this correctly...our sneaky security gurus found a way to interact with the IMAP server using Curl! Yes...the CLI HTTP tool! Here are some helpful docs I found `https://everything.curl.dev/usingcurl/reademail.html`

## Solution

We can leverage the docs linked in the hint to enumerate the mail server using the following steps:

```sh title="List folders"
dosismail @ Neighborhood Mail ~$ curl 'imap://127.0.0.1' -u dosismail:holidaymagic
* LIST (\HasNoChildren) "." Spam
* LIST (\HasNoChildren) "." Sent
* LIST (\HasNoChildren) "." Archives
* LIST (\HasNoChildren) "." Drafts
* LIST (\HasNoChildren) "." INBOX
```

```sh title="List number of mails per folder"
dosismail @ Neighborhood Mail ~$ curl 'imap://127.0.0.1' -u dosismail:holidaymagic -X 'STATUS Spam (MESSAGES)'
* STATUS Spam (MESSAGES 3)
dosismail @ Neighborhood Mail ~$ curl 'imap://127.0.0.1' -u dosismail:holidaymagic -X 'STATUS Sent (MESSAGES)'
* STATUS Sent (MESSAGES 0)
dosismail @ Neighborhood Mail ~$ curl 'imap://127.0.0.1' -u dosismail:holidaymagic -X 'STATUS Archives (MESSAGES)'
* STATUS Archives (MESSAGES 7)
dosismail @ Neighborhood Mail ~$ curl 'imap://127.0.0.1' -u dosismail:holidaymagic -X 'STATUS Drafts (MESSAGES)'
* STATUS Drafts (MESSAGES 2)
dosismail @ Neighborhood Mail ~$ curl 'imap://127.0.0.1' -u dosismail:holidaymagic -X 'STATUS INBOX (MESSAGES)'
* STATUS INBOX (MESSAGES 7)
```

```sh title="Get email 2 in spam folder containing the solution (complete email below)"
dosismail @ Neighborhood Mail ~$ curl 'imap://127.0.0.1/Spam;UID=2' -u dosismail:holidaymagic
Return-Path: <frozen.network@mysterymastermind.mail>
Delivered-To: dosis.residents@dosisneighborhood.mail
Received: from frost-command.mysterymastermind.mail (frost-command [10.0.0.15])
        by mail.dosisneighborhood.mail (Postfix) with ESMTP id GHI789
        for <dosis.residents@dosisneighborhood.mail>; Mon, 16 Sep 2025 12:10:00 +0000 (UTC)
From: "Frozen Network Bot" <frozen.network@mysterymastermind.mail>
To: "Dosis Neighborhood Residents" <dosis.residents@dosisneighborhood.mail>
Cc: "Jessica and Joshua" <siblings@dosisneighborhood.mail>, "CHI Team" <chi.team@counterhack.com>
Subject: Frost Protocol: Dosis Neighborhood Freezing Initiative
Date: Mon, 16 Sep 2025 12:10:00 +0000
Message-ID: <gnome-js-3@mysterymastermind.mail>
MIME-Version: 1.0
Content-Type: text/html; charset=UTF-8
Content-Transfer-Encoding: 7bit

...
```

??? note "Complete spam email 2"

    ``` html
    <html>
    <body>
    <h1>Perpetual Winter Protocol Activated</h1>
    <p>The mysterious mastermind's plan is proceeding... Dosis neighborhood will never thaw!</p>
    <script>
    function initCryptoMiner() {
        var worker = {
            start: function() {
                console.log("Frost's crypto miner started - mining FrostCoin for perpetual winter fund");
                this.interval = setInterval(function() {
                    console.log("Mining FrostCoin... Hash rate: " + Math.floor(Math.random() * 1000) + " H/s");
                }, 5000);
            },
            stop: function() {
                clearInterval(this.interval);
            }
        };
        worker.start();
        return worker;
    }

    function exfiltrateData() {
        var sensitiveData = {
            hvacSystems: "Located " + Math.floor(Math.random() * 50) + " cooling units",
            thermostatData: "Temperature ranges: " + Math.floor(Math.random() * 30 + 60) + "°F",
            refrigerationUnits: "Found " + Math.floor(Math.random() * 20) + " commercial freezers",
            timestamp: new Date().toISOString()
        };

        console.log("Exfiltrating data to Frost's command center:", sensitiveData);

        var encodedData = btoa(JSON.stringify(sensitiveData));
        console.log("Encoded payload for Frost: " + encodedData.substr(0, 50) + "...");

        // pastebin exfiltration
        var pastebinUrl = "https://frostbin.atnas.mail/api/paste";
        var exfilPayload = {
            title: "HVAC_Survey_" + Date.now(),
            content: encodedData,
            expiration: "1W",
            private: "1",
            format: "json"
        };

        console.log("Sending stolen data to FrostBin pastebin service...");
        console.log("POST " + pastebinUrl);
        console.log("Payload: " + JSON.stringify(exfilPayload).substr(0, 100) + "...");
        console.log("Response: {\"id\":\"" + Math.random().toString(36).substr(2, 8) + "\",\"url\":\"https://frostbin.atnas.mail/raw/" + Math.random().toString(36).substr(2, 8) + "\"}");
    }

    function establishPersistence() {
        // Service worker registration
        if ('serviceWorker' in navigator) {
            console.log("Attempting to register Frost's persistent service worker...");
            console.log("Frost's persistence mechanism deployed");
        }

        localStorage.setItem("frost_persistence", JSON.stringify({
            installDate: new Date().toISOString(),
            version: "gnome_v2.0",
            mission: "perpetual_winter_protocol"
        }));
    }

    var miner = initCryptoMiner();
    exfiltrateData();
    establishPersistence();

    document.title = "Frost's Gnome Network - Temperature Control";
    alert("All cooling systems in Dosis neighborhood are now property of Frost!");
    document.body.innerHTML += "<p style='color: cyan;'>❄️ FROST'S DOMAIN ❄️</p>";

    // Cleanup after 30 seconds
    setTimeout(function() {
        miner.stop();
        console.log("Frost's operations going dark... tracks covered");
    }, 30000);
    </script>
    </body>
    </html>
    ```

??? success "Solution"

    The solution to this challenge is `https://frostbin.atnas.mail/api/paste`.

## Images

![answer](../media/mail_detective/mail_detective_1)
/// caption
Challenge terminal.
///

## Response

!!! quote "Maurice Wilson"

    Outstanding work! You've mastered using curl for IMAP - that's some serious command-line skills that would make any Air Force tech proud.

    Counter Hack really is the best job I have ever had, especially when we get to solve problems like this!
