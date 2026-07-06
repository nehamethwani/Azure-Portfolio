+------------------+
                    |  Browser/Postman |
                    +--------+---------+
                             |
                    HTTP GET Request
                             |
                             v
+--------------------------------------------------+
|              Azure Function App                  |
|                                                  |
|  +--------------------------------------------+  |
|  |          HTTP Trigger Function             |  |
|  |               (HelloAPI)                   |  |
|  +--------------------------------------------+  |
+-------------------------+------------------------+
                          |
                          |
                          v
                 +----------------+
                 | Employee Object|
                 |  ID: 101       |
                 |  Name: Neha    |
                 |  Designation   |
                 +----------------+
                          |
                          |
                          v
                 JSON Response
                          |
                          v
        {
          "id": 101,
          "name": "Neha",
          "designation": "SharePoint Technical Analyst"
        }
