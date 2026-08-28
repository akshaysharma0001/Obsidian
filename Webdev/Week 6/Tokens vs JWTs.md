JWT (JSON Web Token)
There is a problem with using `stateful` tokens.
## Stateful
By stateful here, we mean that we need to store these tokens in a variable right now (and eventually in a database).

## Problem
The problem is that we need to `send a request to the database` every time the user wants to hit an `authenticated endpoint`