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
       <label for="example-message"><b>extra:</b></label> <textarea name="extra" id="example-extra" placeholder="Please add an extra data"></textarea>
   </p>
   <p>
       <hidden name="lc_id" id="leadclient-id" value="lc__iRJBvBiIQBEu_ch"></hidden>
   </p>
   <p>
       <button type="submit">Submit</button>
   </p>
</form>
<br/>
<a href="tel:+972529098869;ext=88" lc_phone>my phone number:+(972)52-9098869</a></br>

<a href="tel:+972529098869,88" lc_phone>my phone number second try:+(972)52-9098869</a></br>

<a href="tel:+972529098869p88" lc_phone>my phone number third try:+(972)52-9098869</a>
<br/>
