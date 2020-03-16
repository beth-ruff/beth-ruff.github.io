---
layout: post
title:      "My CLI Project!"
date:       2020-03-16 05:24:29 +0000
permalink:  my_cli_project
---


I can't believe I'm writing this. I've completed my first Flatiron project. *I have completed my first CLI application.* How did I get here?

This process hasn't been a walk in the park. It's consisted of mostly frustrating moments, staring at my screen, refactoring my code, and undoing many hours of work to get to the final product. But boy, does it feel good looking at what I've created.

My CLI app is titled Trails Near Me (clever, right...?), and it provides users with data on hiking trails throughout the US according to information they input, namely their latitude and longitude. I used the [Hiking Project Data API](https://www.hikingproject.com/data) to retreive all trail information - all of this info can be seen publicly on the [Hiking Project](https://www.hikingproject.com/) website. Can you tell I love REI?

For this project, I created a CLI class, API class, and trail class. One of my favorite parts of building this CLI was getting my different classes to interact and passing objects from one to the other. It provided so much clarity on object orientation. This was also my first time working with an API - I loved how easy it was to set up classes and then integrate the API into my code.

The lowest point for me was attempting to iterate on my API hash in order to return multiple trails. I'm not exaggerating when I say I spent 12+ hours trying different ways to iterate using .each, .each_with_index, .map, etc. In the end, I was unsuccessful in returning more than one trail at a time. In its current state, the CLI will only return one trail when users provide their latitude & longitude input (although, they can input various latitudes & longitudes and receive different trails that way). I believe this is due to the way the API data is stored. Here's a snapshot of two trails:

```{
    "trails": [
        {
            "id": 7051489,
            "name": "Educational Loop",
            "type": "Hike",
            "summary": "A great hike to explore volcanoes in Oakland.",
            "difficulty": "blue",
            "stars": 5,
            "starVotes": 3,
            "location": "Orinda, California",
            "url": "https:\/\/www.hikingproject.com\/trail\/7051489\/educational-loop",
            "imgSqSmall": "https:\/\/cdn-files.apstatic.com\/hike\/7048197_sqsmall_1555538705.jpg",
            "imgSmall": "https:\/\/cdn-files.apstatic.com\/hike\/7048197_small_1555538705.jpg",
            "imgSmallMed": "https:\/\/cdn-files.apstatic.com\/hike\/7048197_smallMed_1555538705.jpg",
            "imgMedium": "https:\/\/cdn-files.apstatic.com\/hike\/7048197_medium_1555538705.jpg",
            "length": 4.3,
            "ascent": 622,
            "descent": -621,
            "high": 1596,
            "low": 1159,
            "longitude": -122.1986,
            "latitude": 37.8478,
            "conditionStatus": "Unknown",
            "conditionDetails": null,
            "conditionDate": "1970-01-01 00:00:00"
        },
        {
            "id": 7051486,
            "name": "Martin Luther King Jr. Regional Shoreline",
            "type": "Hike",
            "summary": "Enjoy this birdwatching adventure along San Leandro Bay with fantastic views of San Francisco and Arrowhead Marsh.",
            "difficulty": "green",
            "stars": 4.7,
            "starVotes": 3,
            "location": "San Leandro, California",
            "url": "https:\/\/www.hikingproject.com\/trail\/7051486\/martin-luther-king-jr-regional-shoreline",
            "imgSqSmall": "https:\/\/cdn-files.apstatic.com\/hike\/7048157_sqsmall_1555538610.jpg",
            "imgSmall": "https:\/\/cdn-files.apstatic.com\/hike\/7048157_small_1555538610.jpg",
            "imgSmallMed": "https:\/\/cdn-files.apstatic.com\/hike\/7048157_smallMed_1555538610.jpg",
            "imgMedium": "https:\/\/cdn-files.apstatic.com\/hike\/7048157_medium_1555538610.jpg",
            "length": 7.1,
            "ascent": 23,
            "descent": -23,
            "high": 14,
            "low": 8,
            "longitude": -122.2074,
            "latitude": 37.7312,
            "conditionStatus": "Unknown",
            "conditionDetails": null,
            "conditionDate": "1970-01-01 00:00:00"
        }
    ],
    "success": 1
}
```

When I played around with pry in the beginning, @trails_hash.length returned 2. The first index include all of the trails, and the second was the "success" string at the end. The only way I was able to get an object created from this data was by being really specific and using an index number to pull the data from within the trails (here's a snippet of code below):

``` trails_obj = {
                name: @trails_hash["trails"][0]["name"],
                location: @trails_hash["trails"][0]["location"],
                difficulty: @trails_hash["trails"][0]["difficulty"],
                length: @trails_hash["trails"][0]["length"],
                url: @trails_hash["trails"][0]["url"]
                }
            TrailsNearMe::Trail.new(trails_obj)
						```
						
This snippet above returns only the first trail. I tried iterating through the index to no avail. This is something that still stumps me, but I digress.

In the end, many lessons were learned, perhaps the biggest one being that I still have so much more to learn. I honestly thought I'd be more of a Ruby 'expert' at this point, but I still feel like a complete newbie (imposter syndrome, anyone?). All I can do in moments of discouragement is look back at old lessons and realize that I *do* know so much more than when I started a mere 8 weeks ago. 

Oh, and my favorite takeaway from this project: *I finally understand how to use binding.pry!!!* This was something I really struggled with the last 7 weeks and was honestly quite embarrassed about. Now, it's my go-to method when I'm stuck. This small victory made all the struggle worth it.

