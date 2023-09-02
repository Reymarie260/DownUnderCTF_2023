#  DownUnderCTF 2023 - faraday
## Problem Statement
We've been trying to follow our target Faraday but we don't know where he is.

All we know is that he's somewhere in Victoria and his phone number is +61491578888.

Luckily, we have early access to the GSMA's new location API. Let's see what we can do with this.

The flag is the name of the Victorian town our target is in, in all lowercase with no spaces, surrounded by DUCTF{}. For example, if Faraday was in Swan Hill, the solution would be DUCTF{swanhill}.

## Information

**Point Value**: 100 points

**Category**: OSINT

**Difficulty**: Medium

## Solution

When we enter the API given we notice that to search the location of the number, you have to establish a limit. They ask for coordenates and a radius where the system can search it up and the phone number from Faraday. As the only thing we know is that he is in Victoria, Australia we search on a GPS (or in my case in Google Maps) for the latitude and longitude of Victoria. Also, Australia phone number format is not familiar to me and the system will give me error if its not written correctly, so we search for  it. The format for the phone is +61 04xx xxx xxx.

```json
{
  "device": {
    "phoneNumber": "+61 4 9157 8888"
  },
  "area": {
    "areaType": "Circle",
    "center": {
      "latitude": -36.560389,
      "longitude": 142.828972
    },
    "radius": 200000
  },
  "maxAge": 120
}
```

Response (requested area may not match the area where the network locates the device):

```json
{
  "lastLocationTime": null,
  "verificationResult": "FALSE"
}
```

Now, the maximum radius of an area the API is 200,000 miles. Naturally, that does not cover every part of Victoria, Australia, so we must analyze by sections of Victoria.

Using a mix of Google Maps to search which areas to cover by section and using gps-coordinates.org to get the coordinates, we got near to the area of Glenaroua:

```json
curl -X 'POST' \
  'https://osint-faraday-9e36cbd6acad.2023.ductf.dev/verify' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "device": {
    "phoneNumber": "+61 491 578 888"
  },
  "area": {
    "areaType": "Circle",
    "center": {
      "latitude": -37.1196681,
      "longitude": 144.9609174
    },
    "radius": 200000
  },
  "maxAge": 120
}
'
```
response:
```json
{
  "lastLocationTime": "Sat Sep  2 16:23:59 2023",
  "verificationResult": "TRUE"
}
```

We keep sectioning areas, until we reach to an area where the verification result is TRUE. When that happens, give radius a lesser value and keep repeating process until you reach one town.

```json
{
  "device": {
    "phoneNumber": "+61 491 578 888"
  },
  "area": {
    "areaType": "Circle",
    "center": {
      "latitude": -36.449547,
      "longitude": 146.4312807
    },
    "radius": 2000
  },
  "maxAge": 120
}
```

Response:
```json
{
  "lastLocationTime": "Sat Sep  2 18:06:02 2023",
  "verificationResult": "TRUE"
}
```

At the end, Faraday is in the town of Milawa.

## Flag
DUCTF{milawa}
