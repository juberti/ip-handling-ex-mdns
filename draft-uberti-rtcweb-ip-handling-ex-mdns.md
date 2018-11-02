---
title: WebRTC IP Address Handling Extensions for Multicast DNS
abbrev: ip-handling-ex-mdns
docname: draft-uberti-ip-handling-ex-mdns-00
date: 2018-11-02
category: info

ipr: trust200902
area: General
workgroup: RTCWEB
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: J. Uberti
    name: Justin Uberti
    organization: Google
    email: juberti@google.com
 -
    ins: J. de Borst
    name: Jeroen de Borst
    organization: Google
    email: jeroendb@google.com
 -
    ins: Q. Wang
    name: Qingsi Wang
    organization: Google
    email: qingsi@google.com
 -
    ins: Y. Fablet
    name: Youenn Fablet
    organization: Apple Inc.
    email: youenn@apple.com

normative:
  RFC2119:
  RFC4941:

informative:
  IPHandling:
    target: https://tools.ietf.org/html/draft-ietf-rtcweb-ip-handling
    title:  WebRTC IP Address Handling Requirements
    author:
      ins: J. Uberti
      ins: G. Shieh
    date: 2018-04-18
  mDNSCandidates:
    target: https://tools.ietf.org/html/draft-ietf-rtcweb-mdns-ice-candidates
    title:  Using Multicast DNS to protect privacy when exposing ICE candidates
    author:
      ins: J. de Borst
      ins: Y. Fablet
      ins: J. Uberti
      ins: Q. Wang
    date: 2018-10-22

--- abstract

This document extends the previous WebRTC IP Address Handling Requirements
with new modes that make use of Multicast DNS ICE candidates, and updates
the recommendations accordingly.

--- middle

Introduction {#problems}
============

{{IPHandling}} describes the privacy problems associated with exposing IP
addresses to web applications, but admits that there is no solution to the issue
of exposing private IP addresses that does not carry a corresponding
impact on connectivity.

{{mDNSCandidates}} introduces a new technique based on Multicast DNS (mDNS) that
obscures private IP addresses with mDNS names. This solves the privacy issues
associated with exposing local IP addresses, and mitigates most of the
aforementioned connectivity impact.

This document extends the set of modes defined in {{IPHandling}} with new
options based on the mDNS technique. Different choices are provided, each
with their own benefits and drawbacks.

Terminology
===========

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in {{RFC2119}}.

New Modes
=========

Using the mDNS technique, we define two new modes, namely Mode 2.1 and 2.2.
These modes are identical to Mode 2 from {{IPHandling}}, but the technique from
{{mDNSCandidates}} is used to protect the selected private IP addresses.
Accordingly, the privacy guidelines outlined in {{mDNSCandidates}}, Section 5
MUST be followed in each new mode in order to prevent accidental disclosure
of a private IP address.

Mode 2.1
--------

The local IPv4 address associated with the preferred interface MUST be
replaced with a mDNS name, as described in {{mDNSCandidates}}, Section 3.1.
Any local IPv6 addresses associated with the preferred interface MUST also
be replaced with mDNS names, unless they are {{RFC4941}} privacy-preserving
addresses.

Mode 2.2
--------

All local IPv4 and IPv6 addresses MUST be replaced with mDNS names, as described
in {{mDNSCandidates}}, Section 3.1.

Analysis
========

The only difference between Mode 2.1 and Mode 2.2 is how {{RFC4941}} addresses
are handled. In either case, a direct connection is possible if the mDNS
addresses created for the local IP addresses can be resolved. However, when
mDNS fails, either because it is disabled on the network, or the endpoints
are not on the same segment, Mode 2.1 may allow a direct connection where
Mode 2.2 does not.

The exact impact on applications needs to be determined experimentally. This
document will be updated with a specific recommendation once this information
is known.

Additional Applications
=======================

The mDNS technique may also have value even when all network interfaces are
used by the ICE agent, i.e., in Mode 1 from {{IPHandling}}, by
minimizing the amount of information regarding the local network that is
disclosed to the remote peer. Accordingly, a future update of this document
may define additional modes that apply the mDNS technique to Mode 1.
This is an area for further study.

Security Considerations
=======================

The modes defined here, on their own, present no new security considerations.
Considerations for the mDNS technique are detailed in {{mDNSCandidates}},
Section 6.

IANA Considerations
===================

This document requires no actions from IANA.

