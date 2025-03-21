#wow 
repo containing how to make a list in python 


more details about the code 
# Write your code here :-)
from machine import Pin, PWM
import time


servoD = PWM(Pin(15), freq=50)
servoND = PWM(Pin(5), freq=50)
buzzer = PWM(Pin(33))


green_leds = [Pin(16, Pin.OUT), Pin(17, Pin.OUT), Pin(18, Pin.OUT)]
red_leds = [Pin(19, Pin.OUT), Pin(21, Pin.OUT), Pin(22, Pin.OUT)]


stepper_pins = [Pin(12, Pin.OUT), Pin(13, Pin.OUT), Pin(14, Pin.OUT), Pin(27, Pin.OUT)]
stepper_seq = [
    [1, 0, 0, 0], [1, 1, 0, 0], [0, 1, 0, 0], [0, 1, 1, 0],
    [0, 0, 1, 0], [0, 0, 1, 1], [0, 0, 0, 1], [1, 0, 0, 1]
]


def set_leds(red_on):
    for led in green_leds:
        led.value(0 if red_on else 1)
    for led in red_leds:
        led.value(1 if red_on else 0)


def play_ringtone():
    for freq in [1000, 1200, 800, 1500]:
        buzzer.freq(freq)
        buzzer.duty(512)
        time.sleep(0.3)
    buzzer.duty(0)

def step_motor(steps, direction):
    for _ in range(steps):
        for seq in (stepper_seq if direction == 1 else reversed(stepper_seq)):
            for pin, val in zip(stepper_pins, seq):
                pin.value(val)
            time.sleep(0.005)

while True:
    servoD.duty(30)
    servoND.duty(77)
    set_leds(False)
    buzzer.duty(0)
    step_motor(512, 1)
    print("Position 1 - Green")
    time.sleep(3)

    servoD.duty(77)
    servoND.duty(30)
    set_leds(True)
    play_ringtone()
    step_motor(512, -1)
    print("Position 2 - Red")
    time.sleep(3)
