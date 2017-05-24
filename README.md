Scheduler System
================

Introduction and Task
---------------------

The code sample we're asking you to submit is based on a stripped down version of our candidate scheduler.

The version included here eliminates a lot of considerations one would really have in the production environment,
in the interests of making it viable in the time frame we've asked you to complete it.

In the implementation presented here, all interview slots will always show as available, because the `is_available`
method of the `InterviewSlot` class `scheduler/models.py` returns `True` for all objects.

What we'd like you to do is provide a correct implementation of `is_available` that will correctly identify when
interview slots can be booked or displayed.

An interview calendar contains one or more interview slot objects which define what times are available to book,
as well as the maximum number of candidates that can be booked in each slot. A calendar can also contain interview
conflicts. Interview conflicts specify a date and time range during which no interviews can be booked.

An interview calendar also specifies the shortest time in hours from the current time when any interview slot can
be booked, and the maximum number of hours away from the current time when an interview slot can be booked.

`is_available` is a method on an `InterviewSlot` which accepts an argument `interview_date`, which should be of type
`datetime.date`, and returns whether the invoking `InterviewSlot` is available on that date.

The application should perform the following tasks correctly:
- Visiting the `/calendar/<id>/` path in the application with a start and end date should show only the slots
available between those dates. For example, `/calendars/<id>/?startDate=2017-05-01&endDate=2017-05-10` should show
all remaining available slots between May 1, 2017 and May 10, 2017, taking into consideration conflicts and the
min/max hours out.

- You can create an interview via a POST to `/interviews/` with a JSON object for a slot:
```
{
    "slot_id": 1,
    "start_time": "2017-03-14T12:00:00-05:00",
    "end_time": "2017-03-14T13:00:00-05:00",
    "timezone": "US/Central"
}
```
In the event that the slot is not available, this POST should fail. The application is already set up so that it
will return a 404 if the slot doesn't exist or a 400 if it exists but is not available, so you can use this
to evaluate the behavior of your implementation.

When providing your implementation, please write code like you would for a production environment,
documenting and providing tests for your code as you normally would. We expect this sample to take you about
two hours, but it isn't timed so take as much or as little time as you feel you need to be comfortable
submitting it.


Getting the Application Running
-------------------------------

The scheduler is a Dockerized Django REST application. You'll need to make sure that Python and Docker are installed on
your system and correctly configured in order to run the application. The files in this package should be set up
such that once Docker is configured, you can simply run `docker-compose up --build` to rebuild and run the
application. Depending on your environment, you might need to run `docker-compose` as an administrator in order for
the container to start correctly.

If you don't already have Docker on your machine, Docker provides an [installation guide](https://docs.docker.com/engine/installation/).

Note that if you're running Windows or MacOS X, you'll need to use `docker-machine` to set up a virtual environment
that your containers can run in.

After you run `docker-compose up`, you *should* see something like this:

```
evan@evan:~/mya-scheduler-coding-challenge$ sudo docker-compose up
Starting schedulercodingchallenge_postgres_1
Starting schedulercodingchallenge_web_1
Attaching to schedulercodingchallenge_postgres_1, schedulercodingchallenge_web_1
postgres_1  | LOG:  database system was shut down at 2017-05-19 03:26:01 UTC
postgres_1  | LOG:  MultiXact member wraparound protections are now enabled
postgres_1  | LOG:  database system is ready to accept connections
postgres_1  | LOG:  autovacuum launcher started
web_1       | Operations to perform:
web_1       |   Apply all migrations: admin, auth, contenttypes, scheduler, sessions
web_1       | Running migrations:
web_1       |   No migrations to apply.
web_1       |   Your models have changes that are not yet reflected in a migration, and so won't be applied.
web_1       |   Run 'manage.py makemigrations' to make new migrations, and then re-run 'manage.py migrate' to apply them.
web_1       | Performing system checks...
web_1       | 
web_1       | System check identified no issues (0 silenced).
web_1       | May 18, 2017 - 20:58:28
web_1       | Django version 1.11.1, using settings 'scheduler_api.settings'
web_1       | Starting development server at http://0.0.0.0:8000/
web_1       | Quit the server with CONTROL-C.

```

At this point you should be able to access the Django application by navigating to the address specified in the line `Starting development server at <address>`

If you can't get the Docker container to start correctly, or if the Docker container starts but you see other error messages and can't access the Django application,
please contact us. Resolving issues with Docker or the Django environment isn't part of the interview, and we want to get those ironed out for you as soon as we can.

Setting up a Superuser
----------------------

Once the Docker container is up, you'll probably want to add a superuser to the application, so that you can access the
Django admin console and add calendars, interview slots, and conflicts to the database.

To do this, run `python manage.py createsuperuser`

You'll be prompted to create a username, provide an email address, and set up a password. You can choose whatever is convenient for you.

After you create the superuser, if you navigate to the `/admin` path in the application, you should be able to log in as the superuser
you created and add items to the database through Django's GUI. This will help you test your implementation. Again, if the admin console
doesn't work for some reason and `manage.py` reports that you successfully created the superuser, let us know and we'll try to get things
sorted as soon as we can.


Submitting your solution
------------------------

When you are finished with the implementation to your satisfaction, please archive and compress the package directory as a `tar.gz` file, and return it
to evan@firstjob.com

The product engineering team will take a look at it as soon as possible, and get back to you about next steps in the interview process.

We look forward to seeing your work!
