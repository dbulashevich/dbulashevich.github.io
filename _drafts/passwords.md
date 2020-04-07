---
layout: post
title:  "Whom I trust my online accounts?"
date:   2020-04-07 22:01:23 +0200
---
In this post I describe how I managed [password fatigue](https://en.wikipedia.org/wiki/Password_fatigue) while balancing the following contradicting requirements:
- **Security** Adversary cannot log into my account.
- **Usability** I can effortlessly log into my account whenever I want.
- **Recoverability** I can recover access to my account if something goes wrong.

This is not a comparison of different password managers but an attempt to show threat models for different approaches and provide a background for choosing a right solution based on your own needs.
<cut />
# Introduction
I personally have 20+ accounts (e-mail, social networks, e-shops, e-banking, etc.). Some of those accounts are of no value for me and have not a lot of sensitive information but losing other would be a very unpleasant event.
For a few years I used a common weak password for invaluable accounts, a bit stronger for my primary e-mail, and "Sign with Google" for the rest. But everything has its price and authenticating everywhere with a single account is not an exclusion. One day I waked up with quite an ordinary questions:
1. What if someone hacks into my Google account?
2. Is Google itself able to access every of my bound accounts?
3. What if one day Google terminates my account?
4. What if government blocks the Google?

This led me to a sad conclusion that I "put all my eggs in one basket". Not the best idea, no matter how good and strong the basket is.
Then I started to look for some better way how to store my passwords. After reading through different web sites for a few days I realized that I was looking for some good solution but had no clear requirements for that solution.
So let's talk about requirements for our hypothetical password manager.
# Requirements
- **Security**:
    - If everything is OK an attacker needs excessive amount of time and/or computational power to break into my account.
    - [Intrusion detection](https://en.wikipedia.org/wiki/Intrusion_detection_system). I want the system to warn me about secret data leakage before an attacker is able to log in to my account.
- **Usability**:
    - The less actions to log in is better.
    - The less passwords to remember is better.
- **Recoverability**:  The less [Single Points of Failure (SPoF)](https://en.wikipedia.org/wiki/Single_point_of_failure) is better. By SPoF I don't mean internet provider, cause I can switch to another one or use my phone as mobile hot spot, but rather a password DB stored on a USB Flash with no backups. In other words SPoF is some data (i.e. password) that is vital for logging in but has no backup or recovering method. 
**Side note**: as far as you are able to restore your password with the help of website's technical support it doesn't count as SPoF.

# Threat models for different approaches
- Single password for all accounts
  ![The easiest way](/assets/same_password.svg)

  We have three threats there:
  - someone may steal your password when you enter it (either peeking over the shoulder, hijacking your Bluetooth keyboard, etc.).
  - someone may hack one of services you use and extract your password.
  - a service itself may have malicious intents and gather passwords.
  While you can't do anything with the latest two threats re-using the same password for different accounts amplifies those problems. After hacking one of your accounts the attacker gets access to all of them. Not good at all.

  Two other aspects are quite good. 
  - You only need to remeber a single password, given the assumption you have choosen one satisfying every website's needs like being at least X characters long, having small letter, capital letter and a digit, etc.
  - The password is "stored" in your head and online services are independent of each other. So the only SPoF which is your own brain. While loosing access to all your accounts after a car accident is bad, it isn't the worst problem at all.
- Social login
- Local password manager
- Browser's built-in password manager with cloud synchronization
