# Django Tutorial Polls App

This repository contains the complete code for the [Django](https://www.djangoproject.com/) project's [tutorial](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) `polls` app. The code should mirror the code you've written at the end of [Part 7](https://docs.djangoproject.com/en/2.1/intro/tutorial07/). 

The `SECRET_KEY` variable in `mysite/settings.py` has been scrubbed, and instructions for regenerating the key are available in the accompanying DigitalOcean [tutorial](https://www.digitalocean.com/community/tutorials).

This app is meant to be used as a reference Django app for several DigitalOcean tutorials, and should not be deployed in production.

<b>NOTE: index page in this tutorial will render nothing!</b>
http://127.0.0.1:8000/polls/  renders the application

----

### Quickstart

Polls is a simple Django app to conduct Web-based polls. For each question, visitors can choose between a fixed number of answers.


1. Add `polls` to your `INSTALLED_APPS` setting like this:

```python
INSTALLED_APPS = [
        ...
        'polls',
    ]
```

2. Include the polls URLconf in your project `urls.py` like this:

```python
path('polls/', include('polls.urls')),
```

3. Run `python manage.py migrate` to create the polls models.

4. Create super user 
```python manage.py createsuperuser```

5. Log in as admin and create some questions or use shell

4. Start the development server and visit http://127.0.0.1:8000/admin/
   to create a poll (you'll need the Admin app enabled).

5. Visit http://127.0.0.1:8000/polls/ to participate in the poll.


# Using shell to create question
```>>> from polls.models import Choice, Question

# Make sure our __str__() addition worked.
>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>

# Django provides a rich database lookup API that's entirely driven by
# keyword arguments.
>>> Question.objects.filter(id=1)
<QuerySet [<Question: What's up?>]>
>>> Question.objects.filter(question_text__startswith='What')
<QuerySet [<Question: What's up?>]>

# Get the question that was published this year.
>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>

# Request an ID that doesn't exist, this will raise an exception.
>>> Question.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Question matching query does not exist.

# Lookup by a primary key is the most common case, so Django provides a
# shortcut for primary-key exact lookups.
# The following is identical to Question.objects.get(id=1).
>>> Question.objects.get(pk=1)
<Question: What's up?>

# Make sure our custom method worked.
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
True

# Give the Question a couple of Choices. The create call constructs a new
# Choice object, does the INSERT statement, adds the choice to the set
# of available choices and returns the new Choice object. Django creates
# a set to hold the "other side" of a ForeignKey relation
# (e.g. a question's choice) which can be accessed via the API.
>>> q = Question.objects.get(pk=1)

# Display any choices from the related object set -- none so far.
>>> q.choice_set.all()
<QuerySet []>

# Create three choices.
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

# Choice objects have API access to their related Question objects.
>>> c.question
<Question: What's up?>

# And vice versa: Question objects get access to Choice objects.
>>> q.choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>> q.choice_set.count()
3

# The API automatically follows relationships as far as you need.
# Use double underscores to separate relationships.
# This works as many levels deep as you want; there's no limit.
# Find all Choices for any question whose pub_date is in this year
# (reusing the 'current_year' variable we created above).
>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>

# Let's delete one of the choices. Use delete() for that.
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()```