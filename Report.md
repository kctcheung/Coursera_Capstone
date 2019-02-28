
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
Sample data with the feature can look like the following.  More relevant features will include location, categories, price and stats
``` json
    {
        "id": "4bb6b9446edc76b0d771311c"
        "name": "Wendy's"
        "contact": {
            "twitter": "wendyscanada"
            "facebook": "132856246727566"
            "facebookUsername": "WendysCanada"
            "facebookName": "Wendy's"
        }
        "location": {
            "crossStreet": "Morningside & Sheppard"
            "lat": 43.80744841934756
            "lng": -79.19905558052072
            "labeledLatLngs": [
                {
                    "label": "display"
                    "lat": 43.80744841934756
                    "lng": -79.19905558052072
                }
            ]
            "cc": "CA"
            "city": "Toronto"
            "state": "ON"
            "country": "Canada"
            "formattedAddress": [
                "Toronto ON"
            ]
        }
        "canonicalUrl": "https://foursquare.com/v/wendys/4bb6b9446edc76b0d771311c"
        "categories": [
            {
                "id": "4bf58dd8d48988d16e941735"
                "name": "Fast Food Restaurant"
                "pluralName": "Fast Food Restaurants"
                "shortName": "Fast Food"
                "icon": {
                    "prefix": "https://ss3.4sqi.net/img/categories_v2/food/fastfood_"
                    "suffix": ".png"
                }
                "primary": true
            }
            {
                "id": "4bf58dd8d48988d16c941735"
                "name": "Burger Joint"
                "pluralName": "Burger Joints"
                "shortName": "Burgers"
                "icon": {
                    "prefix": "https://ss3.4sqi.net/img/categories_v2/food/burger_"
                    "suffix": ".png"
                }
            }
        ]
        "verified": false
        "stats": {
            "tipCount": 0
            "usersCount": 50
            "checkinsCount": 80
            "visitsCount": 83
        }
        "url": "http://www.wendys.ca"
        "price": {
            "tier": 1
            "message": "Cheap"
            "currency": "$"
        }
        "likes": {
            "count": 1
            "groups": [
                {
                    "type": "others"
                    "count": 1
                    "items": [
                        "0": {
                            "id": "11615115"
                            "firstName": "Peter"
                            "lastName": "P"
                            "gender": "male"
                            "photo": {
                                "prefix": "https://fastly.4sqi.net/img/user/"
                                "suffix": "/M3D3AS2IWMR2YPYI.jpg"
                            }
                        }
                    ]
                }
            ]
            "summary": "Peter P"
        }
        "like": false
        "dislike": false
        "ok": false
        "rating": 5.9
        "ratingColor": "FF9600"
        "ratingSignals": 3
        "allowMenuUrlEdit": true
        "beenHere": {
            "count": 0
            "unconfirmedCount": 0
            "marked": false
            "lastCheckinExpiredAt": 0
        }
        "specials": {
            "count": 0
            "items": [
            ]
        }
        "photos": {
            "count": 0
            "groups": [
                {
                    "type": "checkin"
                    "name": "Friends' check-in photos"
                    "count": 0
                    "items": [
                    ]
                }
            ]
            "summary": "No photos"
        }
        "reasons": {
            "count": 0
            "items": [
            ]
        }
        "hereNow": {
            "count": 0
            "summary": "Nobody here"
            "groups": [
            ]
        }
        "createdAt": 1270266180
        "tips": {
            "count": 0
            "groups": [
                {
                    "type": "following"
                    "name": "Tips from people you follow"
                    "count": 0
                    "items": [
                    ]
                }
            ]
        }
        "shortUrl": "http://4sq.com/bIsz0D"
        "timeZone": "America/Toronto"
        "listed": {
            "count": 0
            "groups": [
            ]
        }
        "pageUpdates": {
            "count": 0
            "items": [
            ]
        }
        "inbox": {
            "count": 0
            "items": [
            ]
        }
        "venueChains": [
            {
                "id": "556f46f6bd6a007c77390fa9"
                "bestName": {
                    "name": "Wendy's"
                    "lang": "en"
                }
                "logo": {
                    "prefix": "https://fastly.4sqi.net/img/general/"
                    "suffix": "/212169387__T0QUvJoFuI9kYtwYB2X6ykcecr3d5prcZgZTi4C0OM.jpg"
                }
            }
        ]
        "attributes": {
            "groups": [
                {
                    "type": "price"
                    "name": "Price"
                    "summary": "$"
                    "count": 1
                    "items": [
                        "0": {
                            "displayName": "Price"
                            "displayValue": "$"
                            "priceTier": 1
                        }
                    ]
                }
            ]
        }
    }

}
```
## Methodology
The scope of this project only include the Toronto area.  The same methodology will apply to any location.

The first step is to gather a list of restaurant from the area.  All venues are obtained by the API `https://api.foursquare.com/v2/venues/explore`

The id, name, categories, latitude and longitude are extracted to obtain a dataframe like the following:
<table border=\"1\" class=\"dataframe\">
  <thead>
    <tr style=\"text-align: right;\">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>categories</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5227bb01498e17bf485e6202</td>
      <td>Downtown Toronto</td>
      <td>Neighborhood</td>
      <td>43.653232</td>
      <td>-79.385296</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4ad4c05ef964a520a6f620e3</td>
      <td>Nathan Phillips Square</td>
      <td>Plaza</td>
      <td>43.652270</td>
      <td>-79.383516</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4ad4c05ef964a520daf620e3</td>
      <td>Art Gallery of Ontario</td>
      <td>Art Gallery</td>
      <td>43.654003</td>
      <td>-79.392922</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4ad4c062f964a520e5f720e3</td>
      <td>Four Seasons Centre for the Performing Arts</td>
      <td>Concert Hall</td>
      <td>43.650609</td>
      <td>-79.386280</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4adf3c01f964a5208f7821e3</td>
      <td>Aboveground Art Supplies</td>
      <td>Arts &amp; Crafts Store</td>
      <td>43.652646</td>
      <td>-79.390925</td>
    </tr>
  </tbody>
</table>
