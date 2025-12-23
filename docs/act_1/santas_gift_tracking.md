# Santa's Gift-Tracking Service Port Mystery

Difficulty: :material-star::material-star-outline::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Chat with Yuri near the apartment building about Santa's mysterious gift tracker and unravel the holiday mystery.

??? quote "Yuri"

    Hi! I'm Yori.

    I was Ed's lost intern back in 2015, but I was found!

    Think you can check out this terminal for me? I need to use cURL to access the gift tracker system, but it has me stumped.

    Please see what you can do!

## Hints

??? tip "Web Requests without a Browser??"

    Since we don't have a web browser to connect to this HTTP service...There is another common tool that you can use from the cli.

??? tip "Who is Netstat?"

    Back in my day...we just used Netstat. I hear `ss` is the new kid on the block. A lot of the parameters are the same too...such as listing only the ports that are currenting LISTENING on the system.

## Solution

??? success "Solution"

    We need to execute the following commands:

    ``` sh linenums="1"
    ss -tlnp
    curl 0.0.0.0:12321
    ```

## Images

![answer](../media/santas_gift_tracking/santas_gift_tracking_1)
/// caption
Challenge terminal.
///

## Response

!!! quote "Yuri"

    Great work - thank you!

    Geez, maybe you can be my intern now!
