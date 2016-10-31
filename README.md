Public Database of Software Hashes
===================================

# Problem with software authenticity

We all use software. Everywhere. From our servers, to personal computers and our
phones (which are becoming extensions of our brains, BTW). From our Mars
landers, to cars, to home routers, TVs, and even toasters. From medical MRIs, to
pacemakers.

Yet, curiously, the dramatic majority of software and firmware, especially open
source software, can not be _authenticated_ in any meaningful way. By
_authenticating_ we understand a process of establishing the authorship _and_
integrity of the software.

It's probably quite obvious what does _integrity_ mean? Integrity assurance
guarantees that the software has not be tampered nor replaced anywhere between
its author and us.

It might be less obvious that integrity does not imply trustworthiness,
understood as security or non-harmfulness. Indeed, there is nothing that could
stop an author from producing either incidentally buggy or intentionally harmful
software and properly signing it with his or her key.

Moreover, there is nothing that stops others from generating their own keys,
adding whatever description strings to the keys (e.g. "Linux Kernel signing
key"), and using these keys to sign modified copies of the software. In other to
protect against such attacks, the software author needs to somehow ensure that
all the interested people could easily be able to find out which key is the
original signing key. This is typically achieved by publishing the keys in
various places, such as on twitter, keybase.io, etc.

But what if the machine on which the author signs their software gets
compromised? Of course the attacker could then sign modified copies of the
software. More importantly, the attacker can _selectively_ feed backdoored
software to only some of the users, to minimize chances of the attack discovery.

All the above scenarios are of concern when the original author has made an
effort and actually attempted to sign their software. Yet, most of the software
is offered without any kind of signature for verification.

# Proposed solution

Ideally we would like to have a simple oracle that answers a simple question: is
this software safe to install and run? Unfortunately, as briefly explained above
it is not possible to provide any generic mechanism to answer such questions
(even if the author employs some scheme for signing, not to mention if they
don't).

What we, as a community, could do, however, is to attempt to build a publicly
available database of software _hashes_. The database is a collection of
statements from various witnesses of the following form:

    I -- {id} -- have downloaded the software `XYZ`, attempted the best
    verification I could made, and observed it hashes into what is written in
    the `hash` file. The steps I followed to calculate the hash are given in
    `origin.{id}` file. I sign this statement with two detached signatures
    placed in `hash.sig.{id}` and `origin.sig.{id}`.
    
    I attach my public keys in `witnesses/keys/{id}.key` and also provide steps
    to establish my identity and verify my key in `witnesses/{id}` file.

The `hash` and `origin.{id}` files should be placed in the following
directories:

    /{firmware | os | apps}/{vendor name}/{version}/

Here's an example:

    codehash.io/os/qubes/
    ├── 3.2
    │   ├── hash
    │   └── origin.joanna
    └── vendor_keys
        ├── origin.joanna
        ├── qubes-master-key.asc
        └── qubes-release-3-signing-key.asc
 
The following command should be used to create the `hash` file:

    sha256 <file_to_hash> > hash


# Example

    ├── apps
    ├── firmware
    │   └── openwrt
    │       ├── 15.05.1-src
    │       └── 15.05-src
    │           ├── hash
    │           └── origin.joanna
    ├── os
    │   ├── qubes
    │   │   ├── 3.2
    │   │   │   ├── hash
    │   │   │   └── origin.joanna
    │   │   └── vendor_keys
    │   │       ├── origin.joanna
    │   │       ├── qubes-master-key.asc
    │   │       ├── qubes-release-1-signing-key.asc
    │   │       ├── qubes-release-2-signing-key.asc
    │   │       └── qubes-release-3-signing-key.asc
    │   └── tails
    │       ├── 2.4
    │       │   ├── hash
    │       │   ├── origin.joanna
    │       │   └── tails-i386-2.4.iso.sig
    │       ├── 2.6
    │       │   ├── hash
    │       │   └── origin.joanna
    │       └── vendor_keys
    │           ├── origin.joanna
    │           └── tails-signing-key-2015.asc
    ├── README.md
    ├── TODO
    └── witnesses
        ├── joanna
        ├── keys
        │   ├── joanna.key
        │   └── README.md
        └── README.md

# FAQ

* How do you obtain the hashes that are in the repo?

The steps are give in the corresponding `orign.{id}` files, where `{id}`
identifies the witness (key) who checked the hash.

* Why a git repo?

A git repository can be trivially cloned, archived, and forked. It can also be
easily hosted for free at a number of public services, such as GitHub.

Additionally, it provides tracked history of changes and integrity (through
signed tags and/or commits), which can be thought of as an additional security
mechanism to the explicit GPG signatures we display.

* Do you make any guarantees about the software included here?

No, please see Disclaimers.

* Do you assume we must trust GitHub (or any other hosting service)?

No, you should always verify GPG digital signatures to check integrity of any
information contained herein.

* Are you going to accept submission about hashes from anybody?

No, please see Disclaimers.

* Can I fork this?

Yes, please see the License section below.

# License

TODO

# Disclaimers

1. The list of software contained in this repository should not be considered an
   endorsement for using of any of the software by any of the witnesses who
   signed their hashes.

2. There is no guarantee that any of the code referenced in this repository is
   not, were not, or will not become insecure, malicious, harmful, or otherwise
   differing from what its vendors claims about it. The match of the hashes only
   means that certain group of witnesses were exposed to the same code,
   byte-by-byte, as you.

3. You can send a patch/pull request with hashes of non-included software, but
   please keep in mind this will likely be ignored, unless 1) I consider you a
   relatively trustworthy person, 2) I'm capable of checking the authenticity of
   your submission with reasonable assurance (so, I'm quite confident you use a
   reasonably good setup for your keys), and 3) the software about which you are
   to attest is considered of some interest to a wider audience.
