# MetaCTF Writeups

**Event:** MetaCTF April FlashCTF 2026

---

## Table of Contents

1. [Stacked Logs](#stacked-logs)
2. [Name Game](#name-game)
3. [Flags Summary](#flags-summary)

---

## Stacked Logs

### Challenge Description

We are provided with a server log containing routine traffic. The goal is to analyze the log entries and identify any anomalies that may reveal the flag.

### Analysis

The log file consists of standard HTTP request entries. A quick search for the keyword `flag` within the logs yields a critical finding.

### Relevant Log Entries

```log
[2024-04-25 13:09:46,000] INFO  in app: GET /api/user/profile 200 12ms user_id=204 - werkzeug
[2024-04-25 13:09:50,000] INFO  in app: GET /api/announcements 200 15ms user_id=678 - werkzeug
[2024-04-25 13:10:00,000] INFO  in app: POST /api/hints/42/unlock 402 8ms user_id=501 - werkzeug
[2024-04-25 13:10:04,000] INFO  in app: GET /api/hints/42 200 19ms user_id=815 - werkzeug
[2024-04-25 13:10:13,000] INFO  in app: POST /api/submit 400 30ms user_id=204 - werkzeug
[2024-04-25 13:10:25,000] INFO  in app: GET /api/notifications 200 5ms user_id=815 - werkzeug
[2024-04-25 13:10:27,000] INFO  in app: POST /api/team/invite 200 19ms user_id=815 - werkzeug
[2024-04-25 13:10:31,000] INFO  in app: POST /api/auth/refresh 200 9ms user_id=501 - werkzeug
[2024-04-25 13:10:35,000] INFO  in app: GET /static/js/app.bundle.js 200 202ms user_id=501 - werkzeug
[2024-04-25 13:10:36,000] INFO  in app: GET /api/challenges/40 200 9ms user_id=720 - werkzeug
```

### Exception Traceback

An unhandled exception on the `/api/validate` endpoint leaks sensitive information, including the flag, in the local variables dump.

```python
[2024-04-25 13:10:40,000] INFO  in app: POST /api/validate 500 23ms user_id=338 - werkzeug
[2024-04-25 13:10:49,000] ERROR in app: Exception on /api/validate [POST]
Traceback (most recent call last):
  File "/usr/local/lib/python3.11/site-packages/flask/app.py", line 1488, in full_dispatch_request
    rv = self.dispatch_request()
  File "/usr/local/lib/python3.11/site-packages/flask/app.py", line 1466, in dispatch_request
    return self.ensure_sync(self.view_functions[rule.endpoint])(**req.url_values)
  File "/srv/app/api/validate.py", line 47, in validate_submission
    result = validator.check(user_input, flag)
  File "/srv/app/core/validator.py", line 23, in check
    if submission.strip().lower() == expected.strip().lower():
AttributeError: 'NoneType' object has no attribute 'strip'

Local variables (validate_submission):
  request      = <Request 'http://ctf.internal/api/validate' [POST]>
  challenge_id = 42
  user_id      = 338
  flag         = 'MetaCTF{unhandl3d_3xc3pt10ns_l34k_s3cr3ts}'
  user_input   = None
  db_cursor    = <psycopg2.extensions.cursor object at 0x7f3a1c2d4b80>

[2024-04-25 13:10:50,000] INFO  in app: Sending 500 response to user_id=338
```

### Flag

> `MetaCTF{unhandl3d_3xc3pt10ns_l34k_s3cr3ts}`

---

## Name Game

### Challenge Description

We are provided with a `capture.pcap` file containing DNS queries flagged as unusual activity. The challenge title "Name Game" suggests that domain names themselves encode the hidden message.

### Analysis

Upon inspecting the DNS traffic, a suspicious domain stands out:

- `totallynotac2.meatctf.com`

Under this domain, **9 DNS records** were observed. Each subdomain is a hexadecimal string that decodes to a portion of the flag.

| Packet # | Timestamp  | Hex Payload | Decoded ASCII |
|:--------:|:----------:|:-----------:|:-------------:|
| 721      | 240.000000 | `4d657461`  | `Meta`        |
| 724      | 241.029482 | `4354467b`  | `CTF{`        |
| 733      | 242.047030 | `646e735f`  | `dns_`        |
| 736      | 243.002208 | `31355f61`  | `15_a`        |
| 737      | 243.966458 | `6c773479`  | `lw4y`        |
| 740      | 244.922549 | `735f7468`  | `s_th`        |
| 743      | 245.938842 | `335f6375`  | `3_cu`        |
| 748      | 246.934191 | `6c707231`  | `lpr1`        |
| 751      | 247.894408 | `747d`      | `t}`          |

### Reconstruction

Stitching all decoded text chunks together in packet order reveals the hidden message:

```
Meta + CTF{ + dns_ + 15_a + lw4y + s_th + 3_cu + lpr1 + t}
```

### Flag

> `MetaCTF{dns_15_alw4ys_th3_culpr1t}`

---

## Flags Summary

| Challenge    | Flag                                          |
|:-------------|:----------------------------------------------|
| Stacked Logs | `MetaCTF{unhandl3d_3xc3pt10ns_l34k_s3cr3ts}`  |
| Name Game    | `MetaCTF{dns_15_alw4ys_th3_culpr1t}`          |

---

this writeup is incomplete* for now.

