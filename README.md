# Jailerctl

Jailerctl is a controller for [Jailer](http://jailer.dev) over SSH

For a basic idea on what it does and how it works please check the following output, until we write manual pages and add more features.

```
antranigv@zvartnots:~ $ jailerctl inspect
bsd.am ->
  Hostname   : pingvinashen.am
  OS Version : 13.2-RELEASE-p4
  CPUs       : 4
  Load       : 5.1% user,  0.0% nice,  4.7% system,  0.0% interrupt, 90.2% idle
  Memory     : 1331M Active, 4910M Inact, 14M Laundry, 6969M Wired, 2535M Free
  Jailer Dir : 44.7G/420G @ /usr/local/jails
  Jails      : 14
mbp0 ->
  Hostname   : mbp0.evn0.loc.freebsd.am
  OS Version : 13.2-RELEASE-p4
  CPUs       : 8
  Load       : 0.2% user,  0.0% nice,  0.7% system,  0.0% interrupt, 99.1% idle
  Memory     : 257M Active, 5169M Inact, 732M Laundry, 14G Wired, 58K Buf, 3625M Free
  Jailer Dir : 4.38G/224G @ /usr/local/jailer
  Jails      : 2
s0 ->
  Hostname   : s0.loc.freebsd.am
  OS Version : 13.1-RELEASE-p1
  CPUs       : 8
  Load       : 4.8% user,  0.0% nice,  5.5% system,  0.0% interrupt, 89.6% idle
  Memory     : 55M Active, 865M Inact, 32K Laundry, 13G Wired, 1401M Free
  Jailer Dir : 26.6G/125G @ /usr/local/jailz
  Jails      : 6

antranigv@zvartnots:~ $ jailerctl list
HOST    NAME          STATE    JID  HOSTNAME                                     IPv4              GW
bsd.am  blog          Active   12   blog.antranigv.am                            192.168.10.42/24  192.168.10.1
bsd.am  citizen       Stopped
bsd.am  comments      Stopped
bsd.am  feedland      Stopped
bsd.am  git           Active   2    git.bsd.am                          37.252.78.251/24  37.252.78.1
bsd.am  huginn0       Active   8    huginn0.bsd.am                      192.168.10.34/24  192.168.10.1
bsd.am  ifconfig      Active   7    ifconfig.bsd.am                     192.168.10.33/24  192.168.10.1
bsd.am  lists         Stopped
bsd.am  lucy          Active   10   lucy.vartanian.am                   192.168.10.37/24  192.168.10.1
bsd.am  matterbridge  Stopped
bsd.am  mysql         Active   9    mysql.antranigv.am                  192.168.10.50/24  192.168.10.1
bsd.am  newsletter    Active   14   newsletter.bsd.am                   192.168.10.65/24  192.168.10.1
bsd.am  oragir        Active   3    oragir.am                           192.168.10.30/24  192.168.10.1
bsd.am  psql          Active   1    psql.pingvinashen.am                192.168.10.3/24   192.168.10.1
bsd.am  rss           Active   6    rss.bsd.am                          192.168.10.5/24   192.168.10.1
bsd.am  sarian        Active   15   sarian.am                           192.168.10.53/24  192.168.10.1
bsd.am  syuneci       Active   13   syuneci.am                          192.168.10.60/24  192.168.10.1
bsd.am  test0         Stopped
bsd.am  weblog        Active   11   weblog.antranigv.am                 192.168.10.52/24  192.168.10.1
bsd.am  znc           Active   5    znc.bsd.am                          37.252.78.252/24  37.252.78.1
mbp0    jellyfin0     Active   15   jellyfin0.mbp0.evn0.loc.freebsd.am  172.16.100.99/24  172.16.100.1
mbp0    tm0           Active   12   tm0.evn0.freebsd.am                 172.16.100.90/24  172.16.100.1
s0      artifacts0    Active   1    artifacts0.s0.loc.freebsd.am        172.16.70.10/24   172.16.70.1
s0      ooni          Active   2    ooni.s0.loc.freebsd.am              172.16.70.100/24  172.16.70.1
s0      influxdb      Active   3    influxdb.s0.loc.freebsd.am          172.16.70.101/24  172.16.70.1
s0      noch0         Active   4    noch0.s0.loc.freebsd.am             172.16.70.5/24    172.16.70.1
s0      huginn0       Active   6    huginn0.s0.loc.freebsd.am           172.16.70.50/24   172.16.70.1
s0      wp0           Active   7    wp0.s0.loc.freebsd.am               172.16.70.6/24    172.16.70.1

antranigv@zvartnots:~ $ jailerctl help
Usage: jailerctl ...
  help
  version
  remote
    list
    add    RemoteName username@host [sudo]
    remove RemoteName
    rename RemoteName NewRemoteName
  list
    [Remote]
  inspect [Remote]
  run Remote <jailer command>

antranigv@zvartnots:~ $ jailerctl run bsd.am help
Usage: jailer ...
  version
  help [subcommand]
  [subcommand] help
  init [info|patch|zfs|bridge|dhcp]
  image fetch [version]
  image list
  image list remote
  image use [version]
  network use [nettype]
  list
  create  [-D] [-r version | -s snap] [-n] [-d domain]
          [-b bridge] [-t new|eb|ng] [-g gateway -m netmask] [-a addr] [name]
  destroy [-f] name
  start [name ...]
  startall
  stop [-f] [name ...]
  stopall
  console name
  snap jail[@name]
  nat list [jail]
  nat add -i interface [ -a my.second.ip.addr ] jail
  nat del jail
  rdr list [jail]
  rdr add -i interface [ -a my.second.ip.addr ]
                       [ -p tcp|udp -r recvport [ -d destport ] ] jail
  rdr del -j jail [ -i rule_index | -f ]

antranigv@zvartnots:~ $ jailerctl run mbp0 create -a dhcp test0
Creating test0: Done!

antranigv@zvartnots:~ $ jailerctl run mbp0 console test0
root@test0:~ # ifconfig
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> metric 0 mtu 16384
        options=680003<RXCSUM,TXCSUM,LINKSTATE,RXCSUM_IPV6,TXCSUM_IPV6>
        inet6 ::1 prefixlen 128
        inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1
        inet 127.0.0.1 netmask 0xff000000
        groups: lo
        nd6 options=21<PERFORMNUD,AUTO_LINKLOCAL>
epair4b: flags=8863<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500
        options=8<VLAN_MTU>
        ether 58:9c:fc:1d:e9:90
        hwaddr 02:02:f1:34:28:0b
        inet 172.16.100.106 netmask 0xffffff00 broadcast 172.16.100.255
        groups: epair
        media: Ethernet 10Gbase-T (10Gbase-T <full-duplex>)
        status: active
        nd6 options=29<PERFORMNUD,IFDISABLED,AUTO_LINKLOCAL>

root@test0:~ # exit
exit

antranigv@zvartnots:~ $ jailerctl list mbp0
HOST  NAME       STATE   JID  HOSTNAME                                     IPv4               GW
mbp0  jellyfin0  Active  15   jellyfin0.mbp0.evn0.loc.freebsd.am  172.16.100.99/24   172.16.100.1
mbp0  test0      Active  16   test0.mbp0.evn0.loc.freebsd.am      172.16.100.106/24  172.16.100.1
mbp0  tm0        Active  12   tm0.evn0.freebsd.am                 172.16.100.90/24   172.16.100.1

antranigv@zvartnots:~ $ jailerctl run mbp0 snap test0@`date -I`
Taking the snapshot test0@2023-10-25: Done!

antranigv@zvartnots:~ $ jailerctl run mbp0 cons test0
root@test0:~ # cd /
root@test0:/ # cd .zfs
root@test0:/.zfs # ls
snapshot
root@test0:/.zfs # cd snapshot/
root@test0:/.zfs/snapshot # ls
2023-10-25      base
root@test0:/.zfs/snapshot # cd
root@test0:~ # exit
exit

antranigv@zvartnots:~ $ jailerctl run mbp0 destroy test0
Destroying test0: Done!

antranigv@zvartnots:~ $ jailerctl list mbp0
HOST  NAME       STATE   JID  HOSTNAME                                     IPv4              GW
mbp0  jellyfin0  Active  15   jellyfin0.mbp0.evn0.loc.freebsd.am  172.16.100.99/24  172.16.100.1
mbp0  tm0        Active  12   tm0.evn0.freebsd.am                 172.16.100.90/24  172.16.100.1

antranigv@zvartnots:~ $ jailerctl
Usage: jailerctl ...
  help
  version
  remote
    list
    add    RemoteName username@host [sudo]
    remove RemoteName
    rename RemoteName NewRemoteName
  list
    [Remote]
  inspect [Remote]
  run Remote <jailer command>
```
