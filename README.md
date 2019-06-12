# MulesfotCodingChallenge

As Part of Contact-Info API , i exposed 4 Resources 

1. composite - POST
2. address - POST, PUT, GET, DELETE
3. communication - POST,  PUT, GET, DELETE
4.  identification - POST, PUT,  GET, DELETE

Below are the URLs to consume.

  http://ms3inc.us-e2.cloudhub.io/api/composite - POST  (application/json) -- for inserting identification,address and communication
  
  a.  http://ms3inc.us-e2.cloudhub.io/api/address - POST  (application/json) for inserting address 
  
  b. http://ms3inc.us-e2.cloudhub.io/api/address/32 - PUT/ID  (application/json) for updating address 
  
  c. http://ms3inc.us-e2.cloudhub.io/api/address/32 - GET/ID   for retrieving address 
  
  d. http://ms3inc.us-e2.cloudhub.io/api/address/32 - DELETE/ID  for deleting address 
  
  e. http://ms3inc.us-e2.cloudhub.io/api/identification - POST  (application/json) for inserting identification
  
  f. http://ms3inc.us-e2.cloudhub.io/api/identification/45 - PUT/ID  (application/json) for updating identification
  
  g. http://ms3inc.us-e2.cloudhub.io/api/identification/45 - GET/ID   for updating identification
  
  h. http://ms3inc.us-e2.cloudhub.io/api/identification/45 - DELETE/ID for deleting identification
  
  i. http://ms3inc.us-e2.cloudhub.io/api/communication - POST(application/json) for inserting communication
  
  j. http://ms3inc.us-e2.cloudhub.io/api/communication/65 - PUT(application/json) for inserting communication
  
  k. http://ms3inc.us-e2.cloudhub.io/api/communication/65 - GET for inserting communication
  
  l. http://ms3inc.us-e2.cloudhub.io/api/communication/65 - DELETE for inserting communication
  
  
  /composite 
  
  Request :
  ```json
  {
  "Identification": {
    "FirstName": "kapil",
    "LastName": "dev",
    "DOB": "06/21/1980",
    "Gender": "M",
    "Title": "Manager"
  },
  "Address": [
    {
      "Type": "home",
      "Number": "1234",
      "Street": "updated blah St",
      "Unit": "1 a",
      "City": "Somewhere",
      "State": "WV",
      "Zipcode": "12345"
    },
    {
      "Type": "apartment",
      "Number": "1234",
      "Street": "blah blah St",
      "Unit": "1 a",
      "City": "Somewhere",
      "State": "WV",
      "Zipcode": "12345"
    },
    
  ],
  "Communication": [
    {
      "Type": "email",
      "Value": "bfe@sample.com",
      "Preferred": "true"
    },
    {
      "Type": "cell",
      "Value": "304-555-8282"
    }
  ]
}
``` 
Response: 
  ```json
{
  "Identification_Id": 79,
  "Address_Id": [
    70,
    71
  ],
  "Communication_Id": [
    64,
    65
  ]
}
```

Using the Id's in response for each object we can update,get,delete that particular from database.

/address: POST
 Request : For now we are allowing to post only one single address object
   ```json
 {
  "Type": "always",
  "Number": "1234",
  "Street": "blah blah St",
  "Unit": "1 a",
  "City": "Somewhere",
  "State": "WV",
  "Zipcode": "12345",
  "IdentificationId":"71"   // having this field in request can lets you know to which identification it can be attached
}
```
Response: 
 ```json
{
"Id": 73
}
```
/address/73 -PUT,GET,DELETE allows for any specific action on that particular 73 address object




/communication: POST allows you to create one single object on communication table in database

Request:
 ```json
{
   
      "Type": "email",
      "Value": "bfe@sample.com",
      "Preferred": "true",
  "IdentificationId":"71"
}
  ```
  
  Response: 
   ```json
   {
"Id": 66
}
   ```
   
/communication/66 PUT  GET DELETE allows you to perform specific action on 66 communication object


/identification: POST

Request:
 ```json
 {
    "FirstName": "kapil",
    "LastName": "dev",
    "DOB": "06/21/1980",
    "Gender": "M",
    "Title": "Manager"
  }
   ```
   
   Response:
    ```json
    {
"Id": 80
}
      ```
   
   /identification/80 PUT  GET DELETE allows you to perform specific action on 80 identification object
 
Coming to Mulesoft Implementation created `ContactInfo.raml` published to exchange .created a new project in anypoint studio and gave the reference to raml in design center (automatically generates sample flows and api-kit router and console).

from there i created  `global.xml , contact-process-data.xml , contact-db-calls.xml` along with dev-properties.yaml and configuration.yaml for database connection.


I tried to implement experience ,process and system pattern. all the db related interactions happens in `contact-db-calls.xml`. I used Database Connectors (select,insert,update,delete) - The reason why i havent used or implemented for bulk insert is `bulk insert doesnt send back auto generated keys when you insert, so you have to use insert to get the id back in response`.

### **Belo Database Related Information is _extremely_ important**:


### Database details:

- To solve the given assignment, I created a PostgreSQL 10.6 database on AWS.
- **Hostname to connect to database**: ms3incdb.cqwrsogyzoi2.us-east-1.rds.amazonaws.com
- **Database instance name**: ms3incdb
- **username**: alerajesh
- **password**: shared seperately in email.
- **port number**: 5432
- This database has three tables:

  - **Table name**: identification
  
    | Column        | type           |
    | ------------- |:-------------:|
    |id          | integer |
    |firstname   | text    |
    |lastname    | text    |
    |dob         | date    |
    |gender      | text    |
    |title       | text    |
    
  - **Table name**: address
  
    | Column        | type           |
    | ------------- |:-------------:|
    |id                     | integer |
    |type                   | text    |
    |number                 | integer |
    |street                 | text    |
    |unit                   | text    |
    |city                   | text    |
    |state                  | text    |
    |zipcode                | text    |
    |identification_id      | integer |
    
  - **Table name**: communication
  
    | Column        | type           |
    | ------------- |:-------------:|
    |id                     | integer |
    |type                   | text    |
    |value                  | text    |
    |preferred              | text    |
    |identification_id      | integer |

- SQL Queries for creating table:


   -identification: 
   ```
         CREATE TABLE IF NOT EXISTS Identification (
        id serial PRIMARY KEY,
        firstname text,
        lastname text,
        dob date,
        gender text,
        title text
          );
   ```
   
   -communication:
   
   ```
   CREATE TABLE Communication (
	id serial PRIMARY KEY,
    type text,
    value text,
    preferred  text,
    Identification_Id integer,    
    FOREIGN KEY (Identification_Id) REFERENCES Identification (id)
      );
   ```
   
   -address:
   ```
    CREATE TABLE Address (
	id serial PRIMARY KEY,
    type text, 
    number integer,
    street text,
    Unit text,
    City text,
    State text,
    zipcode text,
    Identification_Id integer,
    FOREIGN KEY (Identification_Id) REFERENCES Identification (id)
      );
   ```
