## Commit 1 Reflection
In the handle_connection function, we make a new BufReader instance, which wraps a reference to the stream. It adds buffering, meaning it reads a larger chunk of data into memory at once. http_request collects the lines of the request that the browser sends to our server in a vector. The lines method then splits the stream of data into a String whenever it sees a newline byte, and then each String is mapped to a Result. Because we're slacking on error handling for this one, we just put in unwrap so that if errors show up, we just stop the program. The browser signals the end of a HTTP request by sending two newline characters in a row, so if we get a line that is the empty string, that signifies one request from the stream. After all is done, we print out the result.

## Commit 2 Reflection 
![alt text](image.png)
After this round of modifications, we've added fs to import the standard library’s filesystem module. We also added a status_line variable to hold the message's success data. To handle errors, we use unwrap() to stop the program. The variable contents store the results of hello.html read as a string, and the variable length stores the length of contents. Then, we use format! to add the file’s contents as the body of the success response. The as_bytes method is called on the response to convert the string data to bytes. As mentioned in the book, the write_all method on stream takes a '&[u8]' and sends those bytes directly down the connection. Because of this, we are able to see our hello.html webpage when we access the endpoint.

## Commit 3 Reflection 
![alt text](image-1.png)
### Splitting the Response Handling
To do selective responding, we're going to look at the first line of the HTTP request, so isntead of reading the entire request as a vector, we call next to get the first item in the iterator. This is stored into the request_line variable, and if it is a GET request with a / path, then the if block returns the contents of hello.html. If it doesn't, we must have received some other request, so we'll add an else block to direct it into 404.html.

### Refactoring
When looking at the code, it's easy to notice that there's a lot of repetition going on. We can clean up the code by assigning the status line and filename first, then using those variables later. Now the if and else blocks only return the status line and filename, which is assigned to a tuple with the let statement. The resulting code is easier to maintain, because if we have to make any changes, we only have to do it in one place now. 