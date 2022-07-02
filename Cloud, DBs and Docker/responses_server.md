AUTH endpoints


POST: auth/register 
body request:
{
  "username": "Gioe",
  "email": "email@email.com",
  "password": "pass"
}
body response:
{
  "msg": "User created"
}


POST: auth/login
body request:
{
  "username": "Gio",
  "password": "pass"
}
body response:
{
  "user": "Gio"
}




-------

USERS endpoints

Get all users
GET: /users/ 
No body request

Body response:
[
  {
    "id": 1,
    "name": "Adam10"
  },
  {
    "id": 2,
    "name": "Mark1"
  },
  {
    "id": 3,
    "name": "Apple5"
  },
  {
    "id": 4,
    "name": "Gio"
  },
  {
    "id": 5,
    "name": "Gioe"
  }
]


Get user by username
GET:  /users/:username -> /users/Adam10

No body request

Body response:
{
  "id": 1,
  "username": "Adam10",
  "email": "adams@gmail.com",
  "password": "password"
}




----------

Leaderboard endpoints

Get leaderboard (ordered) 

GET: /leaderboard/

No body request

Body response:
[
  {
    "username": "Apple5",
    "percentage": 1
  },
  {
    "username": "Mark1",
    "percentage": 0.7
  },
  {
    "username": "Adam10",
    "percentage": 0.5
  },
  {
    "username": "Gio",
    "percentage": 0
  }
]


Update score
PATCH: /leaderboard/


Body request: 
{
  "username": "Adam10",
  "correct": 10,
  "total": 10
}
Body response:
{
  "name": "Adam10",
  "correct": 50,
  "total_quest": 60,
  "time": "2008-01-01T00:00:01.000Z",
  "percentage": 0.8333333333333334
}



Post user to leaderboard:

POST: leaderboard/new

Body request:
{
	"username": "Gioel"
}

Body response:
{
  "msg": "user added"
}




Delete  user from leaderboard:

DELETE: leaderboard/remove

Body request:
{
	"username": "Gio"
}

Body response:
{
	"User removed"
}


Body response negative
{
  "err": "Error deleting user: Error: User does not exist"
}









