# Routes in Sinatra

## Objectives

1. What are routes?
2. How do they function to direct users to see different parts of your webiste?

## What are routes?

Let's say we have a web application that helps doctor's track the medications they've prescribed. Our doctor needs to be able to visit a page on our site that shows *all* of the medicines she's prescribed and she needs to be able visit a page that shoes all of her patients. 

If she types into her browser `http://www.medicine.com/medicines`, she should see a list of all the medications. If she types into her browser `http://www.medicine.com/patients`she should see the page that shows a list of all her patients. 

How does our application know what to show a user based on what they've typed into their browser, i.e. the web request they've sent?

This is where routes come in. A route is responsible for matching an HTTP request, sent by a user to your application, with the code that shows that user a particular page, or view. 

In Sinatra, a route is constructed by pairing an HTTP method/verb, like `GET` or `POST` with a URL-matching pattern, i.e. a string that matches what the user types in to their browser when they want to visit our webpage. We'll see an example in just a moment. 

## How Do Routes Work?

Routes match the web request sent by a client to some code in our application that tells the app what data and templates to send back to the client.

The best way to explain routes is by going through an example. We'll stick with our medicine tracking application, described above. Our app has a collection of medicines stored in its database and collection of patients.

The hash below represents our database with 3 medicines total and 3 patients in total. 

```ruby
Medicine.all = [
  {id: 1, name: "Penicillin", group: "antibiotic"},
  {id: 2, name: "Advil", group: "anti-inflammatory"},
  {id: 3, name: "Simvastatin", group: "cholesterol"}
]

Patient.all = [
	{id: 1, name: "Henry Winkler"}, 
	{id: 2, name: "Job Bluth"}, 
	{id: 3, name: "Michael Bluth"}
]
```

When our doctor types into the browser, `http://www.medicine.com/medicines`, our application gets a request: `GET /medicines`. 

Our Sinatra application will match this request to a route that is defined our application's controller. 

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
  
  erb :'medicines.html.erb'
end
```

Let's run through this specific scenario. 

1. The HTTP request verb, `GET` matches the `get` method in our controller. The `/medicines` path in the HTTP request matches the `/medicines` path in our controller method. Yay! We've successfully matched the request to a controller action!

2. Once our app has found it's match, it will run the code in the block that accompanies the route. In this case, the block will query the `Medicine` table for all of it's medicines. The collection of `Medicine` instances that such a query returns is stored in the variable `@medicines`.

3. Finally, the `@medicines` collection of objects is rendered via the `show.html.erb` template: `views/medicines.html.erb`.

### Resources
- [Routes in Sinatra](http://www.sinatrarb.com/intro.html#Routes)
