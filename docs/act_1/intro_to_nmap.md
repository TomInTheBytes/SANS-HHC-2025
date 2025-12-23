# Intro to Nmap

Difficulty: :material-star::material-star-outline::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Meet Eric in the hotel parking lot for Nmap know-how and scanning secrets. Help him connect to the wardriving rig on his motorcycle!

??? quote "Eric Pursley"

    Hey, I'm Eric. As you can see, I'm an avid motorcyclist. And I love traveling the world with my wife.

    I enjoy being creative and making things. For example, a cybersecurity tool called Zero-E that I'm quite proud of, and the Baldur's Gate 3 mod called Manaflare. I'm even in the BG3 credits!

    I also make tools, ranges, and HHC worlds for Counter Hack. Yup, including the one you're in right now.

    But most of the time, I'm helping organizations in the real world be more secure. I do a bunch of different kinds of pentesting, but speciailize in network and physical.

    Some advice: stay laser-focused on your goals and don't let the distractions life throws at you lead you astray. That's how I ended up at Counter Hack!

    Speaking of tools, let me introduce you to one of the most essential weapons in any pentester's arsenal: Nmap.

    It's like having X-ray vision for networks, and I've set up a perfect environment for you to learn the fundamentals.

    Help me find and connect to the wardriving rig's service on my motorcycle!

## Hints

??? tip "Nmap Documentation"

    Nmap is pretty straightforward to use for basic port scans. Check out its [documentation](https://nmap.org/book/man.html)!

??? tip "Ncat Documentation"

    You may also want to check out the [Ncat Guide](https://nmap.org/ncat/guide/).

## Solution

??? success "Solution"

    We need to execute the following commands:

    ``` sh linenums="1"
    nmap 127.0.12.25
    nmap -p- 127.0.12.25
    nmap 127.0.12.20-28
    nmap -p 8080 -sC 127.0.12.25
    nmap -p 8080 -sV 127.0.12.25
    nc 127.0.12.25 24601
    exit
    ```

## Images

![answer](../media/intro_to_nmap/intro_to_nmap_1)
/// caption
Challenge terminal.
///

## Response

!!! quote "Eric Pursley"

    Excellent work! You've got the Nmap fundamentals down - that X-ray vision is going to serve you well in future challenges.

    Now you're ready to scan networks like a seasoned pentester!
