```mermaid
sequenceDiagram

participant Pub as Publisher
participant PC as Publisher DCR
participant AC as Advertiser DCR
participant Adv as Advertiser
participant DSP

PC->>AC: 1.a
AC->>AC: 1.b
PC->>PC: 1.c

Adv->>AC: 2.a.1
AC->>AC: 2.a.2

Pub->>PC: 2.b.1
PC->>PC: 2.b.2

AC->>PC: 2.c
PC->>AC: 2.d
AC->>AC: 2.e
PC->>PC: 2.f
PC->>AC: 2.g
AC->>PC: 2.h

PC->>PC: 3.a
AC->>AC: 3.b
PC->>Pub: 3.c
AC->>AC: 3.d
AC->>DSP: 3.e

```
