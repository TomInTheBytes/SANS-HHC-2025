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

    If I heard this correctly...our sneaky security gurus found a way to interact with the IMAP server using Curl! Yes...the CLI HTTP tool! Here are some helpful docs I found https://everything.curl.dev/usingcurl/reademail.html

## Solution

??? success "Solution to question 1"

```
curl 'imap://127.0.0.1' -u dosismail:holidaymagic
curl 'imap://127.0.0.1' -u dosismail:holidaymagic -X 'STATUS INBOX (MESSAGES)'
curl 'imap://127.0.0.1/INBOX;UID=1' -u dosismail:holidaymagic

dosismail @ Neighborhood Mail ~$ curl 'imap://127.0.0.1' -u dosismail:holidaymagic
* LIST (\HasNoChildren) "." Spam
* LIST (\HasNoChildren) "." Sent
* LIST (\HasNoChildren) "." Archives
* LIST (\HasNoChildren) "." Drafts
* LIST (\HasNoChildren) "." INBOX
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

curl 'imap://127.0.0.1/Spam;UID=2' -u dosismail:holidaymagic
```

## Response

!!! quote "Maurice Wilson"

    Outstanding work! You've mastered using curl for IMAP - that's some serious command-line skills that would make any Air Force tech proud.

    Counter Hack really is the best job I have ever had, especially when we get to solve problems like this!
