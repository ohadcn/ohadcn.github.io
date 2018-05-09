---
layout: post
title:  playing with serverless forms  
---

<form method="post" action="https://www.example.org/" id="example-form-2">
   <p>
       <label for="example-name"><b>Name:</b></label> <input type="text" name="name" id="example-name" placeholder="Please fill in your name" />
   </p>
   <p>
       <label for="example-phone"><b>Phone:</b></label> <input type="tel" name="phone" id="example-phone" placeholder="Please fill in your phone number" />
   </p>
   <p>
       <label for="example-email"><b>Email:</b></label> <input type="email" name="email" id="example-email" placeholder="Please fill in your email address" />
   </p>
   <p>
       <label for="example-message"><b>Message:</b></label> <textarea name="message" id="example-message" placeholder="Please fill in a message"></textarea>
   </p>
   <p>
       <hidden name="lc_id" id="leadclient-id" value="lc__iRJBvBiIQBEu_ch"></hidden>
   </p>
   <p>
       <button type="submit">Submit</button>
   </p>
</form>
<br/>
my phone number: <i lc_phone></i>
<br/>
<script type="text/javascript">
/* <![CDATA[ */
var _leadclient_id = "lc__oUDocZZeDlgM_ch";
var _leadclient_auto_form_inject = true;
/* ]]> */
</script>
<script src="https://code.jquery.com/jquery-latest.min.js"></script>
<script type='text/javascript' src='https://demo.leadclient.net/leadclient.js?CO=lc-rRtQQbRqDdIP-co'></script>
