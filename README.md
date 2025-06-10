# FLASK STARTER TEMPLATE

## QUICK START

1. Clone this repo
2. Create a .env file with:
   SECRET_KEY=your_random_key_here
3. Run:
   pip install -r requirements.txt
   python app.py

## GOTTA REMEMBER

* The app uses sessions - check for 'user_id' in session to protect routes
* Passwords are hashed using werkzeug.security
* DB is SQLite, file will appear when you first run the app
* Static files go in /static (CSS/JS/images)

## WHEN YOU NEED TO ADD STUFF

### Need a new protected page?

1. Add route in app.py:

   ```
   @app.route('/whatever')
         def whatever():
             if 'user_id' not in session:
             return redirect(url_for('login'))
             return render_template('whatever.html')
   ```
2. Make whatever.html in templates/ (copy from existing ones)

### Need to store new data?

1. Add to models.py:

   ```
   class NewThing(db.Model):
         id = db.Column(db.Integer, primary_key=True)
         name = db.Column(db.String(100))
         user_id = db.Column(db.Integer, db.ForeignKey('user.id'))
   ```
2. In app.py after making changes:

   ```
   with app.app_context():
         db.create_all()
   ```

### Need to change styles?

Edit /static/css/style.css - it's linked in all pages

## WHEN THINGS BREAK

* Database acting weird? Delete the .sqlite file and restart
* Sessions not working? Double check your .env has SECRET_KEY
* Getting weird errors? Try:
  pip install -r requirements.txt --upgrade

## QUICK COMMANDS

Create admin user (run in python shell):
from models import User
from werkzeug.security import generate_password_hash
user = User(username='admin', email='admin@admin.com', password=generate_password_hash('admin123'))
db.session.add(user)
db.session.commit()

Reset entire database (temporary route):

```
@app.route('/nuke')
def nuke():
      db.drop_all()
      db.create_all()
      return 'Database nuked'
```
