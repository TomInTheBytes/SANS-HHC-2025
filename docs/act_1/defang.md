# Its All About Defang

Difficulty: :material-star::material-star-outline::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Find Ed Skoudis upstairs in City Hall and help him troubleshoot a clever phishing tool in his cozy office.

??? quote "Ed Skoudis"

    I'm the Founder of Counter Hack Innovations, the team that brings you the SANS Holiday Hack Challenge for the last 22 years.

    I'm also the President of the SANS Technology Institute College, which has over 2,300 students studying for their Bachelor's Degrees, Master's Degrees, and various certificates.

    I was the original author of the SANS SEC504 (Incident Handling and Hacker Attacks) and SANS SEC560 (Enterprise Penetration Testing) courses.

    I love Capture the Flag games and puzzles.

    I've got a steampunk office filled with secret rooms, and I collect antique crypto systems and communication technologies.

    I've got an original Enigma machine (A726, from the early war years), a leaf of the Gutenberg Bible (1 John 2:3 to 4:16) from 1455, and a Kryha Liliput from the 1920s.

    Oh gosh, I could talk for hours about this stuff but I really need your help!

    The team has been working on this new SOC tool that helps triage phishing emails...and there are some...issues.

    We have had some pretty sketchy emails coming through and we need to make sure we block ALL of the indicators of compromise. Can you help me? No pressure...

    Well you just made that look like a piece of cake! Though I prefer cookies...I know where to find the best in town!

    Thanks again! See ya 'round!

## Hints

??? tip "Defang All The Thingz"

    The PTAS does a pretty good job at defanging, however, the feature we are still working on is one that defangs ALL scenarios. For now, you will need to write a custom sed command combining all defang options.

??? tip "Extract IOCs"

    Remember, the new Phishing Threat Analysis Station (PTAS) is still under construction. Even though the regex patterns are provided, they haven't been fine tuned. Some of the matches may need to be manually removed.

## Solution

The answers can be found by using various regexes. Regexes for these types of indicators can be found [online](https://regexlib.com/). Various regexes used to solve this exercise are:

Domains:

```
([a-zA-Z0-9]([a-zA-Z0-9\-]{0,61}[a-zA-Z0-9])?\.)+[a-zA-Z]{2,6}
```

IP addresses:

```
(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[0-9]{1,2})(\.(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[0-9]{1,2})){3}
```

URLs:

```
((((H|h)(T|t)|(F|f))(T|t)(P|p)((S|s)?))\://)?(www.|[a-zA-Z0-9].)[a-zA-Z0-9\-\.]+\.[a-zA-Z]{2,6}(\:[0-9]{1,5})*(/($|[a-zA-Z0-9\.\,\;\?\'\\\+&amp;%\$#\=~_\-]+))*
```

Emails:

```
[0-9a-zA-Z]+([0-9a-zA-Z]*[-._+])*[0-9a-zA-Z]+@[0-9a-zA-Z]+([-.][0-9a-zA-Z]+)*([0-9a-zA-Z]*[.])[a-zA-Z]{2,6}
```

Defang using sed:

```
s/:\//[://]/g;s/http/hxxp/g;s/@/[@]/g;s/\./[.]/g
```

??? success "Solution"

    ![answer](../media/defang/defang_1)
    /// caption
    Answer to challenge.
    ///

## Response

!!! quote "Ed Skoudis"

    finger guns Nice work! You've mastered those firewall fundamentals like a true network security pro.

    Now that was way more fun than sitting through another boring lecture, wasn't it?
