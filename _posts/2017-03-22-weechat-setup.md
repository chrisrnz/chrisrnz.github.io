---
layout: default
title: "Weechat setup"
published: false
date: 2017-03-22
---

```
/secure passphrase this xxxxxxxx is xxxxxxxx my xxxxxxxx secret xxxxxxxx passphrase

/script install buffers.pl buffer_autoclose.py iset.pl screen_away.py

/set weechat.bar.status.color_bg 0
/set weechat.bar.title.color_bg 0
/set weechat.bar.buffers.items buffers
/set weechat.bar.buffers.position top
/set weechat.color.chat_nick_colors "1,2,3,4,5,6,9,13"
/set weechat.look.buffer_notify_default message
/set weechat.look.prefix_align_max 15
/set weechat.look.window_title "IRC"

/set buffers.color.current_bg 0
/set buffers.color.current_fg 7
/set buffers.color.hotlist_message_fg 7
/set buffers.look.hide_merged_buffers server
/set buffers.look.detach 600

/set irc.look.color_nicks_in_nicklist on
/set irc.look.smart_filter on
/filter add irc_smart * irc_smart_filter *

/key bind meta-n /bar toggle nicklist

/secure set bitlbee_password xxxxxxxx
/server add bitlbee hulk.gentoo.geek.nz/6667 -autoconnect
/set irc.server.bitlbee.autorejoin on
/set irc.server.bitlbee.command "/msg &bitlbee identify ${sec.data.bitlbee_password}"
/set irc.server.bitlbee.msg_part "bye"
/set irc.server.bitlbee.msg_quit "bye"
/set irc.server.bitlbee.nicks "_ad_"
/set irc.server.bitlbee.realname "Chris"
/set irc.server.bitlbee.username "cjr"

/secure set p2pnet_password xxxxxxxx
/server add p2pnet irc.p2p-network.net/6667 -autoconnect
/set irc.server.p2pnet.autojoin "#CT.Announce"
/set irc.server.p2pnet.autorejoin on
/set irc.server.p2pnet.command "/msg nickserv identify ${sec.data.p2pnet_password}"
/set irc.server.p2pnet.msg_part "bye"
/set irc.server.p2pnet.msg_quit "bye"
/set irc.server.p2pnet.nicks "ad,ad_,_ad,_ad_"
/set irc.server.p2pnet.realname "Chris"
/set irc.server.p2pnet.username "ad"

/secure set kiwicon_newglobal_key xxxxxxxx
/secure set kiwicon_triumvirate_key xxxxxxxx
/secure set kiwicon_sosdla_key xxxxxxxx

/server add kiwicon li.kiwicon.org/6697 -ssl -autoconnect
/set irc.server.kiwicon.autojoin = "#newglobal,#triumvirate,#sosdla,#kiwicon,#isig ${sec.data.kiwicon_newglobal_key},${sec.data.kiwicon_triumvirate_key},${sec.data.kiwicon_sosdla_key}"
/set irc.server.kiwicon.autorejoin on
/set irc.server.kiwicon.msg_part "bye"
/set irc.server.kiwicon.msg_quit "bye"
/set irc.server.kiwicon.nicks "ad,ad_,_ad,_ad_"
/set irc.server.kiwicon.ssl_verify off
/set irc.server.kiwicon.realname "Chris"
/set irc.server.kiwicon.username "cjr"
```
