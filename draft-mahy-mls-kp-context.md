---
title: KeyPackage Context Extension for Message Layer Security (MLS)
abbrev: MLS KeyPackage Context
docname: draft-mahy-mls-kp-context-latest
ipr: trust200902
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"
area: art
workgroup: MLS
area: sec
category: info
keyword:
 - mls extension
 - keypackage context
 - keypackage extension

stand_alone: yes
pi: [toc, sortrefs, symrefs]

venue:
  group: MLS
  type: Working Group
  mail: mls@ietf.org
  arch: https://mailarchive.ietf.org/arch/browse/mls/
  github: rohan-wire/mls-kp-context/
#  latest: https://github.com/rohan-wire/mls-kp-context/latest

author:
 -  ins: R. Mahy
    name: Rohan Mahy
    organization: Wire
    email: rohan.mahy@wire.com

normative:

informative:

--- abstract

This document describes a Message Layer Security (MLS) KeyPackage extension
to convey a specific context or anticipated use for the KeyPackage. It is useful
when a client provides the KeyPackage out-of-band to another client, and wants
the specific KeyPackage used only in the anticipated context, for example a
specific MLS group.

--- middle

# Terminology

{::boilerplate bcp14-tagged}

The terms MLS client, MLS group, LeafNode, GroupContext, KeyPackage,
GroupContextExtensions Proposal, Credential, CredentialType, and
RequiredCapabilities have the same meanings as in the MLS
protocol {{!I-D.ietf-mls-protocol}}.

# Introduction

In some use cases of MLS, a client might wish to provide a KeyPackage to
another client, but communicate that the specific KeyPackage is only to be
used in a specific context, for example to join a specific MLS group. This
document describes a KeyPackage extension that can convey that context.

# Extension Description

This document specifies a KeyPackage MLS extension `kp_context`
of type ContextPair. The syntax is described using
the TLS Presentation Language {{!RFC8446}}

Each PerDomainTrustAnchor represents a specific identity domain which is
expected and authorized to participate in the MLS group. It contains the
domain name and the specific trust anchor used to validate identities for
members in that domain.

~~~ tls-presentation
enum {
  reserved(0),
  groupid(1),
  uri(2),
  domain(3),
  jwk_thumbprint(4)
  (255)
} ContextType;

struct {
    ContextType context_type;
    opaque context_value<V>;
} ContextPair;

ContextPair kp_context;
~~~


# IANA Considerations

This document proposes registration of a new MLS Extension Type.

RFC EDITOR: Please replace XXXX throughout with the RFC number assigned to this document

## kp_context MLS Extension Type

The `kp_context` MLS Extension Type is used inside KeyPackage objects. It
contains a URN Anchors object representing the trust anchors which are expected
for identity validation inside the MLS group.

~~~~~~~~
  Template:
  Value: 0x000B
  Name: kp_context
  Message(s): This extension may appear in KeyPackage objects
  Recommended: Y
  Reference: RFC XXXX
~~~~~~~~

## urn:ietf:mls:kp_context:group_id URN registration

~~~
   Namespace Identifier:  Requested of IANA (formal) or assigned by IANA
      (informal).

   Version:  1

   Date:  2023-08-01

   Registrant:
     Rohan Mahy
     rohan.ietf@gmail.com

   Purpose:  Described in Section 3 of RFCXXXX.

   Syntax:  Described in Section 3 of RFCXXXX.

   Assignment:  Described in Section 4.1 of RFCXXXX.

   Security and Privacy:  Described in Section 5 of RFCXXXX.

   Interoperability:  Described in Section 3 of this document.

   Resolution:  Described in Section 3 of this document.

   Documentation:  RFCXXXX

   Additional Information:  none

   Revision Information:  n/a


~~~

# Security Considerations

The Security Considerations of MLS apply.

The use of this extension may reveal the client's intentions or wishes
in an out-of-band protocol, which may have weaker privacy protections than
MLS handshake messages.


--- back


