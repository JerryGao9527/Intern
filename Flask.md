## Flask

`WSGI`

Web Server Gateway Interface (WSGI) has been adopted as a standard for Python web application development. WSGI is a specification for a universal interface between the web server and the web applications.

`Routing`

The **route()** function of the Flask class is a decorator, which tells the application which URL should call the associated function.

It is possible to build a URL dynamically, by adding variable parts to the rule parameter. This variable part is marked as <variable-name>. It is passed as a keyword argument to the function with which the rule is associated.

In the following example, the rule parameter of **route()** decorator contains **<name>** variable part attached to URL **‘/hello’**. Hence, if the **http://localhost:5000/hello/TutorialsPoint** is entered as a **URL** in the browser, **‘TutorialPoint’** will be supplied to **hello()** function as argument.

```python
@app.route('/hello/<name>')
def hello_name(name):
   return 'Hello %s!' % name
  
@app.route('/blog/<int:postID>')
def show_blog(postID):
   return 'Blog Number %d' % postID

@app.route('/rev/<float:revNo>')
def revision(revNo):
   return 'Revision Number %f' % revNo
```

`Redirect`

The syntax of the `redirect()` function is as follows:

```python
Flask.redirect(location, statuscode, response)
# return redirect(url_for('success'))
```

In the above functions:

- The location parameter is the URL where the response should be redirected.
- statuscode is sent to the browser header by default 302.
- The response parameter is used to instantiate the response.

`Status codes`

The following status codes are standardized:

- HTTP 300: MULTIPLE_CHOICES
- HTTP 301: MOVED_PERMANENTLY
- HTTP 302: FOUND
- HTTP 303: SEE_OTHER
- HTTP 304: NOT_MODIFIED
- HTTP 305: USE_PROXY
- HTTP 306: RESERVED
- HTTP 307: TEMPORARY_REDIRECT
- HTTP 302: NOT FOUND

`HTTP methods`

- **GET**: Sends data in unencrypted form to the server. Most common method.
- **POST**: Used to send HTML form data to server. Data received by POST method is not cached by server.

The data from a client’s web page is sent to the server as a global request object. In order to process the request data, it should be imported from the Flask module.

Important attributes of request object are listed below −

- **Form** − It is a dictionary object containing key and value pairs of form parameters and their values.
- **args** − parsed contents of query string which is part of URL after question mark (?).
- **Cookies** − dictionary object holding Cookie names and values.
- **files** − data pertaining to uploaded file.
- **method** − current request method.

```html
<html>
   <body>
      <form action = "http://localhost:5000/login" method = "post">
         <p>Enter Name:</p>
         <p><input type = "text" name = "nm" /></p>
         <p><input type = "submit" value = "submit" /></p>
      </form>
   </body>
</html>
```

```python
from flask import Flask, redirect, url_for, request
app = Flask(__name__)

@app.route('/success/<name>')
def success(name):
   return 'welcome %s' % name

@app.route('/login',methods = ['POST', 'GET'])
def login():
   if request.method == 'POST':
      user = request.form['nm']
      return redirect(url_for('success',name = user))
   else:
      user = request.args.get('nm')
      return redirect(url_for('success',name = user))

if __name__ == '__main__':
   app.run(debug = True)
```

`Templates`

Flask will try to find the HTML file in the templates folder, in the same folder in which this script is present.

- Application folder
  - Hello.py
  - templates
    - hello.html

The **jinja2** template engine uses the following delimiters for escaping from HTML.

- {% ... %} for Statements
- {{ ... }} for Expressions to print to the template output
- {# ... #} for Comments not included in the template output
- \# ... ## for Line Statements

```html
<!doctype html>
<html>
   <body>
      {% if marks>50 %}
         <h1> Your result is pass!</h1>
      {% else %}
         <h1>Your result is fail</h1>
      {% endif %}
   </body>
</html>
```

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route('/hello/<int:score>')
def hello_name(score):
   return render_template('hello.html', marks = score)

if __name__ == '__main__':
   app.run(debug = True)
```

`Sending Form Data to Template`

In the following example, **‘/’** URL renders a web page (student.html) which has a form. The data filled in it is posted to the **‘/result’** URL which triggers the **result()** function.

The **results()** function collects form data present in **request.form** in a dictionary object and sends it for rendering to **result.html**.

The template dynamically renders an HTML table of **form** data.

```python
from flask import Flask, render_template, request
app = Flask(__name__)

@app.route('/')
def student():
   return render_template('student.html')

@app.route('/result',methods = ['POST', 'GET'])
def result():
   if request.method == 'POST':
      result = request.form
      return render_template("result.html",result = result)

if __name__ == '__main__':
   app.run(debug = True)
```

```html
<html>
   <body>
      <form action = "http://localhost:5000/result" method = "POST">
         <p>Name <input type = "text" name = "Name" /></p>
         <p>Physics <input type = "text" name = "Physics" /></p>
         <p>Chemistry <input type = "text" name = "chemistry" /></p>
         <p>Maths <input type ="text" name = "Mathematics" /></p>
         <p><input type = "submit" value = "submit" /></p>
      </form>
   </body>
</html>
```

```html
<!doctype html>
<html>
   <body>
      <table border = 1>
         {% for key, value in result.items() %}
            <tr>
               <th> {{ key }} </th>
               <td> {{ value }} </td>
            </tr>
         {% endfor %}
      </table>
   </body>
</html>
```

`Cookies`

A cookie is stored on a client’s computer in the form of a text file. Its purpose is to remember and track data pertaining to a client’s usage for better visitor experience and site statistics.

`Sessions`

Session is the time interval when a client logs into a server and logs out of it. The data, which is needed to be held across this session, is stored in the client browser.

A session with each client is assigned a **Session ID**. The Session data is stored on top of cookies and the server signs them cryptographically. For this encryption, a Flask application needs a defined **SECRET_KEY**.

`Message Flashing`

The Flask flash method shows messages to the users.

```python
flash(message,category)
```

- **message:** The message to display
- **category:** An Optional parameter, which can be set to “error,” “info,” or “warning.”

To extract the flash message from the session, where it is stored, and display it on the template, we use the **get_flashed_messages()** function.

`SQLAlchemy`

- **db.session.add**(model object) − inserts a record into mapped table
- **db.session.delete**(model object) − deletes record from table
- **model.query.all()** − retrieves all records from table (corresponding to SELECT query).