# SAM - Message Flow
> ---> Envio de eventos

> channel-ws<--- Recepcion de mensajes

## Envio de serializable(cablemodem/decodificador/emta/etc)
```
--->{
    "operation": "sendserial",
    "resourceType": "cablemodem",
    "outlet": "1126755555",
    "workOrder": "394055555",
    "action": "swap",
    "serialNumber": "7456121F5F56",
    "equipmentType": "Modem",
    "range": "Cable Modem Wifi",
    "additionalData": {
        "jobNum": "8939473",
        "technicianNum": "25LAD2453",
        "equipTypeCd": "Modem",
        "modelCd": "Cable Modem Wifi",
        "activitySeqNum": "23123",
        "activityId": "213123"
    }
}
```
```
levels<---{
    "type": "CONFIRM_LEVELS",
    "message": {
        "Level_in_range": "YES",
        "equipmentType": "cablemodem",
        "rx": {
            "toOk": 8,
            "toError": 12,
            "fromError": -12,
            "actualValue": "3.5",
            "label": "Recepcion",
            "unit": "dBmV",
            "fromWarn": -10,
            "toWarn": 10,
            "fromOk": -8
        },
        "tx": {
            "fromError": 38,
            "unit": "dBmV",
            "toError": 55,
            "toWarn": 53,
            "fromWarn": 40,
            "fromOk": 42,
            "actualValue": "51.0",
            "toOk": 51,
            "label": "Transmisión"
        }
    },
    "outlet": "1126755555",
    "workOrder": "394055555",
    "serialNumber": "7456121F5F56",
    "timestamp": "2018-07-20T19:18:37.850Z"
}
```

*En simultaneo (en realidad tras en un periodo de tiempo demasiado corto) si los niveles estan en rango, llega el mensaje con la configuracion wifi por defecto.*

```
wifi<----{
    "_id": "5b521ea8819cf148909174ae",
    "type": "ASK_WIFI_CONFIG",
    "message": {
        "ssid": "Fibertel WiFi001"
    },
    "outlet": "1126755555",
    "workOrder": "394055555",
    "serialNumber": "7456121F5F56",
    "timestamp": "2018-07-20T17:39:02.754Z"
}
```
## Envio de la configuracion Wifi a aplicar
```
---->{
    "resourceType": "cablemodem",
    "outlet": "1126755555",
    "workOrder": "394055555",,
    "action": "ADD",
    "equipmentType": "Modem",
    "range": "Cable Modem Wifi",
    "additionalData": {
        "jobNum": "8939473",
        "technicianNum": "25LAD2453",
        "equipTypeCd": "Modem",
        "modelCd": "Cable Modem Wifi",
        "activitySeqNum": "23123",
        "activityId": "213123"
    }
    "operation": "configwifi",
    "serialNumber": "7456121F5F56",
    "message": {
        "ssid": "FibetelWiFi120 5.8GHz",
        "password": "123123123"
    }
}
```
#### Una vez que se pudo aplicar la configuracion wifi, tambien se pasa a instalar el equipo en la plataforma.
```
service<----{
    "_id": "5b684c42819cf148909421d7",
    "type": "CONFIRM_SERVICE",
    "message": {
        "services": [
            "Fibertel 50M",
        ]
    },
    "outlet": "1126755555",
    "workOrder": "394055555",
    "serialNumber": "7456121F5F56",
    "timestamp": "2018-08-06T13:23:17.286Z"
}
```
## Confirmacion de los servicios instalados
#### El campo 'message.result' contiene si el tecnico valida la instalacion o la revierte.
```
---->{
    "_id": "5b57844e819cf1489091f9d9",
    "type": "VALIDATESERVICE",
    "message": {
        "result": "YES" or "NO"
    },
    "outlet": "1126755555",
    "workOrder": "394055555",,
    "serialNumber": "7456121F5F56",
    "timestamp": "2018-07-24T19:55:26.163Z"
}
```
#### Lo cual dependiendo de la respuesta del tecnico puede derivar en dos respuestas finales.
```
success<----{
    "_id": "5b684bad819cf148909421c7",
    "type": "ACTIVATION_SUCCESS",
    "message": {},
    "outlet": "1126755555",
    "workOrder": "394055555",
    "serialNumber": "7456121F5F56",
    "timestamp": "2018-07-24T19:55:58.901Z"
}
```
```
rollback<----{
    "_id": "5b684bad819cf148909421c7",
    "type": "ROLLBACK_SUCCESS",
    "message": {},
    "outlet": "1126755555",
    "workOrder": "394055555",
    "serialNumber": "7456121F5F56",
    "timestamp": "2018-07-24T19:55:58.901Z"
}