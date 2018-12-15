# Apis best practices

Before we step in, into the wonderful and exciting world of Apis, I want to share an interesting fact with you guys.

It is believed that Amazon’s transformation from an online bookstore to one of the world’s most successful internet companies 
started in 2002. More specifically, when **Jeff Bezos issued the infamous Bezos Mandate to his internal development teams** 
regarding how software was to be built at Amazon.
   I believe this made Amazon take a huge leap of success over its competitors then and now.

**Jeff Bezos’ (Key to Success) Mandate**
 - All teams will henceforth expose their data and functionality through service interfaces.
 - Teams must communicate with each other through these interfaces.
 - There will be no other form of interprocess communication allowed: no direct linking, no direct reads of another team’s data store, no shared-memory model, no back-doors whatsoever. The only communication allowed is via service interface calls over the network.
 - It doesn’t matter what technology they use. HTTP, Corba, Pubsub, custom protocols — doesn’t matter. Bezos doesn’t care.
 - All service interfaces, without exception, must be designed from the ground up to be externalizable. That is to say, the team must plan and design to be able to expose the interface to developers in the outside world. No exceptions.
 - **Anyone who doesn’t do this will be fired.**

I hope the above mentioned fact has made you even more excited to get started.

## Let's get started

### What's a Resource?
  Anything that has data associated with it and has methods to operate on it.

 **Example:**
      Employees are resources and delete, add, update are the operations to be performed on this resource.


### What's a Collection?
  Set of resources

 **Example:**
      Company is the collection of employee resource.


### What's an URL?
  Uniform Resource Locator is the path through which the resource can be accessed and operations can be performed on it.


### Whats an Api?
   Api stands for Application Programming Interface, its a defined way by which other programs can interact with ours.


### Whats a RESTful Api?
   REST stands for REpresentational State Transfer, which is an architectural design where our programs are exposed using existing
    protocols, typically considering the HTTP protocol.

## Rule of FOUR:
> There are 4 rules for an api to be RESTful:

### Rule 1: Access through resources.

In typical clients and servers setup, there are exchange of commands: do this, do that. Suppose we want to model a to-do list in a non-REST way. In a technical language, that might look like this:

**NOT REST**

    /changeTodoList.php?item=42&action=changeTitle&title=new_title

Note how this is indeed an instruction: change something.
But a “changeTodoList” is not a thing, it's not a resource.

In the REST architectural style, servers only offer resources. Resources are conceptual things about which clients and servers communicate.

**REST**

    /todolists/7/items/42/

This above thing is not a command, it is the address of a resource, a thing.
You can then use this address to manipulate the to-do list using standard operations, instead of interface-specific commands.


### Rule 2: Represent resources by representations.

A resource is anything that has data associated to it and it can be in different formats. For assumptions, humans might want to see an HTML version, which your browser transforms into a readable layout. But sometimes, interfaces on the Web are used by machines, too. They need a different format, such as JSON.

**NOT REST**

Different formats have different addresses:

    browser: /showTodoList.php?format=html
    application: /showTodoList.php?format=json

*The problem is then that systems using different formats cannot communicate with each other, because they use different addresses for the same things!*

**REST**

Addresses identify things, not formats, so all systems use the same address for the same thing. How can they get different formats then? They explicitly ask for it!

    browser: “I want /todolists/7/, please give me HTML.”
    application: “I want /todolists/7/, please give me JSON.”

The technique that enables this is called **content negotiation.**


### Rule 3: Exchange self-descriptive messages.

In a REST system, the current message should be independent of the context of the previous message,
ie, we should be able to interpret any message without having seen the previous one.

Consider the following conversation:

**NOT REST**

    /search-results?q=todo
    /search-results?page=2
    /search-results?page=3

The first request gets search results for “todo”, the second request gets the second page of that. Now imagine that you only see the second request. How would you know as a server what to do?

**REST**
Each message stands on its own

    /search-results?q=to-do
    /search-results?q=todo&page=2
    /search-results?q=todo&page=3

Note how each request can be interpreted by itself.
Another aspect of this, is that REST clients and servers only use standard operations, which are defined in a specification. For the Web, this specification is called HTTP.


### Rule 4: Connect resources through links

How can you navigate a website you've never seen before? You use links!
You don't have to manually edit the address bar in your browser every time you go to a new page.

In machine interfaces, this is not always the case. Suppose an application asks for your to-do list. It might receive the following representation:

**NOT REST**

    /todolists/7/

    {
      "name": "My to-dos",
      "items": [35, 36]
    }

Now how can you get the items of the list?
Good question! We'd have to read the documentation for that.

In REST, resources connect to each other through hyperlinks:

**REST**

    /todolists/7/

    {
      "name": "My to-dos",
      "items": ["/todolists/7/items/35/", "/todolists/7/items/36/"]
    }


### 1. Prefer plural nouns
   Use plural nouns while naming, that will keep things simple.
   (ie. books instead of book)


#### Example:
                    *GET*                         *POST*                        *PUT*                         *DELETE*
      */books*      to fetch books              to create a book       collective updates of books        delete all books


      */books/12*   to fetch a particular book  (405) Method not allowed    update a particular book      delete a particular book

      Good evening Sir,
           Sir tomorrow there is felicitation ceremony arranged for Donald Sir by Defense Forces tomorrow by 3pm in Surya Palace and  my presence will be needed there Sir . Sir so I will
           be needing half day leave for the 2nd half. I kindly request you Sir to grant me the half day leave Sir.

      Thanking you

      With Regards,
      Vivek Mitra [CTO, VP (R&D)]




      Info of the event

          Event  - Felicitation ceremony for his service towards Defense forces of India
          Venue - Surya Palace Vadodara
          Chief Guest -
          Attendees - Media, Donald Sir Private Senior student


### 2. GET should not alter the state
   Use PUT, POST and DELETE methods instead of the GET method to alter the state.

**Do not**

    GET /users/11?activate or
    GET /users/11/activate


### 3. HTTP headers for serialization formats
   Client and Server should be aware of the format for the communication. The right place to specify it is in the HTTP-Header.

**Content-Type** defines the request format.
**Accept** defines a list of acceptable response formats.


### 4. Sub-resources for related resources

    GET /truck/711/drivers/ Returns a list of drivers for truck 711
    GET /truck/711/drivers/4 Returns driver #4 for truck 711


### 5. Use HATEOAS
   Hypermedia As The Engine Of Application State (HATEOAS) is a unique design feature of the REST application architecture.
 here a client interacts with a network application whose application servers provide information dynamically through hypermedia.

    {
	  "id": 11,
	  "manufacturer": "bmw",
	  "model": "TruckX5",
	  "seats": 5,
	  "drivers":
    [
	   {
	    "id": "23",
	    "name": "Jay malhotra",
	    "links":
      [
	     {
	     "rel": "self",
	     "href": "/api/v1/drivers/23"
	     }
	    ]
	   }
	 ]
	}


### 7. Filtering:
   Its advisable to use unique query parameter for all fields or a query language for filtering.

    GET /truck?color=red Returns a list of red truck
    GET /truck?seats<=2 Returns a list of truck with a maximum of 2 seats


### 8. Sorting:
   Ascending and Descending sorting over multiple fields.

    GET /truck?sort=-manufactorer,+model

   This returns a list of truck sorted by descending manufacturers and ascending models.


### 9. Field selection
   Mobile clients display just a few attributes in a list. They don’t need all attributes of a resource. Give the API consumer the ability to choose returned fields. This will also reduce the network traffic and speed up the usage of the API.

    GET /truck?fields=manufacturer,model,id,color


### 10. Paging
   Use limit and offset. It is flexible for the user and common in leading databases. The default should be limit=20 and offset=0

    GET /truck?offset=10&limit=5

   To send the total entries back to the user use the custom HTTP header: X-Total-Count.

   Links to the next or previous page should be provided in the HTTP header link as well. It is important to follow this link header values instead of constructing your own URLs.

    Link: <https://cyberfreaky.com/sample/api/v1/truck?offset=15&limit=5>; rel="next",
    <https://cyberfreaky.com/sample/api/v1/truck?offset=50&limit=3>; rel="last",
    <https://cyberfreaky.com/sample/api/v1/truck?offset=0&limit=5>; rel="first",
    <https://cyberfreaky.com/sample/api/v1/truck?offset=5&limit=5>; rel="prev",


 ### 11. Versioning your API
   Always release versioned API.

   **Good Practise:**

   - Use a simple ordinal number
   - Avoid dot notation like 1.7
   - Use prefix for version such as "v".

    /blog/api/v2


 ### 12. Crossing path with Errors

It's a difficult path to walk on, with an API that ignores error handling. Returning of a HTTP 500 with a stacktrace is not that helpful.

The HTTP standard provides over 70 status codes to describe the return values. But the top 10 codes that we gonna need quite often are listed down.

**200 – OK** – Eyerything is working

**201 – OK** – New resource has been created

**204 – OK** – The resource was successfully deleted

**304 – Not Modified** – The client can use cached data

**400 – Bad Request** – The request was invalid or cannot be served. The exact error should be explained in the error payload. E.g. „The JSON is not valid“

**401 – Unauthorized** – The request requires an user authentication

**403 – Forbidden**** – The server understood the request, but is refusing it or the access is not allowed.

**404 – Not found** – There is no resource behind the URI.

**422 – Unprocessable Entity** – Should be used if the server cannot process the enitity, e.g. if an image cannot be formatted or mandatory fields are missing in the payload.

**500 – Internal Server Error** – API developers should avoid this error. If an error occurs in the global catch blog, the stracktrace should be logged and not returned as response.

**Usage of error payloads**

All exceptions should be mapped in an error payload. Here is an example how a JSON payload should look like.

	{
	  "errors":
    [
	   {
	    "userMessage": "Sorry, the requested resource does not exist",
	    "internalMessage": "No car found in the database",
	    "code": 34,
	    "more info": "http://dev.mwaysolutions.com/blog/api/v1/errors/12345"
	   }
	  ]
	}



### References
Restful Api Design by Matthias Biehl

RESTful Web APIs: Services for a Changing World by Leonard Richardson, Michael Amundsen

RESTful APIs by Mr.Jauker

Practical API design Book by Jaroslav Tulach
