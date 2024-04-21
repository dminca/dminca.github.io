---
title: "ZapCon 21"
date: 2021-02-25T21:52:27+01:00
draft: false
comments: true
toc: true
images:
categories:
- Conferences
tags:
- devsecops
- itsec
- security
---

[ZapCon '21][1] was scheduled on 9th March 2021

# What was interesting

## [Robot Framework][3]
[Democratizing ZAP with test automation and DSLs][7] by Abhay Bhargav

* BDD (Behaviour Driven Development) in IT-Security
    * apparently, there's this [robotframework][robo] which makes writing test cases
      in quite a simple way, and you can run pentests on your UI application

## [Why using a DAST scanner?][2]
DAST (Dynamic Application Security Testing) keeps app secure:
* test your running app
* find bugs before they are deployed to prod
* mimick how attackers view your system
* test how the whole system works not just components
* give you low **signal to noise ratio**
* reveal real risks not just theoretical ones

## [ZAP automation framework][4]
by Simon Bennetts -- ZAP Project Lead

* [DEMO available][5]
* it will support core features like:
  * passive scanning
    * configuration
    * waiting to complete
  * traditional spider
  * active scanner
* additional addons supported

## [Mobile app security with OWASP ZAP][6]
by Ankush Mohanty & Milan Sen

* [DEMO available][8]
* in mobile AppSec, Traffic Analysis has major role in finding various server
  side vulns
* some common vulns uncovered by Traffic Analysis include (but not limited to)
  * secure communication validation (SSL/TLS or HTTP)
  * server side open ports
  * PII data or Server information leak
  * Server side Brute force/lock out validation
  * validation of resource utilization vulnerability
  * Server side authorization vulns

## [Enhance ZAP with feedback-based fuzzing][9]
by Khaled Yakdan

* Coverage-guided, in-process fuzzing for the JVM -> [github.com/CodeIntelligenceTesting/jazzer][10]
* [DEMO CVE-2021-23899][11]
* feedback-based fuzzing is great for finding bugs that cannot be unvealed
  during blackbox testing

# Wrap up

Overall, the conference was pretty cool and much knowledge was gained, most
interesting part for me was the BDD with the Robot Framework, that's actually
something interesting to implement in your Sec pipelines and you can actually
run it in headless mode (no UI, no browser required).


[1]: https://zapcon.io
[zapcon]: https://youtu.be/xlt---D9I3w
[robo]: https://robotframework.org
[2]: https://youtu.be/xlt---D9I3w?t=890
[3]: https://youtu.be/xlt---D9I3w?t=2406
[4]: https://youtu.be/xlt---D9I3w?t=4256
[5]: https://youtu.be/xlt---D9I3w?t=4561
[6]: https://youtu.be/xlt---D9I3w?t=7190
[7]: https://youtu.be/xlt---D9I3w?t=1190
[8]: https://youtu.be/xlt---D9I3w?t=7790
[9]: https://youtu.be/xlt---D9I3w?t=13245
[10]: https://github.com/CodeIntelligenceTesting/jazzer
[11]: https://youtu.be/xlt---D9I3w?t=13874
