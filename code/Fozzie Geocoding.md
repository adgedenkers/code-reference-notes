# Fozzie - Geocoding Functions

## Get Lat/Lon from an Address
```python
"""
Geocode Address Script
=======================
This script retrieves latitude and longitude for a given address using the OpenStreetMap Nominatim API.

Functions:
----------
- get_lat_lon(address): Fetches latitude and longitude for the input address.

Example Usage:
--------------
    from geocode_address import get_lat_lon
    lat, lon = get_lat_lon('304 Cosen Road, Oxford, NY 13830')
    if lat and lon:
        print(f'Coordinates: Latitude {lat}, Longitude {lon}')

"""

import requests

def get_lat_lon(address):
    """Fetch latitude and longitude for a given address using OpenStreetMap Nominatim API.

    Args:
        address (str): The address to geocode.

    Returns:
        tuple: Latitude and longitude as strings, or (None, None) if not found.
    """
    url = 'https://nominatim.openstreetmap.org/search'
    params = {
        'q': address,
        'format': 'json',
        'limit': 1
    }
    
    try:
        response = requests.get(url, params=params)
        response.raise_for_status()
        data = response.json()
        
        if data:
            lat = data[0]['lat']
            lon = data[0]['lon']
            return lat, lon
        else:
            print('Address not found.')
            return None, None
    
    except requests.RequestException as e:
        print(f"Error fetching coordinates: {e}")
        return None, None


if __name__ == "__main__":
    address = '304 Cosen Road, Oxford, NY 13830'
    lat, lon = get_lat_lon(address)
    if lat and lon:
        print(f'Latitude: {lat}, Longitude: {lon}')

```


## Get Address from Lat/Lon
```python
import requests

def get_address(lat, lon):
    """Fetch the address for given latitude and longitude using OpenStreetMap Nominatim API.

    Args:
        lat (float): Latitude coordinate.
        lon (float): Longitude coordinate.

    Returns:
        str: Address as a string, or None if not found.
    """
    url = 'https://nominatim.openstreetmap.org/reverse'
    params = {
        'lat': lat,
        'lon': lon,
        'format': 'json'
    }
    
    try:
        response = requests.get(url, params=params)
        response.raise_for_status()
        data = response.json()
        
        if 'display_name' in data:
            return data['display_name']
        else:
            print('Address not found.')
            return None
    
    except requests.RequestException as e:
        print(f"Error fetching address: {e}")
        return None

if __name__ == "__main__":
    lat, lon = 42.4406, -75.5907  # Example coordinates
    address = get_address(lat, lon)
    if address:
        print(f'Address: {address}')

```