---
layout: post
title:  playing with whatsapp api  
---

Today I played a little bit with whatsapp, and found few url's (web addresses) to play and interact with whatsapp. Each url has two forms: one that looks like a web address, and the other is in the form of whatsapp://command. The official commands are documented in [whatsapp click to chat help page](https://whatsappen.com/whatsapp-click-to-chat), but some aren't documented there - and can be found here. 

| command | whatsapp - web link | whatsapp - direct link<sup>[1]</sup> | telegram - web link | telegram direct link |
| ------- | ------------------- | ---------------------- | ------------------- | -------------------- |
| Just open the app | https://api.whatsapp.com/ | [whatsapp://chat](whatsapp://chat) | still looking | still looking |
| open chat | https://api.whatsapp.com/send?phone=316123456 | [whatsapp://send? phone=972526783413](whatsapp://send?phone=972526783413) | still looking | still looking |
| open chat with text | https://api.whatsapp.com/send?phone=316123456&text=I'm%20interested%20in%20your%20car%20for%20sale | [whatsapp://send? phone=972526783413  &text=I'm interested in your car for sale](whatsapp://send?phone=972526783413&text=I'm interested in your car for sale) | still looking | still looking |
| open group | https://chat.whatsapp.com/1uOhVhVGhdOHuGZ9Lckxkp <sup>[2]</sup> | [whatsapp://chat? code=1uOhVhVGhdOHuGZ9Lckxkp](whatsapp://chat?code=1uOhVhVGhdOHuGZ9Lckxkp&text=I'm interested in your car for sale)<sup>[2]</sup> | still looking | still looking |


[1] to use this links, use the following html code:
```html
<a href="whatsapp://chat">click me</a> 
````

[2] to get the group code (like `1uOhVhVGhdOHuGZ9Lckxkp` in this example), look at the group invite link. keep in mind that the links are going to expire with the invite link!

[just open whatsapp](whatsapp://chat)

[get to a group](whatsapp://chat?code=1uOhVhVGhdOHuGZ9Lckxkp)

[contact me on chat](whatsapp://send?phone=972526783413&text=hey%2Bohad)

[whatsapp://chat? code=1uOhVhVGhdOHuGZ9Lckxkp](whatsapp://chat?code=1uOhVhVGhdOHuGZ9Lckxkp&text=I'm interested in your car for sale)
