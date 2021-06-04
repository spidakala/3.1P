#include <JsonParserGeneratorRK.h>
#include <Grove_Temperature_And_Humidity_Sensor.h>

#define DELAY_TIME 30000 // 30 seconds between readings
#define DHT_PIN D3

DHT dht(DHT_PIN);

double temp;
double hum;

void postEventPayload(double temp, double humidity){
    JsonWriterStatic<256> jw;
    {
        JsonWriterAutoObject obj(&jw);
        jw.insertKeyValue("temp", temp);
        jw.insertKeyValue("hum", hum);
    }
    Particle.publish("dht11", jw.getBuffer(), PRIVATE);
}

void setup() {
    dht.begin();
    pinMode(DHT_PIN, INPUT);

}

void loop() {
    temp = dht.getTempCelcius();
    hum = dht.getHumidity();
    postEventPayload(temp, hum);
    delay(DELAY_TIME);
}
