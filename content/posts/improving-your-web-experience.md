---
title: "Improving your Web UX | Ads & Tracking free"
date: 2022-08-14T11:46:56+02:00
draft: false
tags: [ rant, privacy, tools ]
---

The fact that Web surfing has deteriorated lately is no news at all. We can thank
the BigCorps for that because they've managed to gain so much control of that
and that's not all, they've f#cked up the way marketing is done as well.

How many times have you tried to peacefully read an article on an online newspaper 
without being disturbed by those pesky pop-ups to Sign-up to Newsletters
or get a free ebook on non-sense? Well, this is breaking usability in many ways.

That's not even the tip of the iceberg, if we go deeper, we see they're tracking
all our movement on the <abbr title="World Wide Web">WWW</abbr> (spelled dub-dub-dub)
for marketing purposes but also selling that information to 3rd parties without
even telling you what's going to happend with **your** data.

# Why Privacy matters?

That's a question with many edges. I think the real questions should be:
* Would you
    * give your home keys to a total stranger?
    * give your banking credentials to a stranger?
    * share all your medical/financial information publicly?

If the answer to the above questions is **No** then you know what **privacy**
is about by this point.

{{< peertube src="https://media.zat.im/videos/embed/35badfed-5322-48ac-b5c1-71b1ad88262e" >}}

# Improving your Web surfing experience

The simplest way we can improve our <abbr title="User Experience">UX</abbr>, and
currently the only way we have full control, is to block ads, pop-ups and tracking.

## Privacy redirect (browser extension)

{{< button href="https://github.com/SimonBrazell/privacy-redirect" target="_blank" >}}Source{{< /button >}}

Supported by {{< icon "firefox" >}} {{< icon "chrome" >}} {{< icon "edge" >}}

## uBlock Origin (browser extension)

{{< button href="https://github.com/gorhill/uBlock" target="_blank" >}}Source{{< /button >}}

Supported by {{< icon "firefox" >}} {{< icon "chrome" >}} {{< icon "edge" >}} {{< icon "opera" >}}

## Privacy Redirect for Safari (€ 1.99 browser extension)

{{< button href="https://github.com/smmr-software/privacy-redirect-safari" target="_blank" >}}Source{{< /button >}}

Supported by {{< icon "safari" >}} {{< icon "apple" >}}

## Newpipe (Android app)

{{< button href="https://newpipe.net" target="_blank" >}}Source{{< /button >}}

Supported by {{< icon "android" >}}

## Freetube (Desktop app)

{{< button href="https://freetubeapp.io" target="_blank" >}}Source{{< /button >}}

Supported by {{< icon "windows" >}} {{< icon "apple" >}} {{< icon "ubuntu" >}}
{{< icon "fedora" >}} {{< icon "flatpak" >}} {{< icon "linux" >}}

# Detox your home WiFi LAN

Get yourself a Raspberry Pi 3. Why 3 and not 4? Because 4 requires a cooler and
some also install a heatsink. There are many opinions on heatsink's efficiency.
To keep things simple, just get a <abbr title="Raspberry Pi 3">RPI3</abbr> kit, it contains everything you need to
boot your OS.

Flash [raspbian](https://www.raspbian.org) on the MicroSD card and continue to
install and setup [PiHole](https://pi-hole.net).

Here are a few useful AdLists:
* https://oisd.nl
* https://github.com/Perflyst/PiHoleBlocklist

{{< alert "lightbulb" >}}
Not possible to block YouTube ads. Unfortunately they use a shitty strategy with
a ton of domains and subdomains which is impossible to come up with a capture-all
pattern. But the above browser extensions help you circumvent that -- it's currently
the only solution.
{{< /alert >}}

Thus you'll have no more ads on your whole WiFi <abbr title="Local Area Network">LAN</abbr>
so you can peacefully read those news articles without any pesky attention grabbers.
