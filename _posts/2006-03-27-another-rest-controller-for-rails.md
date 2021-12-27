---
id: 218
title: (Another) Rest Controller for Rails
date: 2006-03-27T16:27:18-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=218
permalink: /blog/2006/03/another-rest-controller-for-rails/
categories:
  - Software Development
tags:
  - LOP
  - Rails
  - Software
  - Web Applications
---
[Charlie has released his take on a `RestController` for Rails.](http://cfis.savagexi.com/articles/2006/03/26/rest-controller-for-rails)

That is very sweet. It is great to see more work on RESTful Rails. It seems to me that each attempt gets closer to an approach I could believe in and be proud of. And, I get a warm fuzzy feeling any time I see a domain specific language developing. The `resource` handler Charlie has created is definitely part of a [<acronym title='Domain Specific Language'>DSL</acronym>](http://en.wikipedia.org/wiki/Domain-specific_language) there.

He brings up a few issue that result from his implementation. Some of which are important and some, IMHO, are not.

## Leaky Abstractions {#leaky_abstractions}

Charlie points out that the abstractions start to leak a little when you get to creating the views for a `RestController`.

> The main issue is the method renaming. You have to know about it since you need to create templates called get.rhtml, get_member.rhtml, etc. It also comes into play if you want to turn on or off filters.

That is very unfortunate. The easiest way to solve this problem might be rethink what makes up a controller. The `RestController` design seem intent on combining the functionally of a cluster of related resources. In the example he provides the `ProductController` support interaction with the following resources

\* every known product \* the collection containing every known product \* an editor for product resources \* a creator for product resources

This set of resources are very cohesive and quite coupled to one another so combining them into a single controller is reasonable. But it causes this problem that you have know that the `RestController` is going to take

    resource :Member do
      def get
        #..
      end
    end

and turn it into an action named `get_member`.

Perhaps it would be better to conceptualize a controller as a bit of code that mediates interaction with exactly one type of resource. With this view of the world you would end up with more, smaller, controllers. Charlie&#8217;s product example would look more like

    class ProductController < ApplicationController
      include YarController  # that&#39;s YetAnotherRestController
      
      verb :get do
        @product = Product.find(params[:id])
      end
      
      verb :put do
        @product = Product.find(params[:id])
        begin
          @product.update_attributes(params[:product])
          flash[:notice] = &#39;Product was successfully updated.&#39;
          redirect_to :id => @product
        rescue => e
          # Send the current invalid values to the editor via the flash
          flash[:product] = @product
          redirect_to :resource => :editor, :id => @product
        end      
      end
      
      verb :delete do
        Product.find(params[:id]).destroy
        redirect_to :id => nil, :resource => nil
      end
    end
    
    Class ProductsController < ApplicationController
      include YarContoller
      
      verb :get do
        @product_pages, @products = paginate :products, :per_page => 10
      end
      
      verb :post do
        @product = Product.new(params[:product])
        begin
          @product.save!
          flash[:notice] = &#39;Product was successfully created.&#39;
          redirect_to :resource => :collection
        rescue => e
          flash[:product] = @product
          redirect_to :resource => :editor
        end
      end
    end

And so on&#8230; The main benefits of this is that the template to rendered for a get of &#8216;http://mystore.example/product/243&#8217; is &#8216;apps/view/product/get.rhtml&#8217; and, I think, the issues with filters go away, too. The down side is that you end up with four controllers for each basic type of resource you expose. One for the basic resource type you want to expose, one for the collection of all of those basic resources, one for the creator resource and one for the editor resource. I don&#8217;t know if the extra boiler plate code is worth the benefits but it feels like it might be.

## PUTs and DELETEs {#puts_and_deletes}

Charlie also points out

> a pure REST solution does not work with HTML forms since browsers don&#8217;t support PUT and DELETE

He is absolutely correct. However this [tunneling PUT and DELETE over POST](http://microformats.org/wiki/rest/rails#PUT_.26_DELETE) kludge does not bother me very much. I will now take a moment to revel in being more pragmatic than Charlie, quite possibly for the first time since I meet him seven years ago. Anyway, it is ugly that HTML does not support PUT and DELETE but still very workable.

## Handling Bad Data {#handling_bad_data}

Finally there is an issue with handling failed attempts PUT/POST. This is the one that bothers me the most. It is not really all that bad from a pragmatic standpoint, storing this info in state works fine. However, it implies a certain weakness in my world view because I did not see it coming.

> If the post fails we have to store the ill-formed product into the flash and redirect back to the editor since its at a different URL.

The fundamental problem here is that the separate editor resource will PUT the modified resource when you click save/submit. But what if you messed it up and, say, violated the business rule that blue products must have a price that is divisible by three? In a normal Rails app that proposed change would fail validation and the update action would just re-render the edit page with bad fields highlighted. But in a RESTful world the editor and the validation code are separate and it is wrong from REST stand point to just render the editor resource from in response to a product resource request. However, if you don&#8217;t do that you need to get the form data, and which fields are bad, from the previous attempt so that you can re-render the editor with the information the user previously entered and what was wrong with it.

One way you could solve this problem is to allow the creation of &#8220;invalid&#8221; resources. For example, you require a product to have a description. However, you receive a POST to &#8216;http://mystore.example/products&#8217; without a description. You could issue the product an ID and store it in it&#8217;s invalid state (without a description) and then redirect the browser to the editor resource for that newly created, but invalid, product. That feels really clean from a design stand point but I am not sure how difficult it would be to implement. And you would certainly end up to permanently invalid resources, which might be hard to manage in the future.