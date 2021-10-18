#include <Artnet.h>
ArtnetReceiver artnet;

// FastLED
#define NUM_LEDS 1
CRGB leds[NUM_LEDS];
const uint8_t PIN_LED_DATA = 3;

void setup() {
    // setup Ethernet/WiFi...

    // setup FastLED
    FastLED.addLeds<NEOPIXEL, PIN_LED>(&leds, NUM_LEDS);

    artnet.begin();
    // if Artnet packet comes to this universe, forward them to fastled directly
    artnet.forward(universe, leds, NUM_LEDS);

    // this can be achieved manually as follows
    // artnet.subscribe(universe, [](uint8_t* data, uint16_t size)
    // {
    //     // artnet data size per packet is 512 max
    //     // so there is max 170 pixel per packet (per universe)
    //     for (size_t pixel = 0; pixel < NUM_LEDS; ++pixel)
    //     {
    //         size_t idx = pixel * 3;
    //         leds[pixel].r = data[idx + 0];
    //         leds[pixel].g = data[idx + 1];
    //         leds[pixel].b = data[idx + 2];
    //     }
    // });
}

void loop() {
    artnet.parse(); // check if artnet packet has come and execute callback
    FastLED.show();
}
//soy nuevo me. Gustaría que pudieran ayudarme a comprender esto
