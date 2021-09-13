---
layout: post
title: "A quick and dirty way to test unmanaged model in Django using pytest"
categories: testing django
---

## What is unmanaged model

By default, Django will take care creating the database tables for the models in your apps.
But if you want to use an already existing table or say, a view from django, you will have
to take control yourself by setting `managed = False` in your model's Meta class. Like so:

```python
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

    class Meta:
        managed = False
        db_table = 'poll_question'
```

Now, what this means is you will have to take care of creating the table/view, updating
the fields and deleting it if need be. This applies both in production and during test.
Otherwise, Django will complain with `relation <db_table_name> does not exist` or something similar.

## testing unmanaged model

Essentially, the question therefore boils down to how do you ensure that the tables for your unmanaged models
are available to Django during test?
Below, I briefly point out some ways to do just that.

### create the model using the schema editor

In my opinion, this works well if you have one or few unmanaged models. Not that you
cannot use it for many models (I can't say, since I have not tried that), but you
will find other more suitable methods below as well.
An example usage would look like this:

```python
from django.db import connection
from polls.models import Question

@pytest.fixture
def create_question_table():
    # setup: aka this code will run before the test
    with connection.schema_editor() as schema_editor:
        schema_editor.create_model(Question)

    yield
    # teardown: aka this will run after the test
    with connection.schema_editor() as schema_editor:
        schema_editor.delete_model(Question)
```

You can then use this fixture in a test, where the Question model is needed.
For completeness sake, a sample test that uses this might look like so:

```python
from datetime import datetime
from polls.models import Question

def test_question():
    assert Question.objects.count() == 0

    Question.objects.create(question_text='what is unmanaged model?', pub_date=datetime.utcnow())

    assert Question.objects.count() == 1

```

This was adapted for pytest from this [stackoverflow answer](https://stackoverflow.com/a/49800437)
And [here are other operations you can use the schema editor for](https://docs.djangoproject.com/en/3.2/ref/schema-editor/).

### Make use of autouse fixture

This [stackoverflow answer](https://stackoverflow.com/a/50849037) shows how to do that.
The idea is to make unmanaged models become managed under test.

### Unittest bonus: Create a custom test runner

There is a [good blog post by the Cactus Group](https://www.caktusgroup.com/blog/2010/09/24/simplifying-the-testing-of-unmanaged-database-models-in-django/) that shows how this can be done.
The idea is similar to what the autouse fixture above does, and that is to let Django managed
the models for you under test.

### fin

And that is all I have to say on this topic. At least for now, I think.
