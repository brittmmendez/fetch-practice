# fetch-practice
https://scotch.io/tutorials/how-to-use-the-javascript-fetch-api-to-get-data


Using fetch to get data from an API
We are now going to build a simple GET request, in this case, I will use the Random User API and we will get 10 users and show them on the page using vanilla JavaScript.



Let's get started with the HTML, all we really need is a heading and an unordered list:

  <h1>Authors</h1>
  <ul id="authors"></ul>
The idea is to get all the data from the Random User API and display it in list items inside the author's list.

The first step is to actually set the URL we need and also the list we are gonna put the data in, so in the Javascript we write:

  const ul = document.getElementById('authors'); // Get the list where we will place our authors
  const url = 'https://randomuser.me/api/?results=10'; // Get 10 random users
I have set these to consts so you don't risk changing these in the future and these two are meant to be constants through all the project. Now we get into actual Fetch API:

Related Course: Getting Started with JavaScript for Web Development
fetch(url)
  .then(function(data) {
    // Here you get the data to modify as you please
    })
  })
  .catch(function(error) {
    // If there is any error you will catch them here
  });   
Let's review this code, shall we? So first we are calling the Fetch API and passing it the URL we defined as a constant above and since no more parameters are set this is a simple GET request. Then we get a response but the response we get is not JSON but an object with a series of methods we can use depending on what we want to do with the information, these methods include:

clone() - As the method implies this method creates a clone of the response.
redirect() - This method creates a new response but with a different URL.
arrayBuffer() - In here we return a promise that resolves with an ArrayBuffer.
formData() - Also returns a promise but one that resolves with FormData object.
blob() - This is one resolves with a Blob.
text() - In this case it resolves with a string.
json() - Lastly we have the method to that resolves the promise with JSON.
Looking at all these methods the one we want is the JSON one because we want to handle our data as a JSON object so we add:


  fetch(url)
  .then((resp) => resp.json()) // Transform the data into json
  .then(function(data) {
    // Create and append the li's to the ul
    })
  })
Now let's get to the part we create the list items, for that, I created two helper functions at the top of my file just to make the code simpler down the line:

  function createNode(element) {
    return document.createElement(element); // Create the type of element you pass in the parameters
  }

  function append(parent, el) {
    return parent.appendChild(el); // Append the second parameter(element) to the first one
  }
All these functions do is append and create elements as you can see. Once this is done we can move on to the resolution of our promise and add the code we need to append these list items to our unordered list:

then(function(data) {
    let authors = data.results; // Get the results
    return authors.map(function(author) { // Map through the results and for each run the code below
      let li = createNode('li'), //  Create the elements we need
          img = createNode('img'),
          span = createNode('span');
      img.src = author.picture.medium;  // Add the source of the image to be the src of the img element
      span.innerHTML = `${author.name.first} ${author.name.last}`; // Make the HTML of our span to be the first and last name of our author
      append(li, img); // Append all our elements
      append(li, span);
      append(ul, li);
    })
  })
So first we define authors as the response we get from the request then we map over all the authors and for each we create a list item, a span, and an image. In the image source, we place the picture of the user, the HTML of the span will be the first and last name interpolated and then all we need to do is append this to their rightful parents and voilá, our HTTP request in vanilla JavaScript is done and returning something to the HTML.
