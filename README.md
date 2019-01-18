# iredmail Full-Featured Mail Server on Ubuntu 18.04 with iRedMail
iredmail

Quote from Spamhaus news article:

outsourcing (the mail service) does not come without costs, even when the outsourced service appears to be "free". Hidden costs include:

Another organization can see the content of all messages. In some cases, the contents of messages are stored on the outsourcing company's servers indefinitely. External access to unencrypted emails poses privacy and confidentiality issues. Furthermore, the outsourcing company may be located in another country and be subjected to different regulations and obligations.

In some cases, the outsourcing company's terms and conditions allow it to search the content of emails to aid in targeting advertising, which poses even greater privacy and confidentiality problems.

The organization no longer has control of its own email security. Server-based encryption and authentication is managed by the outsourcing company, requiring end-to-end encryption for sensitive communications.

Large companies with many customers are often a target of cybercrime attacks aimed at stealing customer data, and some of these attacks have succeeded.

Inspection of SMTP transaction logs may be impossible for the end user. Troubleshooting failed deliveries and other email problems requires interacting with an external support desk. Support desks are sometimes slow to respond. First-line support, in particular, might lack the training and access to fix any but simple problems, requiring escalation and further delays.

Sharing a mail server with other organizations can cause delivery issues when a user at another organization sends spam through that mail server. When the outsourcing company fails to detect and block spam, or is slow to terminate service to spammers, the likelihood of problems increases substantially.

Why iRedMail
Privacy
You have all personal data on your own hard disk, you can control the email security, inspect transaction log. No other organization can see the content of all messages.

Open Source
All components used in iRedMail are open source softwares, and you get the bug fixes and updates from the Linux/BSD venders you trust. iRedMail is the right way to build your mail server with open source softwares.

Secure By Default
End users are forced to use mail services through secure connections (POP3/IMAP/SMTP over TLS, webmail with HTTPS). Emails are encrypted in transit using TLS if possible. Passwords are stored in SSHA512 or BCRYPT (BSD).

Webmail
Manage mails, folders, sieve filters, vacation directly on a intuitive and easy to use web UI (Roundcube webmail or/and SOGo groupware).

Calendars/Contacts/ActiveSync*
Manage your calendars (CalDAV), address books (CardDAV), tasks on a easy to use web UI or your mobile devices (iOS, Android, BlackBerry 10, Windows Phone).
* CalDAV, CardDAV and ActiveSync are offered by SOGo Groupware.

Unlimited Accounts
Forget about the products which pricing based on number of mailboxes, you can create as many mail accounts (domains, users, mailing lists, admins) as you want.

Main Stream Linux/BSD
iRedMail works on Red Hat Enterprise Linux, CentOS, Debian, Ubuntu, FreeBSD, OpenBSD. No matter you switch to which Linux/BSD distribution supported by iRedMail, you get the same setup in just few minutes.

Backends
Stores mail accounts in your favourte backend: OpenLDAP, MySQL, MariaDB, PostgreSQL.

Antispam & Antivirus
SpamAssassin, ClamAV, SPF, DKIM, greylisting, whitelisting, blacklisting. Quarantining detected spam into SQL database for further review.

Web Admin Panel
Manage your mail accounts with a web admin panel. iRedMail ships a free and easy to use web admin panel along with it's product. - iRedMail also offers a separate, paid edition iRedAdmin-Pro with more features. Features list of iRedAdmin-Pro.

Reproduceable Deployment
Get a reproduceable, easy to use, flexible, stable mail server in just few minutes, easy to migrate an old server, or restore a crashed server to new iRedMail server.
