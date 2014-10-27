# multiple gateways
`RTNETLINK answers: File exists`
or
`SIOCADDRT: File exists`
when running ifup, restart network or booting.

that means you make too much gateway in your configuration file:
`/etc/network/interfaces`

Figure it out. Linux is simple.
