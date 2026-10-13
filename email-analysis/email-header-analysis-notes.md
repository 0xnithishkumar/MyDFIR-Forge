# Email Header Analysis: Notes

## Header fields

1. **Received:** Each mail server adds one Received header.
   - The server closest to the receiver is at the top.
   - The server closest to the sender is at the bottom.
   - Read from the bottom up to follow the path of the email.
2. **Return-Path:** The address that gets the email back if delivery fails (the bounce address).
3. **Authentication-Results:** Shows the SPF, DKIM, and DMARC results (pass or fail).
4. **Date:** The sender can change this value. Do not trust it alone.
5. **Boundary:** Marks where each part of the email starts and ends. If an email has a boundary, it has more than one part (for example, plain text, HTML, and attachments).

## Email authentication

1. **SPF (Sender Policy Framework):** A list of servers that can send email for a domain. The list is in DNS.
2. **DKIM (DomainKeys Identified Mail):** The sending server signs the header and body with its private key. Its public key is in DNS.
   - The receiving server uses the public key to check the signature.
   - DKIM gives integrity and authenticity. It does not give confidentiality, because the email is not encrypted.
3. **DMARC:** Tells the receiving server what to do when an email fails the checks. For example: send it to spam, reject it, or do nothing.
   - DMARC passes if SPF passes and the SPF domain matches the From domain,
   - or if DKIM passes and the DKIM domain matches the From domain.

## Mailing lists and ARC

**Mailing list:** One email goes to all people on the list. Example: as a TA, I send one email to the class list, and it goes to all students.

**Example:** A is the sender, B is the receiver, and M is the mailing list server.

1. A signs the email with DKIM and sends it to M.
2. M checks SPF, DKIM, and DMARC. All pass.
3. M adds an ARC set (i=1). The ARC-Authentication-Results header records "SPF pass, DKIM pass, DMARC pass." M signs this set.
4. M adds "[List]" to the subject and sends the email to B from M's own IP address.
5. B checks SPF. It fails, because M's IP is not in A's SPF record.
6. B checks DKIM. It fails, because the subject changed.
7. B checks DMARC. It fails, because SPF and DKIM both failed.
8. B validates the ARC chain. The chain is valid, so B can read M's record: "the email passed all checks at M."
   - If B trusts M as an ARC sealer, B can accept the email.
   - If B does not trust M, B applies A's DMARC policy.

## Key methodology

1. **Do not trust the file extension.** Check the magic bytes (the first few bytes of the file) to confirm the real file format.
2. **Always look for hidden files** in a folder (for example, with `ls -la`).
3. **Do OSINT on the mail servers:**
   - IP address
   - Domain
   - Mail server
4. **If SPF fails, investigate further.**
5. **Find all users who got a similar email.** Search by:
   - Subject
   - From address
   - Message-ID
