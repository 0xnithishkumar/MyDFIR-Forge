# Planet's Prestige: Email Analysis Report

## Background

This lab is from Blue Team Labs Online, as part of the MyDFIR First Win investigation.

CoCanDa, a planet known as "The Heaven of the Universe", has had a bad year. Someone keeps abducting its citizens, the CoCanDians, and riots have broken out across the planet.

The Planetary President called a war-room meeting with his best minds and military leaders. After the meeting, he learned that his daughter had disappeared.

CoCanDa agents on many planets searched for her day and night. After two days, there was still no clue and no ransom demand. On the third day, the CoCanDa representative on Earth, an Army Major, received an email.

## What we know

- CoCanDian citizens were abducted.
- The President's daughter is missing.
- For two days, there was no clue and no ransom demand.
- On the third day, the Army Major on Earth received an email.

## Findings

- **Date and time:** Tue, 26 Jan 2021 01:41:18 -0500 (EST)
- **From:** billjobs[@]microapple[.]com
- **To:** themajoronearth[@]gmail[.]com
- **Reply-To:** negeja3921[@]pashter[.]com
- **Mailer service:** emkei[.]cz
- **Sending server IP:** 93.99.104.210
- **Attachment:** PuzzleToCoCanDa[.]pdf (really a ZIP file)
- **Files in the ZIP:** DaughtersCrown.jpeg, GoodJobMajor.pdf, Money.xlsx
- **Attacker name (from PDF metadata):** Pestero Negeja
- **Attacker location:** The Martian Colony, Beside Interplanetary Spaceport

## Investigation

On 26 January 2021, the attacker sent a spoofed email to themajoronearth[@]gmail[.]com. He used emkei[.]cz, a fake mailer service, to make the email look like it came from billjobs[@]microapple[.]com.

The email failed the SPF check. The Reply-To address, negeja3921[@]pashter[.]com, did not match the From address. Any reply would go to the attacker, not to the shown sender.

The email body was Base64 encoded. I decoded it with CyberChef. It was a ransom note to "TheMajorOnEarth". It demanded 1 billion CoCanDs for the abducted CoCanDians, among them the President's daughter. The email had no malicious links.

The attachment was named PuzzleToCoCanDa.pdf, but it was not a PDF. I checked it with `file` and a hex editor. Its magic bytes were 50 4B 03 04 ("PK"), which is the signature of a ZIP file.

The ZIP had three files. I checked the magic bytes of each one, and each file was the type its extension said:

- **DaughtersCrown.jpeg:** A picture of a crown. This is proof that the attacker has the President's daughter.
- **GoodJobMajor.pdf:** Tells the reader to look in the Excel file. ExifTool showed the author as Pestero Negeja. "Negeja" is also in the Reply-To address.
- **Money.xlsx:** A declaration of war and the attacker's location, "The Martian Colony, Beside Interplanetary Spaceport".

The ransom demand was a trick. The Excel file shows that the real goal was to start a war.

### 5W1H

- **Who:** The attacker, Pestero Negeja, sent the email to themajoronearth[@]gmail[.]com. He posed as billjobs[@]microapple[.]com.
- **What:** A spoofed ransom email with a ZIP file disguised as a PDF.
- **When:** Tuesday, 26 January 2021, at 01:41:18 EST (UTC-05:00).
- **Where:** The email came through emkei[.]cz (93.99.104.210), and replies went to pashter[.]com. The attacker is in the Martian Colony, beside the Interplanetary Spaceport.
- **Why:** He demanded 1 billion CoCanDs for the abducted CoCanDians. But the ransom was a trick. The real goal was to start a war.
- **How:** He used a fake mailer to spoof the sender and set a different Reply-To address. He encoded the message in Base64 and hid a ZIP file behind a PDF name. The files inside led the reader from one clue to the next.

## Recommendations

1. **Brief the President.** Share the ransom note, the declaration of war, and the attacker's location with the agents who search for the missing CoCanDians.
2. **Save the evidence first.** Keep a copy of the email, its full headers, the attachments, and their file hashes before anything is deleted.
3. **Check before you act.** Find out if the email or its files are harmful before you delete or isolate anything. Here, there were no malicious links, and the files in the ZIP were real file types.
4. **Remove the email from all mailboxes.** After the evidence is saved, find the same email in other mailboxes and remove it.
5. **Isolate only when needed.** If a user clicked a link or opened a file, and there are signs of malware, isolate that computer and investigate further.
6. **Think about the attack stage.** If data was already stolen, do not make changes to the affected computers. Forensic teams, legal, and the authorities may need them as they are.
7. **Block the attacker's addresses.** Block negeja3921[@]pashter[.]com and pashter[.]com. Treat email from emkei[.]cz as suspicious, because it is a known fake mailer.
8. **Use a sandbox.** Open files from unknown senders only in a sandbox.
9. **Train users.** Teach users to spot spoofed emails and suspicious attachments, and to report them.
