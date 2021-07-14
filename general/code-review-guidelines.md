# Code Review Guidelines

## General

### The application or service should be runnable
Make sure reviewer clone, and run the service / application. If it’s not running, never merge it to master.

Or, it’s better if we have Integration Test.

### Linter is passed
You need to define linter for your application / service, and use it as a standard part of your CI/CD.
Never merge code that hasn’t passed the linter.

### The code should be correct
The logic should be correct according to specs or TODO list.

### The code should be defensive

The code should be checked against NULL and invalid values.
You can use assertions or just plain if clause

```python
def add_number(first: int, second: int):
	assert first is not None, “first number should not be None”
	assert second is not None, “second number should not be None”

	return first + second
```

### Unit test is green
Make sure all unit test is green, code coverage is between 60-75 percent is categorised as  [sufficient](https://testing.googleblog.com/2014/07/measuring-coverage-at-google.html) .

### The code should be easy to understand
Can you understand the code *without asking the committer*?

Is the structure clearly defined?

Is the naming on methods and variables readable enough?

### The code should follow clean code principles
You can read the book [Clean Code: A Handbook of Agile Software Craftsmanship Book](https://learning.oreilly.com/library/view/clean-code-a/9780136083238/) and  [this](https://benscabbia.co.uk/programming/clean-code-cheatsheet)  as a summary.

Also, here’s  [some examples](https://github.com/ryanmcdermott/clean-code-javascript) written in JS.

Mostly should follow DRY, KISS, YAGNI.

### The code should be efficient
Is the code use efficient data structures?

Is the code use efficient algorithm?

Is the code have potential Time Bomb issues:

Looks Good To Me (LGTM), but when the time comes, will produce errors or exception.

*  [Shlemiel The Painter Algorithm](https://wiki.c2.com/?ShlemielThePainter) , basically O(n) algorithm, with n large enough and it iterates often, it will become slower.

* Silent error: open a file, but not closing it.

* Running a query against unindexed column.

* StackOverflow error

* And many more…


### The merge request should use template
You should use  [template](https://kata.quip.com/lW7lA6I0JkLY/Pull-Request-Template) for your merge request, it makes the reviewer easier to review the code.

### The code should use “good enough” 3rd Party Library

It should be:
* popular, shown by github stars
* actively maintained
* have unit tests with coverage
* easy to use

## Error Handling

### Error / Exception should be caught, then logged to stdout
Follow guidelines of your programming language and your favourite framework.

If your framework have minimal support on error handling, ditch it.

### API should have proper error code
Common API error code, use error code according to its usage, never use the same error code for a lot of different errors.

Several common error code, message, and reasons:

400
Bad Request
Payload from user is invalid

401
Unauthorized
Invalid auth token, no token, or expired token

403
Forbidden
User is authenticated, but does not have access to resource

404
Not Found
No resource is available at the endpoint

500
Internal server error
App encounters unhandled exceptions, or uncaught errors

### Error message should be meaningful
You need to provide context to each error message you logs.
Mostly write *what your error is*and *provide reasoning*why it’s happening.

*Bad*
“Error on converse”

*Good*
“Error on converse, you need to provide channelID and botID”

*Bad*
“Error on transaction, code: 112”
*Good*
“Error on transaction, code: 112, reason: insufficient fund”

*Bad JSON Response*
500 Internal server error — Written as text

*Good JSON Response — Written as JSON*
{
“status”: “error”,
“message”: “Internal server error”
}

# Comments
### Never comment each line of code, only use if necessary

Example

```javascript
// DON’T DO THIS
function username(fullname) {
    if (!fullname) {
       // throw error
       throw new Error(“no fullname”)
    }
    // check if name is valid
    const isValid = validateName(fullname)

    // return firstname
    return isValid ? username.split(“”)[0] : null
}
```

```javascript
// THIS IS BETTER
function username(fullname) {
    if (!fullname) {
       throw new Error(“no fullname”);
    }

    // check if name is valid
    const isValid = validateName(fullname);

    return isValid ? username.split(“”)[0] : null
}
```

### Never use comment for disabling line(s) of code
```javascript
function validName(name) {
    // console.log(name) DON’T DO THIS!
    return name.length > 3;
}
```

### Remove any print-like code, use logger instead.

# Documentations
### Always document public methods
Public methods should have documentation.

It should explain the intent of the methods, arguments, and return values.

```
// EXAMPLE in Javascript
/**
 * Is configured checks config has been configured or not
 *
 * @param config in Json
 * @returns true if it’s configured, false otherwise
 */
function isConfigured(config: Json): bool {
    return false;
}
```

### Always have README for each service or applications
You can use this  [README](https://kata.quip.com/HfKJAbbcRSa5/Kataai-Repo-Documentation-Template) template.

## Package

Make sure all dependencies needed for running an app / servce is written inside a repository / project and committed.

Otherwise the build won't run or we're late to know if the language is an interpeter one.


## Security
Checks for top  [10 OWASP](https://owasp.org/www-project-top-ten/)

### All data inputs from frontend should be validated
Use trusted ORM and database driver to prevent SQL injection.

Checks for data type conversion from JSON fields to Object fields.
Example:
string to number, etc
