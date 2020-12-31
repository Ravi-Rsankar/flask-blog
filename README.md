# Flask Blog

From the [Python flask tutorials]([Flask Tutorials - YouTube](https://www.youtube.com/playlist?list=PL-osiE80TeTs4UjLw5MM6OjgkjFeUxCYH)) by `Corey Schafer`

------

## What is jinja2 used for?

**Jinja2** is a modern day templating language for Python developers. It was made after Django's template. It is **used to** create HTML, XML or other markup formats that are returned to the user via an HTTP request.

## SQLite queries used to create db and persist data

Open the python terminal is the project directory and execute the following commands

```sqlite
from flask_blog import db
db.create_all()
from flask_blog import User, Post
user_1 = User(username='Corey', email='C@demo.com', password='password')
db.session.add(user_1)
user_2 = User(username='John', email='jd@demo.com', password='password') 
db.session.add(user_1)
db.session.add(user_2) 
db.session.commit()

User.query.all()
[User('Corey', 'C@demo.com', 'default.jpg'), User('John', 'jd@demo.com', 'default.jpg')]
User.query.first() 
User('Corey', 'C@demo.com', 'default.jpg')

User.query.filter_by(username='Corey').all() 
[User('Corey', 'C@demo.com', 'default.jpg')]
user = User.query.filter_by(username='Corey').first() 
>>> user    
User('Corey', 'C@demo.com', 'default.jpg')
>>> user.id
1
user.posts
[]
>>> post_1 = Post(title='Blog 1', content='First post content', user_id=user.id)
>>> post_2 = Post(title='Blog 2', content='Second post content', user_id=user.id)
>>> 
>>> db.session.add(post_1)
>>> db.session.add(post_2) 
>>> db.session.commit()
>>> user.posts
[Post('Blog 1', '2020-12-31 10:01:36.887287'), Post('Blog 2', '2020-12-31 10:01:36.892239')]
>>> post = Post.query.first() 
>>> post  
Post('Blog 1', '2020-12-31 10:01:36.887287')
>>> post.user_id
1
>>> post.author
User('Corey', 'C@demo.com', 'default.jpg')
>>> db.drop_all()
```

## How did flask handle the validate_username and validate_email() methods in User class?

For anyone wondering how validate_username() and validate_email() are being called, these functions are called with the FlaskForm class that our RegistrationForm class inherited from. If you look at the definition for validate_on_submit(), and from there, the definition for validate(), that validate function contains the following line: 

`inline = getattr(self.__class__, 'validate_%s' % name, None)`.  There is a lot going on in the background, but from what I can tell, Flask is checking for extra functions created with the naming pattern: "validate_(field name)", and later calling those extra functions.