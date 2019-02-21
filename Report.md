
# Recommendation of Alternative Restaurants

## Introduction
Nowadays it is very common for people to research for a restaurant online before a meal and/or make reservation.  What if the restaurant has no table when you visit or make reservation?  You start the search all over again.  The goal of this project is to provide an algorithm to recommend an alternate restaurants that are the "closest" to your first choice.
## Business Problem
The audience of this project can be any website or application that suggests restaurants.  (e.g. Opentable, Yelps etc)
The following can be the use cases of the algorithm:
* Booking is not available when the user trys to make a reservation online
* No table is available when the user visits and the user wants to look for an alternative restaurant
A naive approach is to just list all the restaurants nearby sorted by the distance.  However it can do better by recommending one that is the "closest" to the original one, in terms of location, type of cuisine and pricing etc.  The smarter the recommendations are the more pleasant the experience will be for the user of the website or application.
## Data
This project will be using data about venues from Foursquare.  Data will include features like location, category/categories of cuisine and pricing.  To limit the amount of API calls and data storage the scope of data will only include restaurants in Toronto area.
Sample data with the feature can look like:
``` json
[
      {
        "id": "5642aef9498e51025cf4a7a5",
        "name": "Mr. Purple",
        "location": {
          "address": "180 Orchard St",
          "crossStreet": "btwn Houston & Stanton St",
          "lat": 40.72173744277209,
          "lng": -73.98800687282996,
          "labeledLatLngs": [
            {
              "label": "display",
              "lat": 40.72173744277209,
              "lng": -73.98800687282996
            }
          ],
          "distance": 8,
          "postalCode": "10002",
          "cc": "US",
          "city": "New York",
          "state": "NY",
          "country": "United States",
          "formattedAddress": [
            "180 Orchard St (btwn Houston & Stanton St)",
            "New York, NY 10002",
            "United States"
          ]
        },
        "categories": [
          {
            "id": "4bf58dd8d48988d1d5941735",
            "name": "Hotel Bar",
            "pluralName": "Hotel Bars",
            "shortName": "Hotel Bar",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/travel/hotel_bar_",
              "suffix": ".png"
            },
            "primary": true
          }
        ],
        "venuePage": {
          "id": "150747252"
        }
      }
    ]
```
