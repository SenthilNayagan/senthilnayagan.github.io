---
layout: page
title: Contact
permalink: /contact
comments: false
---

<form action="{{ site.contact-me }}" method="POST">    
    <p class="mb-4">Please feel free to send your message to me. I will respond as soon as I can.</p>
    <div class="form-group row">
    <div class="col-md-6">
    <input class="form-control" type="text" name="name" placeholder="Name*" required>
    </div>
    <div class="col-md-6">
    <input class="form-control" type="email" name="_replyto" placeholder="E-mail Address*" required>
    </div>
    </div>
    <textarea rows="8" class="form-control mb-3" name="message" placeholder="Message*" required></textarea>    
    <input class="btn btn-dark" type="submit" value="Send">
</form>