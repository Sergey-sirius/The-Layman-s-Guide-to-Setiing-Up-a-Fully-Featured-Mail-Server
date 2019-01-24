# How to Set Up a Fully Featured Mail Server on Ubuntu 18.04

## This guide will be showing you how to use iRedMail to quickly set up a full-featured mail server on Ubuntu 18.04, allowing you to take control of your email.  :envelope:
***

### Why should I host my own email? If your not interested in the myriad of reasons of why you **should** host your own email, feel free to scroll past this and dive into the nitty-gritty.
***
### For those interested in a small privacy and security rant:

### You need not look any further then media headlines regarding recent data breaches which expose **your** personally identifying information [PII](https://en.wikipedia.org/wiki/Personally_identifiable_information) and sensitive personal information [SPI](https://en.wikipedia.org/wiki/Personally_identifiable_information) to the world: 

[Equifax Data Breach](https://www.ftc.gov/equifax-data-breach) :sob:

[Yahoo Email Data Breach](https://money.cnn.com/2017/10/03/technology/business/yahoo-breach-3-billion-accounts/index.html) :astonished:

[Google+ Data Leak/Breach](https://www.wired.com/story/google-plus-bug-52-million-users-data-exposed/) :disappointed:

[Cambridge Analytica Data Scandal](https://en.wikipedia.org/wiki/Facebook%E2%80%93Cambridge_Analytica_data_scandal) :unamused:

That's just to name a few of the **larger** breaches of 2018; not including the "smaller" ones like:

[CarePlus Privacy Breach](https://www.wfla.com/news/florida/careplus-health-plans-notifying-customers-of-privacy-breach/1030535344)  :anguished:

[FedEx Data Leak/Breach](https://gizmodo.com/119-000-passports-and-photo-ids-of-fedex-customers-foun-1823035669)  :weary:

[Suntrust Banks Data Breach/Theft](https://clark.com/protect-your-identity/suntrust-data-breach-customer-records/)  :worried:

The long and short of it is:
>Our personal data is being sold, traded, and shared in ways that we’re only **beginning** to fully unravel.
> 
> Louise Matsakis - Wired

[We’re all Just Starting to Realize the Power of Personal Data](https://www.wired.com/story/2018-power-of-personal-data/)

### By the end of this guide, you will be able to:
- Install
- Configure
- Manage
- Maintain 

A secured email server.
***
### Hints and Tips and DIscolsures
I have done the legwork and types **ALL** of these commands manually to test, and it does work. I promise. I did it enough times, that the commands work.

SSH is your friend. It will make your install much easier.\

I am platform agnostic (I am also AWS Certified). This install was tested/configured/ and verified on Digital Ocean (initially due to cost). All the Digital Ocean links **do** include my referral code. that hels me cover the cost of my droplets. If you dont want to use my referral code, that is understandable, and you can sign up directly.

### Prerequisties & Assumption
- Domain name
  - Not required if you just want to test the install.
  - I prefer (use) [Namecheap.com](https://www.namecheap.com/) due to good pricing and a strong Anti-SOPA stance.
- VPS with Ubuntu 18.04 x64
  - I prefer (use) [DigitalOcean](https://m.do.co/c/285be2a21159).
- Familiarity using Linux from either CLI or Console
- Anywhere you see `sillydomain.site` substitute for **your** domain.
***



### Buy a Domain
1. Head to [Namecheap.com](www.namecheap.com).
2. Create a new [account](https://www.namecheap.com/myaccount/signup.aspx).
3. Purchase a new domain.
   - I recommend using a standard TLD such as: `.com` ; `.org` ; `.net` ; or `.us`.
   - For this guide, I have purchased, and am going to use: `sillydomain.site`
  
![sillydomain](http://i67.tinypic.com/ibgljt.png)
***
### DNS

I dont want to have to deal with DNS in multiple places, so I am going to have DigitalOcean manage my DNS for me.

1. Scroll down to the Nameservers section of your Domain Control Panel.
2. Change your Nameservers choice from *Namecheap Basic DNS* to *Custom DNS*.

![sillydomain-dns](http://i67.tinypic.com/2ronuvp.png)

3. Now Add:
```
ns1.digitalocean.com
ns2.digitalocean.com
ns3.digitalocean.com
```
Like below:
![sillydomain-ns123](http://i66.tinypic.com/2jeuyaa.png)

4. Click the Green Check mark to commit that changes. 
5. Your DNS is now being pointed to [DigitalOcean](www.digitalocean.com) to manage. In my experience this was updated in about 30 minutes, but it _can_ take up to 48 hours.
***
### DNS and MX Records
The MX record specifies which host or hosts handle emails for a particular domain name. For example, the host that handles emails for `sillydomain.site` is `mail.sillydomain.site`. If someone with a Gmail account sends an email to `somebody@sillydomain.site`, then Gmail server will query the MX record of `sillydomain.site`. When it finds out that `mail.sillydomain.site` is responsible for accepting email, it then query the A record of `mail.sillydomain.site` to get the IP address, thus the email can be delivered.

Since [DigitalOcean](https://m.do.co/c/285be2a21159) is managing our DNS, this becomes a *fairly* painless process.

>If you have not already created an account, go to [DigitalOcean](https://m.do.co/c/285be2a21159) and sign up for an account.

1. Once you have logged into your [DigitalOcean](https://m.do.co/c/285be2a21159) portal, click on Networking in the left hand navigation bar.
![do-networking](http://i68.tinypic.com/29puhc9.png)

2. Click on Domains
![do-networking-add-domain](http://i65.tinypic.com/j9aiqv.png)

3. Enter the Domain Name and click Add Domain
![do-networking-add-domain-add](http://i66.tinypic.com/2pqp5ki.png)

4. We can now create a new MX record for our domain name. Under `Create a new record,` click on `MX`
![do-networking-add-mx](http://i63.tinypic.com/r8ytxu.png)

1. Enter `@` in the Hostame field, enter `mail.sillydomain.site` in Mail Providers Mail Server Value field, enter `10` in the Priotity field, and `1800` in the TTL Field.
![](http://i66.tinypic.com/20p1mok.png)
6. Click Create Record.
7. You will nowe have a new record, as shown below.
![](http://i66.tinypic.com/2ijp8pw.png)
8.  After creating MX record, you also need to create an `A` record for `mail.sillydomain.site` , so that it can be resolved to an IP address.
9.  Click on Networking in the left hand navigation bar.
10. Click on the your domain name.
11. We can now create a new MX record for our domain name. Under `Create a new record,` click on `A`
12. Enter `@` in the `Hostame` field, enter `mail.sillydomain.site` in `Will Direct To Field` field, amd `90` in the TTL Field.

This record will allow you to connect to the iRedMail Admin panel via Hostname instead of IP address after the install.
>You will be addding more DNS records later in this guide, but this is a good primer to get stareted.
***
### Build and Configure a VPS aka 'Droplet'
To set up a complete email server, you need a server with at least 1GB RAM. This guide is done on a $5/month DigitalOcean Droplet or VPS (virtual private server). I recommend [DigitalOcean](https://m.do.co/c/285be2a21159) because it doesn’t block port 25, and is fairly user friendly. Additionally, if you are using a personal domain, 1 GB RAM with 25 GB Disk space should be sufficent to run everything you need.

1. Once you have logged into your [DigitalOcean](https://m.do.co/c/285be2a21159) portal, click on Droplets in the left hand navigation bar.
![do-Droplets]()
2. Under Distro choose: Ubuntu 18.04 x64
3. Under Choose size: scrool left and click on 5/month
4. Under datacenter region, pick the location closest to you. I am choosing New York 1 for this guide.
5. Under choose a hostname, you **must** make the Droplet hostname the same as your domain. so i wchanged mine to: mail.sillydomain.site
6. Click Create
7. In a few moments your Droplet will be available.
> SSH is a more secure way to access your Droplet, and should be used. However, for simplicity, I will do everyuthing via the built in Droplet console. You can find more inforamtion about using ssh to conenct to yoru Droplet here: [How to Connect to Droplets with SSH](https://www.digitalocean.com/docs/Droplets/how-to/connect-with-ssh/)
8. When your Droplet is available click the console button and login using `root` using the autogenerated password.
9. Change the `root` password to something more memeorbale for you.
10.  You should now see a window similar to this
11.  
***
### Updates and Hostname
1. First we need to update our Droplet to to the latest. Type the following into the console window.
```
# sudo apt update
# sudo apt upgrade
```
>this process check for application updates and security pathes on your Droplet, then applies them.
2. Press Y
3. If you see a screen like this, press Tab, then OK, to keep the local version currently installed.
4. set a fully qualified domain name (FQDN) for your server with the following command.
```
# sudo hostnamectl set-hostname mail.sillydomain.site
```
5. also need to update /etc/hosts file.
```
sudo nano /etc/hosts
```
6. dit it like below:
```
127.0.0.1 mail.sillydomain.site localhost
```
7. Press Ctrl+X to Save
8. Press Y
9. Press Enter
10. . To see the changes, re-login and then run the following command to see your hostname.
```
hostname -f
```
11. If performed correctly; you should have an output similar to this:
```
mail.sillydomain.site
```
***
### Setting up iRedMail Mail Server on Ubuntu 18.04 with 
iRedMail is a shell script that automatically installs and configures all the necessary mail server components on your Linux server (Droplet), eliminating manual installation and configuration. With iRedMail, you can easily create as many mailboxes as you want in a web-based admin panel. Mailboxes can be stored in MariaDB/MySQL, PostreSQL database or OpenLDAP. The Open-source software used in iRedMail is:

- Postfix SMTP server
- Dovecot IMAP server
- Nginx web server
- OpenLDAP, ldapd
- MySQL/MariaDB, PostgreSQL
- Amavised-new
- SpamAssassin
- ClamAV
- Roundcube webmail
- SOGo Groupware
- Fail2ban
- mlmmj mailing list manager
- Netdata server monitoring
- iRedAPD Postfix policy server for greylisting

1. Type the following command download the iRedMail Bash installer with `wget`. At the time of this writing, the latest version of iRedMail is 0.9.9, released on December 17, 2018. Please go to iRedMail download page (http://www.iredmail.org/download.html)  to check for the latest version.
```
# wget https://bitbucket.org/zhb/iredmail/downloads/iRedMail-0.9.9.tar.bz2
```
2. Extract the tarball.
```
# tar xvf iRedMail-0.9.9.tar.bz2
```
1. Naviagte into the newly created directory using `cd`.
```
# cd iRedMail-0.9.9
```
4. Add executable permissions to the iRedMail.sh script.
```
# chmod +x iRedMail.sh
```
5. Next, run the Bash script with sudo privilege.
```
$ sudo bash iRedMail.sh
```
6. You should now see the iRedMail installer
![](http://i66.tinypic.com/jkan83.png)

7. LCick Yes (Enter)
8.  You can use the default storage location which will be `/var/vmail`, press Enter.
![](http://i64.tinypic.com/2q0qblu.png)

9.  It’s **highly recommended** that you choose to run a web server because you need the web-based Admin Panel to add email accounts, domains, and users. Also it allows you to access the Roundcube webmail. By default, Nginx web server is selected, so you can simply press Enter.  (*An asterisk indicates the item is selected and will reprsent that for the rest of this guide*.)
![](http://i63.tinypic.com/fo18xs.png)

10. Select the storage backend. This guide uses MariaDB. Press up and down arrow key and press the space bar to select.
![](http://i63.tinypic.com/10qzyuw.png)
>If you are unsure which to use, use MariaDB.
11. If you selected MariaDB or MySQL, then you will need to set the MySQL root password.
![](http://i65.tinypic.com/vgr3up.png)

12. Enter your first mail address. You can add additional mail: domains, addresses, and users, later in the Admin Aanel. This guide assumes that you want an email account like `john.doe@your-domain.com` or `admin@sillydomain.site`
![](http://i67.tinypic.com/fa4gwi.png)

>**Do not press the space bar after your domain name!** iRedMail will copy the space character along with your domain name, which will result in installation failure.

1.  set a password for the mail domain administrator.
![]()
14. Choose additional components. By default, four items are selected. Unless you ened SOGo groupware, *dont select it*, press Enter.
15. Now you can review your configurations. Type Y to begin the installation of all mail server components.

>tThis will take a little bit of time. Treat yourself to a :coffee:, :tea:, :beer:, :wine_glass:or  :tropical_drink:, you deserve it.

16. At the end of installation, choose y to use firewall rules provided by iRedMail and restart firewall.
17. iRedMail installation is complete. You will be notified the URL of webmail, SOGo groupware and web admin panel and the login credentials. The iRedMail.tips file contains important information about your iRedMail server.
18. Reboot your Ubuntu 18.04 server.
```
sudo shutdown -r now
```












***
### To Do
- Write/Test against DigitalOcean Ubuntu 
