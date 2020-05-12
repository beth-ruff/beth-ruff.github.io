---
layout: post
title:      "My Sinatra Project // It's a Marathon, Not a Sprint"
date:       2020-05-12 01:02:54 +0000
permalink:  my_sinatra_project_its_a_marathon_not_a_sprint
---


"It's a marathon, not a sprint" has been my mantra as I've built my Sinatra project from the ground up. Perhaps this is why I chose to log and track marathon races for my project! I also happent to love running. Behold, the Marathons Sinatra Project. A place where a user can log in (or sign up if you're a first time user), view an index of marathons, add your own marathons, and edit or delete the marathons you've added.

I started by building out my database and marathons controllers and views. I then moved on to the users controllers and views. Finally, I inserted the has_many and belongs_to relationships in the models and fine-tuned the flow of my app. Although I had my fair share of fun adding my own buttons and style to the pages, I have to say my favorite (and most challening) part of building this app was finagling with my database. 

Creating the marathons and users tables was interesting for multiples reasons. Because I was working in my local environment rather than the learn IDE that I've become so accustomed to, it took me a while to ensure I had access to all the rake demands I needed. When first starting my project and creating my marathons and users tables, my schema.rb file wouldn't populate. After playing around with my migrations and trying endless commands (rollback, create, reset, migrate, etc.), I finally succumbed to deleting my db directory and recreating it. Alas, this worked!

The second time I had to work on my database was when I realized I used the incorrect data type for one of my columns. I used "datetime" for the date attribute, and this looked weird on my actual webpage, so I created a new migration to update the data type to "date." This time, I simply used rake db:rollback and rake db:migrate, and it worked like a charm. My schema updated, and the change was reflected.

I wasn't so lucky the third time I had to edit my database. As I was building my user and marathon associations, I realize I had to create a user_id attribute in the marathons table. I didn't realize this until I tried setting the current_user to @marathon.user in my post '/marathons' do controller action and received countless "Missing Attribute" errors. I attempted to create an "add column" migration but ran into a similar problem as before where my schema wouldn't update even when I tried rolling my tables back (or even dropping my tables) and migrating. Again, the only way I was able to solve this problem was to physically delete the entire database and recreate the migrations. Perhaps not the most efficient method, but it worked nonetheless!

One of my biggest takeaways from this experience was learning the ins-and-outs of creating migrations and updating databases. I feel much more confident in my ability to research problems online and found many helpful articles on how to deal with certain situations I ran into. 

Another huge takeaway was the importance of taking on a user perspective when using Sinatra. It's a very manual platform, particularly when it comes to weaving together the user experience (from controller action to view to controller action to view etc....), and at times it felt dizzying thinking of where the user should go next.

Once again, I come back to my mantra: "It's a marathon, not a sprint." This keeps me focused and motivated knowing that not only am I pacing myself, but I can tell I am beginning to pick up my pace. My mile times are shorter than they were during my CLI project. Negative split, baby!
