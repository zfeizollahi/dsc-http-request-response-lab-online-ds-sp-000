
# HTTP Request/Response Cycle - Lab

## Introduction 

In this lab, we'll make use of the `requests` module commands and properties seen in the previous lesson, to extract information for a web service called **"Open Notify"** to access NASA's space data. 

## Objectives

You will be able to:

* Understand and explain the HTTP Request/Response cycle
* Make http requests in Python using the ‘requests’ library

## Open Notify 

[Open Notify](http://open-notify.org/)  is an an open source project to provide a simple programming interface for some of NASA’s awesome data. This takes live raw data from NASA's systems and turn them into APIs related to space and spacecraft. We can access following information from open notify. 

* Current Location of the International Space Station

* Overhead Pass Predictions for the International Space Station

* Number of People in Space
    
### API endpoints

OpenNotify has several API endpoints. 
>An endpoint is a server route that is used to retrieve different data from the API. 

For example, the `/comments` endpoint on the Reddit API might retrieve information about comments, whereas the `/users` endpoint might retrieve data about users. To access them, you would add the endpoint to the base url of the API.

For the OpenNotify API, we have following end points 

1. Current Location of the International Space Station `/iss-now.json`
2. Overhead Pass Predictions for the International Space Station `/iss-pass.json`    
3. Number of People in Space `/astros.json`

The json extension simple tells us that the data is being returned in a JSON format.

In this lab, we'll be querying a this API to retrieve live data about the International Space Station (ISS). Details on OpenNofity , endpoints, syntax and services it offers can be viewed [Here](http://open-notify.org/Open-Notify-API/)

![](images/iss.jpg)

### Current location of International Space Station

The first endpoint we'll look at on OpenNotify is the` iss-now.json` endpoint (current location of international space station). This endpoint gets the current latitude and longitude of the International Space Station.  Perform following tasks 
* Make a get request to get the latest position of the international space station from the opennotify api's `iss-now` endpoint at http://api.open-notify.org/iss-now.json
* Check the status code of the response
* Interpret the returned code


```python
# You Code Here
import requests
r = requests.get('http://api.open-notify.org/iss-now.json')
r.status_code == requests.codes.ok
```




    True




```python
# Your comments 
r.text
```




    '{"message": "success", "iss_position": {"longitude": "116.2795", "latitude": "-38.7472"}, "timestamp": 1568681574}'



It's currently off the coast of Australia. (Kinda of far out to say "off the coast" but it's the closest landmark)

* Print the contents of the response and identify its current location


```python
# You Code Here
```


```python
# Interpret your results using the API
```

### Check the next pass of International space station for a given location

Let's repeat the above for the second endpoint `iss-pass.json`. This end point is used to query the next pass of the space station on a given location. Let's just run as above and record your observations.


```python
# You Code Here
r_pass = requests.get('http://api.open-notify.org/iss-pass.json')
r_pass.status_code == requests.codes.ok
```




    False




```python
# Your comments 
r_pass.status_code
```




    400



So clearly there is something wrong as we had a 400 response. This is how you should always test your responses for validity. 

if we look at the documentation for the OpenNotify API, we see that the ISS Pass endpoint requires two parameters.

> The ISS Pass endpoint returns when the ISS will next pass over a given location on earth. In order to compute this, we need to pass the coordinates of the location to the API. We do this by passing two parameters -- latitude and longitude.

We can do this by adding an optional keyword argument, params, to our request. In this case, there are two parameters we need to pass:

* lat -- The latitude of the location we want.
* lon -- The longitude of the location we want.

Perform the following tasks :
* Set parameters to reflect the lat and long of New York  (40.71, -74)
* Send a get request to OpenNotify passing in the lat long parameters as k:v pairs in a dictionary
* Check the status code and interpret
* Print the header information and the returned content


```python
# You Code Here
parameters = {"lat": 40.71, "lon": -74}
r_pass = requests.get('http://api.open-notify.org/iss-pass.json',params=parameters)
r_pass.status_code == requests.codes.ok
```




    True




```python
# Check the API and interpret your results - when will ISS pass over NEW York next ?
r_pass.headers
```




    {'Server': 'nginx/1.10.3', 'Date': 'Tue, 17 Sep 2019 00:58:11 GMT', 'Content-Type': 'application/json', 'Content-Length': '519', 'Connection': 'keep-alive', 'Via': '1.1 vegur'}



when will ISS pass over NEW York next ? Unix timestamp 1568684071
GMT: Tuesday, September 17, 2019 1:34:31 AM


```python
print(r_pass.text)
```

    {
      "message": "success", 
      "request": {
        "altitude": 100, 
        "datetime": 1568681891, 
        "latitude": 40.71, 
        "longitude": -74.0, 
        "passes": 5
      }, 
      "response": [
        {
          "duration": 647, 
          "risetime": 1568684071
        }, 
        {
          "duration": 601, 
          "risetime": 1568689917
        }, 
        {
          "duration": 553, 
          "risetime": 1568695803
        }, 
        {
          "duration": 607, 
          "risetime": 1568701637
        }, 
        {
          "duration": 648, 
          "risetime": 1568707438
        }
      ]
    }
    


### Finding the number of people in space

OpenNotify has one more API endpoint, `/astros.json`. It tells you how many people are currently in space. The format of the responses can be studied [HERE](http://open-notify.org/Open-Notify-API/People-In-Space/).

Read the above documentation and perform following tasks:

* Get the response from astros.json endpoint
* Count how many people are currently in space
* List the names of people currently in space.


```python
# You Code Here
r_astros = requests.get('http://api.open-notify.org//astros.json')
r_astros.status_code == requests.codes.ok
```




    True




```python
# Interpret the Results - How many people are in space and what are their names 
r_astros.headers
```




    {'Server': 'nginx/1.10.3', 'Date': 'Tue, 17 Sep 2019 01:00:44 GMT', 'Content-Type': 'application/json', 'Content-Length': '312', 'Connection': 'keep-alive', 'access-control-allow-origin': '*'}




```python
print(r_astros.json())
```

    {'message': 'success', 'people': [{'name': 'Alexey Ovchinin', 'craft': 'ISS'}, {'name': 'Nick Hague', 'craft': 'ISS'}, {'name': 'Christina Koch', 'craft': 'ISS'}, {'name': 'Alexander Skvortsov', 'craft': 'ISS'}, {'name': 'Luca Parmitano', 'craft': 'ISS'}, {'name': 'Andrew Morgan', 'craft': 'ISS'}], 'number': 6}



```python
len(r_astros.json()['people'])
```




    6



## Summary 

In this lesson we saw how we can use request and response methods to query an Open API. We also saw how to look at the contents returned with the API calls and how to parse them. Next, we'll look at connecting to APIs which are not OPEN, i.e. we would need to pass in some authentication information and filter the results. 
