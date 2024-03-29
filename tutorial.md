### @activities 1

## Battlebot - programmering av en enkel fjernkontroll

### Programmer fjernkontroll @unplugged
Vi skal nå programmere en avansert fjernkontroll som styrer battleboten vår.   
Vi skal bruke sensorene til micro:bit for å styre battleboten.  
__Pitch__ - styrer battleboten _fremover_ eller _bakover_  
__Roll__ - styrer om battleboten svinger til _høyre_ eller _venstre_  
__Når ristes__ - starter/stopper battleboten  

## Styring

### Steg 1
For å skru battlebot på eller av skal vi bruke et signal. For eksempel når vi rister på micro:bit. Her kan dere velge hva som skal bety skru av og på selv. 
Lag en variabel _AvPÅ_ 
Bruk ``||input:Når ristes||`` og legg inn en ``||logic:Hvis ellers||``
Hvis ``||variables:AvPå = 0||`` sett AvPå til 1, ellers sett AvPå til 0. 


```blocks
input.onGesture(Gesture.Shake, function () {
    if (AvPå == 0) {
        AvPå = 1
    } else {
        AvPå = 0
    }
})
```

### Steg 2

Vi skal nå hente inn verdiene for __Pitch__ - hvor mye vi tipper microbit fremover eller bakover. Hvis vi tipper den mot oss blir verdien positiv. Hvis vi tipper den fra oss blir den negativ. 
Microbit måler da hvor mange grader den blir tippet. 
Lag en variabel __Hastighet__ og sett den til ``||input:helinig fremover - bakover||`` 
Plasser den i ``||basic:gjenta for alltid||`` 

```blocks
basic.forever(function () {
    Hastighet = input.rotation(Rotation.Pitch)
})
```

### Steg 3

Vi skal gjøre det samme for å hente en verdi for __Roll__ som skal la oss svinge battlebot.
Lag en variabel __Sving__ og sett den til ``||input:helinig venstre - høyre||`` 

```blocks
basic.forever(function () {
    Pitch = input.rotation(Rotation.Pitch)
    Roll = input.rotation(Rotation.Roll)
})
```

### Steg 4

For å gjøre det enklere å styre er det lurt å visualisere hvordan vi heller microbit på skjermen.
Vi bruker ``||leds:tenn (x) (y)||`` men skal bruke et triks:  
Vi må regne om verdien fra hvordan vi bikker på microbit -45 - 45 til et koordinat på skjermen til mirobit.
Vi regner derfor om hastighet fra lav -45 høy 45 til lav 0 høy 4 og setter det som (y)  
Gjør det samme for sving og sett den verdien for (x)
For å oppdatere skjermen legger vi inn blokken tøm skjerm først i gjenta for alltid.
```blocks
basic.forever(function () {
    basic.clearScreen()
    Pitch = input.rotation(Rotation.Pitch)
    Roll = input.rotation(Rotation.Roll)
    led.plot(Math.map(Sving, -45, 45, 0, 4), Math.map(Hastighet, -45, 45, 0, 4))
})
```
### Steg 5

Det siste vi må gjøre er å sende verdiene til battlebot. Vi bruker radio send verdi:
Send H med verdien til _Hastighet_  
Send A med verdien til _AvPå_  
Send S med verdien til _Sving_  

```blocks
basic.forever(function () {
    basic.clearScreen()
    Pitch = input.rotation(Rotation.Pitch)
    Roll = input.rotation(Rotation.Roll)
    led.plot(Math.map(Sving, -45, 45, 0, 4), Math.map(Hastighet, -45, 45, 0, 4))
    radio.sendValue("H", Hastighet)
    radio.sendValue("A", AvPå)
    radio.sendValue("S", Sving)
})
```

### Steg 6
For at vi skal kunne kommunisere med riktig micro:bit er det viktig at både fjernkontroll og mottaker er på samme radiogruppe.   
Hent ``||radio:sett radiogruppe||`` fra radio og plasser i ved start. Sett radiogruppe til et nummer som ingen av de andre gruppene bruker. 

```blocks
radio.setGroup(xx)
```
