```shell
curl -X 'POST' 'https://api.denkers.co/queue/' 
	-H 'accept: application/json' 
	-H 'x-token: 6b1f9eeb7e3a1f6e3b3f1a2c5a7d9e1b' 
	-H 'Content-Type: application/json' 
	-d '{ 
			"user_id": 1, 
			"raw_text": "DV Elaine size 11 in Rose Gold", 
			"options": {
				"object_type": "shoes-for-sale"
			}, 
			"properties": {}, 
			"images": [] 
			} 
		}'

curl -X 'POST' 'https://api.denkers.co/queue/' -H 'accept: application/json' -H 'x-token: 6b1f9eeb7e3a1f6e3b3f1a2c5a7d9e1b' -H 'Content-Type: application/json' -d '{ "user_id": 1, "raw_text": "DV Elaine size 11 in Rose Gold", "options": {"object_type": "shoes-for-sale"}, "properties": {}, "images": [] }'

# Response
{
	"detail":
		[
			{
				"type":"json_invalid",
				"loc":[
					"body",141
				],
				"msg":"JSON decode error",
				"input":{},
				"ctx":{"error":"Extra data"}
			}
		]
	}

{"detail":[{"type":"json_invalid","loc":["body",141],"msg":"JSON decode error","input":{},"ctx":{"error":"Extra data"}}]}
```