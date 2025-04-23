#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1
#define START_PIN 6
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Game Variables
int velocity = 500, vchange = -20, error_spot = -1, beeplong = 160;
const int MAX_LEVEL = 99, BUZZER = 12;
int sequence[MAX_LEVEL], your_sequence[MAX_LEVEL], level = 0;
bool game_started = false, first_attempt = true;

// Setup
void setup() {
    for (int i = A0; i <= A3; i++) pinMode(i, INPUT);
    pinMode(6, INPUT); // Start switch changed to A6
    for (int i = 2; i <= 5; i++) pinMode(i, OUTPUT), digitalWrite(i, LOW);
    pinMode(BUZZER, OUTPUT);
    randomSeed(analogRead(A5)); // Improved random seed

    if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
        while(1);
    }
    display.clearDisplay();
    display.setTextSize(2);
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(20, 20);
    display.print("Start");
    display.display();
    delay(2000);
    show_number(0);
}

// Play tones
void game_over_tone() {
    int tones[] = {1000, 800, 600, 400, 200};
    for (int i = 0; i < 5; i++) {
        tone(BUZZER, tones[i], 200);
        delay(250);
    }
    noTone(BUZZER);
}

void switch_tone() {
    tone(BUZZER, 1200, 100);
    delay(100);
    noTone(BUZZER);
}

void start_tone() {
    tone(BUZZER, 1500, 200);
    delay(250);
    tone(BUZZER, 1700, 200);
    delay(250);
    noTone(BUZZER);
}

// Show number on OLED
void show_number(int num) {
    display.clearDisplay();
    display.setTextSize(3);
    display.setCursor(40, 20);
    display.print(num);
    display.display();
}

// Main loop
void loop() {
    if (!game_started) {
        show_number(0);
        if (digitalRead(6) == LOW) { // Changed from A4 to A6
            while (digitalRead(6) == LOW);
            delay(100);
            start_tone();
            game_started = true;
            level = 1;
            generate_sequence();
        }
    } else {
        show_number(level);
        if (level == 1) generate_sequence();
        if (digitalRead(6) == LOW || level != 0) show_sequence(), get_sequence(); // Changed from A4 to A6
    }
}

// Show LED sequence
void show_sequence() {
    if (first_attempt) {
        delay(3000);
        first_attempt = false;
    }
    for (int i = 0; i < level; i++) {
        digitalWrite(sequence[i], HIGH);
        switch_tone();
        delay(velocity);
        digitalWrite(sequence[i], LOW);
        delay(200);
    }
}

// Get user input with debounce
void get_sequence() {
    for (int i = 0; i < level; i++) {
        int flag = 0;
        while (!flag) {
            for (int j = 0, pin = 5; j < 4; j++, pin--) {
                if (digitalRead(A0 + j) == LOW) {
                    delay(50);
                    if (digitalRead(A0 + j) == LOW) {
                        digitalWrite(pin, HIGH);
                        switch_tone();
                        your_sequence[i] = pin;
                        flag = 1;
                        delay(200);
                        if (your_sequence[i] != sequence[i]) return error_spot = i, wrong_sequence();
                        digitalWrite(pin, LOW);
                    }
                }
            }
        }
    }
    right_sequence();
}

// Generate a new sequence
void generate_sequence() {
    for (int i = 0; i < MAX_LEVEL; i++) sequence[i] = random(2, 6);
}

// Handle wrong input
void wrong_sequence() {
    for (int i = 0; i < 3; i++) {
        for (int j = 2; j <= 5; j++) digitalWrite(j, HIGH);
        show_number(0);
        game_over_tone();
        delay(250);
        for (int j = 2; j <= 5; j++) digitalWrite(j, LOW);
        show_number(0);
        delay(250);
    }
    level = 0, velocity = 500, error_spot = -1;
    first_attempt = true;

    while (digitalRead(6) == HIGH); // Changed from A4 to A6
    delay(100);
    start_tone();
    game_started = true;
    level = 1;
    generate_sequence();
}

// Handle correct input
void right_sequence() {
    for (int j = 2; j <= 5; j++) digitalWrite(j, LOW);
    delay(250);
    for (int j = 2; j <= 5; j++) digitalWrite(j, HIGH);
    for (int k = 1; k < 500; k++) {
        digitalWrite(BUZZER, HIGH);
        delay(abs(k * sin(1.0 / k)) / 2);
        digitalWrite(BUZZER, LOW);
        delay(abs(k * sin(1.0 / k)) / 2);
    }
    delay(500);
    for (int j = 2; j <= 5; j++) digitalWrite(j, LOW);
    delay(500);
    if (level < MAX_LEVEL) level++;
    velocity += vchange;
}
