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
       <button type="submit">Submit</button>
   </p>
</form>

<script type="text/javascript">
/* <![CDATA[ */
var _leadclient_id = "lc__oUDocZZeDlgM_ch";
/* ]]> */
</script>
<script type='text/javascript' src='https://demo.leadclient.net/leadclient.js'></script>
<script type="text/javascript">
$(document).ready(function() {
   $("#example-form-2").on('submit', function(event) {
      event.preventDefault(); // Prevent default event (Disable form sending)

      $.leadClientSubmit({
         data: {
            name: $("#example-form-2 input[name=name]").val(),
            phone: $("#example-form-2 input[name=phone]").val(),
            email: $("#example-form-2 input[name=email]").val(),
            message: ["Message", $("#example-form-2 textarea[name=message]").val()]
         },
         done: function() {
            $("#example-form-1 input").empty(); // Clear form's inputs

            alert("Form sent!");
         },
         error: function(error_code) {
            switch (error_code) {
               case 'no_channel_id':
                  alert("Error: There is no channel code.");
                  break;
               case 'channel_not_found':
                  alert("Error: The channel was not found");
                  break;
               case 'must_contain_phone_or_email':
                  alert("Error: You must fill in a phone number or an email address");
                  break;
               case 'duplicated_lead':
                  alert("Error: This lead has already been sent");
                  break;
               default:
                  alert("Error: A general error has occurred");
                  break;
            }
         }
      });
   });
});
</script>
