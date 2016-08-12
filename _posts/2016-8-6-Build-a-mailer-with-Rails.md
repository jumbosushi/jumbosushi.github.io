---
layout: post
title: "Build a mailer with Rails"
published: true
---

![mailer_top](https://media.giphy.com/media/3oEdv9OpWdiMIcCnYc/giphy.gif)

Guys, I have a confession to make.

Believe it or not, I find emails to be ... cool.
There. I said it.

I was working on a system-failure notification system at my internship, and that was when I was introduced to the concept of mailers in rails.  
Building your own email service? How cool is that!?

In this tutorial we will learn how to set up your own local email service in your rails app!

This article is aimed at people who have some idea of how MVC works in Rails, but havent really touched that "mailer" directory ever since "rails new" command (we've all been there ヽ(´ー｀)ノ)

Haven't used Rails in a while or just wanna know enough to go through this article?  
I found [this article](http://adrianmejia.com/blog/2011/08/11/ruby-on-rails-architectural-design/) by Adrian Mejia super helpful when I was trying to grasp the overview of the framework. It's bit lengthy but it'll be worth it. Check it out!

What you'll soon find out is that mailer is nothing to be scared of. In fact, it follows a familiar MVC logic that the rest of the rails app has. It's almost like learning to ride a penny board after knowing how to ride skateboard. Piece of cake.

We will make a local email newsletter called TeaHouse that send am email with tea emojis  when the action is triggeered.

Join me. Its time to maken emails cool again.

![it's time](https://media.giphy.com/media/13YvCtTJCT4mGI/giphy.gif)

## Setting up the applicatin

Close your Slack and let's get started!
(But you can keep open #catgifs for you know ... for ... emergencies)


**My Development Environment:**
- **Ruby version: 2.3.1**
- **Ruby on Rails version: 4.2.6**
- **OS: Ubuntu 16.04 LTS**
**

To make my life simpler, I'll assume that you are using UNIX environment and already have ruby + rails installed on your machine (Sorry Windows guru :/ maybe next time)

The first step is to make a new app.

```bash
rails new teahouse
```

Awesomeeeee. Do you see that mailer directory?

![mailer directory](http://i.imgur.com/peLf97X.png)

Now time to make the ActionMailer! Think of it like a Controller in good ol' rails page rendering.

```bash
rails g mailer tea_mailer
```

Inside `app/mailers/tea_mailer.rb`, we'll create a new method called welcome_email, and this will handle the action of sending our tea email. It takes the user object (don't worry we'll make it soon) as param.

```ruby
class TeaMailer < ApplicationMailer
  def welcome_email(user)
    @user = user
    mail(to: @user.email, subject: 'Hello from TeaHouse!')
  end
end
```

Now it's time to make the actual content of the email!  
Make a new html file at `app/views/tea_mailer/welcome_email.html.erb`  
This will be template of the email we'll be sending in our app. Feel free to make it anything you would expect from TeaHouse newsletter (obviously tea emojis). Here's mine!

```html
<!DOCTYPE html>
<html>
  <head>
    <meta content='text/html; charset=UTF-8' http-equiv='Content-Type' />
  </head>
  <body>
  <h1>Hey <%= @user.name %>!</h1>
  <p> Welcome to Tea House newsletter! </p>
  <p> This is where you'll get your daily dose of tea emojis!</p>
  <br/>
  <p> 🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵 </p>
  <p> 🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵 </p>
  <p> 🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵 </p>
  </body>
</html>
```

According to [the rails doc](http://guides.rubyonrails.org/action_mailer_basics.html), it is good practice to send the text version of the email on top of the html. This is so that some email clients that does't not support html template can still receive our email (aka my grampa's email client). Its best not to make too many assumptions about our users.

`app/views/tea_mailer/welcome_email.txt.erb` should be made, and here's how mine turned out.

```
Hey <%= @user.name %>!

==============================

Welcome to Tea House newsletter!
This is where you'll get your daily dose of tea emojis

🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵
🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵
🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵🍵
```

Here we have an email that is ready to be sent, and now we just need something like a Santa Clous to deliver these packages of joy to people around the world (btw shoutout to my university's [President Santa](https://twitter.com/ubcprez))

Just like Santa, we need the names and email addresses to complete a delivery. Let's set that up first.

```bash
rails g scaffold user name:string email:string
rails db:migrate
```

This creates a User table with name and email columns, both data types being a string. In other words, we are all set! We know who to send the emails to, we know what to send on the email, and now we gotta figure out how.

Before we go to the next step, I know what you're thinking if you are like me when I tried it out on my own

![I read your mind](http://img.topyaps.com/wp-content/uploads/2016/06/HO.gif)

No worries! Rails made it all simple for us.


## Mail delivery Logic

We'll use a gmail's stmp domain to send out our emails!
It sends email through [STMP](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) (think HTTP but for email), except it's way simpler than traditional setup when used in rails (if you are interested in how STMP works, Microsoft busted out a [great write-up](https://technet.microsoft.com/en-us/magazine/cc160769.aspx) on how it works if used as it is)

Head over to `config/environments/development.rb`, and add these magical lines at the end of the file:

```ruby
# Send mailer
config.action_mailer.delivery_method = :smtp
config.action_mailer.smtp_settings = {
 :address              => "smtp.gmail.com",
 :port                 => 587,
 :user_name            => ENV["GMAIL_USERNAME"],
 :password             => ENV["GMAIL_PASSWORD"],
 :authentication       => "plain",
 :enable_starttls_auto => true
}
```

It's almost always a good idea to store user credentials somewhere else safe, so that's what we'll do. Since this is a local app, let's save it in your local environment. Open `~/.bashrc` (or `~/.zshrc` if you're using zsh), and add this.

```
# Gmail credentials
export GMAIL_USERNAME="YOUR_GMAIL_NAME"
export GMAIL_PASSWORD="YOUR_GMAIL_PASSWORD"
```

Our email delivery has to be triggeered from somewhere, so we'll do exactlthat in `app/controllers/users_controller.rb`

```ruby
# POST /users
# POST /users.json
def create
  @user = User.new(user_params)

  respond_to do |format|
    if @user.save

      # Send email when user is created
      TeaMailer.welcome_email(@user).deliver_now

      format.html { redirect_to @user, notice: 'User is created!' }
      format.json { render :show, status: :created, location: @user }
    else
      format.html { render :new }
      format.json { render json: @user.errors, status: :unprocessable_entity }
    end
  end
end
```

Boom. Thanks big G. On to the (even more) fun part. Let's make all of this work!

## Triggering the email

As you can see from the comment on `app/controllers/users_controller.rb`, create method is called from a POST request. We can do this real simply with the help of an amzing API tool called [Postman](https://www.getpostman.com/)!

Once you downloaded the app, you will see this UI

![Postman before](http://i.imgur.com/JmpTail.png)

Let's make it work for our app!

- Change GET request to POST request
- Add localhost:3000/users to the request URL field
- Click on param, and add add these key & value pairs:
  * key: user[name],  value: John Doe
  * key: user[email], value: YOUR_EMAIL

Using user[something] syntax is used to send a hash to our application through Postman.  
In this case, what is being send is:

```ruby
user = {
  "name" => "John Doe",
  "email" => "YOUR_EMAIL"
}
```

Afterward, it should look something like this:

![Postman after](http://i.imgur.com/97ouf2q.png)

So how will this work? Let's bring it back to earilier MVC topic.
The steps taken by our application will be something like:

POST to `localhost:3000/users`  
-> `routes.rb` triggers `users_controller`'s create method  
-> `users_controller` triggers   TeaMailer.welcome_email()  
-> Email is sent!

From herem it's clear that ActionMailer is integrated well into the rail's MVC workflow.  
Just a simple POST, and Rails takes care of the rest for us.  
Pretty cool eh?

It's all set! Change the address to your own gmail, and click the send button.

Ready for the surpsrise? Go check your gmail!

Result:

![Result](http://i.imgur.com/Or1KOUq.png)

And that's it! You just learned how to send an email through your own email application 🎉🎉🎉

![Pretty cool eh](https://media.giphy.com/media/a7YAu5i1LuRhK/giphy.gif)

## More resources

Hope you enjoyed this tutorial! I used [this mailer article](https://launchschool.com/blog/handling-emails-in-rails) written by Saurabh B as well as [Ruby on Rails documentation](http://guides.rubyonrails.org/action_mailer_basics.html) to write this up, so highly encouraged go check them out for more information about mailers.

Feel free to tweet me at [@jumbosushi](https://twitter.com/jumbosushi) if you have any questions / suggestions for this post!

Till next time. Sayonara folk!

![last one](https://media.giphy.com/media/UjBvt2DJobX7q/giphy.gif)
