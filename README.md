# Thinking about Identity

This started as a response to a very common question, should I change my password (tl;dr is yes, you should) and turned into a short rambling thing about identity.

Proving identity is about confirming a subject is who they claim to be.  This is different from authorization (what can you do if you prove your identity) which isn't going to be covered here.  This work is specifically about proving identity in the context of a digital system.

I should add a word about identity and access management (IAM).  This is a big and growing area of the security business but one I've only been indirectly involved in.  You can spend a lot of money (some of them, a very large amount of money!) getting a complicated system from someone who may or may not know what they're doing.  It's also an area that has undergone radical changes in the last few years.  In any event, IAM as a discipline or tool set is beyond what I plan to cover here, this essay won't get anywhere near that deep.

So, how do you prove someone is who they claim to be?  Traditionally, for digital systems the view is that there are three specific things that can prove identity: 	

	1. Something you know,
	2. Something you have, and
	3. Something you are.

The process is that a person would present 1 or more (multi-factor/MFA or two-factor/2FA) of these with an asserted identity.  The system then checks and says yes or no.  So what are they, how do they work and what should an aspiring security professional know about them?

The discussion is best started with #3 as the shortest to cover.  It is a reference to biometrics.  I'll assume most readers are at least familiar with the idea of finger-print or retina scanners, voice-print systems, etc.  All of those are examples of biometrics.  More generally, a biometric system consists of a sensor that reads some physical characteristic of the person claiming an identity, some method to digitize it then a mechanism that validates it as correct (or not) for the identity.  

Sensors have come a long way since the first generation of finger-print sensors that could be spoofed with a photocopy of the correct fingerprint.  They do, though, still have some weaknesses to think about.  Most systems, for example, don't directly use the biometric data itself, but rather a digital representation that is usually significantly simplified and in many ways acts similarly to a certificate.  Exploiting this might be similar to attacking a hash using collisions, for example, where you simply find a false positive and off you go.  The more individual elements of the underlying biometric data incorporated into the digital representation the more difficult finding an arrangement that produces the correct result will be, so maybe not always the easiest attack method.  It might also be possible to capture/re-inject the required data into a system to allow requested access through other than the expected path (in other words insert an ethernet device in the line, or hijack the I2C link, or clip onto the jtag interface, hijack a wifi session, ...)  Those of you that unlock your phone with your fingerprint or face are also using biometrics.  It might (or not) be possible to intercept the biometric data with malware for future use as one possible attack vector.  As well, the underlying data link might be a useful target.  

I can't add a lot more to the topic of biometrics since they're not really a feature of my life these days.  They're a technology and tool like any others and at that level not hard to understand or fit into a security program.  Such devices also act in a physical sense to reduce opportunistic tampering, much in the way a visible lock, even a really bad one, will discourage anyone who doesn't have a reason from trying to enter.  I wouldn't rely on this, but if you're not an obvious target, such things don't hurt.  If you are an obvious target, be aware of such systems providing a false sense of security.

The second factor is something you have.  Traditionally this would be a physical device (hardware dongle or similar) or codebook.  More recently digital tokens are starting to become common.  It's important not to mistake a secondary SMS as fitting this category.  The important element of a token is that it can't be separated from the key it provides.  SMS doesn't meet this requirement as it's not that hard (look up SS7 security, SIM cloning and hijacking, shoulder surfing, etc.) to get at the data without having the phone it was intended for.  An SMS is very much closer to a time-constrained second password.  That said, it's better than using only a normal password, it's just not a second factor (it's the same factor twice, which is a different thing).  You could (as I do) have a digital token installed on your phone rather than keep track of a USB dongle, Google Authenticator is an example.  There is (presumably) no way to get the key generated by the token without my phone so it actually is different from SMS and is in fact a token.  

Less commonly today, physical grid cards and code books can be used in this space.  There's a risk of a digital copy being made of the codebook or grid card which can lead to compromise and as physical things they can be lost or stolen.  In modern use it's common to see a lack of operational security awareness from the people grid cards and code books get issued to as it tends to be the less-technologically engaged groups who need a non-digital second factor.  If you have these things in use in your environment, awareness and training are probably your most imporant response to the issue of humans being human.

Used correctly and not otherwise compromised (bad implementation, copied grid cards, etc.) this factor can be very hard to get around.  That's not to say such systems haven't been compromised, but it's usually not the token that failed.  A growing trend is social-engineering or straight hacking to get the target to provide the correct second factor response as a way to get around the need for the token.

That brings us to the big piece, the "something you know", passwords.  This is the one thing everybody relies on to some degree.  The first consideration is that a password has the same value as the data it protects and should be treated that way.  I don't think I'd cry too long if someone hijacks my reddit acount and steals my fake Internet points, but my reaction would be very different if my bank account was compromised (which is a strong argument for 2FA/MFA on such accounts).  Therefore, I should have a different level of focus on each.  Another key consideration for passwords is that they are a rarely acknowledged example of security through obscurity.  A password is only effective at proving your identity if only you know it.  This becomes a serious concern if someone gets your password without your knowledge.  Which is why (see the top) the correct answer to the question of whether or not you should change your password is yes if you have any reason at all to ask the question (let alone if you suspect it's been compromised).  The worst time to find out someone got your password is when they take over your account.  This same issue is why it is so important to never use the same password for more than one system.  It eliminates risk from other (potentially less defended) systems being compromised leading to a breach in an important system and allows you to save better passwords for more important things.  Depending on what it's for, there's no problem with leaving a good password unchanged for a long time.  I generally recommend, though, that passwords for bank accounts and primary email should be changed regularly, at least a couple times each year and certainly if you see anything suspicious at all.  Remember, changing a password is really easy and quick, recovering an account (and anything that account controlled) is a major pain and never easy.  This is helped by a reasonable password management solution, which we will get to.

So what is a good password.  XKCD covers this quite well, but I think a bit of additional writing can help.  Lots of folk talk about entropy with very fuzzy ideas of what that might be.  In the context of a password hash, entropy is a representation of all the different combinations of characters that could have created the hashed value you're looking at.  If your password hash is derived from a string limited to only lower case alphabetic and 5 characters length there is much less entropy (fewer possible combinations of all the things available) than if you have upper and lower case, alphabetic and numeric and special characters, etc and 15 characters long.  This also tells you that passwords with a certain "profile", that is those that are selected using a common sequence of upper case, lower case, alphabetic, numeric, etc are less secure (more guessable, less entropy) than ones where the sequence (of character types) or profile is random or at least unpredictable. That doesn't mean password approaches that use a common pattern are necessarily bad, just that you need to be aware that they might not be as good as you might want or need.

Managing passwords can be a problem for many as well.  There is nothing wrong with a decent password manager and it is a common recommendation.  It's very important to understand how whichever one you choose actually works (where are your passwords actually stored, how are keys managed, what is persistent in memory, etc.) if you want to understand the actual risk.  It's also worth noting as I mentioned above, the value of a password is in what it protects.  That means the password for your manager is the most valuable one you have.  A passphrase is best, but see the previously mentioned XKCD.  

More old school is the notebook in a desk routine.  Assuming you're at low risk for bad-guys breaking into your house in preparation for a hacking attack against you  or your employer, this is also a pretty reasonable solution.  It's also a bit helpful for us older folks if the partner needs to find passwords after something has happened.  Don't (I can't stress this enough) put passwords on yellow sticky-notes attached to the monitor, especially if you let TV crews into your office for interviews.  Also, a generic text file is a very poor substitute for a password manager though not completely horrible if it lives only on an external storage device (USB key) that is not usually attached.  I wouldn't do that or recommend it, but it probably has a place in the tool box for some people.

There are some who take changing passwords to a new level and have a new password for every login.  They simply choose a long complex password they never have to remember for each login and simply use the forgot my password/password reset feature next time.  This might sound like a good idea but it has some risks.  Clearly, this can't be used for your primary email account.  That effectively makes your email password your global account password.  As well, if you lose your email, you immediately lose access to everything, which would not happen with a standard approach to passwords.  This can also hide signs of a compromised account, as one indicator of a problem is a password reset request that wasn't initiated by you.  If your in-box is full of password reset requests, there's a good chance you'd never see the one triggered by bad guys.  It's another of those things I generally wouldn't do, but you get to make your own decisions.  As always in this business, you have to decide if this is OK for you as you're the one who will bear the consequences of that choice.  

The last thing I want to mention is closely tied to password management.  A lot of bad guys have noticed a tendency to overshare on social media, including answers to account recovery questions.  These function like a password, in that the answers are expected to be known by you and no one else.  Unfortunately, a lot of the questions are reality based (your dog's name, your high school, whatever) and also the kinds of details that turn up in social media feeds.  If an attacker can get to an account recovery mechanism that asks questions he has the answers to, that account is compromised.  A more secure approach to account recovery is a second or recovery account, which is the standard for much of the digital industry these days.  Account recovery questions are not quite as bad as storing passwords in plain text, but if I see it on an account I consider important, I'm likely to look for a new provider of whatever service is involved.

Proving identity in a digital context is not that different from doing so in a physical context (driver's license is something you have, a picture is a biometric check, etc) but it does come with some additional risks.  The biggest is that these are, in the end, complex systems that can have errors in implementation or execution or any of a variety of other vulnerabilities.  Other than awareness and operational security training, all we can do is stack elements of a solution (for example MFA) and hope that they are each independant enough to not fail as a collective at the worst possible time.
