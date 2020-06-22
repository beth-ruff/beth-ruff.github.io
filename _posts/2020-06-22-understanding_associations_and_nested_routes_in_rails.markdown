---
layout: post
title:      "Understanding Associations and Nested Routes in Rails"
date:       2020-06-22 20:35:42 +0000
permalink:  understanding_associations_and_nested_routes_in_rails
---


I believe it was Aristotle who said “The more you know, the more you know you don't know.” Albeit cheesy, this has remained my mantra throughout my Flatiron experience (as stated in a previous blog post, imposter syndrome, anyone?!) While programming my Rails app, this mantra resounded heavily, particularly as I worked through my associations. I thought I understood the intricacies of associations prior to starting my Rails project. But suddenly, I had to connect my User, YogaClass, Studio, and Student models together, and it made my head spin. I spent 3-4 days just trying to put together the infrastructure of my Rails app via these associations, and amidst that struggle, I gained some clarity. Let's break it down here:

A **user** *has_many* yoga_classes, and *has_many* studios *through* yoga_classes.
A **studio** *has_many* yoga_classes, and *has_many* users *through* yoga_classes.
A **student** *belongs_to* a yoga_class.
A **yoga_class** *belongs_to* a studio, *belongs_to* a user, and *has_many* students.

Vital to my organization (and to help limit my migraines), I created the following table to illustrate these associations:

![](https://docs.google.com/drawings/d/e/2PACX-1vRMBI8oTGU0hgEXBc3-eGXC57JkGisHaN_OW1SOQPlAHTJSrFAYW7LSOu5ZpChBBARYBMUa63Fnt1-H/pub?w=960&h=720)

Users, in this case, are yoga teachers who will use this web app to organize their yoga classes, studios, and ultimately students within those classes. As you can see from the above associations, the yoga_classes table is operating as the joins table, connecting users to studios.

I first built out these associations in my models. I then built out my yoga_classes controller with some vanilla code, using @yoga_classes = YogaClass.all within my index controller action, @yoga_class = YogaClass.new in my new controller action, and so on and so forth. I quickly realized that this vanilla code would not allow users to create a studio as they were creating their yoga classes (hello, nested forms!). In addition, this initial code would not connect the yoga_classes to the current_user, and anyone who logged in would be able to see and track *all* of the yoga_classes within the database. Not cool.

I reworked the code in my controller so that objects were being built with these associations in mind (BIG lesson here - building and persisting objects with these associations in the controller is vital!) Here is a snippet of the 'new and improved' code from the yoga_classes controller:

```
def index
    @yoga_classes = current_user.yoga_classes.future_classes
    @yoga_classes = @yoga_classes.order(date: :asc)
  end

  def new
    @yoga_class = current_user.yoga_classes.build
    @studio = @yoga_class.build_studio 
  end

  def create
    @yoga_class = current_user.yoga_classes.new(yoga_class_params)
      if @yoga_class.valid?
        @yoga_class.save
        redirect_to yoga_class_path(@yoga_class) 
      else 
        render :new
      end 
  end
```

And then in yoga_classes/new.html.erb, I set up a form to handle these associations with nested fields:

```
<%= form_for @yoga_class do |f| %>
<%= f.label :name %>
<%= f.text_field :name %><br>
<%= f.label :length%>
<%= f.text_field :length %><br>
<%= f.label :difficulty%>
<%= f.select(:difficulty, YogaClass::DIFFICULTY_LIST )%><br>
<%= f.label :date%>
<%= f.date_field :date %><br>
<%= f.label :time %>
<%= f.time_field :time %><br>
<%= f.label "Select a Studio" %>
<%= f.collection_select :studio_id, current_user.studios.all.uniq, :id, :name, {:include_blank => "Please select"} %><br>

<h3>Or Add a New Studio:</h3>
<%= f.fields_for :studio, @studio do |studio| %>
    <%= studio.label :name %>
    <%= studio.text_field :name %><br>
    <%= studio.label :address %>
    <%= studio.text_field :address %><br>
    <%= studio.label :phone_number %>
    <%= studio.text_field :phone_number %><br>
    <% end %>

<%= f.submit "Submit", class: "btn btn-large btn-primary" %>
<% end %>
```

(*Note:* Don't forget to add **accepts_nested_attributes_for** to the parent model!)

This form takes in both a @yoga_class *and* @studio model object and establishes their association to one another thanks to the handy build method in the new controller action and the accepts_nested_attributes_for class method in the parent model. By adding an array for studio_attributes to the strong params in the yoga_classes controller, both a yoga_class and studio object are then created and persisted to the database.

Once I tackled the associations in the yoga_classes controller, my next hurdle was setting up a nested route for students and building out the students controller and forms.

First, setting up the nested route is pretty simple. In config/routes.rb:

```
resources :yoga_classes do 
    resources :students
  end
```

Once the routes were set up themselves, the most difficult item to wrap my head around was the route helpers. On many occasions, I had to find the correct helpers using trial and error. Other than that, building out the students controller was pretty similar to the yoga_classes controller in terms of establishing associations. In each controller action, I had to set the yoga class (@yoga_class = YogaClass.find_by(id: params[:yoga_class_id])) and then either build the student object or find the student object using that class since students belong_to a yoga_class. For example, here is the code in my new and create controller actions:

```
def new 
     @yoga_class = YogaClass.find_by(id: params[:yoga_class_id])
		 if params[:yoga_class_id] && !YogaClass.exists?(params[:yoga_class_id])
        redirect_to yoga_classes_path, alert: "Yoga class not found."
      else 
         @student = @yoga_class.students.build(yoga_class_id: params[:yoga_class_id])
      end 
    end
    
    def create
      @yoga_class = YogaClass.find_by(id: params[:yoga_class_id])
      @student = @yoga_class.students.new(student_params)
        if @student.valid?
          @student.save
          redirect_to yoga_class_student_path(@yoga_class, @student)
        else 
          render :new
        end 
    end
```

And then the student/new.html.erb form takes in both a @yoga_class and @student model object:

```
<%= form_for [@yoga_class, @student] do |f| %>
<%= f.label :name %>
<%= f.hidden_field :yoga_class_id %>
<%= f.text_field :name %><br></br>
<%= f.submit "Submit", class: "btn btn-large btn-primary"  %>
<% end %>
```

After creating all of the controller actions, I created a set_yoga_class private method in order to refactor and clean up my code a bit.

So there you have it! I now have a much better understanding of associations and nested routes. The best way to gain deep understanding of these complicated ideas is to work with a blank slate and create a project from scratch (...or with the help of the Rails gem). Although this project was a considerable feat, my mantra remains true: the more I know, the more I know I don't know. Here's to many more months of learning. Cheers!
