---
layout: page
title: About me
permalink: /about
comments: true
---
{% assign author = site.authors["senthil"] %}

<div class="row justify-content-between">
    <div class="col-md-8 pr-5">
        <p>My name is Senthil Nayagan. I am a seasoned IT professional with years of expertise in designing and building complex business solutions with open-source technologies. I've been working in the field of data engineering for the past few years. Simply said, I am a Data Engineer by profession, a Rustacean by interest, and an avid Content Creator.</p>
        <p>It's my tech blog where I explore various data engineering concepts. I'm also making up stories about design and coding principles. I'm attempting to make all of my stories as clear as possible while yet providing enough detail for you to comprehend the ideas.</p>
        <p>If you like my stories or have suggestions for their improvement, please feel free to contact me at <a href="mailto:hello@senthilnayagan.com">hello@senthilnayagan.com</a>.</p>
        <p>Thank you for your support!</p>
    </div>
    <div class="col-md-4">
        <img src="https://www.gravatar.com/avatar/{{ author.gravatar }}?s=350" alt="{{ author.display_name }}">
    </div>
</div>