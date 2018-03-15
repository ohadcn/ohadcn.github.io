---
layout: post
title:  playing with whatsapp api  
---


Whatsapp is a realy popular instatnt messaging platform. but unlike many other platforms - this platform is realy bounded to a phone and phone number, in the last year they enabled 

Today I played a little bit with whatsapp, and found few url's (web addresses) to play and interact with whatsapp. Each url has two forms: one that looks like a web address, and the other is in the form of whatsapp://command. The official commands are documented in [whatsapp click to chat help page](https://whatsappen.com/whatsapp-click-to-chat), but some aren't documented there - and can be found here. 

| command | whatsapp - web link | whatsapp - direct link<sup>[1]</sup> | whatsapp web link |
| ------- | ------------------- | ---------------------- | ------------------ |
| Just open the app | https://api.whatsapp.com/ | [whatsapp://chat](whatsapp://chat) | https://web.whatsapp.com |
| open chat | https://api.whatsapp.com/send?phone=316123456 | [whatsapp://send? phone=972526783413](whatsapp://send?phone=972526783413) | https://web.whatsapp.com/send?phone=316123456 |
| open chat with text | https://api.whatsapp.com/send?phone=316123456&text=I'm%20interested%20in%20your%20car%20for%20sale | [whatsapp://send? phone=972526783413  &text=I'm interested in your car for sale](whatsapp://send?phone=972526783413&text=I'm interested in your car for sale) | https://web.whatsapp.com/send?phone=316123456&text=I'm%20interested%20in%20your%20car%20for%20sale |
| open or join group | https://chat.whatsapp.com/1uOhVhVGhdOHuGZ9Lckxkp <sup>[2]</sup> | [whatsapp://chat? code=1uOhVhVGhdOHuGZ9Lckxkp](whatsapp://chat?code=1uOhVhVGhdOHuGZ9Lckxkp)<sup>[2]</sup> | can't find one |


Few more resources:

A. to create more costumised url's (e,g: https://whatsapp.oodi.co.il/), use [this](https://github.com/amizz/Whatsapp-Direct-Messaging-API) tool (haven't tested).

B. [this](https://github.com/leohmoraes/Whatsapp) looks like an automation tool for whatsapp. it seems publicly unmaintained, but this method can still work (see [here](https://github.com/zvovov/whatsapp-web)).

C. [this](https://github.com/andreas-mausch/whatsapp-viewer) tool can show you the content of your whatsapp backup from google drive [this one](https://github.com/marcuscaisey/WhatStats) can use to extract statistics from it.

D. [apiwha](http://apiwha.com/) seems to have a public api to integrate whatsapp with other tools, it seems they reversed whatsapp web protocol - and you can get some functionality from it.

E. Github is full with whatsapp [api](https://github.com/tgalal/yowsup), [bots](https://github.com/gojigeje/wasapbot) and tools, most of them aren't maintained and can't be used (blocked by whatsapp).

F. [whatsapp web client](https://github.com/Enrico204/Whatsapp-Desktop)

G. projects that seems maintained:

[whatsapp-framework - for costum bot](https://github.com/danielcardeenas/whatsapp-framework)

[web search whatsapp bot](https://github.com/jctissier/whatsapp-assistant-bot)

[franz - multi messaging desktop app](https://meetfranz.com/) (this is the only desktop project here that not seems to depend on selenium!)

[whatsapp api](https://github.com/VISWESWARAN1998/Simple-Yet-Hackable-WhatsApp-api), [and one more](https://github.com/mukulhase/WebWhatsAPI)

[Whatsapp Spammer](https://github.com/sailesh1999/Whatsapp-Spammer) - uses autogui (instead of selenium)

[whatsapp tools for rooted phones using exposed](https://github.com/suraj0208/WhatsappExtensions)

Notes:

[1] to use this links in your website, use the following html code:
```html
<a href="whatsapp://chat">click me</a> 
````

[2] to get the group code (like `1uOhVhVGhdOHuGZ9Lckxkp` in this example), look at the group invite link. keep in mind that the links are going to expire with the invite link!

