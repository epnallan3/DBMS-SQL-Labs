### Intro to JSON

The trick behind JSON, is to turn the object into a string that is syntactically correct JavasSript.  Since JavaScript is an evaluated language, the string can be transferred and then evaluated upon receipt.  Here's an example compliments of Wikipedia:

```json
{    "firstName": "John",
     "lastName": "Smith",
     "age": 25,
     "address": { 
                  "streetAddress": "21 2nd Street",
                  "city": "New York",
                  "state": "NY",
                  "postalCode": 10021
                },
      "phoneNumbers": [
                {
                  "type": "home",
                  "number": "212 555-1234"
                },
                {
                  "type": "fax",
                  "number": "646 555-4567"
                }
      ]
}
```

Just for fun, [open up your javascript console](http://webmasters.stackexchange.com/questions/8525/how-to-open-the-javascript-console-in-different-browsers)

Copy the JSON string *above* to the clipboard.  In the browser's console type `example=`, then paste the JSON string.  You may now hit enter.  (Microsoft's Edge Browser is a bit different-- but you get the idea)

Typing `example<enter>` in the console should give you access to an attribute viewer of some sort (the exact details depend upon the browser).

Play around with the contents of this for awhile.  The object inspector will show you the structure but you can interact with the properties in a bit more of a classic manner-- using `print(Object.keys(example))` or `Object.keys(example)`.

As expected, you can access individual entries (capitalization matters) using dot or bracket notation: 

* `example.firstName` 
* `example["firstName"]`
 
The dot notation is often easier to read, but lacks the flexibility of the bracket notation; In particular, the bracket notation allows the use of variables:
```js
property="lastName"
example[property]
```

If the return value isn't atomic you'll get back something that can be clicked or otherwise manipulated to open an object or array browers:
```js
example.phoneNumbers[0] example.phoneNumbers["0"]
```

An array (in Javascript) is really just a special type of object.  It has some extra methods, but under the hood it is just a collection of attribute:value pairs (just like all the other objects):

```js
example.phoneNumber[0]
example.phoneNumbers["0"]
```

But the dot notation does not allow attributes (or properties as they tend to be called in Javascript) to start with a number.  so

```js
example.phoneNumber.0
```

will give you an error.

[This slideshow](http://www.slideshare.net/blueskarlsson/using-json-with-mariadb-and-mysql) gives a good overview and introduces us to how we can interface MariaDB with JSON data. I understand that the stuff on dynamic columns will be a bit mysterious (we will get back to that down below).  What I want you to get out of this is a better feel for JSON, and a back-of-the-head awareness of how one might start going about interfacing the two technologies:

Now that you have a few examples under your belt here's a very good overview of the structure of JSON (that is totally incomprehensible without the experiences you've already had):  <http://www.json.org/>.   

**NOTE** JSON objects are the fundamental data structures underlying MongoDB-- a NoSQL database that we will spend some time with later.  We might as well play around with it a little bit.  
