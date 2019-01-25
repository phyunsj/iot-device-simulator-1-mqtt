# IoT Device Simulator (Node-RED) + MQTT Broker

Create a node to generate(or simulate) typical IoT data (Number, String) for a testing purpose. Originally consider [Subflow](https://nodered.org/docs/user-guide/editor/workspace/subflows) but decide to create a new one instead (NOTE : _a subflow cannot contain an instance of itself_). Basic design idea (Add/Remove parameters) was from [`ui_form`](https://github.com/node-red/node-red-dashboard/blob/master/nodes/ui_form.html) from [`node-red-dashboard`](https://github.com/node-red/node-red-dashboard). 

## First, IoT Simulator + Dashboard

<p align="center">
<img src="https://github.com/phyunsj/iot-device-simulator-with-node-red/blob/master/images/iot-simulator-action-text.gif" width="700px"/>
</p>

Example flow 
```
[{"id":"cb83517b.a5e1e","type":"debug","z":"d70af7bd.4d2698","name":"Display msg.payload","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","x":640.3001174926758,"y":112.3333854675293,"wires":[]},{"id":"3be047fd.f44378","type":"inject","z":"d70af7bd.4d2698","name":"msg.payload Trigger","topic":"","payload":"","payloadType":"date","repeat":"2","crontab":"","once":false,"onceDelay":0.1,"x":142.3000030517578,"y":163.53335762023926,"wires":[["deecd2bb.11469"]]},{"id":"deecd2bb.11469","type":"iot-simulator","z":"d70af7bd.4d2698","name":"","options":[{"label":"Temperature","value":"30","range":"10"},{"label":"Air_Quality","value":"40","range":"10"},{"label":"UV_Index","value":"6","range":"5"}],"preOptions":[{"label":"Temperature","value":"30","range":"10"},{"label":"Air_Quality","value":"40","range":"10"}],"timestamp":true,"allinone":false,"optionEdited":true,"x":361.30010986328125,"y":165.40005779266357,"wires":[["cb83517b.a5e1e","c077ade6.ec0a"]]},{"id":"c077ade6.ec0a","type":"function","z":"d70af7bd.4d2698","name":"Payload Split","func":"var msg1 = null\nif ( msg.payload[\"Temperature\"] !== undefined) {\n   msg1 = {};\n   msg1.payload = msg.payload[\"Temperature\"];\n}\n\nvar msg2 = null\nif ( msg.payload[\"Air_Quality\"] !== undefined){\n   msg2 = {};\n   msg2.payload = msg.payload[\"Air_Quality\"];\n}\n\nvar msg3 = null\nif ( msg.payload[\"UV_Index\"] !== undefined){\n   msg3 = {};\n   msg3.payload = msg.payload[\"UV_Index\"];\n}\nreturn [msg1, msg2, msg3];","outputs":3,"noerr":0,"x":524.3000946044922,"y":216.60003185272217,"wires":[["18af797e.554067"],["661f5e00.71c9b4"],["9e42205.31f5ae"]]},{"id":"18af797e.554067","type":"ui_gauge","z":"d70af7bd.4d2698","name":"Temperature","group":"5b9e6fe9.d44e3","order":0,"width":"4","height":"4","gtype":"gage","title":"Temperature","label":"°C","format":"{{value}} °C","min":0,"max":"100","colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":689.3003082275391,"y":169.40005111694336,"wires":[]},{"id":"661f5e00.71c9b4","type":"ui_chart","z":"d70af7bd.4d2698","name":"Air Quality","group":"5b9e6fe9.d44e3","order":1,"width":"4","height":"4","label":"Air Quality","chartType":"line","legend":"false","xformat":"HH:mm:ss","interpolate":"linear","nodata":"","dot":true,"ymin":"","ymax":"","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"3600","cutout":0,"useOneColor":false,"colors":["#1f77b4","#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],"useOldStyle":false,"x":686.3002815246582,"y":226.40008354187012,"wires":[[],[]]},{"id":"9e42205.31f5ae","type":"ui_gauge","z":"d70af7bd.4d2698","name":"","group":"5b9e6fe9.d44e3","order":2,"width":"4","height":"4","gtype":"gage","title":"UV Index","label":"units","format":"{{value}}","min":"1","max":"12","colors":["#00b500","#e6e600","#ca3838"],"seg1":"6","seg2":"9","x":677.5234375,"y":290.9609375,"wires":[]},{"id":"5b9e6fe9.d44e3","type":"ui_group","z":"","name":"SensorData","tab":"a1882c32.fc82a","order":2,"disp":true,"width":"12","collapse":false},{"id":"a1882c32.fc82a","type":"ui_tab","z":"","name":"Dashboard","icon":"dashboard","order":1}]
```

## IoT Simulator + [flespi MQTT Broker](https://flespi.com/mqtt-broker)

- Install `mqtt-plus` (MQTT over websocket),

```
git clone https://github.com/btsimonh/node-red-contrib-mqtt-plus
cd ~/.node-red
npm install <download>/node-red-contrib-mqtt-plus
```

- Publisher Topic `ny-10001/sensor1` from Node-RED MQTT output node
- Subscriber Topic `ny-10001/#` (use  `#` multi-level wildcard or `+` single-level wildcard)  from flespi MQTT Broker Panel

<p align="center">
<img src="https://github.com/phyunsj/iot-device-simulator-with-node-red/blob/master/images/iot-simulator-mqtt.gif" width="700px"/>
</p>

- Connection Settings from flespi MQTT Broker Panel

<p align="center">
<img src="https://github.com/phyunsj/iot-device-simulator-with-node-red/blob/master/images/mqtt_broker_settings.png" width="500px"/>
</p>

## IoT Simulator Editor

<p align="center">
<img src="https://github.com/phyunsj/iot-device-simulator-with-node-red/blob/master/images/iot-simulator-editor.png" width="500px"/>
</p>

#### Related Posts :

- [Node-RED:Creating Nodes](https://nodered.org/docs/creating-nodes/)
- [flespi MQTT](https://flespi.com/mqtt-broker) (Other options: [cloudMQTT](https://www.cloudmqtt.com/), [HiveMQ](https://www.hivemq.com/) )
- [How to connect ESP8266 to MQTT broker](https://medium.com/@flespi/how-to-connect-esp8266-to-secure-mqtt-broker-know-it-all-and-get-it-done-approach-c33b94f37d88)
- [node-red-contrib-mqtt-plus](https://github.com/btsimonh/node-red-contrib-mqtt-plus)

#### See Also :

- [AWS IoT Device Simulator](https://aws.amazon.com/answers/iot/iot-device-simulator/)
- [LOSANT IoT Device Simulator](https://docs.losant.com/devices/simulator/)
