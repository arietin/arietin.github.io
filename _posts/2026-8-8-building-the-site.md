---
layout: post
title: Building the Site
---

I've had personal sites before. When I was a kid, I designed sites in drag-and-drop editors like they were legos. By high school, I had developed some basic HTML and built a website on Wordpress. I took classes in web development and built more sites. This isn't about that. Forget all that. I did, though it would have saved me a lot of confusion if I hadn't. So this is a different story.

I set out on my journey at the start of my summer, around May. I had just graduated, and, with a couple blank calendar pages on my wall, I finally had time for rest and relaxation. I decided to pull my hair out instead. The last personal site I had was woefully out of date, and building a site on GitHub seemed a) free and b) like a new skill to learn.

I started researching, and quickly learned:

- GitHub has developed a framework called Jekyll for building static sites (like this one)
- Loads of website templates already exist
- To get started I needed to install a program called Ruby

Perfect. I downloaded Ruby. I followed the Jekyll setup guide to run a site on my computer. I found a template I liked. I tried importing it. Everything broke. 

Great. I figured out how Jekyll works under the hood. I deleted some files and combined others. I had a local site (this means I could see it on my computer and other computers on my wifi network)!

I had everything documented, so I confirmed replicability before continuing. My footsteps could be followed. It was time to figure out how to get this up to GitHub. It was also time to do everything on the now-full calendar pages on my wall (I'm sure they're magic: as soon as I look away, they start filling up). 

A month passed. A calendar page turned. I moved to Washington DC. I turned on my laptop. My old code stared back at me.

It was a piercing stare, a stare hardened by years of deprecation. If I wanted to get this site running on GitHub, I would need to untangle a knot of old versions not being compatible with new code. I needed to figure out how syntax changed, and where. I needed to understand the nuances of each part of the configuration. I needed help.

I took the opportunity to try out a new tool: an LLM. In university, these were ubiquitous, but I never found a good use for them: I really like learning, so I *wanted* to do things the hard way, at least at first &mdash; to understand *how* they're done. At this point I did understand how a lot of this development was done. What's more, the final pieces of the puzzle were something I wasn't worried about not learning. If a solution only half-worked, but worked in all the important ways, I was happy. I finally found a use for an LLM. 

Through some prompting, I debugged the key knot and managed to complete the rest without further use of an LLM. I stripped down the site as best as I could, and uploaded a copy for others to use as a template. The website was ready. The technical documentation was complete. I customized what I wanted to, left the rest unchanged, and the result is what you are now reading. You can find a technical-ish walkthrough of how I did this on the [`How I Built This`]({{ site.url }}/building.md) page. 

So that's my journey. I'm glad you're here to read it. 
