# lora-chirpstack-as923




**Note:** 請確認安裝npm（node.js）、Docker並可執行docker-compose v3.7。

## 運行步驟

1. 切換目錄至chirpstack-lora此目錄
```
cd chirpstack-lora
```

2. 執行以下指令 (如果已經有請略過或將container stop&rm 後docker remove mtrnet)。mtrnet 可以置換成你的名稱；目的是在於在docker container 與 container之間，再增加新的container後能在同一個網段中相互溝通，只是要在你的file 中service加上network，如同範例撰寫的。
```
sudo docker network create --subnet=172.19.0.0/16 mtrnet
```

3. 執行docker-compose指令，你將看到lora server 會運行
```
sudo docker-compose up
```

**Note:** during the startup of services, it's OKAY to see the following errors:

* ping database error, will retry in 2s: dial tcp 172.20.0.4:5432: connect: connection refused
* ping database error, will retry in 2s: pq: the database system is starting up

After all the components have been initialized and started, you should be able to open http://YOUR_HOST:8080/ in your browser. E.g. http://localhost:8080/

4. API 位置在http://YOUR_HOST:8080/api

5. 請確認部署機器的port號是否打開能夠存取相對應的服務？（請見docker-compose-env）

6. 嘗試增加自己的container，像是server、front-end； 


## Configuration

The ChirpStack stack components components are pre-configured to work with the provided
`docker-compose.yml` file and defaults to the AS_923 LoRaWAN band. Please refer
to the `configuration/chirpstack-network-server/examples` directory for more configuration
examples.

## Device uplink topic

```
application/{application_id}/device/{device_id}/rx

or

application/#

```

## Device payload

```
{
  "applicationID": "1",
  "applicationName": "R718WA_ABP_Test",
  "deviceName": "R718WA_Dumpling",
  "devEUI": "00137a10000081cb",
  "rxInfo": [
      {
      "gatewayID": "000080029c2b2a47",
      "uplinkID": "14510624-5d24-4600-8624-5ee3e940304b",
      "name": "GemTekGW01",
      "rssi": -121,
      "loRaSNR": -4.5,
      "location": {
        "latitude": 0,
        "longitude": 0,
        "altitude": 0
       }
      }
   ],
     "txInfo": {
               "frequency": 923200000,
               "dr": 2
     },
       "adr": true,
       "fCnt": 72,
       "fPort": 6,
       "data": "ATIBJAAAAAAAAAA=",
       "object": {
        "isLeak": false,
        "voltage": 3.6,
        "reportTime": 155555555
       }
  }
```
