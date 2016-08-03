## Culture Counts Api

### Login
Login in culture counts api requires **apikey**.

**apikey** is unique key generated by an organisation for specific user. 
Contact your Organisatin to get your **apikey**.

  - **Request parameters**
  
    - **url** : http://localhost:8000/api/auth/login/
    -  **apikey** : generated by the organisation . for ex : 1dce26ed
  
  - **Example of Login via api**
  
  ```PHP
  $apikey = "1dce26ed";
  $ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, "http://localhost:8000/api/auth/login/");
	curl_setopt($ch, CURLOPT_USERAGENT,'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/32.0.1700.107 Chrome/32.0.1700.107 Safari/537.36');
	curl_setopt($ch, CURLOPT_POST, true);
	curl_setopt($ch, CURLOPT_POSTFIELDS, "apikey=".$apikey);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_COOKIESESSION, true);
	curl_setopt($ch, CURLOPT_COOKIEJAR, 'cookie.txt');  //could be empty, but cause problems on some hosts
	curl_setopt($ch, CURLOPT_COOKIEFILE, '/var/www/ip4.x/file/tmp');  //could be empty, but cause problems on some hosts
	$response = json_decode(curl_exec($ch));
	
	if (curl_error($ch)) {
	    echo curl_error($ch);
	}
  ```
  
  - **Response of Login**
  
  ```JSON
  {
    "csrftoken": "eF20IMFIQD0RNgIgJT7rt8Q7iYbkTKZH",
    "demographics": {},
    "user": {
        "id": 194307,
        "client_id": "",
        "created": "2016-08-02T02:55:30.597971Z",
        "modified": "2016-08-03T12:07:45.310734Z",
        "tombstate": null,
        "email": "bot@culturecounts.cc",
        "is_active": true,
        "roles": [
            "Admin"
        ],
        "names": [
            "ccbot",
            ""
        ],
        "settings": {}
    },
    "auth": "logged_in"
  }
  ```
- **csrftoken** : Required to make further request to server.
    - **demographics** : dont know
    - **email** : email id for user.
    - **is_active** : check if user is enabled or disabled.
    - **roles** : role of user.
    - **names** : list containing first name and last name.
    - **settings** : different settings.
    - **auth** : status of user login.
    
    
### Get all Evaluations
  - **Request parameters**
  
    - **url** : http://localhost:8000/api/evaluation/
    -  **header** : {'X-CSRFToken' : csrf token in login response}
  
  - **Example**
  
  ```PHP
  curl_setopt($ch, CURLOPT_URL, "http://localhost:8000/api/evaluation/");
	curl_setopt($ch, CURLOPT_POST, false);
	curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    	'X-CSRFToken: "eF20IMFIQD0RNgIgJT7rt8Q7iYbkTKZH",
    ));
	$response = curl_exec($ch);
	
	if (curl_error($_SESSION['ch'])) {
	    echo curl_error($ch);
	}
  ```
  
  - **Response**
  
  ```JSON
  {
    "count": 1,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": 75,
            "has_shares": true,
            "no_of_responses": 1009,
            "tombstate": null,
            "client_id": "",
            "created": "2015-03-04T16:00:00Z",
            "modified": "2016-08-03T06:38:47.430266Z",
            "name": "Glasgow"
        }
    ]
  }
  ```
- **id** : evaluation id.
    - **has_shares** : If evaluation is shared with users.
    - **no_of_responses** : Total number of survey responses.
    - **name** : Name of evaluation.



Further examples are explained in php folder of code.
To use this code enter your api key in **api-key.txt**.

Include CCapi.php in your code and then you can request CC server like:
  ```PHP
  $response = requestCC('http://localhost:8000/api/evaluation/', "get");
  ```
  **Parameters**
  - **url** : request url
  - **request_type** : get or post.
  - **params** : associative array of data ex ["id"=>75, "name"=> "glasgow"].
  - **ccapikey** : If you want to send api key rather than adding it on api-key.txt file.


