# Routes in Sinatra

## Objectives

1. What are routes?
2. How do they function to direct users to see different parts of your webiste?

## What are routes?

Every time a person clicks on a link, types in a URL, or submits a form, they are making HTTP requests to a specific URL on your application. Routes are the part of an application that connect HTTP requests to a specific part of your application code built to handle responding to such a request (that part of code is called a Controller Action).

Let's say we have a web application that helps doctors track the medications they've prescribed. The doctor would need an easy way to see all the medicines she's prescribed.

In the application's navigation is a link "All Medicines". When the user clicks on that link, it directs the browser via the link's `HREF` attribute to the URL "/medicines" on the domain. The URL "/medicines" must trigger the part of your application built to load the appropriate medicines and send a HTML response with a list of medicines.

Also in the application's navigation is a link "All Patients". When the user clicks on that link, it directs the browser via the link's `HREF` attribute to the URL "/patients" on the domain. The URL "/patients" must trigger the part of your application built to load the appropriate patients and send a HTML response with a list of patients.

These mappings are called Routes.

Mapping `http://localhost:9393/medicines` to a Sinatra Action for a response.

![Sinatra Route Example 1](https://dl.dropboxusercontent.com/s/unlxkqbg841b1xh/2015-09-15%20at%209.46%20PM.png)

Mapping `http://localhost:9393/patients` to a Sinatra Action for a response.

![Sinatra Route Example 2](https://dl.dropboxusercontent.com/s/t3mgmc0qwr9hfsi/2015-09-15%20at%209.48%20PM.png)

Users interact with our application by requesting specific URLs via HTTP. URLs are the interface to a web application.

How does our application know what to show a user based on the web request they sent? Through the routes we program in our application.

Being able to draw a route in a web application to respond to an HTTP request is the absolute first step in building anything on the web.

In Sinatra, a route is constructed by pairing an HTTP method/verb, like `GET` or `POST` with a URL-matching pattern, i.e. a string that matches what the user types in to their browser when they want to visit our webpage. We'll see an example in just a moment.

## How Do Routes Work?

Routes match the web request sent by a client to some code in our application that tells the app what data and templates to send back to the client.

Let's go into more detail on how our two routes, `/medicines` and `/patients`, from the example application above.

When our doctor types into the browser, `http://localhost:9393/medicines`, our application gets a request: `GET /medicines`.

Our Sinatra application will match this request to a route that is defined in a controller.

The matching route defined the controller would look like this:

```ruby
get '/medicines' do
	# some code to get all the medicines
	# some code to render the correct view page
 end
```

Once the request has been matched to the route in the controller, called the **controller action**, then it executes the code inside of the controller action's block, as shown below:

```ruby
# medicines_controller.rb

get '/medicines/:id' do
  @medicines = Medicine.all

  erb :'medicines/index.html.erb'
end
```

Let's run through this specific scenario.

1. The HTTP request verb, `GET` matches the `get` method in our controller. The `/medicines` path in the HTTP request matches the `/medicines` path in our controller method. Yay! We've successfully matched the request to a controller action!

2. Once our app has found it's match, it will run the code in the block that accompanies the route. In this case, the block will query the `Medicine` table for all of it's medicines. The collection of `Medicine` instances that such a query returns is stored in the variable `@medicines`.

3. Finally, the `@medicines` collection of objects is rendered via the `index.html.erb` template: `views/medicines/index.html.erb`. The HTML returned from the template is sent by the contoller action as a response to the user.

4. If no matching route for the web request is found, our application will respond with a 404 informing the user's browser that our application cannot find a match for that request URL.

### Resources

- [Routes in Sinatra](http://www.sinatrarb.com/intro.html#Routes)
