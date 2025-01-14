# sigbro-auth
Documentation for the Sigbro AUTH

## How to use Sigbro AUTH API

1. Generate new unique UUID4 and send it to Sigbro Auth API:
```shell
curl -s https://random.api.nxter.org/api/auth/new -d '{"uuid":"F73185DC-D760-4E6E-BAE7-43861601C6F8"}'
# {"result":"ok","msg":"The new record `F73185DC-D760-4E6E-BAE7-43861601C6F8` was saved."}
```

2. Using the same UUID4 generate URL `sigbro://F73185DC-D760-4E6E-BAE7-43861601C6F8` and make QR-code. Put it on your login form. You may use this url as a **deeplink** for mobile devices. Sigbro Mobile app will handle it and open.

3. Start loop with 5 sec delay between iterations and wait until user scan & sign this QR-code. 
```shell
curl -s https://random.api.nxter.org/api/auth/status -d '{"uuid":"F73185DC-D760-4E6E-BAE7-43861601C6F8"}'
# {"result":"wait","msg":"Waiting for the token","accountRS":"","token":"","uuid":""}
```
when user sign this UUID4 you will receive: 
```shell
# {"result":"ok","msg":"The token for the key `F73185DC-D760-4E6E-BAE7-43861601C6F8` was generated.","accountRS":"ARDOR-W5ZG-ZXXA-8DK9-HZXTD","token":"qmd1ea9g7d31lsdtnqgs3d5r0j81flht0q5ooe3k4rj1bom49g4eurmqghmn7706ienm6cem830q9ltgh1e4ct9gqvooar31cl67h1djpiague3jboi1qb6llrr0sfoju434457glki37s5o9l0ijrsvqfthof3n","uuid":"F73185DC-D760-4E6E-BAE7-43861601C6F8"}    
```

4. Now you have user's `token` and `accountRS` and you **must** check the token via Ardor API (you may use any available node): 
```shell
curl -s "https://random.api.nxter.org/ardor?requestType=decodeToken&website=F73185DC-D760-4E6E-BAE7-43861601C6F8&token=qmd1ea9g7d31lsdtnqgs3d5r0j81flht0q5ooe3k4rj1bom49g4eurmqghmn7706ienm6cem830q9ltgh1e4ct9gqvooar31cl67h1djpiague3jboi1qb6llrr0sfoju434457glki37s5o9l0ijrsvqfthof3n"

# {
#     "account": "17719342362906202094",
#     "accountRS": "ARDOR-W5ZG-ZXXA-8DK9-HZXTD",
#     "requestProcessingTime": 1,
#     "timestamp": 105646319,
#     "valid": true
# }
```

5. Now you may use `accountRS` as a login on your site. That is it. 