---
id: 754
title: 'Rails tip #72: hands off other&#8217;s private parts'
date: 2012-09-28T09:24:26-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=754
permalink: /blog/2012/09/rails-tip-72-hands-off-others-private-parts/
aktt_notify_twitter:
  - 'yes'
aktt_tweeted:
  - "1"
categories:
  - Software Development
tags:
  - Rails
---
In Ruby on Rails the most common way to pass data from the controller to the views is by [allowing views direct access to the controller&#8217;s instance variables.](http://guides.rubyonrails.org/getting_started.html#listing-all-posts) [Encapsulation](http://en.wikipedia.org/wiki/Encapsulation_(object-oriented_programming)) is one of the cornerstones of software engineering. Why is it thrown out the window for views? Allowing external code access to an objects private parts is just wrong! Seriously, god kills a kitten every time someone does this. What is worse this anti-pattern is perpetrated in pretty much every tutorial on [Rails Guides](http://guides.rubyonrails.org/) and every other RoR tutorial i have ever seen.

Forgoing encapsulation for controllers has the [same issues as it has for any other type of object](http://thecodelesscode.com/case/3). You couple the implementations of the two components in a way that has very high [connascence](http://en.wikipedia.org/wiki/Connascence_(computer_programming)). This means that if either the view or the controller change it is very likely to require a change to the other. All of this adds up to more fragility and maintenance and less fun.

Consider this basic posts controller implemented in the current nominal RoR style

<pre><code class="brush: ruby">
class PostsController &lt; ApplicationController
  def index
    @posts = Post.all
  end
  
  def new
    @post = Post.new
  end

  def show
    @post = Post.find params[:id]
  end
    
  def update
    @post = Post.find params[:id]
    @post.update_attributes params[:post]
  end
  
  def destroy
    @post = Post.find params[:id]
    @post.destroy
  end
end
</code></pre>

That repetition around finding the post sets my teeth on edge. You could fix the repetition by pulling it out into a before filter but that complexifies the code and makes the [already deep stack even deeper](http://www.infoq.com/interviews/patterson-ruby-performance#answer2). The indirect invocation nature of filters makes it easy to overlook their existence and it requires you remember which actions they get invoked on and which ones they don&#8217;t. You even have to keep that stuff in mind when writing partials that are many levels of inclusion removed from the controller. Having to keep all that state in your brain slows you down. It also increases the risk of misremembering something and writing a view that doesn&#8217;t work.

Fortunately, Rails has a good solution to this problem. Consider the following refactor

<pre><code class="brush: ruby">
class PostsController &lt; ApplicationController
  def update
    post.update_attributes params[:post]
  end
  
  def destroy
    post.destroy
  end
  
  def post
    @post ||= if params[:id]
                Post.find params[:id]
              else
                Post.new
              end
  end
  helper_method :post
  
  def posts
    @posts ||= Post.all
  end
  helper_method :posts
end
</code></pre>

And an accompanying view partial

<pre><code class="brush: html">
&lt;h1&gt;&lt;%= post.title %&gt;&lt;/h1&gt;
...
</code></pre>

Now we have two efficiently memoized accessor methods for the data we want to expose to the views. The resulting code is better in several ways

  * The code is DRYer. If we want to implement visibility control for posts we can do it in exactly one place.
  * The logic of the action is easier to follow because the data lookup code is called explicitly.
  * The views and controller are less connascent. For example, if the show view wants to display a list of all posts in the side bar, only the view needs to change, not the controller.
  * The code is easier to keep efficient because those database lookups only happen if they are needed. There is little chance of looking up data that is not used by any one. The lookups only happen if the controller or views explicitly request the data.
  * The view code is more reusable. If you want to reuse that partial in another view (say by fully displaying the ten most recent posts in the index view) you can easily do so by rendering it with a `post` local variable.

Encapsulation is just as good a policy for controllers as it is for models.