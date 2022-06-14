curl --location --request POST ' https://api.bookkeeperai.com/admin/v1/adminuser' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJBdXRoZW50aWNhdGlvbiIsImF1ZCI6InJvYm9hZGp1dG9yLWF1ZGllbmNlIiwicm9sZSI6IlNVUEVSQURNSU4iLCJpc3MiOiJodHRwOi8vMC4wLjAuMDo4MCIsInR5cGUiOiJBQ0NFU1MiLCJleHAiOjE2NTMxMTk1ODMsInVzZXIiOiJ7XCJ1c2VySWRcIjoxMTUsXCJ1c2VyVHlwZVwiOjEwfSIsImlhdCI6MTY1MzAzMzE4M30.1_U3TwRLoanHegX-25OErwAhNiKmAabVtI7sOaUFj7821nqTmqGn4ADiKaRQEqg9h0lPZwaFnzScifGgSs9OhQ' \
--data-raw '{
  "role": "ADMIN",
  "phone": "19529321817",
  "email": "rderbenev+1631211@icerockdev.com",
  "password": "123123",
  "company": [10],
  "firstName": "qui magna voluptate in",
  "secondName": "consequat dolor dolor laborum",
  "lastName": "sunt anim qui consectetur",
  "position": "consequat pariatu",
  "status": "Active"
}'


curl --location --request POST ' https://api.bookkeeperai.com/admin/v1/auth/signin' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data-raw '{
  "email": "qwe@gmail.com",
  "password": "1234561"
}'
