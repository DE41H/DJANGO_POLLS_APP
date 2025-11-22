# DJANGO POLLS APPLICATION

This project is the classic "Polls" application created by following the
official Django tutorial. It demonstrates fundamental Django concepts,
including models, views, templates, URL routing, and the Admin
interface.

## FEATURES

-   **Public Interface:** Allows users to view available polls, select a
    choice via a radio button form, and submit their vote.
-   **Results View:** Displays the total vote counts for each choice
    after a user votes.
-   **Admin Interface:** Fully customized admin site for creating
    questions, adding choices, and managing publication dates.
-   **Atomic Voting:** Implements secure, concurrent voting using
    Django's `F()` expressions to prevent race conditions during
    database updates.
-   **Clean Architecture:** Utilizes separate `forms.py` for validating
    user submissions, separating concerns between the view logic and
    form validation.

## SETUP AND INSTALLATION

Follow these steps to get the project running locally.

### Prerequisites

-   Python 3.8+
-   pip

### Steps

#### Clone the Repository

``` bash
git clone https://github.com/your-username/django-polls-app.git
cd django-polls-app
```

#### Create a Virtual Environment

It's best practice to isolate project dependencies.

``` bash
python -m venv venv
source venv/bin/activate  # On Windows, use venv\Scripts\activate
```

#### Install Dependencies

``` bash
pip install -r requirements.txt
```

*(You may need to create a `requirements.txt` file containing at least
`Django==5.0.0` or your desired version.)*

#### Run Migrations

Apply the database schema changes (including the polls app models).

``` bash
python manage.py makemigrations
python manage.py migrate
```

#### Create Superuser (Admin Access)

``` bash
python manage.py createsuperuser
```

#### Run the Server

``` bash
python manage.py runserver
```

The application will be accessible at http://127.0.0.1:8000/

------------------------------------------------------------------------

## KEY ENDPOINTS

  -----------------------------------------------------------------------
  Path                         Purpose
  ---------------------------- ------------------------------------------
  `/polls/`                    Index page, listing all available
                               questions.

  `/polls/5/`                  Detail view for Question 5, allowing the
                               user to vote.

  `/polls/5/vote/`             The POST target for submitting a vote.

  `/polls/5/results/`          Displays the results (vote counts) for
                               Question 5.

  `/admin/`                    The administrative interface for managing
                               polls.
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## CORE IMPLEMENTATION NOTES

### 1. Atomic Voting (`F()` Expression)

The vote view uses the `F()` object to ensure data integrity during
concurrent updates:

``` python
selected_choice.votes = F('votes') + 1
selected_choice.save()
```

This prevents race conditions by instructing the database to perform the
increment directly, ensuring no votes are lost if multiple users submit
simultaneously.

### 2. Form Validation

The vote view leverages a separate `VoteForm` class for validation:

``` python
form = VoteForm(request.POST)
if form.is_valid():
    # ... logic proceeds only if a choice was selected
else:
    # ... render with error_message
```

This pattern isolates the check for a missing choice key from the view
logic, adhering to Django best practices for cleaner validation.

### 3. Admin Customization

The `admin.py` file customizes the admin interface using:

-   `list_display`: Shows fields like `was_published_recently` in the
    change list.
-   `fieldsets`: Groups fields for better organization on the edit page.
-   `inlines`: Allows `Choice` objects to be edited directly within the
    Question editing page.

------------------------------------------------------------------------
