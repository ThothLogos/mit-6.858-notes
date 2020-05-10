# Lecture 1 - Introduction

My notes here will be brief. This was a high-level survey of security concerns, systems design,
policy considerations, and examples of when things go wrong. Real world policy failures and design
bugs are discussed to illustrate that weaknesses in security aren't just about software and network
configurations. In a world of interlinked and dependent services with various policy weaknesses,
it's possible that seemingly unrelated problems when combined can create unforseen failures.

Attackers are creative; security is difficult. A focus on __increased cost__ of attacks can be a
prudent approach to security. Making things costly or unrealsitic is often easier and results in
better security payoffs versus engineering or assuming perfect reliability of some security scheme.

The class ends with a demonstration of a basic buffer overflow in C-code. The instructor uses `gdb`
debugger to step through the process and show the 