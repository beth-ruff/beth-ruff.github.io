---
layout: post
title:      "My Love/Hate Relationship With Javascript"
date:       2020-07-20 16:13:33 +0000
permalink:  my_love_hate_relationship_with_javascript
---


Prior to joining this Flatiron cohort, I thought Javascript would be my favorite section. In general, I thought I would enjoy front-end programming more than backend. I didn't quite anticipate struggling with my understanding of JS as much as I did! 

But as always, project week has brought me greater understanding of the ins and outs of JS. For my project, I decided to create a concerts web app where you can track and manage concerts and venues. As far as associations go, a venue has many concerts, and a concert belongs to a venue. I was not surprised to see that associations are drawn up in a similar way both in the back and front end. 

One of the most helpful gems I used for this project was active_model_serializer. Unlike the last project, this web app consists of a Rails backend and JS frontend; rather than having the controllers render HTML and forms in the views for the user to read, the controllers directly render JSON. Active_model_serializer aids in this process by creating serializers to customize the data you want to be available to the user. And...the serializers know about your associations, much like the models! 

Enough about the backend. The real learning curve for me was coding the front end. The most difficult piece was refactoring my code to include front end classes when creating new objects. In our curriculum we learned all about classes and OOJS (easy enough, right?). When it came down to actually building a web app, though, I was a bit lost on how to actually use classes and integrate them into the front end. This piece of the app definitely took the longest amount of time for me to complete. 

In the end, I've learned that building JS classes is similar to instantiating a new object by setting its properties. It's also a place where you can write lengthy and/or repetitive HTML code that you want to render on the page so that your index.js is shorter and cleaner. Here's an example of my Venue class: 

```
class Venue {
    constructor(venue) {
        this.id = venue.id 
        this.name = venue.name
        this.address = venue.address
        this.phone_number = venue.phone_number
        this.concerts = venue.concerts
    }

    renderVenue() {
        return `
                <li id="venueLi-${this.id}">
                <a href='#' data-id='${this.id}'>${this.name}</a>
                <button id="add-concert" data-id="${this.id}">Add Concert</button>
                <button id="update-venue" data-id='${this.id}'>Edit</button>
                <button id="delete-venue" data-id="${this.id}">Delete</button>
                <ol id="concerts-ol"></ol>
                </li>
                `
    }

    renderConcerts() {
        let ol = document.querySelector(`#venueLi-${this.id} #concerts-ol`)
        this.concerts.forEach(concert => {
            ol.innerHTML += `
                <li id="concert-${concert.id}">
                ${concert.artist} - ${concert.date}
                <button id="delete-concert" data-id="${concert.id}">Delete</button>
                </li>
                `
        })
    }

}
```

Now in my index.js file, when a fetch post request is sent to create a new venue, I simply have to code new Venue(venue) to instantiate and save that new object. It's less magical than rails, but it certainly gets the job done!

All in all, my relationship with JS is love/hate. I still think I have a lot to learn, and I plan to practice a little bit every day to get as comfortable with it as I am with Ruby and Rails. Nevertheless, I've completed another project and am looking forward to my final one.
