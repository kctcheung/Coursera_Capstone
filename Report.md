
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

The first step is to gather a list of restaurant from the area.  All venues are obtained by the API 
```
https://api.foursquare.com/v2/venues/explore
```

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

The id is used for obtaining more detailed information about the venues, the latitude and longitude are for mapping and calcuating the distance between the original restaurant and the alternative restaurant.

Now we need to extract all the restaurants out of the venues.  We cannot assume all restaurants' category will contain the word "Restaurant".  They can also be for example "Steakhouse".  We need to be more careful and look at all the unique categories.

The unique categories returned by FourSquare are 'Neighborhood', 'Plaza', 'Art Gallery', 'Concert Hall',
       'Arts & Crafts Store', 'Sushi Restaurant', 'Gastropub',
       'Vegetarian / Vegan Restaurant', 'Coffee Shop', 'Breakfast Spot',
       'Clothing Store', 'Speakeasy', 'Thai Restaurant',
       'Mediterranean Restaurant', 'Shopping Mall', 'Comic Shop',
       'American Restaurant', 'Hotel', 'Sandwich Place', 'Theater',
       'Burrito Place', 'Pizza Place', 'Brazilian Restaurant', 'Café',
       'Restaurant', 'Movie Theater', 'Gym', 'Record Shop',
       'French Restaurant', 'Bookstore', 'Cosmetics Shop',
       'Sporting Goods Shop', 'Shoe Store', 'Japanese Restaurant',
       'Comedy Club', 'Mexican Restaurant', 'Diner', 'Italian Restaurant',
       'Organic Grocery', 'Dessert Shop', 'Train Station', 'Museum',
       'Cocktail Bar', 'Supermarket', 'Food Truck', 'Beer Bar', 'Bar',
       'Seafood Restaurant', 'Scenic Lookout', 'Spanish Restaurant',
       'Monument / Landmark', 'Middle Eastern Restaurant', 'Brewery',
       'BBQ Joint', 'Spa', 'Park', 'Hostel', 'Farmers Market',
       'Steakhouse', 'Salad Place', 'Aquarium', 'Street Art'

We decided to include the venues with category containing the words 'Restaurant', 'Café', 'Tea Room', 'Breakfast Spot', 'BBQ Joint', 'Steakhouse' or 'Salad Place'

Out of the 100 venues we obtained 34 restaurants:
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
      <td>509bb871e4b09c7ac93f6642</td>
      <td>JaBistro</td>
      <td>Sushi Restaurant</td>
      <td>43.649687</td>
      <td>-79.388090</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4aeb711ef964a52017c221e3</td>
      <td>Vegetarian Haven</td>
      <td>Vegetarian / Vegan Restaurant</td>
      <td>43.656016</td>
      <td>-79.392758</td>
    </tr>
    <tr>
      <th>2</th>
      <td>537773d1498e74a75bb75c1e</td>
      <td>Eggspectation Bell Trinity Square</td>
      <td>Breakfast Spot</td>
      <td>43.653144</td>
      <td>-79.381980</td>
    </tr>
    <tr>
      <th>3</th>
      <td>529612de11d2ab526191ccc9</td>
      <td>Pai</td>
      <td>Thai Restaurant</td>
      <td>43.647923</td>
      <td>-79.388579</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5321f4d9e4b07946702e6e08</td>
      <td>Byblos Toronto</td>
      <td>Mediterranean Restaurant</td>
      <td>43.647615</td>
      <td>-79.388381</td>
    </tr>
    <tr>
      <th>5</th>
      <td>506db1a9e4b0a3f3b31412f0</td>
      <td>Richmond Station</td>
      <td>American Restaurant</td>
      <td>43.651569</td>
      <td>-79.379266</td>
    </tr>
    <tr>
      <th>6</th>
      <td>51755dc7498ece19b7261641</td>
      <td>Banh Mi Boys</td>
      <td>Sandwich Place</td>
      <td>43.659188</td>
      <td>-79.382131</td>
    </tr>
    <tr>
      <th>7</th>
      <td>52ec621e498ec68fa15ee922</td>
      <td>Copacabana Grilled Brazilian</td>
      <td>Brazilian Restaurant</td>
      <td>43.648333</td>
      <td>-79.388151</td>
    </tr>
    <tr>
      <th>8</th>
      <td>514627d1e4b0dba1b85e9ba8</td>
      <td>Dineen Coffee</td>
      <td>Caf\u00e9</td>
      <td>43.650497</td>
      <td>-79.378765</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4ad4c05df964a52059f620e3</td>
      <td>Canoe</td>
      <td>Restaurant</td>
      <td>43.647452</td>
      <td>-79.381320</td>
    </tr>
    <tr>
      <th>10</th>
      <td>55a9c018498e8b05f7f870f1</td>
      <td>Alo</td>
      <td>French Restaurant</td>
      <td>43.648574</td>
      <td>-79.396243</td>
    </tr>
    <tr>
      <th>11</th>
      <td>4edeae9d61af80fe89aa3f54</td>
      <td>Banh Mi Boys</td>
      <td>Sandwich Place</td>
      <td>43.648650</td>
      <td>-79.396859</td>
    </tr>
    <tr>
      <th>12</th>
      <td>56d4d1b3cd1035fe77e1492c</td>
      <td>Page One Cafe</td>
      <td>Caf\u00e9</td>
      <td>43.657772</td>
      <td>-79.376073</td>
    </tr>
    <tr>
      <th>13</th>
      <td>4b2bd898f964a52042bc24e3</td>
      <td>Kinka Izakaya Original</td>
      <td>Japanese Restaurant</td>
      <td>43.660596</td>
      <td>-79.378891</td>
    </tr>
    <tr>
      <th>14</th>
      <td>52138db911d22803b334c641</td>
      <td>Mos Mos Coffee</td>
      <td>Caf\u00e9</td>
      <td>43.648159</td>
      <td>-79.378745</td>
    </tr>
    <tr>
      <th>15</th>
      <td>50427a03e4b08d9f5931f593</td>
      <td>Seven Lives - Tacos y Mariscos</td>
      <td>Mexican Restaurant</td>
      <td>43.654418</td>
      <td>-79.400545</td>
    </tr>
    <tr>
      <th>16</th>
      <td>4ad4c05cf964a5200ff620e3</td>
      <td>Fresh On Spadina</td>
      <td>Vegetarian / Vegan Restaurant</td>
      <td>43.648048</td>
      <td>-79.396008</td>
    </tr>
    <tr>
      <th>17</th>
      <td>4af618daf964a520220122e3</td>
      <td>GEORGE Restaurant</td>
      <td>Restaurant</td>
      <td>43.653346</td>
      <td>-79.374445</td>
    </tr>
    <tr>
      <th>18</th>
      <td>4d5effa95b276dcbc3b201c6</td>
      <td>TOCA</td>
      <td>Italian Restaurant</td>
      <td>43.645431</td>
      <td>-79.387059</td>
    </tr>
    <tr>
      <th>19</th>
      <td>4a8d5b48f964a520840f20e3</td>
      <td>The Moonbean Cafe</td>
      <td>Caf\u00e9</td>
      <td>43.654147</td>
      <td>-79.400182</td>
    </tr>
    <tr>
      <th>20</th>
      <td>4b49183ff964a520a46526e3</td>
      <td>Terroni</td>
      <td>Italian Restaurant</td>
      <td>43.650927</td>
      <td>-79.375602</td>
    </tr>
    <tr>
      <th>21</th>
      <td>574ad72238fa943556d93b8e</td>
      <td>Gyu-Kaku Japanese BBQ</td>
      <td>Japanese Restaurant</td>
      <td>43.651422</td>
      <td>-79.375047</td>
    </tr>
    <tr>
      <th>22</th>
      <td>51438b33e4b0a40e33fe5e77</td>
      <td>Jimmy's Coffee</td>
      <td>Caf\u00e9</td>
      <td>43.654493</td>
      <td>-79.401311</td>
    </tr>
    <tr>
      <th>23</th>
      <td>51a261ca498ea3d3eaf40876</td>
      <td>FIKA Cafe</td>
      <td>Caf\u00e9</td>
      <td>43.653560</td>
      <td>-79.400402</td>
    </tr>
    <tr>
      <th>24</th>
      <td>4af0c5c1f964a5200fdf21e3</td>
      <td>Rodney's Oyster House</td>
      <td>Seafood Restaurant</td>
      <td>43.644975</td>
      <td>-79.396587</td>
    </tr>
    <tr>
      <th>25</th>
      <td>4fa027ede4b0e4be23b3374e</td>
      <td>Patria</td>
      <td>Spanish Restaurant</td>
      <td>43.645384</td>
      <td>-79.396478</td>
    </tr>
    <tr>
      <th>26</th>
      <td>4b72f550f964a5202e922de3</td>
      <td>Mystic Muffin</td>
      <td>Middle Eastern Restaurant</td>
      <td>43.652484</td>
      <td>-79.372655</td>
    </tr>
    <tr>
      <th>27</th>
      <td>506880f3e8893b5a347c4043</td>
      <td>Triple A Bar (AAA)</td>
      <td>BBQ Joint</td>
      <td>43.651658</td>
      <td>-79.372720</td>
    </tr>
    <tr>
      <th>28</th>
      <td>50b7c53616485cd9efad60d5</td>
      <td>Sukhothai</td>
      <td>Thai Restaurant</td>
      <td>43.648487</td>
      <td>-79.374547</td>
    </tr>
    <tr>
      <th>29</th>
      <td>56437cac498e5a88165d13ce</td>
      <td>Porchetta &amp; Co</td>
      <td>Sandwich Place</td>
      <td>43.644664</td>
      <td>-79.398813</td>
    </tr>
    <tr>
      <th>30</th>
      <td>4b22f410f964a520005124e3</td>
      <td>Jacobs &amp; Co.</td>
      <td>Steakhouse</td>
      <td>43.645339</td>
      <td>-79.398020</td>
    </tr>
    <tr>
      <th>31</th>
      <td>4b2d2ab2f964a52007d024e3</td>
      <td>Hibiscus</td>
      <td>Vegetarian / Vegan Restaurant</td>
      <td>43.655454</td>
      <td>-79.402439</td>
    </tr>
    <tr>
      <th>32</th>
      <td>5346c98a498ed612110d0f60</td>
      <td>iQ Food Co</td>
      <td>Salad Place</td>
      <td>43.642851</td>
      <td>-79.382081</td>
    </tr>
    <tr>
      <th>33</th>
      <td>544fc36c498ea63958331891</td>
      <td>Wilbur Mexicana</td>
      <td>Mexican Restaurant</td>
      <td>43.644810</td>
      <td>-79.398644</td>
    </tr>
  </tbody>
</table>

Next we fill in more detailed information about each restaurant with the API 
```
https://api.foursquare.com/v2/venues/{venue_id}
```

We extract the price tier and rating of each restaurant.  We would also like to obtain a measure of how busy a place is.   Unfortunately the free account does not include the visits count.  As an alternative we used the tipCount property as it should be positively correlated to the visits count.

Our data becomes:
<table border=\"1\" class=\"dataframe\">
  <thead>
    <tr style=\"text-align: right;\">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>categories</th>
      <th>lat</th>
      <th>lng</th>
      <th>tipCount</th>
      <th>visitsCount</th>
      <th>priceTier</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>509bb871e4b09c7ac93f6642</td>
      <td>JaBistro</td>
      <td>Sushi Restaurant</td>
      <td>43.649687</td>
      <td>-79.388090</td>
      <td>71.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>8.8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4aeb711ef964a52017c221e3</td>
      <td>Vegetarian Haven</td>
      <td>Vegetarian / Vegan Restaurant</td>
      <td>43.656016</td>
      <td>-79.392758</td>
      <td>31.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>8.8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>537773d1498e74a75bb75c1e</td>
      <td>Eggspectation Bell Trinity Square</td>
      <td>Breakfast Spot</td>
      <td>43.653144</td>
      <td>-79.381980</td>
      <td>55.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>8.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>529612de11d2ab526191ccc9</td>
      <td>Pai</td>
      <td>Thai Restaurant</td>
      <td>43.647923</td>
      <td>-79.388579</td>
      <td>192.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>9.4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5321f4d9e4b07946702e6e08</td>
      <td>Byblos Toronto</td>
      <td>Mediterranean Restaurant</td>
      <td>43.647615</td>
      <td>-79.388381</td>
      <td>76.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>9.4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>506db1a9e4b0a3f3b31412f0</td>
      <td>Richmond Station</td>
      <td>American Restaurant</td>
      <td>43.651569</td>
      <td>-79.379266</td>
      <td>110.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>9.2</td>
    </tr>
    <tr>
      <th>6</th>
      <td>51755dc7498ece19b7261641</td>
      <td>Banh Mi Boys</td>
      <td>Sandwich Place</td>
      <td>43.659188</td>
      <td>-79.382131</td>
      <td>88.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>9.2</td>
    </tr>
    <tr>
      <th>7</th>
      <td>52ec621e498ec68fa15ee922</td>
      <td>Copacabana Grilled Brazilian</td>
      <td>Brazilian Restaurant</td>
      <td>43.648333</td>
      <td>-79.388151</td>
      <td>47.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>8.7</td>
    </tr>
    <tr>
      <th>8</th>
      <td>514627d1e4b0dba1b85e9ba8</td>
      <td>Dineen Coffee</td>
      <td>Caf\u00e9</td>
      <td>43.650497</td>
      <td>-79.378765</td>
      <td>142.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4ad4c05df964a52059f620e3</td>
      <td>Canoe</td>
      <td>Restaurant</td>
      <td>43.647452</td>
      <td>-79.381320</td>
      <td>74.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>9.1</td>
    </tr>
    <tr>
      <th>10</th>
      <td>55a9c018498e8b05f7f870f1</td>
      <td>Alo</td>
      <td>French Restaurant</td>
      <td>43.648574</td>
      <td>-79.396243</td>
      <td>19.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>9.2</td>
    </tr>
    <tr>
      <th>11</th>
      <td>4edeae9d61af80fe89aa3f54</td>
      <td>Banh Mi Boys</td>
      <td>Sandwich Place</td>
      <td>43.648650</td>
      <td>-79.396859</td>
      <td>185.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>9.2</td>
    </tr>
    <tr>
      <th>12</th>
      <td>56d4d1b3cd1035fe77e1492c</td>
      <td>Page One Cafe</td>
      <td>Caf\u00e9</td>
      <td>43.657772</td>
      <td>-79.376073</td>
      <td>18.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>9.1</td>
    </tr>
    <tr>
      <th>13</th>
      <td>4b2bd898f964a52042bc24e3</td>
      <td>Kinka Izakaya Original</td>
      <td>Japanese Restaurant</td>
      <td>43.660596</td>
      <td>-79.378891</td>
      <td>223.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>9.1</td>
    </tr>
    <tr>
      <th>14</th>
      <td>52138db911d22803b334c641</td>
      <td>Mos Mos Coffee</td>
      <td>Caf\u00e9</td>
      <td>43.648159</td>
      <td>-79.378745</td>
      <td>11.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>8.8</td>
    </tr>
    <tr>
      <th>15</th>
      <td>50427a03e4b08d9f5931f593</td>
      <td>Seven Lives - Tacos y Mariscos</td>
      <td>Mexican Restaurant</td>
      <td>43.654418</td>
      <td>-79.400545</td>
      <td>93.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>9.2</td>
    </tr>
    <tr>
      <th>16</th>
      <td>4ad4c05cf964a5200ff620e3</td>
      <td>Fresh On Spadina</td>
      <td>Vegetarian / Vegan Restaurant</td>
      <td>43.648048</td>
      <td>-79.396008</td>
      <td>128.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>8.8</td>
    </tr>
    <tr>
      <th>17</th>
      <td>4af618daf964a520220122e3</td>
      <td>GEORGE Restaurant</td>
      <td>Restaurant</td>
      <td>43.653346</td>
      <td>-79.374445</td>
      <td>24.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>8.9</td>
    </tr>
    <tr>
      <th>18</th>
      <td>4d5effa95b276dcbc3b201c6</td>
      <td>TOCA</td>
      <td>Italian Restaurant</td>
      <td>43.645431</td>
      <td>-79.387059</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>8.7</td>
    </tr>
    <tr>
      <th>19</th>
      <td>4a8d5b48f964a520840f20e3</td>
      <td>The Moonbean Cafe</td>
      <td>Caf\u00e9</td>
      <td>43.654147</td>
      <td>-79.400182</td>
      <td>62.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>8.9</td>
    </tr>
    <tr>
      <th>20</th>
      <td>4b49183ff964a520a46526e3</td>
      <td>Terroni</td>
      <td>Italian Restaurant</td>
      <td>43.650927</td>
      <td>-79.375602</td>
      <td>92.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>8.8</td>
    </tr>
    <tr>
      <th>21</th>
      <td>574ad72238fa943556d93b8e</td>
      <td>Gyu-Kaku Japanese BBQ</td>
      <td>Japanese Restaurant</td>
      <td>43.651422</td>
      <td>-79.375047</td>
      <td>11.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>8.7</td>
    </tr>
    <tr>
      <th>22</th>
      <td>51438b33e4b0a40e33fe5e77</td>
      <td>Jimmy's Coffee</td>
      <td>Caf\u00e9</td>
      <td>43.654493</td>
      <td>-79.401311</td>
      <td>54.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>8.8</td>
    </tr>
    <tr>
      <th>23</th>
      <td>51a261ca498ea3d3eaf40876</td>
      <td>FIKA Cafe</td>
      <td>Caf\u00e9</td>
      <td>43.653560</td>
      <td>-79.400402</td>
      <td>47.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>8.7</td>
    </tr>
    <tr>
      <th>24</th>
      <td>4af0c5c1f964a5200fdf21e3</td>
      <td>Rodney's Oyster House</td>
      <td>Seafood Restaurant</td>
      <td>43.644975</td>
      <td>-79.396587</td>
      <td>80.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>9.1</td>
    </tr>
    <tr>
      <th>25</th>
      <td>4fa027ede4b0e4be23b3374e</td>
      <td>Patria</td>
      <td>Spanish Restaurant</td>
      <td>43.645384</td>
      <td>-79.396478</td>
      <td>63.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>8.9</td>
    </tr>
    <tr>
      <th>26</th>
      <td>4b72f550f964a5202e922de3</td>
      <td>Mystic Muffin</td>
      <td>Middle Eastern Restaurant</td>
      <td>43.652484</td>
      <td>-79.372655</td>
      <td>25.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>8.8</td>
    </tr>
    <tr>
      <th>27</th>
      <td>506880f3e8893b5a347c4043</td>
      <td>Triple A Bar (AAA)</td>
      <td>BBQ Joint</td>
      <td>43.651658</td>
      <td>-79.372720</td>
      <td>59.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>8.8</td>
    </tr>
    <tr>
      <th>28</th>
      <td>50b7c53616485cd9efad60d5</td>
      <td>Sukhothai</td>
      <td>Thai Restaurant</td>
      <td>43.648487</td>
      <td>-79.374547</td>
      <td>85.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>8.7</td>
    </tr>
    <tr>
      <th>29</th>
      <td>56437cac498e5a88165d13ce</td>
      <td>Porchetta &amp; Co</td>
      <td>Sandwich Place</td>
      <td>43.644664</td>
      <td>-79.398813</td>
      <td>27.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>9.4</td>
    </tr>
    <tr>
      <th>30</th>
      <td>4b22f410f964a520005124e3</td>
      <td>Jacobs &amp; Co.</td>
      <td>Steakhouse</td>
      <td>43.645339</td>
      <td>-79.398020</td>
      <td>69.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>8.9</td>
    </tr>
    <tr>
      <th>31</th>
      <td>4b2d2ab2f964a52007d024e3</td>
      <td>Hibiscus</td>
      <td>Vegetarian / Vegan Restaurant</td>
      <td>43.655454</td>
      <td>-79.402439</td>
      <td>41.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>8.8</td>
    </tr>
    <tr>
      <th>32</th>
      <td>5346c98a498ed612110d0f60</td>
      <td>iQ Food Co</td>
      <td>Salad Place</td>
      <td>43.642851</td>
      <td>-79.382081</td>
      <td>15.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>8.9</td>
    </tr>
    <tr>
      <th>33</th>
      <td>544fc36c498ea63958331891</td>
      <td>Wilbur Mexicana</td>
      <td>Mexican Restaurant</td>
      <td>43.644810</td>
      <td>-79.398644</td>
      <td>71.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>9.1</td>
    </tr>
  </tbody>
</table>

**Note** Since the free account can only make 50 of that API calls the data is stored in a csv file.
Since a free account can only make 50 of those call per day.

Before we can do clustering we need to one-hot encode the categorical features (category) and normalize the numeric ones (tip count, price tier and rating)

Pandas get_dummies was used to do the one-hot encoding and sklearn.preprocessing StandardScaler was used to normalize the numeric features.

The first 5 rows of the one-hot encoded data looked like:
<table border=\"1\" class=\"dataframe\">
  <thead>
    <tr style=\"text-align: right;\">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>categories</th>
      <th>lat</th>
      <th>lng</th>
      <th>tipCount</th>
      <th>priceTier</th>
      <th>rating</th>
      <th>American Restaurant</th>
      <th>BBQ Joint</th>
      <th>...</th>
      <th>Middle Eastern Restaurant</th>
      <th>Restaurant</th>
      <th>Salad Place</th>
      <th>Sandwich Place</th>
      <th>Seafood Restaurant</th>
      <th>Spanish Restaurant</th>
      <th>Steakhouse</th>
      <th>Sushi Restaurant</th>
      <th>Thai Restaurant</th>
      <th>Vegetarian / Vegan Restaurant</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>509bb871e4b09c7ac93f6642</td>
      <td>JaBistro</td>
      <td>Sushi Restaurant</td>
      <td>43.649687</td>
      <td>-79.388090</td>
      <td>71.0</td>
      <td>3.0</td>
      <td>8.8</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4aeb711ef964a52017c221e3</td>
      <td>Vegetarian Haven</td>
      <td>Vegetarian / Vegan Restaurant</td>
      <td>43.656016</td>
      <td>-79.392758</td>
      <td>31.0</td>
      <td>2.0</td>
      <td>8.8</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>537773d1498e74a75bb75c1e</td>
      <td>Eggspectation Bell Trinity Square</td>
      <td>Breakfast Spot</td>
      <td>43.653144</td>
      <td>-79.381980</td>
      <td>55.0</td>
      <td>1.0</td>
      <td>8.7</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>529612de11d2ab526191ccc9</td>
      <td>Pai</td>
      <td>Thai Restaurant</td>
      <td>43.647923</td>
      <td>-79.388579</td>
      <td>192.0</td>
      <td>2.0</td>
      <td>9.4</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5321f4d9e4b07946702e6e08</td>
      <td>Byblos Toronto</td>
      <td>Mediterranean Restaurant</td>
      <td>43.647615</td>
      <td>-79.388381</td>
      <td>76.0</td>
      <td>2.0</td>
      <td>9.4</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

The clustering algorithm used will be K-Means because:
- It is Easy to implement
- With a large number of variables, K-Means may be computationally faster than hierarchical clustering (if K is small).
- K-Means may produce higher clusters than hierarchical clustering

K-Means requires the number of clusters predefined.  Elbow method gives us an idea on what a good 𝑘 number of clusters would be based on the sum of squared distance (SSE) between data points and their assigned clusters' centroids. We pick 𝑘 at the spot where SSE starts to flatten out and forming an elbow. We'll use the geyser dataset and evaluate SSE for different values of 𝑘 and see where the curve might form an elbow and flatten out. 

The plot of k and SSE was
