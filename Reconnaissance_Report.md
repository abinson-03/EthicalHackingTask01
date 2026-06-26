# Ethical Hacking Task 01: Information Gathering & Reconnaissance Report

## Part A: Target Selection
- **Website Name**: Massachusetts Institute of Technology (MIT)
- **URL**: `https://mit.edu`
- **Reason for choosing the target**: MIT is a well-known public educational institution. It is a safe and openly accessible target for passive reconnaissance.

## Part B: WHOIS Lookup
*(Data gathered via EDUCAUSE WHOIS database)*

- **Domain Name**: MIT.EDU
- **Registrar**: EDUCAUSE
- **Registration Date**: 23-May-1985
- **Expiry Date**: 31-Jul-2027
- **Name Servers**:
  - ASIA2.AKAM.NET
  - NS1-37.AKAM.NET
  - NS1-173.AKAM.NET
  - EUR5.AKAM.NET
  - USW2.AKAM.NET
  - ASIA1.AKAM.NET
  - USE5.AKAM.NET
  - USE2.AKAM.NET
- **Domain Status**: Active

### WHOIS Output Evidence
```text
Domain Name: MIT.EDU
Registrant: Massachusetts Institute of Technology
Domain record activated:    23-May-1985
Domain expires:             31-Jul-2027
```

## Part C: DNS Enumeration
*(Data gathered via DNS lookup)*

- **A Record**: `23.15.150.186`
- **MX Record**: `mit-edu.mail.protection.outlook.com` (Preference: 100)
- **NS Record**: `usw2.akam.net`, `eur5.akam.net`, `use5.akam.net`, `ns1-37.akam.net`, `ns1-173.akam.net`, `asia1.akam.net`, `use2.akam.net`, `asia2.akam.net`
- **TXT Record**: No TXT record found directly on the root `mit.edu` domain.

### DNS Output Evidence
```text
Name      Type   TTL   Section    IPAddress
----      ----   ---   -------    ---------
mit.edu   A      20    Answer     23.15.150.186
```

## Part D: Website Technology Identification
- **Web Server**: Apache
- **CDN**: Akamai (Identified via `Akamai-GRN` HTTP header and Akamai NS records)
- **Email Service**: Microsoft Outlook (Identified via MX record `mit-edu.mail.protection.outlook.com`)

**Summary**: The website is hosted using the Apache web server and sits behind the Akamai Content Delivery Network (CDN) for performance and security. Email services are handled externally by Microsoft Outlook (Office 365).

## Part E: HTTP Security Headers

| Header | Present (Yes/No) | Purpose |
| :--- | :--- | :--- |
| **Content-Security-Policy (CSP)** | Yes | Restricts the resources (like scripts and images) that a page can load, mitigating XSS attacks. The site uses `frame-ancestors 'self'...` |
| **X-Frame-Options** | Yes | Prevents clickjacking by restricting whether the site can be embedded in an iframe. The site uses `SAMEORIGIN`. |
| **X-Content-Type-Options** | No | Prevents MIME-sniffing, forcing the browser to stick with the declared content type. |
| **Strict-Transport-Security (HSTS)** | Yes | Enforces secure (HTTPS) connections to the server. The site uses `max-age=600`. |
| **Referrer-Policy** | No | Controls how much referrer information is included with requests made from the website. |

### HTTP Headers Evidence
```text
Connection: keep-alive
Content-Security-Policy: frame-ancestors 'self' https://web.mit.edu https://www.mit.edu http://web.mit.edu http://w...
X-Frame-Options: SAMEORIGIN
Strict-Transport-Security: max-age=600
Akamai-GRN: 0.9cfed417.1782455940.18d88bea
Server: Apache
```

## Part F: Robots.txt & Sitemap Analysis

- **Does the website have a robots.txt file?** Yes. (`https://mit.edu/robots.txt` returned a valid file).
- **Does it have a sitemap?** No. (`https://mit.edu/sitemap.xml` returned a 404 Not Found error).
- **What information can be learned from these files?**
  - The `robots.txt` file reveals several restricted paths that administrators do not want search engine crawlers to index.
  - Excluded directories include administrative and functional paths such as `/afs/`, `/cgi-bin/`, `/user/`, `/org/`, `/activity/`, `/contrib/`, `/dept/`, `/software/`, and `/bin/`.
  - There is also a rule disallowing crawling of anything matching `*/Public/*`.

### Robots.txt Evidence
```text
User-agent: *
Disallow: /afs/
Disallow: /cgi-bin/
Disallow: /user/
Disallow: /org/
Disallow: /activity/
Disallow: /contrib/
Disallow: /dept/
Disallow: /software/
Disallow: /bin/
Disallow: */Public/*
```
