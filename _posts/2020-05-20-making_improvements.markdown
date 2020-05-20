---
layout: post
title:      "Making Improvements"
date:       2020-05-20 15:15:56 +0000
permalink:  making_improvements
---


I had my Sinatra assessment last week, and it was incredibly helpful talking my application over with someone and understanding where improvements could be made. I have my second assessment tomorrow morning and wanted to take a moment and talk about two of the improvements I made: validations and flash errors.

Validations tripped me up a bit during the assessment! I didn't really think about how many validations (both server side and client side) one should have to truly keep their app safe and smart. For my marathons Sinatra app, I added some code to validate users are only able to edit and delete their own marathons rather than others. I did so with the following:

```patch "/marathons/:id" do 
        find_marathon
        if @marathon.user = current_user 
            @marathon_params = update_whitelist(params)
            @marathon.update(@marathon_params)
            redirect "/marathons/#{@marathon.id}"
        else 
            redirect '/marathons'
        end 
    end ```
		
I also added a vlidation in my post '/users/signup' controllera ction by ensuring session[:user_id] = @user.id before allowing them to sign in and directing them to the marathons index.

The second improvement I made was adding flash errors. This took a bit of research as I haven't used them before. After adding them to my app I honestly can't see how I haven't used them before, they're so great. 

I first added a flash error to my post '/users/signup' controller: 

```
post '/users/signup' do 
        @user = User.new(params)
        if @user.save
            session[:user_id] = @user.id
            redirect '/marathons'
        else
            flash[:existing_user] = "This user already exists."
            redirect to '/signup'
        end 
    end 
		```
		
I also added an error to the get '/marathons/:id' controller action. Before, if a user clicked on one of my marathons that no longer exists, it would cause the app to crash. Now, with the help of a handy flash error, the app doesn't crash!

```
get '/marathons/:id' do 
        # params to find use params[:id]
        find_marathon
        if @marathon.user == nil
            flash[:nil_id] = "The marathon you are trying to look for no longer exists."
            redirect '/marathons'
        end 
        erb :'marathons/show'
    end 
		```
		
Aside from these changes, I also made some minor structural changes, like taking some details out of my index page and making the marathons on the index page clickable so that users will see more details once they get to the show page. Although I learned a lot about validations, structure, and flash errors in this experience, I think the biggest takeaway for me was the importance of talking your code over with someone and taking feedback in order to improve your app. 
