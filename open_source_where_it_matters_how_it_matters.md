Free Software: Where it matters, How it matters
===============================================

The following discussion exposes very personal opinions, you can [skip the first part)(#what-is-free-software) if you dont want to be bothered. Opinions expressed are solely my own and do not necessarily express the views or opinions of my employer or my coworkers.

Why should it matter?
---------------------

Every one of us has moral values. Our moral values are influenced by our surroundings, but I am one of those who thinks that the moral comes from a Godly realm. The moral is not for us to decide based on our preferences, but it is an absolute truth that we can only aspire to approach.

In the ancient times, you would say God dictates the moral, but for present times, I would prefer to refer to the cosmology. What is ultimately good is something that improves the state of the universe overall. The evil is what makes the universe worse. The moral is this distinction between the ultimate good and evil.

And because we don't know the whole universe, and we will never know, we can only guess as what is good and evil. This is our personal sense of the moral, that is different for everyone, and must be distinguished from the ultimage good and evil we may never fully understand.

Now, when we look at free software, that's obviously a good thing. it advances the state of things. In French we would say *que ça fait avancer le shilblick*.

And non free software is evil.

What is Free Software
---------------------

The first definition is the one from the [Free Software Foundation](https://www.gnu.org/philosophy/free-sw.html):

- The freedom to run the program as you wish, for any purpose (freedom 0).
- The freedom to study how the program works, and change it so it does your computing as you wish (freedom 1). Access to the source code is a precondition for this.
- The freedom to redistribute copies so you can help your neighbor (freedom 2).
- The freedom to distribute copies of your modified versions to others (freedom 3). By doing this you can give the whole community a chance to benefit from your changes. Access to the source code is a precondition for this.

This can be considered quite abstract though, so the [Debian guidelines](https://www.debian.org/social_contract#guidelines) might help. A free software:

- must allow free redistribution
- must contain its source code
- must allow derived works
- may impose restrictions of how the derived works is distributed to separate what is the original work and what is the derived work.
- must not discriminate against persons or groups
- must not discriminate against fields of endavor
- must not restrict redistribution as a free software
- must not restrict the software freedoms to the Debian distribution only
- must not impose restriction on unrelated software

This was lated updated to form the [Open Source definition](https://opensource.org/osd).

What makes the Debian definition unique though are the compliance tests to ensure the software is free. Quickly:

- You must be able to use the free software on a desert island with no external contact.
- In case you are a dissident, and you contribut to free software, you must not be forced to share your identity or the modifications you made to third parties. Only the persons you distribute the modified software to are entitled to get the modified source.
- In case a free softare author is hired by an evil organization, the author must not be able to revoke the freedom to software he wrote.

### What is free software, again ###

All softwares are free, there is no software that isn't free because non free softwware is evil. It should thus be considered illegal, and it it morally acceptable to consider all software to be completely free.

If you are living in a legal framework that allows copyright, you might not want to apply this universal human right because you may be prosecuted for this.

### What are Free Software Licences ###

Free Software licences are legal tools to enable free software to exist in a world that do not recognize this universal human right. As tools, they need to be employed thoughtfully depending on the effect you want to achieve.

What are the other threats?
---------------------------

Non Free software is not the only threat to our civilisation. Let's look at the other threats:

- **Locked down hardware** are an issue because even if the source code is free, it may perform counter-features such as DRM or leaking personal information. Free software is not good enough, the device owner must be able to replace the software running on the device.
- **Software as a Service (SaaS) and centralized protocols** is also a problem because people loose control over their own data. Their information is stored and processed by external entities beyond their control.
- **closed formats, standards and protocols** is also a problem. The data must be stored in a format that ccan be read back even when the original tools to produce that format are no longer available.

How can free software licences mitigate the threats
---------------------------------------------------

### Liberal License (MIT, BSD, ...) ###

Those licenses are best suited to widely spread an open standard, format or protocol. It doesn't matter if it is made closed source because the goal is to make sure that the standard is widely adopted, even by non free software. Examples of correct usage of these licenses are:

- For a free format implementation. This could be a video or audio codec, a document format, ...
- An implementation of a protocol that can help fix the SaaS and centralization problem

### Lesser GPL ###

The Lesser GPL can be used instead of the most libral licenses when the developped code must stay free software. The problem is that it may prevent some proprietary software to adopt the technology employed by the software.

### Version 3 of GPL, LGPL and AGPL ###

The third version of these licences is useful to prevent vendor lock down as they resuire to share the installation methods, which includes keys to install the software. These licenses are most useful in:

- Software likely embedded such as a kernel, boot loader, C library
- Any other software in general as it guarantees that the software stays free

### Affero GPL ##

The Affero GPL is mostly useful for softwares that could be used as a service. For example:

- Web applications should be licensed using the Affero GPL as they are the most likely subject to beeing used in a SaaS.
- Any application should consider the AGPL license as it can always be transformed to a SaaS application. This could help mitigate this.
