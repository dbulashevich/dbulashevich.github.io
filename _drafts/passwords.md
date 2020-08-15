---
layout: post
title:  "Whom I trust my online accounts?"
#date:   2020-04-07 22:01:23 +0200
---
In this post I describe how I managed [password fatigue](https://en.wikipedia.org/wiki/Password_fatigue) while balancing the following contradicting requirements:
- **Security** An adversary cannot log into my account.
- **Recoverability** I can recover access to my account if something goes wrong.
- **Usability** I can effortlessly log into my account whenever I want.

This is not a comparison of different password managers but an attempt to show how threat modelling helps you choose a right solution based on your own needs.

## Introduction
I personally have 20+ accounts (e-mails, social networks, e-shops, etc.). Some of those are of no value and have not any sensitive information but losing others would be a very unpleasant event.
For a few years I used a common weak password for invaluable accounts, a bit stronger for my primary e-mail, and "Sign with Google" for the rest. But everything has its price and authenticating everywhere with a single account is not an exclusion. One day I waked up with quite ordinary questions:
1. What if someone hacks my Google account?
2. Is Google itself able to impersonate me?
3. What if one day Google terminates my account?
4. What if government blocks the Google?

This led me to a sad conclusion that I "put all my eggs in one basket". Not the best idea, no matter how good and strong the basket is.
Then I started to look for some better way of storing my passwords. After reading through different web sites for a few days I realized that I was looking for a solution without clear understanding of my requirements. Next day was a Monday, so I stopped googling and started working. I hadn't returned to this topic until I met my old school mate with whom I used to discuss my most crazy ideas. Then we spent not one hour and drunk not one cup of Glögg over discussion how to combine all our requirements in one system. 

## Requirements
Defining your requirements is a crucial step for choosing a proper tool. Cause different requirements contradict each other, one need clearly understand trade-offs made in one or another solution. Someone may prefer more secure but less user-friendly approach, while another prefers easy login process.

### Security
<details><summary>The fewer 3rd party services I need to trust the better.</summary>

Every new service adds its own problems - security vulnerabilities, human mistakes, insider attacks, etc. Unless end-to-end encryption is used, every service has technical possibility to read your data passed through them.
</details>

<details> 
<summary>The more efforts an attacker needs to stole my credentials the better.</summary>

A common way to improve security is adding another factor of authentication. One of the benefits of a properly designed [multi-factor authentication][MFA] is that every factor requires an attacker to use different attack. The same as securing your car with a steering wheel lock and an electronic immobilizer, so a thief should be able to both crack the electronic code and lockpick the wheel lock. 

Ideally all factors should be orthogonal to each other, so breaching one factor doesn't compromise others. Let's demonstrate it with simple diagrams below.
![Entagled MFA](/assets/mfa.svg)

Here we assume classical scheme of 2FA with a password and a single use verification code sent in SMS. Blue and yellow figures represent attacks on a password and SMS respectively. If the password can be reset via phone, those two factors are not orthogonal anymore as succefful breach of the SMS factor compromise the password too.

By the way, I wouldn't recommend SMS as an authentication factor in any schema because of known [attacks][SS7] on SS7 protocol and SIM swap [crimes][SIM]. Authentication apps provide much better security and at least comparable usability.
</details>

<details><summary>The more time I have to mitigate an attack the better.</summary>

Let's use a simple analogy of a door with two independent locks. If one evening you came home and found one lock is lock-picked while the second one is intact you know that someone has tried to break in your home. Even the door still holds you would probable replace the broken lock to restore "two locks" security.

The same goes for information security. Whenever you receive an SMS with a one-time code for a login attempt you have never made, you may assume your password is compromised and change it immediately.
</details>

### Recoverability
The second worst thing after getting your account hacked is having it gone for good cause you've lost the authentication data.
The less [Single Points of Failure (SPoF)][SPoF] the better. SPoFs are:
<details><summary>Data without backups.</summary>

Including passwords stored in your own brain.
</details>

<details><summary>3rd party services storing your data, unless you have a backup (either local or stored in another service).</summary>

Giants like Google, Facebook, Twitter, and others may appear reliable enough to use them alone as a single account for all your favorite e-shops, online cinemas, etc. The problem arises when your account has been permanently deleted, you disagree with new Terms of Service, the service itself has been blocked in your country, etc.
</details>

### Usability
No matter how secure a system is, it will be useless if nobody want to use it. So I would focus on the following requirements for usability.
<details><summary>The less manual actions per login the better.</summary>

If a system will require too many actions to login, i.e. type a password, than retype a code from SMS, and scan user's fingerprint it may be very secure but I would bet you wouldn't use such a system for every online account. For banking maybe, but not for local newspaper.
</details>

<details><summary>The less passwords to remember the better.</summary>

Passwords are a good security tool but have a little nasty drawback. Strong password is very long, at least, and most times hard to remember. Also passwords must be unique so it's unlikely one would like to remember more than 1-3 really strong passwords.
</details>

<details><summary>The less frequently I change my passwords the better.</summary>

Even decent passwords may be cracked if an attacker has managed to steal database of password hashes from an attacked system. The time needed to guess a password depends on the password strength. For more details on this topic you may refer either to [Password strength][passwords] article on Wikipedia or NIST publication [Electronic Authentication Guideline][NIST_EAG].

For our goals the simple rule is "a long password gives you more time to change it after compromising the password's hash". The culprit is detecting of hash compromising. For regular online services you may assume that someone stoles databases immediately after every password change, cause most probably system administrator can download the database. This is the reason why some services require password to be changed regularly.

What could really help here is an alarm that someone has just stolen your data. This allows you to change passwords not regularly, but only after an incident.
</details>

## Threat models for different approaches
Now, after setting criterias, we may actually compare different approaches. The list below is far from being exaustive but my goal is to demonstrate some most common schemas. Also there are many more vurnerabilities than discussed here but ones presented are enough to demonstrate the importance of understanding whom and what you trust.

### The same password for all accounts
  ![The easiest way](/assets/same_password.svg)

<details><summary>Security</summary>
  
We have three threats there:
  - someone may steal your password when you enter it (either peeking over the shoulder, hijacking your Bluetooth keyboard, etc.).
  - someone may hack one of services you use and extract your password.
  - a service itself may have malicious intents and steal your password.

  Hacking any of your accounts puts all of them at risk. Not good at all.
</details>

<details><summary>Recoverability</summary>

Recoverability is quite good cause you don't need anything but your password to log into any account.
</details>

<details><summary>Usability</summary>

Usability is a bit more tricky for this case:
  - you only need to remember (and type) the single password, which is very good.
  - but different services may have different password policies. If you have a password like "123qwerty!{}" and want to register on a service requiring password to have at least one capital letter you need to change your password on ALL other services too.
  - whenever your password expires on one service you need to change it in all others too.

  Two other aspects are quite good. 
  - You only need to remeber a single password, given the assumption you have choosen one satisfying every website's needs like being at least X characters long, having small letter, capital letter and a digit, etc. Downside is changing password for every account once expired. If you use a service with a very paranoidal expiration time, like 90 days, it may become a real headache.
  - The password is "stored" in your head and online services are independent of each other. So the only SPoF which is your own brain. While loosing access to all your accounts after a car accident is bad, it isn't the worst problem at all.
</details>

### Social login
  Very popular practice is using a single identity to log into different services. A lot of websites allow to use your existing Facebook or Google account without registering a separate account on the website itself.

  In terminilogy of [OpenID](https://en.wikipedia.org/wiki/OpenID) the service storing your account is called Identity Provider (IP) and services using the identity provided by IP are called Relying Parties (RP).

  ![Social login](/assets/social_login.svg)

<details><summary>Security</summary>

  Althoug this schema has much smaller attack surface then the previous one, some threats still exist:
  - someone may steal your password when you enter it. This treat may be significantly reduced using good practices like [two-factor authentication][MFA].
  - someone may hack your account on IP and log into RP.
  - IP itself may impersonate you.
</details>

<details><summary>Recoverability</summary>

  The biggest problem of using single IP to log into every service is your dependency on IP. If IP has technical issues, blocks your account, goes bunkrupt or gets blocked by authorities you can't use any RP. This means your identity provider is your SPoF now.

  In other words you trust your IP with all your online accounts.
</details>

<details><summary>Usability</summary>

  From usability perspective this schema is even better than using the same password everywhere. You still need to remember only one password (and provide the 2FA if used) but password changing occurs rarer and is much easier.
</details>

### Local password manager
  If you don’t want to trust a 3rd party with all your accounts but still have one password to remember, you may use some local [password manager](https://en.wikipedia.org/wiki/Password_manager). Schematically almost every password manager is an encrypted datastorage (ED) with your passwords protected by an encryption key derived from your master password (MP).

  ![local manager](/assets/local_manager.svg)

<details><summary>Security</summary>

  Threats:
  - someone may steal your master password when you enter it AND your encrypted data. There may be many ways of stealing your data depends on how you manage it. Some ways are:
    - directly steal it from your device.
    - steal it from backup if you have any.
    - in case you use synchronize database on different devices, i.e. on home computer and an office one, an attacker may to steal the data from the synchronization media (USB stick, file-hosting, email, etc.)

  Overall security resembles two-factor authentication with factors of knowledge (your master password) and possession (encrypted storage). The advantage is not trusting an external Identity Provider anymore.
</details>

<details><summary>Recoverability</summary>
  You need realiablie backups. If you lost your password database, all your passwords stored there are lost as well. So unless you regularly backup your data, your database is SPoF.
</details>

<details><summary>Usability</summary>
  
  - Login process may be fairly simple, like typing your master password into the password manager and then copy (usually via copy-paste procedure) the unique password into a webpage.
  - To use your password manager on another machine you need to copy encrypted data somehow.
  - If you want to update managed passwords from different machines (i.e. a home desktop and a work laptop) you need to synchonize password database somehow.

  Unless you use your password manager on a single device and don't doing any backups, the usability is not as good. Securing your backups or synchonization channel may be untrivial tasks.
</details>
  
### Password manager with cloud synchronization
As I have told above, the worst problem of using a local password manager is managing backups of your password database and synchronizing it between different devices. The most convinient way is a file hosting service in some cloud.

![cloud manager](/assets/cloud_manager.svg)

<details><summary>Security</summary>

While putting your passwords into a cloud may seems unsafe it doesn't pose a security risk if you use a password manager with a decent encryption. As a potential attacker can't extract your passwords from the database without knowing the master password, you can use whatever cloud service you want.

To be on safe side we need assume that any information put into 3rd party system on the Internet will be stolen somewhen. This means that while being protected we lost our 2FA of database possession. As long as you think your strong master password is kept secret you are fine with this.

But if you want even more security, you may add one more authentication factor, i.e. hardware token. The actual implementation depends on the password manager used but in this article we may see it as a compound encryption key consisting of your master password and some data stored in the token.

This gives the best security of all described schemas:
- an attacker must have you master password, database, and token at the same time.
- you don't need to trust any 3rd party service.
</details>

<details><summary>Usability</summary>

- you still need to type your password.
- you need to present your token.
</details>

<details><summary>Recoverability</summary>

- You have no ways to recover you master password in case you forgot it.
- You need to have at least two tokens opening your data. It's a good idea to store one at home while bring the other with you. And sometimes to verify that both tokens are functional.
- If you lose your computer, you can download your database from the cloud.
</details>

## Summary table
Let's try to summarize different aspects of described approaches in a comparison table.

| Method | Authentication factors | Points of trust | Single points of failure | Synchronizing | When to change password |
| ------ | ---------------------- | --------------- | ------------------------ | ------------- | ----------------------- |
| Common password | Password | Every single service you use | Password | Not needed | After expiration on any service |
| Social login | Password, 2FA\* | Identity Provider | Password, 2FA\*, Identity Provider | Not needed | After expiration on the Identity Provider |
| Local password manager | Password, database\*\* | Password manager software | Password, (your device, if no backups used) | Portable media | When database or backup is stolen |
| Password manager with cloud sync and a token | Password, token | Password manager software | Password | Via file hosting | When token is stolen |  

\* depends on Indentity Provider<br>
\*\* if both main database and all backups are kept secret<br>

## Credits
<div>Credits for icons go to
<a href="https://www.flaticon.com/authors/good-ware" title="Good Ware">Good Ware</a>, 
<a href="https://www.flaticon.com/authors/freepik" title="Freepik">Freepik</a>, 
<a href="https://www.flaticon.com/authors/those-icons" title="Those Icons">Those Icons</a>, 
<a href="https://www.flaticon.com/authors/smashicons" title="Smashicons">Smashicons</a>, 
<a href="https://www.flaticon.com/authors/flat-icons" title="Flat Icons">Flat Icons</a>, 
<a href="https://www.flaticon.com/authors/srip" title="srip">srip</a>,
<a href="https://www.flaticon.com/authors/pixel-perfect" title="Pixel perfect">Pixel perfect</a>,
<a href="https://www.flaticon.com/authors/alfredo-hernandez" title="Alfredo Hernandez">Alfredo Hernandez</a>, 
<a href="https://www.flaticon.com/authors/payungkead" title="Payungkead">Payungkead</a> 
from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a></div>


[MFA]: https://en.wikipedia.org/wiki/Multi-factor_authentication
[SPoF]: https://en.wikipedia.org/wiki/Single_point_of_failure
[SS7]: https://www.theregister.co.uk/2017/05/03/hackers_fire_up_ss7_flaw/
[SIM]: https://www.wired.com/story/sim-swap-attack-defend-phone/
[passwords]: https://en.wikipedia.org/wiki/Password_strength
[NIST_EAG]: https://web.archive.org/web/20040712152833/http://csrc.nist.gov/publications/nistpubs/800-63/SP800-63v6_3_3.pdf
