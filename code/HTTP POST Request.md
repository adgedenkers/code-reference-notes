
```python

text = "dolche vita Elaine size 12 womens"
text = "Hoka M Bondi 8 size 11 and a half d"
text = "New Balance WXNRGBK womens size 11 739655347597" # USE
text = "New Balance WXNRGBK womens size 11"              # USE
text = "New Balance womens size 11 739655347597"         # DO NOT USE
text = "for becky bp 148/91 pulse 78 spo2 99%"
text = "bp 113/87 pulse 74 spo2 99%"
text = "Oxford 8th grade basketball - practice 3:15 to 4:45 at the middle school"
text = "Oxford 8th Grade Basketball - Game vs. Afton (@Afton) - 1730"

# Data To Send?
data = {
	"user_id": "1",
	"raw_text": f"{text}",
	"status": "received"
}

endpoint = "/queue/"

# ------------------------------------

root_url = "https://api.denkers.co"
url = f"{root_url}{endpoint}"

headers={
	"user_id": "1",
	"x-token": "6b1f9eeb7e3a1f6e3b3f1a2c5a7d9e1b"
}

response = requests.post(
	headers=headers,
    url='https://api.denkers.co/',
    json=data
)
```
