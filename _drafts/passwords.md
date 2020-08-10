---
layout: post
title:  "Whom I trust my online accounts?"
date:   2020-04-07 22:01:23 +0200
---
In this post I describe how I managed [password fatigue](https://en.wikipedia.org/wiki/Password_fatigue) while balancing the following contradicting requirements:
- **Security** An adversary cannot log into my account.
- **Recoverability** I can recover access to my account if something goes wrong.
- **Usability** I can effortlessly log into my account whenever I want.

This is not a comparison of different password managers but an attempt to show how threat modelling helps you choose a right solution based on your own needs.
<cut />
### Introduction
I personally have 20+ accounts (e-mails, social networks, e-shops, etc.). Some of those are of no value and have not any sensitive information but losing others would be a very unpleasant event.
For a few years I used a common weak password for invaluable accounts, a bit stronger for my primary e-mail, and "Sign with Google" for the rest. But everything has its price and authenticating everywhere with a single account is not an exclusion. One day I waked up with quite an ordinary questions:
1. What if someone hacks into my Google account?
2. Is Google itself able to access every of my bound accounts?
3. What if one day Google terminates my account?
4. What if government blocks the Google?

This led me to a sad conclusion that I "put all my eggs in one basket". Not the best idea, no matter how good and strong the basket is.
Then I started to look for some better way how to store my passwords. After reading through different web sites for a few days I realized that I was looking for a solution without clear understanding of my requirements.

### Requirements
Defining your requirements is a crucial step for choosing a proper tool. Cause different requirements contradict each other, one need clearly understand trade-offs made in one or another solution. Someone may prefer more secure but less user-friendly approach, while another prefers easy login process.
# Security
  - The fewer 3rd party services I need to trust the better.
  - The more efforts an attacker needs to stole my credentials the better.
  - The more time I have to mitigate an attack the better.

While "don't trust 3rd party" is a quite obvious recommendation, I would like to focus on the second bullet.

A common way to improve security is adding another factor of authentication. One of the benefits of a properly designed [multi-factor authentication][MFA] is that every factor requires an attacker to use different attack. The same as securing your car with a steering wheel lock and an electronic immobilizer, so a thief should be able to both crack the electronic code and lockpick the wheel lock. 

Ideally all factors should be orthogonal to each other, so breaching one factor doesn't compromise others. Let's demonstrate it with simple diagrams below.
![Entagled MFA](/assets/mfa.svg)

Here we assume classical scheme of 2FA with a password and a single use verification code sent in SMS. Blue and yellow figures represent attacks on a password and SMS respectively. If the password can be reset via phone, those two factors are not orthogonal anymore as succefful breach of the SMS factor compromise the password too.

By the way, I wouldn't recommend SMS as an authentication factor in any schema because of known [attacks][SS7] on SS7 protocol and SIM swap [crimes][SIM]. Authentication apps provide much better security and at least comparable usability.

# Recoverability
The second worst thing after getting your account hacked is having it gone for good cause you've lost the authentication data.
The less [Single Points of Failure (SPoF)][SPoF] the better. SPoFs are:
  - Data without backups (including passwords in my brain).
  - 3rd party services storing my data, unless I have a backup in other place (locally or another service).

Giants like Google, Facebook, Twitter, and others may appear reliable enough to use them instead of independent accounts for your favorites e-shop, online cinema, etc. The problem arises when your account has been permanently deleted, you disagree with new Terms of Service, the service itself has been blocked by your government, you've moved to a country where this service is not available, etc.

# Usability
  - The less manual actions per login the better.
  - The less manual actions for setting up new devices the better.
  - The less passwords to remember the better.
  - The less frequently I change my passwords the better.

Even the most secure solution is useless, if it has so terrible UX that you don't want to use it. Just imagine someone offers you a super secure authentication method requiring you to enter a PIN on a GPS-enabled pinpad while looking at a GPS-enabled eye iris scanner. This may be a very secure method but you are unlikely to use it for every online account.

### Threat models for different approaches
Now, after setting criterias, we may actually compare different approaches. The list below is far from being exaustive but my goal is to demonstrate some most common schemas. Also there are many more vurnerabilities not discussed here but ones presented are enough to demonstrate the importance of understanding whom and what you trust.
# Common password for all accounts
  ![The easiest way](/assets/same_password.svg)

  We have three threats there:
  - someone may steal your password when you enter it (either peeking over the shoulder, hijacking your Bluetooth keyboard, etc.).
  - someone may hack one of services you use and extract your password.
  - any service you use may have malicious intents and steal your password.
  Hacking any of your accounts puts all of them at risk. Not good at all.

Recoverability is quite good cause you don't need anything but your password to log into any of the account.

Usability is a bit more tricky for this case:
  - you only need to remember (and type) the single password, which is very good.
  - but different services may have different password policies. If you have a password like "123qwerty!{}" and register to a new service that requires password to have at least one capital letter you need to change your password on ALL other services too.
  - whenever your password expires on one service you need to change it in all others too.

  Two other aspects are quite good. 
  - You only need to remeber a single password, given the assumption you have choosen one satisfying every website's needs like being at least X characters long, having small letter, capital letter and a digit, etc. Downside is changing password for every account once expired. If you use a service with a very paranoidal expiration time, like 90 days, it may become a real headache.
  - The password is "stored" in your head and online services are independent of each other. So the only SPoF which is your own brain. While loosing access to all your accounts after a car accident is bad, it isn't the worst problem at all.

# Social login
  Very popular practice is using a single identity to log into different services. A lot of websites allow to use your existing Facebook or Google account without registering new account on the website itself.

  In terminilogy of [OpenID](https://en.wikipedia.org/wiki/OpenID) the service storing your account is called Identity Provider (IP) and services using the identity provided by IP are called Relying Parties (RP).

  ![Social login](/assets/social_login.svg)

  Althoug this schema has much smaller attack surface since your password (or its hash) isn't stored by every service you use, some threats still exist:
  - someone may steal your password when you enter it. This treat may be significantly reduced using good practices like [two-factor authentication][MFA].
  - someone may hack your account on IP and log into RP.
  - IP itself may impersonate you.

  From usability perspective this schema is very similar to using the same password everywhere. You still need to remember only one password (and provide the 2FA if used).

  The biggest problem of using single IP to log into every service is your dependency on IP. If IP has technical issues, blocks your account, goes bunkrupt or gets blocked by authorities you can't use any RP. This means your identity provider is your SPoF now.

  In other words you trust your IP with all your online accounts. IP may leak your credentials (intentionally or not), impersonate you, delete your account, etc.

# Local password manager
  If you don't want to trust a 3rd party all your accounts but still has a one password to remember you may use some kind of local [password manager](https://en.wikipedia.org/wiki/Password_manager). Using a single master password (MP) you unlock the password's database (PD) storing all your protected passwords (PP) in some local storage.

  ![local manager](/assets/local_manager.svg)

  Threats:
  - someone may steal your master password when you enter it AND your PD. There may be many ways of stealing your PD depends on how you manage it. Some ways are:
    - directly steal it from your devices.
    - steal it from backup if you have any.
    - in case you use your PD on different devices, i.e. on home computer and an office one, an attacker may try to steal PD from the synchronization media (USB stick, file-hosting, email, etc.)

  This resembles two-factor authentication with factors of knowledge (your master password) and possession (media with your password database). It is both good news and bad news at the same time. On the good side you don't need to trust an external Identity Provider anymore.
 
  On the bad side you have the following challenges to solve:
  - Backups. If you lost your password database all your passwords stored there are lost as well. Similar to situation when you had enabled 2FA in your social account and then lost your phone.
  - If you want to use your password manager on different machines (i.e. a home desktop and a work laptop) you need to synchonize password database somehow.

  Unless you use your password manager on a single device and don't doing any backups, the usability is not as good. Securing your backups or synchonization channel may be untrivial tasks.

  Unless you store your PD on at least two independent media and keep those in sync your PD is a SPoF. If you lost it you may wave goodbye to your passwords.
  
# Password manager with cloud synchronization
As I have told above, the worst problem of using a local password manager is managing backups of your password database and synchronizing it between different devices. The most convinient way is a file hosting service in some cloud.

![cloud manager](/assets/cloud_manager.svg)

While putting your PD in a cloud may seems unsafe it doesn't pose a security risk if you use a password manager with a decent encryption. As a potential attacker can't extract your passwords from PD without knowing the master password, you can use whatever cloud service you want.

To be on safe side we need assume that any information put into 3rd party system on the Internet will be stolen somewhen. This means that while being protected we lost our 2FA of PD possession. As long as you think your strong master password is kept secret you are fine with this.

But if you want even more security you may add one more authentication factor, i.e. hardware token. The actual implementation depends on used password manager but in this article we may see it as a compound encryption key consisting of your master password and some data stored in the token.

Now let's summarize different aspects of this solution.

Security:
- an attacker must have you master password, database, and token at the same time.
- you don't need to trust any 3rd party service.

Usability:
- you still need to type your password.
- you need to present your token.

Recoverability:
- You have no ways to recover you master password in case you forgot it.
- If your token allows to create it's duplicate you need to do it to have a backup. It's a good idea to store one at home while bring the other with you.
- If you lost your computer you can download your database from the cloud.

# Summary table
Let's try to summarize different aspects of the discribed approaches in a comparison table.


| Method | Authentication factors | Points of trust | Single points of failure | Synchronizing | Password expiration |
| ------ | ---------------------- | --------------- | ------------------------ | ------------- | ----------------- |
| Common password | Password | Every single service you use | Password | | On any single service |
| Social login | Password, 2FA\* | Identity Provider | Password, 2FA\*, Identity Provider | | On Identity Provider |
| Local password manager | Password, database\*\* |  | Password, (your device, if no backups used) | Portable media | |
| Password manager with cloud sync and a token | Password, token |  | Password | Via file hosting | |  

\* depends on Indentity Provider<br>
\*\* depends on backups security<br>

### <a name="IDS"></a> Early warning system
The purpose of it is very simple: provide you enough time to change all your passwords if something goes wrong. I.e. if I have enanbled SMS as 2FA and suddenly got a verification code I haven't requested I can suspect that my password is stolen and someone is trying to log into my account. 

Even if I'm safe for now, it's better to change password before the attacker got my phone number and intercepted my SMS. This kind of attack [was described in 2014](https://www.theregister.co.uk/2014/12/26/ss7_attacks/) and succesfully [exploited in 2017](https://www.theregister.co.uk/2017/05/03/hackers_fire_up_ss7_flaw/). Please, don't use it in 2020.

If I don't want to change my password every few months I need some indicator that someone tries to hack me. In an example above it was SMS generated by the enabled two-factor authentication that show me that other factor (the password) is compromised.

Let's analyze the following case: I use a good password manager that stores my passwords in a file encrypted with a master password.
Under assumption that the encryption can't be broken in less than 3 months (depends on the strength of the master password and an attacker capabilities) I have two-factor authentication already. Those factores are the master password and the file.

As far as I can detect leaking any of them (including the file's backups) I am safe. 
- In case I leaked my master password I need to change it in every copy of the encrypted file to protect my passwords in case of leaking the encrypted file.
- In case I leaked the encrypted file I need to change all password stored there and the master password in no more than 3 months.

[MFA]: https://en.wikipedia.org/wiki/Multi-factor_authentication
[SPoF]: https://en.wikipedia.org/wiki/Single_point_of_failure
[SS7]: https://www.theregister.co.uk/2017/05/03/hackers_fire_up_ss7_flaw/
[SIM]: https://www.wired.com/story/sim-swap-attack-defend-phone/
