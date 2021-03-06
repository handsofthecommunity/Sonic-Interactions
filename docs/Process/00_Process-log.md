# Sonic Interactions Research Process log

#### 17.11.08
## 1. Install PD on Pi #
- Pd doesn’t work on Newest Raspbian OS (stretch)
- Try on Jessie, Jessie lite is recommended (without gui)
- without gui can’t configure wifi without command line
update: Feb 2018 PD nows works on Raspbian Stretch

[https://guitarextended.wordpress.com/2013/01/28/rpi-as-guitar-effects-processor-installing-and-configuring-pd/](https://ccrma.stanford.edu/wiki/Lab1_Making_FX_in_Pd_2013)

[https://github.com/redFrik/raspberrypiWorkshop/blob/master/README.md](https://ccrma.stanford.edu/wiki/Lab1_Making_FX_in_Pd_2013)

controlling Pd from command line:
<code> pd -stderr -nogui -verbose testsines.pd</code>

#### 17.11.14:
Successful installed Pd on Raspbian Stretch.

#### 17.11.20
## how to get sensor data into PD?
### 1. Serial messages or 2. OSC
1. getting data from Sensor Hat in python Communication with python attempt 1: using socket - Pdsend
http://ccrma.stanford.edu/planetccrma/man/man1/pdsend.1.html

#### 17.11.24 - got it working
#### 17.11.26 sending joystick x value to Pd to change note
#### 17.11.26
- accelerometer from sense hat
- sending data to Python script then port it over to Pd.
- Reading Making Music Apps (oreilly)

## OSC communication
[https://github.com/toddtreece/osc-examples.git](https://puredata.info/downloads/osc)
[https://puredata.info/downloads/osc](https://puredata.info/downloads/osc)
get help on routeOSC and packOSC
they are native in Pd - no, actually not native, part of mrpeach which doesn't work with pi

#### 17.12.14
Learning about Live Looping with Ableton using Apogee Gio.

#### 18.02.07
trying pyOSC python library that sends OSC messages, from here http://pdonapi.blogspot.ca/2015/10/using-sensors-in.html

On the pure data side, use a version 0.47 or later and use the [netsend]/[netreceive] and [oscformat]/[oscparse] objects

### Lots of Pots (shield)
https://moderndevice.com/news/using-the-analog-input-on-the-lots-of-pots-board-for-raspberry-pi/
https://moderndevice.com/wp-content/uploads/2013/07/LotsOfPots.pdf

#### 18.02.10
- LOP shield built but not working yet, could be that it's made for older version of RPi, 26pin
- try:
    -   test the solder connections for connectivity
    - look at LOP schematic)

#### 18.02.12
- looks like the solder is fine
- there's a few in the header that could be redone but the connections seem fine using my (iffy) multimeter.


#### 18.02.15
Finally got Lots of Pots working! reinstalled Raspian and
SPI software seems to be working, might test it with a breadboard and ADC MC3008

- try building the rpieffectsbox (in software folder)
or this simple spi volume control example
https://learn.adafruit.com/reading-a-analog-in-and-controlling-audio-volume-with-the-raspberry-pi/script

more about SPI here:
https://elinux.org/RPi_SPI
http://ipsolutionscorp.com/raspberry-pi-spi-utility/

#### 18.02.17
- Pedal-Pi in case, cut and drilled case to fit
- now working with looper effect running on startup
https://guitarextended.wordpress.com/2012/08/28/running-pd-on-a-headless-raspberry-pi

#### 18.02.19
- added sensehat py scripts to Loppi repo
- make Loppi into synth first then try guitarFX - rpieffectbox (it's in software folder)

#### 18.02.25
- started getting lop2osc.py working

#### 18.02.26
- getting buttons working on LOP, discovered that pots don't work with Pd, need to bug fix
18.02.27 figured out the bug, can't use udp in python script, got buttons and pots working in py


#### 18.03.03-05
trying to get lop2pd.py and step-vibrato.pd launching on bootup

#### 18.03.06
- got it working
- had to put the bash script 'pd_startup.sh' into /etc/profile.d/

* Next: get buttons connected to functions in Pd - step-vibrato script.

#### 18.04.04
## creating KalimPi
1. figure out looper in Pd
2. figure how to control looper with LotsOfPots
3. figure how to assemble
4. do I need a screen OLED for this project?

#### 18.04.28
- trying osc2pd using "socket" low-level UDP
- didn't work on OSx
- or should I try pyOSC
https://github.com/ptone/pyosc

#### 18.04.29
- works on OSx but doesn't seem to - work for python3 in pi
- works in python2, but gets an error because...
- mrpeach routeOSC not working with PD on pi

#### 18.05.02
- kalimPi working but while trying to shoot video for Sound Intensive
- board overheated and stopped working. now won’t even startup. :-(

#### 18.05.06
- Build Google nSynth Super project with Adan, Eli and Evan
- got them working with Pi3v1 not working with Pi3v2
- MicroUSB jack broke after using once, but the MicroUSB jack on the Pi seems to work fine, connect a powerboost100, battery and switch. MIDI keyboard only triggers some notes and there’s a delay.
next steps: figure out how to get seamless notes, multi-note?
seems like others in github issues have the same problem with the semi-assembled version of board.
get refund or new board?

#### 18.05.10
After desoldering a new header on LOP and having the same shorting out/no power on both pi and lop board. After hours of resoldering headers and pots found out that board is toast. pulled pots for use elsewhere. I'm going to trash this lop and buy a new one.

#### 18.05.12
figure out how to convert UUID from phones to a sequence of notes for cicadaPi project.
typical U

UID OO:EE:BD:DB:16:E1

rhythm, notes, variation
tried Text-to-music project, ruby script reads text from rss feed and sends

#### 18.05.30
nsynth MIDI issue hacking:
sudo service open-nsynth stop

sudo mount -o remount, rw/
now my nsynth isn't working, screen or pots not working, tried firmware again but nothing.
18.05.31
worked on nsynth again, tried to recompile the code with the original MidiThread.cpp file but still getting the same problems.

### SoundPiKit (AudioRigPi)(AudioRpiKit)
a taxonomy of different functions for workshop

| Input  | DSP (PureData)	  | Output  |
|---|---|---|
|mic |  amplify | speaker  |
|sensor(s), buttons, touch strip | synthesize  |  line-out |
|Sensehat  |  sequencer | line-out  |
|Sensehat | looper  |  line-out |
|Pickup & Pots  | looper  |  line-out |
|Device detection  | sequencer  | MIDI/Audio  |
|MIDI device | sequencer  | physical output (marimba) servo motors to mallets  |
|	air pressure	|		|	vibration	|
|water pressure		|		|		|    |

#### 18.05.18
### Create an PD Essentials tutorial
Digital audio
http://write.flossmanuals.net/pure-data/what-is-digital-audio/

array(table), sequencer
random
common audio effects (DSP)
simple synth? envelopes, filters, sawwave generator -> enveloper filter

### Make Pd cheatsheet

Basics:
    1. Digital Audio
    2. Graphical Programming
    3. Interface: nodes - inlets(on top), outlets (outlets)
                    inlets can be hot or cold (left to right) left executes first
                    Edit Play modes
    4. Syntax:
        a. messages, symbols & comments
        b. GUI objects - bang, toggle, number2, vSlider, hSlider, vRadio


### Pd Exercises:
    01 - basic oscillator [osc~] [dac~]
    02 - OSC with variable [vslider] [number]
    03 - Midi Notes to frequency
    04 - hSlider with rounding/integer
    05 - Introducing Array/Table
    06 - Table with Metronome (Metro) (show table as list)
    07 - Downsampling with phasor
    08 - Audio Input
    09 - Potentiometers (hardware communication) pots/buttons from LOP to PD
    10 - Simple synth
    11 - Simple sequencer
    12 - Audio Effect - delay


Parking lot:
    loading audio file
    record audio
    looping

Roadmap:
    look into converting LOP to a analog input (breadboard) for bringing in analog sensors.

    Hacking Pd
    https://makezine.com/2008/11/03/howto-hacking-rjdj-with-p/

or
there's Python-OSC
https://pypi.org/project/python-osc/
https://github.com/attwad/python-osc


on the PD end:
http://write.flossmanuals.net/pure-data/osc/

Arduino Music and Audio Projects

https://projects.raspberrypi.org/en/projects/ultrasonic-theremin
https://learn.adafruit.com/raspberry-pi-open-sound-control/setting-up-your-raspberry-pi

#### SenseHat to PD
Next Steps:
1. Sense Hat with synth & sequencer

2. Ukulele with LOP and step vibrato.pd
done

3. Axoloti synth & case

#### 4. Kalimpi with LOP - looper with octave effects
using Alfaloop -seems robust, midi or keyboard keys to trigger record/overdub
very complex Pd patch, edit is disabled (press x three times to unlock)
open the midi/keys area, dig around for where to send a bang to record and over buttons, feedback up and down, ducking up and down.

#### 18.06.13
In the bottom right corner of main (Alfaloop.pd) is a link to pd stuff - a central hub to all the discrete components.
After watching more Cheetomoskeeto videos I learned that from my button input I need to change the value from a bang to an actual value and I'm doing this in Pd with the Change object.
Then I've put the trigger into the midi_receive.pd and changed the value to 127 which turns the Record mode on/off.
Next try the Over and Feedback values:

#### 18.06.14
feedback and ducking connected to pots now
over btn doesn't stop after
would be useful to turn on a record light on KalimPi
this might help:???

#### 18.05.15
recoded the buttons to work with different logic not a wait loop. Works better but could be improved further.
also, testing the audio, after making a new Pd file called audio-input.pd I found out that the piezo is not connected. will resolder the connect to get both audio sources to mix for sound.
will use two more pots for volume for each pickup.
next, set up the octaver.


#### 18.06.20
use the _PureData/pd-patches/octave-lower-example.pd_
integrate into Alfaloop2.pd and connect with buttons.

Learned that USB Audio module does not have stereo input and will need to get rid of piezo mic on KalimPi, rewire the guitar pickup for input into USB module.
Have it working with mono 1/8 jack but doesn't seem to work with USB Audio.

https://rbnrpi.wordpress.com/raspberry-pi-control-gpio-and-your-pi-camera-using-your-phone/
https://www.youtube.com/watch?v=Kk9KO6Stqp4


5. nSynth
https://github.com/googlecreativelab/open-nsynth-super
built but MIDI input isn't polyphonic or allowing for playing.
Github issue 29 - suggests MIDI over USB, adding ofxMIDI library and recompiling of file.
getting new parts shipped
parts shipped and soldered into place but still the same problem
give up on this for now...

6. Cicadapi - scans for devices and makes musical patterns from device UIDs
make import bluetooth library work in python3
also, getting Pd to intrepret UID codes, shall that conversion be done in Python? or just splitting te original sequence into space delimited for easy
this might give some hints https://github.com/jwktje/langorhythm


7. Wekinator & Pi - machinelearning with pi
- sensor that learns your gestures

8. Hybrid Guitar - use the acoustic qualities of the guitar but add DAW capabilities, fx and looping(bluetooth pedalswitch).
guitar pickups > Pi > Pd > looping > amp > internal speaker

NetPd - (in PureData Patches)
try playing in a networked situation this summer

Webcam > colour to snd conversion > output sonic composition

ForceSensorResistor with RPi tutorial
https://acaird.github.io/computers/2015/01/07/raspberry-pi-fsr

### Write about:
describe the interactions with musical instruments
Stability of the interface, Signal-near representation, Close coupling to interaction, Sonic complexity

Sound for Augmented-Reality
Sound for Human-Robot Interaction

## Ideas for instruments
- simple jitterbot that amplifies/modifies your voice when you talk to it.
- make a looper that you can walk with
- ambient noise capture device - location, image and sound
- handheld auto-tune mic
- ukulele with fx-controlled by accelerometer
- tiny tap tempo metronome
- use wiimote as a controller (https://www.youtube.com/watch?v=rxHIJx-O3iU)


Piano Forte Guitar (guitar with keyboard)

### Creating Research area
- blog (recruite Evan to help with website and social media)
plan out online content - instructions, questions - faq
connect to others doing this work.
use Github as code repo, wiki and pages
- online forum (created talk.sonicinteraction.design Discourse forum, now configure and populate)
- Meetup monthly


## idea: Exquisite Instrument Game
a variations of the Exquisite Corpse game were you create an instrument with a group by passing along an incomplete version of an instrument with a provocation.

interesting links
https://www.bitpi.co/category/raspberrypi/
https://github.com/kasperkamperman/MobileCameraTemplate
https://www.kasperkamperman.com/blog/
http://www.katjaas.nl/home/home.html


#### 18.06.19
Researching Arthur Carabot
https://www.arthurcarabott.com/
https://github.com/acarabott

Yuri Suzuki
http://yurisuzuki.com/design-studio/z-machines
In 2013, Yuri Suzuki Design Studio was asked for initial concept development for Z-Machines, an all robot music band built to perform beyond the capabilities of the most advanced human musicians.

The challenge was to design a system that could play emotionally engaging music while rediscovering conventional instruments.

David Cranmer
http://www.nervoussquirrel.com/
http://www.nervoussquirrel.com/ferrofluid.html

https://contextsequencer.wordpress.com/


Notes on Field Recording
https://www.creativefieldrecording.com/start-here/

http://daily.redbullmusicacademy.com/2017/03/the-art-of-field-recording

OSC
https://en.wikipedia.org/wiki/Open_Sound_Control


Goals of the workshop
Explore ways of creating instruments
Better understand the nuances of how humans interact with instruments
What makes a great instrument - playability, responsiveness, social
Explore the liminal space between digital and analogue


<h3>References</h3>
http://www.instructables.com/id/Pure-Pi-Control-custom-stompbox-effects-on-a-Raspb/
http://www.instructables.com/id/PiMiDi-A-Raspberry-Pi-Midi-Box-or-How-I-Learned-to/
https://guitarextended.wordpress.com/2011/12/12/installing-pure-data/

questions for Discourse
https://www.quora.com/What-data-structures-and-algorithms-are-used-in-programming-for-music-audio-clips

how is ai and machine learning shaping music?
http://www.wired.co.uk/article/how-ai-and-machine-learning-are-shaping-the-future-of-music

http://www.wired.co.uk/article/ai-music-robot-shimon


https://musictechfest.net/
http://www.vahakn.co.uk/


Sonic Interactions Mission
https://musictechfest.net/musictechifesto/

Seeing that the fertility of music technology as a subject that bridges computational, scientific, social scientific and humanistic approaches, we looked for common ground across those areas.

Music technology has never been more exciting. Every day brings new ways to be musical through new software, instruments, devices, platforms and protocols for fans, artists, and organizations. People are experimenting with new ways to perform, collaborate and compose, to listen to their favorite artists and discover new ones, and to share the music they love.

Meaningful innovation is sustainable and just – yet the current landscape of music technology favors short-term profit-making, too often at the expense of deeper cultural concerns. Landfills swell with carelessly-designed consumer electronics, discs, cartridges, instruments, toys and gadgets. Like other cultural workers, many who contribute most to the richness of musical cultures lead increasingly precarious economic lives. But those who stand to profit the most economically have the biggest say in policy discussions. Too often music technologies are used as tools of exclusion rather than inclusion. Because what counts as “music,” “technology,” and “music technology” is unsettled, those with the most power create the most powerful definitions.

We call for technologies to be created with an eye for the long-term. Musical objects should last as long as the materials out of which they are made or they should be modular, recyclable, or transformable. They should be forward-compatible whenever possible. Data must be portable and not bound to a particular company or platform. At the same time, standards must not become coercive. Music is not standard. We must cultivate the freedom to build and use nonstandard tools.


workshop
- explain intention (workshop goals)
- show structure
- theory
- examples (TED)


Reaching out to other Schools
McGill marcelo.wanderley@mcgill.ca


####  July 2018 - Tumo Notes

students thought it was more about programming.
need more women and musicians in the class

went through Pd exercises 1-7 in first class
played a chord at the end of exercises

Day 2
- play music - explain rhythm connect
- connect physical control. (midi keyboard or Push)
- look for game controllers


Day 2 Plan:

Pd Exercises
1. Tables, Graphs and Arrays
http://write.flossmanuals.net/pure-data/arrays-graphs-tables/

http://write.flossmanuals.net/pure-data/sequencer/
challenge: write a 16 note melody, change the tempo with messages.

2. Step sequencer
http://write.flossmanuals.net/pure-data/step-sequencer/

3. AudioFiles

4. MIDI Controller
http://write.flossmanuals.net/pure-data/controlling-the-synth/

Day2 notes:
Today went well.
went through 4 Pd exercises and had them build their own sequences.
Spent too much time letting them find their own audio files.


Day 3
A. RPi - audio and LOP soldering
B. Music - play simple rhythm together
C. Pd Exercises


Pd Exercises
1. Audio input (exercises 12-13) & connecting to LOP board
2. Simple Sequencer and synth (14-15)
3. Delay FX (16)
4. Audio playback manipulation (17)

Day 4
Talked about making the instrument, looked at some ideas.
Played music -
Pd Sequencer

Day 5 Monday
start building instruments
Pd - learn how to make fx

Day 6 Tuesday
Start building instruments
Pretty good day

Day 7 Wednesday
slow to build instruments because of materials
don't know how to add buttons to Pi, buttons have LED and need power
Looked at Sampling again - Pd - fx and sampling

Day 8 Last day to build instruments
if we don't have an instrument for each team -  I will assign the ukelele or Kalimpi to group
Simple drum machine
Sampler- looper
https://www.rebeltech.org/2016/05/18/pure-data-tutorial-2-sampler-looper/
Synth with delay
https://www.rebeltech.org/patch-library/patch/Arp_with_Delay_and_Envelope

Spent a lot of time getting Pi startup script working properly. lots of voodoo
paths, permissions
looks like one group has pd file running, you can hear the mic and fx but python doesn't seem to be communicating even though its running.

Also, spent a lot of time rebuilding files because everyone is altering their version of simple delay, chorus and tremolo.

* for next time, build FX into the file as an abstraction, instead of separate pd file

#### 18.09.01
courses on kadenze
    machine learning for musicians
    reinventing the piano
    concurrence in python


#### 18.09.26
bought new sensors (soft Potentiometers and sliders)
desoldered one pot and connected a soft Potentiometer

look into connecting controllers (midi keyboard,xbox controller, ps controller, and wii remote)
came across https://www.circuito.io/app?components=512,8680,11021

Cheetomoskeeto uses the hid Pd library - https://www.youtube.com/watch?v=HB_oVny33wA

got hid working on pd extended on mac.
but hid doesn't seem to work properly with Pd .48
FB comment says that it needs to be compiled:

#### try this out
https://github.com/avilleret/hid
looks the same as this:
https://puredata.info/downloads/hid/releases/0.7

also there's this...
https://puredata.info/docs/faq/how-can-i-set-permissions-so-hid-can-read-devices-in-gnu-linux


there's also the ctlin object
http://www.pd-tutorial.com/english/ch04s03.html

wii nunchuck adapter
https://www.robotshop.com/en/wiichuck-adapter-arduino.html?gclid=Cj0KCQjwxbzdBRCoARIsACzIK2lxjNh-FKnSq35UGZDdvLkNQhoeGVD5v1Pi5GZkZsJoofF81cQ5BQQaAl5KEALw_wcB

https://computers.tutsplus.com/tutorials/using-a-wii-nunchuck-to-control-python-turtle--cms-20984

https://pimylifeup.com/raspberry-pi-wiimote-controllers/
https://pimylifeup.com/xbox-controllers-raspberry-pi/

Try installing Pd extended on RPI
http://pi.bek.no/pd-ExtendedInstall/

WiringPi
https://github.com/WiringPi/WiringPi-Python
http://wiringpi.com/

https://www.keithmcmillen.com/blog/manipulating-midi-with-pure-data/

## Nunchuck Working
I got a C program reading the Nunchuck (after lots of headaches, one corrupt SD card and many different wiring configs). I used this code:
https://raspberrypi.stackexchange.com/questions/29720/how-do-i-access-the-wii-nunchuck-using-wiringpi

now to get it over to Pd

FluidSynth: http://www.fluidsynth.org/
http://andrewdotni.ch/blog/2015/02/28/midi-synth-with-raspberry-p/

## This looks like promising way to get the MPK mini working
update: got MPK working but the following site will be helpful when automating startup with Pi

https://lucidbeaming.com/blog/tag/synthesizer/
https://github.com/MarquisdeGeek/FluidPi

this is another good site for automating midi startup
http://andrewdotni.ch/blog/2015/02/28/midi-synth-with-raspberry-p/


Soft Synths
https://github.com/zynthian

## came across this, looks good
http://www.earcatching.com/pdconv/

ABLETON tuts
https://blog.kadenze.com/creative-technology/10-ableton-live-tutorials-for-learning-music-production/?utm_source=kadenze_blog&utm_medium=referral&utm_campaign=biweekly_blog_email

theremin with Sonic Pi and ultrasonic sensor
https://projects.raspberrypi.org/en/projects/ultrasonic-theremin


Hey, let's learn React
https://www.taniarascia.com/getting-started-with-react/

https://linuxmusicians.com/

This was helpful: http://tedfelix.com/linux/linux-midi.html
$ cat /proc/asound/cards
brings up the AKAI MPKmini2

do i need to install this?
https://qjackctl.sourceforge.io/

#### 18.10.01
##### Well, that took ages, 5 days to figure out Midi into Pd on Pi!
sudo aconnect 24:0 128:0

24:0 is the client 24: MPKmini2 and :0 is port
128 is the client num for Pd

here's another advanced midi SETUP
https://forum.critterandguitari.com/t/how-to-advanced-midi-setup-3-1/2264/24

to list out USB devices use:
## lsusb

Write a Pd external
https://github.com/pure-data/externals-howto

##### This tutorial helped me install Pd 49
https://forum.pdpatchrepo.info/topic/11626/pd-48-on-raspberry-pi-3/3

Then I noticed that the [hid] object throws an error that says that the ARM architecture is incompatible with the OS. That made me think that the pd_linux file in the library was not compiled for Raspbian Stretch. So I looked at getting the version of Hid library with the source and make files to recompile in for the current ARM64 architecture.


getting xBox controller working on a Mac
https://github.com/360Controller/360Controller/releases

https://github.com/360Controller/360Controller/blob/master/Readme.md#xbox-one-controllers-connected-with-usb

#### 18.10.09
### Creating SD cards for Sonic Interactions Kit for Workshops
usually making duplicates of SD cards starts with copying from the command line:
sudo dd if=/dev/disk2 of=~/Desktop/SonicInteractions-Kit.dmg

but after a 20 minute process there was a dmg file but it didn't work when trying to create a new disk in Etcher.

I remembered that i used a application called ApplePi Baker when I was duplicating SD cards for Mineblock. So I tried that and it worked with out a problem.
https://www.tweaking4all.com/software/macosx-software/macosx-apple-pi-baker/


## Workshop Learning Resources:
PD Floss Manuals
http://write.flossmanuals.net/pure-data/introduction2/

Cheetomoskeeto's youtube channel
https://www.youtube.com/channel/UC-RatzHn1ukuuINLqnbBYeg

##### * idea
Alter the way that the pick hits the string. use spring to have the pick bounce back and forth.


#### 18.10.10 - 30
mom in hospital, no much to work on research, but thinking about a few things:

- other ways of getting sensor data to Pd quicker (more responsive).
- looked into OpenFrameworks to create a script to pull data, maybe quicker and lower overhead

other protocols:
http://www.eclipse.org/paho/

### LEARN OpenFrameworks
https://openframeworks.cc/ofBook/chapters/of_philosophy.html

TCP vs UDP vs osc
https://openframeworks.cc/ofBook/chapters/network.html

GPIO on OpenFrameworks
https://github.com/kashimAstro/ofxGPIO

Cross compiling for RPi
https://forum.openframeworks.cc/t/cross-compiler-for-of-0-9-0-jessie-arm6-rpi1/21336

https://hackaday.com/2016/09/01/how-to-use-docker-to-cross-compile-for-raspberry-pi-and-more/

## not enough good tutorials on this topic above. leaving cross-compiling for now.

#### 18.11.04
### Article about architecture and digital media
https://solidhaptik.ghost.io/architecture-and-new-media-a-survey/

Love this piece by Brian House
https://brianhouse.net/works/quotidian_record/

CMU Making Interactive things
http://mti.code.arc.cmu.edu/2014/

Daito Manabe
http://www.daito.ws/en

Screen Tearing
https://en.wikipedia.org/wiki/Screen_tearing

## Music reminds us that we are connected


### getting Oh Eye working
first need to detect both ADCs (MCP3008)
try this command:
sudo i2cdetect –y 1
nope!

https://elinux.org/Interfacing_with_I2C_Devices#External_Links


#### 2018.11.07
## Taking inventory of Sonic Interactions projects

1. Sensepi (check for updates, create a synth that responds to accelerometer)
2. Nukele (check for updates)
3. Loppi (in pedal box with generative synth on startup)
4. Pedalpi
5. Cicadapi
6. Axoloti
7. n-synth
8. PiSound
9. campi (zero with cam, and cv with synth)

amplipi (hifiberry)


## 2018.11.
## PiSound with Modep (is amazing)
https://blokas.io/modep/docs/
Mod duo uses LV2 plugins (interesting)

log into your pedal from a browser and configure from a web interface:
http://modepi.local


https://www.moddevices.com/blog/2016/09/08/create-lv2-plugin-part-1
http://lv2plug.in/book/

#### 2018.11.08
went to LA for Ableton Loop
DIY instrument workshop was rubbish, the Novalia boards look interesting but what has been made are all pretty lame marketing efforts, with the exception of the IceCube Album cover that had great art and a functional sampler.

https://www.novalia.co.uk/

watch talks from last year:
https://www.ableton.com/en/blog/loop/talks/

Motors, Magnets and Motion: Electronic Music Instruments from the Physical World
https://www.youtube.com/watch?v=hJHwhb99Bzo
Koka Nikoladze
Gijis Glaskes
Alice Eldridge - open ended experimental, tweaking no-fixed-plan exploration

bring together best of digital and physical instrument  making
speaker bolted into back of cello to create a tight coupling of sound source (strings) and output to create feedback loop

Loop talk with Prince producer, Susan Rogers on working with Prince, audio perception, and making a career in music.
Geggy Tah - Greg Kurstin and Tommy Jordan

[Greg Kurstin] | [https://en.wikipedia.org/wiki/Greg_Kurstin]

melody logic:
small intervals sound angry or sad
large interval jumps sound joyful and happy

audio perception
https://plato.stanford.edu/entries/perception-auditory/
auditory perception psychology

Philosophical thinking about perception has focused predominantly on vision. The philosophical puzzle of perception and its proposed solutions have been shaped by a concern for visual experience and visual illusions. Recently, however, other perceptual modalities have attracted attention. In addition to auditory perception and the experience of sound, touch and tactile awareness have generated philosophical interest concerning, for instance, the tactile and proprioceptive experience of space, the objects of touch, whether contact is required for touch, and whether distinct modalities detect pressure, heat, and pain (see, e.g., O'Shaughnessy 1989, Martin 1993, Scott 2001, Fulkerson 2013).

1. TRANSLATE what we have learned from the visual case into terms that apply to other modalities. This approach is relatively conservative. It assumes that vision is representative or paradigmatic and that we have a good understanding of perception that is derived from the case of vision. One example of this kind of approach would be to develop an account of the representational content of auditory experience.

2. EXTEND our vision-based understanding of perception. Non-visual cases might draw attention to new kinds of phenomena that are missing from or not salient in vision. If so, a vision-based account of perception is satisfactory as far as it goes, but it leaves out critical pieces. For example, speech perception, multimodal perception, and flavor perception might involve novel kinds of perceptual phenomena absent from the visual case.

3. CHALLENGE vision-based claims about perception. If falsifying evidence is discovered in non-visual cases, then theorizing beyond vision may force revision of general claims about perception that are supported by vision. For example, if olfactory experience is not diaphanous, the transparency thesis for perceptual experience fails.

read more about Psychoacoustics:

    AudioUI02acoustics-buxton.pdf
    psychoacoustics-book.pdf
    sound-of-innovation-stanford-andrew-j-nelson6271

#### 2018.11.14
## OhEye Board Coding (cntd):

You can use the command find /sys/bus | grep spi in a terminal if you want to view the connected SPI devices. It will show you something a bit like this:

~~~~
pi@raspberrypi ~ $ find /sys/bus | grep spi
    /sys/bus/spi
    /sys/bus/spi/devices
    /sys/bus/spi/devices/spi0.0
    /sys/bus/spi/devices/spi0.1
    /sys/bus/spi/drivers
    /sys/bus/spi/drivers/mcp251x
    /sys/bus/spi/drivers/mcp251x/bind
    /sys/bus/spi/drivers/mcp251x/module
    /sys/bus/spi/drivers/mcp251x/uevent
    /sys/bus/spi/drivers/mcp251x/unbind
    /sys/bus/spi/drivers/spidev
    /sys/bus/spi/drivers/spidev/bind
~~~~

The code to get data from Spi:

~~~~
#!/usr/bin/env python

# MCP3008 connections are:-
# CLK  =>  SCLK  
# DOUT =>  MISO
# DIN  =>  MOSI
# CS   =>  CE0

import spidev
import time
import os
import urllib
import RPi.GPIO as GPIO

GPIO.setmode(GPIO.BCM)

spi_ce0 = spidev.Spidev()
spi_ce1 = spidev.Spidev()

spi_ce0.open(0,0)
spi_ce1.open(0,1)

def ReadChannel(channel):
    adc = spi.xfer2([1,(8+channel)<<4,0])
    data = ((adc[1]&3)<<8)+adc[2]
return data

sensor1 = 0
sensor2 = 1
sensor3 = 2
sensor4 = 3
sensor5 = 4
sensor6 = 5
sensor7 = 6
sensor8 = 7

sensor9 = 8
sensor10 = 9
sensor11 = 10
sensor12 = 11
sensor13 = 12
sensor14 = 13
sensor15 = 14
sensor16 = 15

delay = 5
~~~~

There's also this...

~~~~
#!/usr/bin/env python

# MCP3008 connections are:-
# CLK  =>  SCLK  
# DOUT =>  MISO
# DIN  =>  MOSI
# CS   =>  CE0

import spidev
import time

spi_ce0 = spidev.Spidev()
spi_ce1 = spidev.Spidev()

spi_ce0.open(0,0)
spi_ce1.open(0,1)


adc1 = spi_ce0.xfer([1,(8+0)<<4,0])
adc2 = spi_ce0.xfer([1,(8+1)<<4,0])
adc3 = spi_ce0.xfer([1,(8+2)<<4,0])
#... add more lines
adc14 = spi_ce1.xfer([1,(8+6)<<4,0])
adc15 = spi_ce1.xfer([1,(8+7)<<4,0])
adc16 = spi_ce1.xfer([1,(8+8)<<4,0])


sensor1 = ((adc1[1]&3)<<8)+adc1[2]
sensor2 = ((adc2[1]&3)<<8)+adc2[2]
sensor3 = ((adc3[1]&3)<<8)+adc3[2]
#... add more lines
sensor14 = ((adc14[1]&3)<<8)+adc14[2]
sensor15 = ((adc15[1]&3)<<8)+adc15[2]
sensor16 = ((adc16[1]&3)<<8)+adc16[2]

delay = 5
~~~~

After a couple of tweaks I settled on the following code which works. Might need to test the performance but data seems to be coming in for all 16 channels.

Would like to optimize the button code next.

~~~~
#!/usr/bin/python

import os        # import os for sending messages to PD
import spidev    # import the spidev module
import time      # import time for the sleep function
import RPi.GPIO as GPIO

# Open SPI bus
spi_1 = spidev.SpiDev()
spi_2 = spidev.SpiDev()

spi_1.open(0,0)
spi_2.open(0,1)

spi_1.max_speed_hz=1000000
spi_2.max_speed_hz=1000000

waitTime = .2
bounceTime = 0.1

btn1alreadyPressed = False
btn2alreadyPressed = False
btn3alreadyPressed = False
btn4alreadyPressed = False

GPIO.setmode(GPIO.BCM)
## GPIO.setup(4, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(4, GPIO.IN)
GPIO.setup(17, GPIO.IN)
GPIO.setup(18, GPIO.IN)
GPIO.setup(27, GPIO.IN)

def send2Pd(message=''):
    # Send a message to Pd
    os.system("echo '" + message + "' | pdsend 3000 localhost udp")

def readadc(channel):
    if channel > 7 or channel < 0:
        return -1
    # spi.xfer2 sends three bytes and returns three bytes:
    # byte 1: the start bit (always 0x01)
    # byte 2: configure bits, see MCP3008 datasheet table 5-2
    # byte 3: don't care
    r = spi_1.xfer2([1, 8 + channel << 4, 0])
    # Three bytes are returned; the data (0-1023) is in the
    # lower 3 bits of byte 2, and byte 3 (datasheet figure 6-1)
    v = ((r[1] & 3) << 8) + r[2]

    return v;

def readadc2(channel):
    if channel > 7 or channel < 0:
        return -1
    # spi.xfer2 sends three bytes and returns three bytes:
    # byte 1: the start bit (always 0x01)
    # byte 2: configure bits, see MCP3008 datasheet table 5-2
    # byte 3: don't care
    r = spi_2.xfer2([1, 8 + channel << 4, 0])
    # Three bytes are returned; the data (0-1023) is in the
    # lower 3 bits of byte 2, and byte 3 (datasheet figure 6-1)
    v = ((r[1] & 3) << 8) + r[2]

    return v;

while True:
    btn1pressed = not GPIO.input(4)
    btn2pressed = not GPIO.input(17)
    btn3pressed = not GPIO.input(18)
    btn4pressed = not GPIO.input(27)

    input_right = GPIO.input(4)
    input_left = GPIO.input(17)
    input_down = GPIO.input(18)
    input_up = GPIO.input(27)

    if btn1pressed and not btn1alreadyPressed:
        message = '8 1'
        send2Pd(message)
    btn1alreadyPressed = btn1pressed

    if btn2pressed and not btn2alreadyPressed:
 #       print('2 Pressed')
        message = '8 2'
        send2Pd(message)
    btn2alreadyPressed = btn2pressed

    if btn3pressed and not btn3alreadyPressed:
 #       print('3 Pressed')
        message = '8 3'
        send2Pd(message)
    btn3alreadyPressed = btn3pressed

    if btn4pressed and not btn4alreadyPressed:
 #       print('4 Pressed')
        message = '8 4'
        send2Pd(message)
    btn4alreadyPressed = btn4pressed

    values = [0]*8
    values2 = [0]*8

    for i in range(8):
        values[i] = readadc(i)
        values2[i] = readadc2(i)
        message = str(i) + ' ' + str(values[i]) # make a string for use with Pdsend
        message = message + str(i+8) + ' ' + str(values2[i])
        send2Pd(message)

# consider creating a message that has all values in one string rather than separate messages
    print('| {0:>4} | {1:>4} | {2:>4} | {3:>4} | {4:>4} | {5:>4} | {6:>4} | {7:>4} '.format(*values))
    print('| {0:>4} | {1:>4} | {2:>4} | {3:>4} | {4:>4} | {5:>4} | {6:>4} | {7:>4} '.format(*values2))
#    print(message)
    time.sleep(waitTime)
~~~~

#### 2018.11.16
After speaking with Adan last night I've decided that I'm going to try to get Nsynth working today.

Here's what's wrong with it so far...
1. when it starts up it intermittently doesn't engage the OLED screen, i think it's a connection problem because the RPi to board connection seems to not be sturdy and the RPi board moves.

2. MIDI doesn't seem to work well with the DIN connection and I'd like to try to get the USBMIDI working.

First up, how to get it connected to the net to update.

Disk is readonly by default, here's how you change that:

7. Lock the filesystem (optional)
When you have set up your device and are happy with the configuration, it is advisable to lock the file system to prevent corruption upon a hard power down (i.e. when the power cable is removed from the device). Reconnect the keyboard and screen for these steps.

Open fstab in a text editor

$ sudo vi /etc/fstab
Add ,ro to the root partition to lock the filesystem at next boot:

proc 					/proc           proc    defaults  	        	0 		0
PARTUUID=7762b82d-01 	/boot           vfat    defaults				0 		2
PARTUUID=7762b82d-02 	/               ext4    defaults,noatime,ro 	0 		1
Reboot, and test the filesystem by using touch xyz to create a new file named xyz. You should be denied this if the system is in read-only mode.

To remount as read-write (to make changes), you can remove the ,ro flag from fstab. To make this change temporarily (e.g. to edit some files), you can remount the filesystem as read-write:

$ sudo mount -o remount,rw /
When you restart, the filesystem will once more be read-only. Your instrument is now fully set up – you can proceed to build it into a case and start to make music with it!
_______________________________________

compiles and works with my Akai MPKmini2, playable but you need to bang on keys to get the sound to register, I think I need to tweak some midi settings to make the sound work even with a softer touch. good for now...

Next, let's try to get the nunchuck working with OhEye Board.

#### 18.11.15
got Python reading data from nunchuck, now to get that data sending to Pd

a few resources:
https://www.raspberrypi.org/magpi/wii-nunchucks-raspberry-pi/
https://github.com/Boeeerb/Nunchuck

Stumbled onto this interesting stuff about VR sound with Pd
http://www.matthiaskronlachner.com/?tag=pure-data
and this project
https://github.com/kronihias/mcfx

## OSC Again
Maybe we can try OSC again for this (more of a standard protocol)
https://rbnrpi.wordpress.com/sonic-pi-3-says-hello-to-raspberry-pi-gpio/

Using OSC will open up making instruments with phones as part of the interface:
https://hexler.net/software/touchosc-android

*still need to get data from oi2pd.py into Pd*

### Ableton Magenta ML plugins
very cool, needed to install Max 8 but didn't license it, still worked.
just have to drag the magenta Ableton file into a track.


#### 18.11.21
Back to nunchuck into Pd
http://www.winko-erades.nl/uncategorized/making-music-using-a-computer-wiimote-and-puredata/

This site has some amazing interviews
http://www.ubu.com/sound/

listening to Brian Eno

Ableton Live
Figuring out how to get input from Lightblock (MPE - Midi polyphonic Expression)
https://github.com/juhot/MPE_Util

#### 18.11.22
trying to figure out how to parse the UID's into a list of note values

#### 18.11.24
*My mom died today.
everything is different*

going to install Purr Data on OSx

https://github.com/agraef/purr-data
compiled PurrData, works, looks good has all the externals of extended but still didn't seem to load HID library.

### Research on Pd Externals
There are two types of external libraries, they either come compiled in a single binary pack or each object comes as a separate binary. The first case is the classic library format and needs to be loaded in Pd's startup (Preferences => Startup). The second case doesn't need any of that but you need to add the library's folder to the search paths (Preferences => Path).

Some thoughts on research topics:
- physical design of Instruments
- perception of music, audio and Instruments
- what is unique about my research?



Learning about React:
https://scrimba.com/p/p7P5Hd/cV7M2uR

#### 18.12.16
writing some music
Tellegio
Massive (Good Mourning)

#### 18.12.19
1. learning more about Ableton as composition tool
2. how to use Ableton as an improvisational tool

Sidechain compression
https://www.ableton.com/en/blog/sidechain-compression-part-2-common-and-uncommon-uses/

MasterClass with Herbie Hancock and Deadmau5
https://learningmusic.ableton.com/notes-and-scales/keys-and-scales.html


#### 19.01.02
Finished Herbie and Deadmau5 tutorials
writing Agnes in live

make note of what was learned and Goals
make goals for 2019

https://trello.com/b/lyzvHLtm
diets
https://www.reddit.com/r/Fitness/wiki/faq#wiki_diet_details

recording idea: compose and play armenian style songs all over during travel, invite people to play.

Diaspora (title idea):
from the Greek word diaspeirein, meaning “to scatter, spread about.”

Displacement - to remove from the usual or proper place
budge, dislocate, disturb, move, relocate, remove, reposition, shift, transfer, transpose


## Cowrite with Morgan Friend
a few ideas for songs:
    Future seemed so swell
    Hope Street
    Armenian minor tune on the Piano


Hope Street
I'm feeling good today, I'm living on Hope St.
Not much more to say, I'm living on Hope St.

(write verses with Hero's journey idea)


Write a manifesto (steve Martin)
What's the thing that drives you?

Live tips:
To deactivate, or mute, a note (or notes) in the MIDI Editor, select it and press 0

Notes on a talk with Richard Freeman
write 10 songs, record an album and do a tour of Canada
instrumentation and arrangement are as important as the song

https://medium.com/jampp/app-the-human-story-why-we-were-onboard-the-minute-we-heard-about-it-7f204281088f

Improvising over Confirmation:
https://www.jazzadvice.com/charlie-parkers-secrets-to-confirmation/

Bebop scales
https://www.jazzadvice.com/mastering-the-bebop-scale-and-jazz-improvisation/


Workshop improvements:
integrate the tech with fun/music -  play little pieces together.
loops


# Advice for new musicians:
1. Less is more.
Music production is complicated, so it’s ridiculously easy to go deeper, to always want more tools and tricks.

But do you really need to watch YouTube tutorials on that latest gadget you bought or should you just spend some time with it. Do you

Most professionals will tell you that learning one instrument inside and out will go much further than learning 5 different instruments at a mediocre level.

I’m not saying there isn’t value in staying hungry (more on that later), but in my experience, less is because it forces you to make progress with your music by looking inside yourself instead of always reaching for something external or abstract.

2. Chill, play the long game.
Remember when you thought you could out-work all of your competition. And sure, it might’ve helped a little – but music doesn’t work that way. Life doesn’t work that way.

So, take your time; there is literally no rush. The race is long and you've got all the time in the world.

## 3. You are never too old to start.
So first and foremost, it doesn’t matter how old you are, if making music is something you want to try, start today. Not tomorrow. Today!

Just be clear about what kind of musician you want to become. Are you going to be a studio hire or a film composer? a hobbist performer or a sideman musician?

## 4. Never stop learning
You’ll have to realize that it never stops.

Whether you’re a superstar like Skrillex, or just a kid in their mom’s basement, the drive to get better and learn more should never diminish. The second you think you have succeeded – whether commercial or personal – it’s only the next step in a never-ending staircase.

## 5. Writer’s block is a myth.
Speaking of creativity – writer’s block is absolutely, definitely, undeniably, 100% – a myth. Creating music is a sum of your habits. And you’ll need to learn that in order to get in the habit of starting, progressing and finishing music, you need to make finishing music an absolute priority.

It doesn’t always come easy, but it does get easier. The hardest part is sitting down and getting started. But once the idea progresses, or you feel that initial spark of excitement – I often find that I get motivated after the fact.

That means mood follows action. Next time you’re tired from a long day, or simply feel like shit for whatever reason – just sit down. And get started. You’ll be amazed by what follows.

## 6. Put yourself out there.
This one’s important – so listen up.

Don’t be afraid to show the world who you truly are as an artist. Don’t get caught up in what’s cool right now. You don’t get burned-out because of the work itself, you get burned out because you forgot why you’re doing it in the first place.

## 7. Kill self-doubt & fear.
I can guarantee you that every great artist in history has gone through a period of self-doubt and frustration. One of the true measurements of your talent as an artist is simply your ability to foster self-doubt in a positive light.

## 8. Trust your taste.
At the end of the day, there are very few things that make producers different from each other. You have access to the same tools, the same sounds – you are playing the same game in the same reality as everyone else – but what really sets you apart? Well, it’s your taste.

Your taste determines what we think is dope. It makes you different. It acts as a driving guideline and forces you to work towards certain standards.

## 9. Check your ego.
I’ll leave this one short. But hey, chill out on the ego. Be nicer to everyone.

Disliking pop music doesn’t make you cool, it makes you short-sighted. Hating on the latest trends doesn’t make you some bad-ass hipster with better taste and ripped jeans – every style, every genre has something worth learning from. If you can’t eventually grasp that, you should find a different career.


Different modes of music mixing
listening
Analyzing
Balancing
Optimizing
Sweetening

### Music Production Links
https://www.attackmagazine.com/
https://www.soundonsound.com/
https://www.edmprod.com/
https://music.tutsplus.com/
https://theproaudiofiles.com/
https://www.sonicacademy.com/
https://bedroomproducersblog.com/
http://futuremusic.com/
https://howtomakeelectronicmusic.com/
https://www.homestudiocorner.com/
https://behindthespeakers.com/
https://www.davidglennrecording.com/
https://www.soundgym.co/?aff=9011

San Holo Documentary of the making of a debut Album

### Creating a good melody
https://www.edmprod.com/ultimate-melody-guide/

A melody is the main idea of the track. A motif or phrase is a short musical idea – it might be a few notes placed in a certain order or rhythm, but it isn’t the main feature.



## LOOP kadenze course

big take aways:
variations: inversions, transpose, retrogrades
vary rhythm or timing
use Live in Session mode to launch a follow action after a loops
by choosing 'other' from the follow action you can have it randomly create an arrangement

Process composition (steve reich)
Questions to ask when creating:
1. Did you feel your agency as a composer diminish?
2. Was that a positive thing for you?

EQing your samples
pinpoint specifics to accentuate, and remove unwanted

##Strategies for getting blocked:

Use contrast in arrangement:
    loud/soft
    fast/slow
    harsh/smooth
    rhythmic/non

Give yourself constraints:
- Limit the number of voices in a piece
- exploit the unique qualities of the voice that you're working with
- Use one method throughout whole piece, ie. specific delay - altered for each instrument
- Limit your tweaking- give yourself 2mins to find a sound and get to recording the phrase, change sound later

Create a container:
study a variety of forms, not just musical forms but any form can inform...
ie.
the shape of a leaf can inspire the shape of your composition.
the shape of a tree
copy the form of something

So much can be done with a computer, the options are virtually limitless and


### 19.01.20
Edit the 2nd Podcast Recording
Refine the Intro to 1st. podcast


Making Ambient music
    - stretch out phrases and loops
    - play with interpolation algorithms in Live
    - offset loops

### 19.01.21
syncing pocket operators
modePi working on battery - prepping for field Recording

sonic interaction design. from Sonification Handbook
One of the ultimate goals of SID is the ability to provide design and evaluation guidelines for interactive products with a salient sonic behavior. SID addresses the challenges of creating interactive, adaptive sonic interactions, which continuously respond to the gestures of one or more users. At the same time, SID investigates how the designed gestures and sonic feedback is able to convey emotions and engage expressive and creative experiences. SID also aims at identifying new roles that sound may play in the interaction between users and artifacts, services, or environments. By exploring topics such as multisensory experience with sounding artifacts, perceptual illusions, sound as a means for communication in an action-perception loop and sensorimotor learning through sound, SID researchers are opening up new domains of research and practice for sound designers and engineers, interaction and interface designers, media artists and product designers, among others1 . SID emerges from different established disciplines where sound has played an important role. Within the field of human-computer studies, the subtopics of auditory display and sonification have been of interest for a couple of decades

## Making Music
https://makingmusic.ableton.com/catalog-of-attributes

### 1. Musical Attributes
problem:
Creative musicians find inspiration in other music. While we seek to make music that is uniquely our own, every other piece of music we hear is automatically processed and becomes an unconscious part of our musical vocabulary. Taking too much is theft. Taking too little fails to acknowledge our influences.

Listen carefully—and many times—to the piece that inspires you (the “source”). Study it, element by element and layer by layer, until you can write down a catalog of its attributes. Once the catalog feels complete, set aside the original source, instead referring only to the catalog as a template for your own new work (the “target”).

Consider the attributes of sound, harmony, melody, rhythm, and form. Write something concrete about what you hear for each attribute. If you re comfortable with notation, feel free to use it in your catalog, but sparingly; the goal is to capture only the framework or scaffolding of the source, including the aspects that make it inspiring, but without simply recreating it. You should end up with a description, not a transcription.

### 2. Active Listening
Problem:
Although you listen to a lot of music, you don’t really have a sense that you’re learning from what you listen to. You know what you like, but you don’t really understand why you like it or how to extract compositional or technical ideas from what you hear so that you can reuse them in your own music.
Active listening simply means listening as the primary activity, and it’s an important skill to develop. Rather than using music as the background for another activity, try listening without doing anything else. This requires time, quiet, and focus, which are skills you need for your own production work anyway. A good way to start is by just putting on some music and then turning your attention to it entirely. If you’re listening at your computer, close any open applications (and, ideally, your eyes as well). At this point, you’re not trying to listen with a particular focus, but rather a general one. If you can concentrate and avoid distraction, you’ll be amazed by how much more you hear than in a passive listening state.

The next step in active listening is to start trying to deconstruct what’s happening in the music you’re listening to. Here are some tips for doing this:

Listen in Layers
A great way to actively listen is to listen to the same piece multiple times and force yourself to focus on a different specific parameter each time. For example, spend one pass listening only for:

Sound: What are the timbral characteristics of this music? What instruments are used? What is the texture (dense vs. sparse)? Are there some specific production techniques that you recognize (either from your own or other music)? What kind of acoustic “space” is suggested by the music (dry vs. reverberant, near vs. far, etc.)?

Harmony: What key (if any) is the song in? What chords are used? Is there a chord progression that happens throughout, or does it change from section to section? If there are no overt chords (as in some minimal or experimental music), is harmony implied in another way?
Melody: What’s happening in the melody? Does it have a wide or narrow range? What is its general contour: Angular, with lots of leaps? Stepwise, with motion mostly by one or two semitones? What instrument or voice has the melody? Does this ever change? If there is no overt melody (as in some minimal or experimental music), is melody implied in another way?

Rhythm: How are events distributed within short time ranges like a bar or phrase? Are there patterns that repeat, or do rhythmic gestures happen only once? Are rhythms and tempo overtly identifiable, or is the music free and largely arrhythmic? What instruments have the most impact on the rhythm? What do the less rhythmic instruments do?

Form: How does the song evolve over time? Are there clear sectional divisions or are there Fuzzy Boundaries between regions? What defines one section versus another? Do certain instruments play only in some sections or is the instrumentation the same in every section?

Additionally, if there are specific instrumental or vocal parts that you’d like to understand better, try spending an entire listening pass focusing entirely on only one part. For example, the best way to learn how the bass line works in a particular song is to tune out everything else and focus just on the bass line.

Listen Subjectively
In addition to helping you learn how a particular piece of music “works,” active listening can also help you understand your subjective responses to music. For example, are there particular aspects of the song that sound familiar, nostalgic, emotional, etc.? Can you explain why (perhaps with reference to the parameters discussed earlier)? When listening passively, it’s common to have some kind of emotional response. But via active listening, you have a chance to understand what it is, specifically, that causes that response. And once you understand a technique or musical gesture, you’ll be able to adapt it for use in your own music.


## 3. Arbitrary Constraints
My freedom thus consists in my moving about within the narrow frame that I have assigned to myself for each one of my undertakings. I shall go even further: my freedom will be so much the greater and more meaningful the more narrowly I limit my field of action and the more I surround myself with obstacles. Whatever diminishes constraint diminishes strength. The more constraints one imposes, the more one frees oneself of the claims that shackle the spirit.
— Igor Stravinsky, Poetics of Music

Problem:
Music production with a computer offers a limitless field of possibilities. Any sound can be made, manipulated, re-recorded, re-manipulated, etc. But while an infinite range of options might sound appealing, it also means that decision making is hard. The more options you see, the more you need to make active choices about which ones to pursue and which ones to ignore.
Limiting the field of possibilities isn’t just about making it easier to work. It’s also about making it possible to begin at all. If every possible starting direction is equally appealing, how could you ever choose one?


### 19.01.26
Faust Kadenze course
https://faust.grame.fr/tools/editor/
Ctrl-d to launch online documentation on a specific function where your cursor is.


Song Arrangement and Structure

https://ask.audio/articles/8-intelligent-music-arrangement-tips-for-producers

Melody:
A melody consists of, or is characterized by a few elements:

Contour
Range
Intervals
Structure
Scale

Contour
A memorable melody follows a contour, a line that ascends, descends, arches or dips. There’s no particular formula. You don’t have to have a contour that rises and then falls, and you don’t need to have a certain number of drops or leaps. It’s completely down to preference. But you will notice how different contours elicit a different emotional reaction from the listener. For example: a melody that ascends may sound more uplifting than one that descends.

Range
The range is the distance between the highest and lowest note of the melody. Some melodies occupy a very large range (2 octaves and up) while others have a much smaller range (half an octave). Range is important to consider when writing melodies as a wide range will make a melody more difficult to hum, whistle, and remember – whereas a narrow range will have less variation in pitch and won’t sound as interesting.

Intervals
A melody uses more than one note, so there’ll always be at least one melodic interval. Does the melody jump up to certain notes? Or does it move up to them incrementally. It’s handy to know the different intervals and the musical quality they contain.

Structure
Melodies have structure too. You could have an A and B section to your melody, maybe even a C. Think call and response, up and down, etc.

Scale
Melodies are formed from scales. There are numerous types of scales:

Modal: variable patterns of Major/minor scale. Starting at different points
Major and minor: makes up the majority of Western music.
Chromatic: all twelve notes.
Pentatonic Scale: 5-note scale. Often used in blues and rock.

Motif
A motif or phrase is a short musical idea – it might be a few notes placed in a certain order or rhythm, but it isn’t the main feature. Another characteristic of motifs is that they’re generally repeated. Ideas that play frequently throughout the song and may vary slightly from section to section.

## Music Arrangement Basics: Articulation

Here are the topics that we are going to cover over the next 6 weeks:

Pitch
Rhythm
Dynamics
Timbre
Articulation
Level of Activity

### Pitch
A pitch is a reference to a note value that describes how high or low a note sounds.
In music, a note value refers to a note such as C, D, E, F, G, A, B.
Each note has a frequency. For example, note A (A4) above note middle C (C4) has a frequency of 440hz.
The lower the frequency the lower the pitch of a note, the higher the frequency, the higher the pitch of a note.
2. What does this mean in arrangement?

When you string a few notes together of different pitches, you get a melody.
As you would know, catchy melodies are usually melodies that have a good string of notes with varying pitches and rhythm.
Crafting such melodies takes years of experience and attention to details.
Because pitch relates to frequency, your instrument choice or melody you write, ultimately affects the sonic space of your arrangement.

### Articulation
Articulation specifies how individual notes are to be played or performed.
In the classical world, there are four main types of articulation.
Staccato: This means the note is to be played shorter than its duration.
Tenuto: This means the note is held for its full value. Think of it as “leaning” into a note.
Accent: This means that a note is to be played percussively – louder, with a strong attack.
Marcato: This means the note has to be played even louder and harder. Usually used when a band stops right before a break.
2. Articulation & Music Arrangement

You may be wondering why and how articulation would help in producing and arranging music. Although it is not necessary to think of articulation in terms of its “classical” sense, it’s worth having knowledge about it and here’s why:

Articulation will help with dynamics in your arrangement.
Changing the ADSR and velocity parameters of a synth patch or drum hit, can add that extra bit of detail that your arrangement is lacking.
Articulation is the foundation of the groove and vibe of a song. Playing a note hard or soft, holding it longer or shorter, all messes with the groove. Choosing the right kind of articulation for different rhythm and melodic patterns can steer your arrangement closer to where you vision it to be.

### Rhythm
Rhythm is the arrangement of sounds as they move through time.
Specifically, each note and chord have a given time where sound is on and sound is off.
This time value, in music, is known as rhythm notation or note length.
Note lengths are relative to time signatures.
Common time signatures are 4/4, 2/2, 3/4 , 6/8.
2.  What does this mean in arrangement?

Rhythm is one of the most important aspects of music and it’s everywhere.
Most listeners are attracted to songs with a strongly crafted rhythm and groove.
Most listeners would not be able to tell a bad note or a wrong chord, but most would be able to tell when the rhythm goes awry.
Rhythms are usually written in patterns.  A mastery of common rhythm pattens can be great use to you as an arranger.
Check this website out to learn more about developing rhythmic patterns.
3. Practical Applications

To improve your rhythm vocabulary, try writing down or notating your favorite songs. Do this for as many songs as you can and try to find a similar pattern and use that same pattern in your music arrangements.

To put it simply, dynamics means how loud or soft a piece of music is.
In classical music, composers would use Italian terms to mark different dynamic levels.
Terms such as piano (p) means to play a phrase / section, softly or forte (f) means to play loudly.
2. What does this mean in music arrangement?

### Dynamics
Dynamics is a form of expression and you can use it in various musical forms. You can plan out a dynamic structure for the length of your song or even dynamic structure for a specific instrument.
Most pop / electronic music out there do not have much dynamics. That means, the loudest and softest part of the song are almost negligible. This is not ideal as your ears actually perceive music better when there’s a certain level of dynamic activity. What is worth noting is that a more dynamic mix would sound better on streaming platforms.
Dynamics help tell your musical story. The ebb and flow of a dynamic music piece aid with conveying emotions in a song.

Use dynamics to create tension and release. A typical example of dynamics is that you start your intro soft, slowly build dynamics through the verse, thus creating some tension, and release the loudest part of your song at the chorus. The larger the dynamic jumps between sections, the more of a dynamic range you’ll have.
Make your MIDI drums more realistic by programming actual dynamics into the playing. You can vary the velocity of your hi-hat patterns to emulate how an actual drummer would play, or even change the velocity settings on the snare drums from soft to loud between verse and chorus to create a dynamic shift.

### Timbre
Timbre is the unique, distinguishing quality of a sound made by a music instrument.
Timbre allows you to identify two different instruments playing the same note.
For example, both a piano and a guitar may be playing the note C3, but because each instrument has its own unique timbre, the listener is able to identify that there are two different instruments playing.
2. Timbre Qualities & Music Arrangement

It is important to know what an instruments sounds like and to be able to describe it.
Once you know the timbre quality of an instrument, you can make informed decisions on how to use it in your arrangements.
For example, a violin may have a timbre quality of sounding delicate, high and penetrating when played at certain ranges. With that knowledge in hand, you might want to write a violin melody in that range, during a loud chorus to cut through your arrangement.
Similarly, electronic instruments have timbre qualities as well but with greater shaping possibilities.
When you built a sound from scratch, you are deciding the timbre qualities of your sound. The flexibility that synth sounds have over an acoustic instrument is that the timbre of your synth can change in any way you like and is not confined to the physicality of the instrument itself. It that sense, synths are very malleable.
3. Practical Applications
Keep a list of music / timbre descriptors when planning out your arrangement.
Some examples of descriptors are: gloomy, smooth, narrow, metallic, sharp, soft, delicate, bright, round, high, low, damped, sandy.
Tweaking the ADSR curve on your synth helps shape the timbre quality of an instrument.
When arranging, try to listen and isolate the different instruments you have. If you can do that, it means you have a well defined timbre structure. If not, try re-thinking the arrangement and evaluating your sounds based on the descriptors above.

### Level of activity
We would like to assign different sounds / instruments a certain level of activity in a music arrangement.
We categorize activity levels into three types: low, medium, and high.
Levels of activity are essentially the rhythm that an instrument is playing.
Think of activity as a big picture – ask yourself, what pulls your ear into your arrangement? The thing that sticks out the most, most likely has the highest level of activity.

Instruments like pads have a low level of activity. There isn’t much “movement” to attract attention and is meant more of a foundation.
However, certain synth lines or arpeggiated synths have high level of activity that often than not attract the listener’s attention.
Level of activity and tone also go together. If you have a lead line that has a lot of movement, but has a dark tone in the mix, it is just going to create a lot of unnecessary confusion in your arrangement.
Practical Applications
When starting out an arrangement, think of your musical form and which instrument or part should take the highest level of activity.
Should that be the melody? hook? a cool synth part?
Bring instruments / parts forward or backward by giving them different levels of activity. Control what you want your listener to hear.

#### next blog posting?
amateur music making

## 19.02.01 Starting February
- starting with Fmaj riff from a long time ago. tracked in live
- day2 added a second chord progression - could be a chorus
- the theme is clothing and will write about "Coffee coloured shirt"

spills and thrills
drips

## 19.02.07
back from IxD19 Edu Summit, bored by the interaction folks and the same questions.

Ableton video tutorials - finished, good for a tips but very standard song arranging approach.

Masterclass - Hans Zimmer
tempo
Music Diary - write a piece everyday

## Workshop Hardware:
- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported

- [x] Order 8 Workshop Kits for Estonia - April 14-18
- [x] Order Workshop Hardware for Stepanagert - April 20
- [x] Order Workshop Hardware for Dundee - May 24



Eesti Kunstiakadeemia, Interaktsioonidisain
Põhja puiestee 7
Tallinn 10412, Estonia




##19.02.11
## Write experiments on blog

# Experiment 1 Sense Synth
# Experiment 2 Nukulele
# Experiment 3 nSynth
# Experiment 4 Cicada - phone uids into Melodies

publish this one:
# Experiment 5: KalimPi
# Experiment 6 sensor with OhEye - percussion instrument

Optional:
# Experiment 7: Open Theremin
# Experiment 8: Pisound Pedalboard


Electronic circuits:
http://www.pavouk.org/en_index.html


Circuit bending
https://www.youtube.com/watch?v=KHDL9iGxDPM

Music and Tech at SFA
song writing workshop
https://blog.landr.com/write-song/

19.02.22 Conversation with Ryan Betts - digital physical hybrids


Materials for acoustic instruments
 - windshield wipers. or spring steel

voice
percussion - shaker, rattle,
strings - uke



Kalimba - thumb piano
https://www.instructables.com/id/Make-a-Thumb-Piano-Mbira/

Marimba -
https://www.instructables.com/id/Build-Your-Own-Marimba/

Research Project on ResearchGate:
https://www.researchgate.net/project/Sonic-Interactions


Destroy the idea that you have to be constantly working or grinding in order to be successful. Embrace the concept that rest, recovery, reflection are essential parts of the progress towards a successful and happy life - Zach Galifianakis

6 WAYS TO CAPTURE MAGIC IN THE STUDIO
https://blog.kadenze.com/creative-technology/6-ways-to-capture-magic-in-the-studio/

1. Leave the Mic Running
The brief moments that precede and follow recorded audio takes can be packed with valuable content. A drummer sits on the throne and picks up her sticks, gently shutting the hats before giving the four-count. Band members converse excitedly as they enter the tracking room. The prospect of recording your music is exhilarating, and that energy might translate to tape. A live track’s conclusion is no different. Band members can’t help but laugh at how hard they just rocked. A dead-serious frontman sighs “That’s the one” after baring his soul on a lead vocal track. Not to mention, capturing your surroundings is a great way to establish the kind of atmosphere or energy you want your track to embody.

For a lot of people, especially the die-hard fans, an artist revealing their personality is a special gift. By injecting a little humanity into your studio tracks, you can circumvent the sterile fourth wall that separates you from your audience.

2. Go Organic
Everybody’s process is different these days. But one constant across the spectrum of production routines is the use of digital audio processing. MIDI is completely non-destructive, giving you total control over your performance. Plug-ins are sounding better every day, and they’re much cheaper than their hardware counterparts. It’s easy to digitally produce a track, but you may find that something is missing when you listen back. And the answer is organic audio. If you’re shaking your head, thinking there’s no way you could afford the studio time or equipment, stop doing that. It doesn’t take much to infuse a little realness. Chances are you already have a microphone, so add real claps and stomps.

Gabriel only used a computer to layer the 25 overdubs that build the claps & stomps of “6 8″, everything else was recorded directly to 2” tape. His track later got sampled on Drake’s track Jungle, one of his biggest songs to date.

3. Utilize Outtakes and Ad-Libs
If you’re running an efficient session, you know what needs to be captured and you do so methodically. Once you’ve got everything you need from your vocal or instrumental, create a new track and ditch the script. This is a time for the musician to have fun and do what comes naturally. Tracking ad-libs will surely improve the quality of your pop or hip-hop vocal. Allowing your guitarist to jam on the track could result in a fresh lick that drives the hook.
Stimulate these vocal or instrumental fragments further by dropping them into Arcade and using its simple modulation engine to transform a sample into something extraordinary. Inspired moments like this, where anything can happen, are what take productions to the next level.

4. Experiment with Plug-ins
We’ve stressed the importance of organic elements, but let’s be honest. If it weren’t for 1’s and 0’s, modern music as we know it wouldn’t exist. The elements that set you apart in today’s scene will almost certainly require the use of a computer, whether it’s a software instrument or a glossy mix.

Output focuses on instruments and plug-ins that challenge the standing ideologies of artists and producers. From the award-winning Rev to Analog Brass & Winds, our software puts a unique spin on familiar sounds, programmed to facilitate effortless modulation. Tweak some knobs, combine strange textures, or drop in your own samples and let Arcade do the work!

5. Collaborate Often
Sound familiar? Believe it or not, toiling alone in your cave with the curtains drawn for weeks on end will not yield optimal results. Falling into the trap of “I can do it myself” not only limits your resources, but it’s also rather unhealthy for the mind and spirit. Collaboration starts as a seed before blossoming into a full-on rain forest. On the most basic level, working with other minds will expand the possibilities of what your song can become.

The right element to revitalize your track could come from anywhere at any moment, and being open to such an occurrence is paramount. On a broader scale, every positive collaborative experience will lead you to a larger network and, ultimately, more opportunities to work with inspirational artists. Community begets music, so engage yourself with your surroundings and witness real, human magic.

6. …and One for Good Measure
The oldest trick in the book. Once you’ve got the perfect take, especially after straining for hours to do so, you may be tempted to say “got it” before slamming your laptop shut (after saving the project) and heading to Denny’s. After all, they do have delicious all-day breakfast at affordable prices. You may think capturing the take you’ve been after is the knockout punch, but it’s more like your opponent leaving himself open for an uppercut. Once you’ve got a good take, the shackles of a high-pressure recording environment are broken, and your musician can breathe the sweet air of relief. Use this opportunity to get one or two more takes. When “messing up” is no longer a complete failure, an artist can really let go and, more often than not, cut a performance that’s far beyond what you’d imagined.

https://output.com/arcade


learning VR in kadenze
SkyBox -  the cube that is the largest object in your VR World
"Hello Skybox"

Google Expeditions:
create an expedition:
https://support.google.com/edu/expeditions/answer/9103284?hl=en&ref_topic=9007515

https://medium.com/master-of-code-global/how-to-create-virtual-reality-content-5000e0fdd68

https://hackernoon.com/how-to-make-a-vr-app-with-zero-experience-927438e2dede

https://aframe.io/showcase/

# 19.03.06
March 6
creating Bluetooth Device Scanning synth
installed Bluez

found this interesting article about using python to send bluetooth messages - GATT server
https://scribles.net/creating-ble-gatt-server-uart-service-on-raspberry-pi/

more on GATT here:
https://learn.adafruit.com/introduction-to-bluetooth-low-energy/gatt


# 19.03.15
NonInstrument - 2 insta posts

It's unethical to record people's UID numbers, but if I turn it into a melody and record that is it better?

# 19.03.16
Write about Sync and Ableton Link.
expand on playing on the grid, quantization, groove templates and how humans actually play

*- ALSO after listening to 20k podcast on the Booj*

I want to write a piece of music that uses the formula and cliches of a movie trailer:
1. start with a single note
2. build to a quirky cover version of a known track
3. A drop with a booj or two
4. recap the cover with a build
5. second drop
6. the dramatic end


Website How to...
terms and conditions when first entering but never after
https SSL certificate.

#19.03.18
MachineLearning Resource
https://github.com/ml4a/ml4a-guides

# had trouble installing iemlib and zexy libraries on mac pd 0.49.1

ORCA - open source live coding
https://github.com/hundredrabbits/Orca


having trouble getting OI board to not have jumpy values. works with buttons with 10K pulldown resistor.
Looking for how to wire soft pot and found this:
connect a 10K resistor from the middle pin to GND.

    TIP: the soft pot will only work while you're actively
    pressing on it; at other times it will "float" to random
    values. To prevent this, we've added a 10K pull-down resistor
    to the middle pin (output voltage). This will keep the output
    at zero volts when the pot is not being pressed.

# Leonard Paul explaining Pd
https://www.gamasutra.com/view/feature/131258/audio_prototyping_with_pure_data.php

#19.03.20
Soft Pot Arduino code
https://engineersportal.com/blog/2018/10/9/membrane-potentiometer-with-arduino-circular-softpot


#19.03.21
few days left before leaving for Europe.
TO DO:
finalize SIK Disk image and burn 12 SD cards - DONE
Solder:
    Breakout boards - where to put resistors so pots don't jump around- actually, they're fine without them. Just need to solder pins and buy 3wire connector cables. - DONE
    OI boards - DONE
    Mics - DO THIS LATER

Guitar wiring diagrams
    https://robrobinette.com/How_The_TMB_Tone_Stack_Works.htm

#19.03.24
working on IRCAM talk:

twitter: people working on hybrid instruments:
@instrumentslab - Andrew McPherson
@fabiomorreale - Fabio Morreale - Magpick: an augmented Guitar Pick for Nuanced control.
@Halldorophone: an electro-acoustic feedback instrument
@belaplatform -
@g_Giacomo Lepri

Effect of Instrument Structure Alterations on Violin Performance:
Psychology article:
https://www.frontiersin.org/articles/10.3389/fpsyg.2018.02436/full

quick quote:
The relationship between performer and instrument is interesting both for its complexity and for the common experience “that the musical instrument has become part of the body” (Nijs et al., 2009). This phenomenon is explained by the theory of embodied music cognition (Leman, 2008; Loeffler et al., 2016), which suggests that the musical instrument is a mediation technology between the mind and a musical environment. At expert level, the instrument becomes transparent to the performer (Rabardel, 1995; Leman et al., 2010); the bodily operations of manipulating the instrument become automatic, so the performer's full attention can focus on the action of creating music (Nijs et al., 2009). In other words: the challenge of understanding performer-instrument interaction is precisely that the performer is not consciously thinking about the instrument while playing. Neuroscientists attribute this transparency to a number of internalized mental mechanisms that musicians acquire with time which guide complex motor activities needed to operate the instrument as if it was an extension of the body (Repp, 1999; Stambaugh, 2016).

Sound and Physical Interaction (SOPI) @ Aalto
https://sopi.aalto.fi/

Mark Cetilia
http://mark.cetilia.org/about/

AUGMENTED PERFORMANCE LAB in Copenhagen
https://www.en.cph.aau.dk/collaboration/students/labs-campus/labs/augmented-performance-lab.cid309567


#IRCAM::
<ircam talk notes>
    #Why:
    How can our design students rapidly prototype their musical instrument ideas?

    Why do so little of the new instruments being launched have any success?

    How do we create new instruments that have the qualities of acoustic instruments and the capabilities of modern digital music creation tools?

    How do we leverage the nuanced relationship between the player and the instrument to learn how to create better instruments?

    Is there something wrong with the approach of separating the controller/interface from the sound source?

    How can the discipline of interaction design help advance the efforts in Instrument design?

    Ultimately, I'm here to get feedback on this approach
    suggestions on how to improve the workshop
    evolve Sonic Interactions workshop into a formal stream of our IXD program.

    #What:

    Sonic Interactions Workshops
        Requirements: Open Source Affordable Audio Prototyping Kit and iterative process.
        Description: Provocation is to create an instrument that has immediate response and uses digital sound processing (DSP).
        Technology: RaspberryPi, OhEye Board, USBAudio,
        Structure: Combination of sketching, prototyping, coding and testing

    Reflection:
        Coding can be daunting for design students.
        typical programming (computer science) approach doesn't work with designers and it potentially gender-biased

        Interdisciplinary Approach: too much switching hats


    Experiment Quickly
        the whole idea behind rapid prototyping is to quickly create a convincing human trial to evaluate a concept.
        Currently hardly any musical instrument companies use a rapid prototyping method beyond form explorations using 3D printing. Most people working to make new instruments are creating specific one of a kind instruments for personal use and performance.

        The type of prototyping we need to be exploring in "how it works" prototypes rather than "how it looks"

        Fender Musical Instruments
        http://www.rapidprototyping-china.com/de/5-famous-examples-of-companies-using-rapid-prototyping/

        Fender is an American Manufacturer of amplifiers and string instruments and mostly known all over the world for its famous Stratocaster used by many musicians like Eric Clapton and Jimi Hendrix. The Fender was founded in 1946 and has an international reputation for its famous electric, acoustic guitars and warm toned amplifiers. What is not very well known is that Fender uses in-house 3D printing for its rapid prototyping needs. Previously, Fender was using an external service for rapid prototyping its instruments’ parts, but a lead-time of about two weeks was too much for Fender to cope with. So they decided to purchase Stratasys industrial 3D printers and do it in-house. Fender now 3D prints prototypes overnight. According to the company, 3D printing one cubic inch of material costs about $4 USD. This is significantly less than they had to pay before moving the process in-house.

        They utilize this technology for designing say new knobs on their amplifiers, new designs for guitars and much more. Their design team is not just the only group that is happy about this in-house capability. Marketing is thrilled too. For example, they can have a marketing and design meeting to discuss some new ideas and concepts and by the end of the day or next morning have a prototype. This accelerates their innovation and new product development enormously.

    looking inside
    Adrian Borda - http://www.adrianborda.com/
</ircam talk notes>

<IRCAM talk>
    structure:
    intro - who am I
        1. Provocation:
        2. Background: evidence of failures in instrument making (Jammy)
        3. Methods: interviews, surveys, experiments, literary review
        4. Findings: instruments need to be playable, expressive and social
        5. Hybrid instruments - physical/acoustic merged with digital
                social
        6. Workshop new instruments by Hacking traditional ones.
        7. The Kit
        Workshop requirements:
        open-source, embedded, realtime audio processing, audio input, sensor inputs, audio output, accessible for non-programmers.
        8. Workshop Outcomes:
        9. Moving forward
        10. References:

    why hybrid instruments?
    Even today most people have a mental model of how music works based on traditional instruments like piano, guitar and violin. They visualize musical intervals and patterns based on these instruments, most new instruments build on this shared knowledge (ex. Ableton Push, Roli's seafoam keyboard).
    Historically the interface of the instrument has been directly connected to the mechanism that makes the sound, creating a tight coupling of the user's action to the resulting sound. It's this tight auditory coupling that is at the heart of the relationship between the instrument and its player.
    By hacking existing instruments we are forced to use the tight coupling and make something new.

    Books:
    The History of Musical Instruments - Curt Sachs
    Origins and Development of Musical Instruments - Jeremy Montagu
    Studies in Musical Acoustics and Psychoacoustics - Albrecht Schneider
    Psychology for Musicians - Lehmann, Sloboda & Woody

    Analog Days: The Invention and Impact of the Moog Synthesizer - Trevor Pinch
    Pink Noise - Tara Rodgers
    Sonification Handbook -
    Promising Avenues:

    1. Get a Pd external written in C that grabs sensor data with less overhead and latency. Currently the Python script takes 25-30% of CPU processing compared to Pd only using 5-8%.

    x. Add a layer of Machine Learning

    According to the paper: Rapid Prototyping of New Instruments with CodeCircle -
    Zbyszynski, Grierson & Yee-King

    Hartmann, Abdulla, Mittal and Klemer (2007)[7] have identied that "programming by demonstration" with a layer of machine learning technology can expedite the development process and improve results. RapidLib provides a machine learning library which allows such development andfits with the rapid prototyping philosophy of CodeCircle.

    However, timing can be problematic. Web Audio API scripts are run in the browser's main thread, which can be prone to latency and jitter. As this paper will show, connectivity to sensors is also a potential pain point.

    possibles direction: use Rebecca Fiebrink's Wekinator in RPi.
</IRCAM talk>

_______________________________________

<Estonia IxDA talk>
    Designers of Tomorrow
    Description
    We experience so many innovations daily that we tend to forget that before their acceptance, many of these innovations have been met by resistance among the public, or by people whose livelihoods are threatened by them. By looking into past innovation case studies we begin to understand the underlying reasons for resistance of innovations, and become better equipped to propose innovative designs for our uncertain future to become the designers of tomorrow.

    Some things to add:
    how to be a better designer in the future:
        Learn more - up your game by adding new skills
        Be a polymath - bring past life experience into your practice
        Know your big why - ROI isn't enough, how will you make a difference
        Adaptability - how can you become better at change
</Estonia IxDA talk>

<UX Tartu>
    Panel talk

    The Dark Side of Design
    description:
    Dark Patterns are deceptive interactions, designed to mislead or trick users into doing something they don’t want to do. In order to generate more sales, get subscriptions, and hit target numbers designers are constantly asked to create deceiving user interfaces to manipulate users. This talk will be about how we can all become better designers by saying no to Dark Patterns.

    creating Dark Patterns is not a good, ethical practice and how designers should be responsible for their actions and focus on creating transparency with the users instead of manipulating them.

    In terms of web now, Dark Patterns are much more complex and sneaky in nature. Brownlee (Fast Co. Design, 2015) illustrates a key example of that through LinkedIn and its automated follow-up email reminders on a new user’s behalf to any contacts harvested from his or her webmail accounts, which are presented in such a way that they appear as if they came directly from the user.

    Campbell-Dollaghan (Fast Co. Design, 2017) believes that we are in a new era now. The main goal of the old world was to design ways to engage and make their lives easier but for the new era, she encourages us to “give users the agency to understand and challenge the products they’re being sold.”
    https://www.fastcodesign.com/3066586/the-year-dark-patterns-won

    There are no easy solutions or alternatives to dark patterns. Industry insiders like Bunker (2013) have suggested that designers should have an ethical code of conduct where privacy, honesty and respect should be the core elements.

    “It’s become substantially worse,” agrees Brignull, discussing the practice he began critically chronicling almost a decade ago. “Tech companies are constantly in the international news for unethical behavior. This wasn’t the case 5-6 years ago. Their use of dark patterns is the tip of the iceberg. Unethical UI is a tiny thing compared to unethical business strategy.”
</UX Tartu>


*- Stuff to prepare for:
1. is it 3 or 4 day workshop for EKA ?
2. make 3 day schedule
3. Prepare laptops with Pd, Bring headphones & bluetooth speaker(if you have one)
4. prep workshop space - projector, speakers, wifi
-*

### challenge 1 : create a sonic composition that communicates a story (woke up this morning and...)

### challenge 2 : create a non-verbal sonic conversation between two workshop participants

### challenge 3: create a composition with 3 or more instruments, remember to use silence as an essential part of music making


# Workshop Schedule:

## Day 1
- AM:: Basics
- Introduction Slides
<Day01>
    <activity>
    ### Activity 1
        Make your first object

        To place an object on the canvas, select Put→Object from the menu or use
        CTRL+1 on the keyboard.
        An active, dotted box will appear.
        Move it somewhere on the canvas using the mouse and click to fix it in place.
        You can now type the name of the new object, so type the multiplication character into the box. When you have finished typing, click anywhere on the
        blank canvas to complete the operation.
        When Pure Data recognizes the object name you give, it immediately changes the object box boundary to a solid line and adds a number of inlets and outlets. You should see a |* | on the canvas now.
    </activity>
<Pd-exercises-1>
### Pd Exercises 1:
        01.hello_world.pd
        02.basic_elements.pd
        03.getting_help.pd
        04.edit_mode.pd
        05.objects.pd
        06.connections.pd
        07.messages.pd
        08.hello_audio.pd - Simple OSC and DAC
        09-audio-creation.pd - Simple OSC and DAC - changing frequency
        10-midi-notes.pd - OSC to play notes (mtof)
        11-rounding-notes.pd - Slider, basic rounding
        12-waveform.pd - Table, tabwrite
        13-waveform-2.pd - Sin wave in Table refreshed
        14-optional-samplehold.pd - Sin and Square waves combined using sampleHold
</Pd-exercises-1>

- PM::
<activity 2>
    ### Activity 2
        Play together:
        everyone plays individual notes of a chord progression.

        1. program in the notes for each person.
        2. establish a tempo and a progression (use a drum beat to keep time)
        3. rehearse and then record.
        4. improvise and record again

        One Thing - San Holo
        https://www.edmprod.com/theory-arrangement-san-holo-one-thing/

        C#min – A – E – F#min || C#min – A – E – B ||

        C#min:  C# – E – G# (midi: 61, 64, 68)
        A:      C# – E - A (midi: 61, 64, 69)
        E:      B - E – G# (midi: 59, 64, 68)
        F#min:  C# - F# – A  (midi: 61, 66, 69)

        C#min:  C# – E – G# (midi: 61, 64, 68)
        A:      C# – E - A (midi: 61, 64, 69)
        E:      B - E – G# (midi: 59, 64, 68)
        B:      B – D# - F# (midi: 61, 63, 66)
</activity 2>
<sketch>
    # Activity 3
        a scenario where two people are communicating non-verbally using sound/music.
        describe/act/draw
        list the features of the instrument
        draw the instrument and how one interacts with it.
</sketch>
</Day01>

## Day02
    - AM:
    - Sequencing & Hardware

<Day02>
<activity 3>
    ### Activity 3
    Play a generative piece of music
</activity 3>

<Pd-exercises-2>
## Pd Exercises 1:
    15.pd - Simple Sequencer - write your own melody
    16.pd - writing values to a table a- simple
    17.pd - writing values to a table b. better visually
    18.pd - Write values to table with expression
    19.pd - Changing shape of note with |line~|
    19.pd - Envelope with Subpatch
    20.pd - Subpatch with BPM
    21.pd - Ableton_sync_sequencer
    Challenge: Add random into sequence
    22.pd - Samplehold-with-OI
    23.pd - input-with-OI
    24.pd - OI-master
    25.pd - Simple OI synth
</pd-exercises-2>
#   PM::
    <Activity 2>
        Everyone's Blues:
        compose a C major pentatonic melody
            Midi note numbers for C Pentatonic
            36 38 40 43 45
            48 50 52 55 57
            60 62 64 67 69
            72 74 76 79 81

        Play:
        pick a drumbeat on PO-16
        trading bars: first 1 then 2 then 4 bars.zx

        Option:
        Issue: talk about the issue of synchronization
        introduce Ableton Link and create sequencer that syncs to my master Live
        * is everyone on the same network?
    </Activity 2>
    <prototype>
        create the simplest prototype of your concept
        what does it do in its simplest form
        create the Pd version and move it over to the Pi
    </prototype>
</Day02>

<Day03>
#   Audio & Hardware
#   AM::

<pd-exercises-3>
Audio Sampler
Hardware and mapping
button
Potentiometer
Sensor (soft Potentiometer)
xbox controller

</pd-exercises-3>
<activity 3>
    ### Sample This:
        Find/Steal/Record 3 Samples
        prepare a sampler with 3 samples, play with drum loop and others.
        Load these into the sampler.pd

    Play:
    pick a drumbeat on PO-16
    trading bars: first 1 then 2 then 4 bars.

    Issue: talk about the issue of synchronization
    introduce Ableton Link and create sequencer that syncs to my master Live
        everyone on the same network.
</Activity 3>
</Day03>


TODO:
- [x] Learn how to fold in Atom, what makes a fold, a tag?
- [x] Make an Ableton track in C major and A minor blues.
- [x] Finish Designers of Tomorrow talk
- [x] Finish UX Dark Patterns talk

## idea: abandoned music, give life to ideas that are left as orphans. give them a home on the web -keyword searchable db

https://wp.nyu.edu/maxhorwich/2017/11/13/generative-drone-music-in-p5/

19.05.01
Algorithmic composition
https://medium.com/processing-foundation/a-platform-for-algorithmic-composition-on-p5-js-271cd872d648

https://github.com/processing/p5.js/blob/master/developer_docs/project_wrapups/junshern_gsoc_2018.md

https://junshern.github.io/algorithmic-music-tutorial/part1.html


19.05.02
VR Coding:

https://developer.mozilla.org/en-US/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_A-Frame#Environment_setup

Reminded of Lerp function in P5 from Shiffman
# use to finish zooming photo Mike Bernard project
make photoframe that tracks your eyes. digital mona lisa

setting up my Strats with perfect action:
https://www.youtube.com/watch?v=1dEZxBykRto

19.05.07
working on DiStefanoJaud site, researching wp themes:
Impreza seems very fexible $50

look into sliders: Soliloquy and Revolution slider
https://soliloquywp.com/


TODO:
- [ ] Learn how to use Impreza
    [x] home page - full screen slides with text
    [x] Revolution slider - posts and animations
    [x] portfolio - projects, categories
    [ ] figure out different grid options for porfolio
    [x] Project pages - slider
    [X] Sticky header
    [ ] Bring content under header for Main pages - not for Posts & Pages
- [x] People custom post type - don't need a separate type, use page
- [x] Studio pages
- [x] Contact page
- [ ] instagram feed in sidebar
- [ ] Figure out DiStefano hosting

#19.05.08
trying to figure out Impreza theme options. layout, grid and custom layout for home page.

Questions for DiStephano:
how do we want to categorize the projects:
commercial, industrial, institutional, residential?
where do you see testimonials fit in and do you have any?
The newsletter is something we should aim for but after we launch the site and we're confident that the content for a letter is achievable monthly.


#19.05.09
Trip to Brussels to visit Chris

* IDEAS
# Build a html/JS Fullscreen Video & Audio Player like Mirror.fm one and put content on music.haigarmen.com or inner.haigarmen.com

# Fermata podcast name, get a domain for it.

Songwriting Tips:
https://www.secretsofsongwriting.com/

#19.05.12

Rewrite my website copy:

For over 25 years I have been dedicated to becoming a great designer. Driven to learn everything there is to know about how we interact with each other, creating digital tools for work, creativity and music. My career has taken me to many unexpected places, which has lead me to teaching the next generation of designers.

But it wasn't always like this. There was a time when I worked as a professional musician playing guitar all the time. For years I practiced my craft during the day and gigged constantly. I studied composition and jazz in university in Montréal and graduated with a huge interest in using computers as a tool in the studio. You have to consider that was the mid 1990s and there just wasn't that many people doing that. But this was a pivotal time for me, I realized that I interested in how we use technology in our creative practice. It's a theme that remains in almost all the work I do.

But before that, I studied architecture and began working in an architectural firm in Ottawa. Over the years I have designed corporate identities, book covers, ads, album artwork and promotional items for prestigious clients like CBC, BMW, Chanel & Nokia. I used to run a design agency called LiFT studios and currently I'm a professor at Emily Carr University.All that to say, I've had a lot of interests throughout my life and not surprisingly, I'm focusing on where music and design intersect.

If you're interested in a more official Biography click here


#19.05.15
Working on DJA website, learning new WP stuff, Impreza theme, Revolution Slider, WPBakery PageBuilder

# 19.06.17
Algorithmic Composition Workshop at Tumo
Day 1: lessons 1-13
Day 2: review of 1-10, lessons 10-20 Sound Part I (lessons 1-5)
Day 3: review of For loop, Lessons 21-30
    20-24 grid (loop in loop),
    25-
Sound Part II
OOP -
Random vs. Noise:


# 19.06.26
Prepping VR Workshop

here's what's written so far:
Global Social Place-making in VR

This design seminar explores the way in which physical places may intersect, digitally. VR seems the obvious platform for an immersion of this sort, however designing for specific (yet broadly geographic) intersections (for example, the Armenian diaspora) would seem to require a closer analysis of our relationship (the “viewer”’s relationship, that is) to place and perspective.

This two week design seminar focuses on a theory of place in the 21st century met with an approach to designing meaningful immersive, digital experiences. Students will select local and (globally) remote sites that they want to connect in the context of the Armenian diaspora and create physical-virtual experiences to achieve this end. For example, a public park in Yerevan may have affinitive physical, emotional, cultural, digital or even aural touchpoints with a street corner in East Hollywood, CA.

These explorations draw upon historical works and theory presented by various media and fine artists including Robert Smithson, Andy Goldsworth, James Turell and Kit Galloway and Sherrie Rabinowitz:

“Public Communications Structures"
https://iphiahenry.wordpress.com/2011/09/20/hole-in-space-1980-installation-by-kit-galloway-and-sherrie-rabinowitz

“Site / Non-site Dialectic"
https://www.robertsmithson.com/essays/provisional.htm

“Sensing Spaces"
https://www.artinamericamagazine.com/news-features/news/space-conditioning-james-turrell-and-las-vegas/

“Ephemerality and collaborations with Nature"
http://text-relations.blogspot.com/2010/12/andy-goldsworthys-transient-art.html

Overall, the coursework will allow students to explore critical decision and experience making around the notion of connected spaces. Key questions of why site selection and framing are important for the meaningful construction of virtual-digital experiences as well as range of allowable human interaction within that frame will be considered. Applied learning is relevant to a variety of contexts including commercial retail, scientific exploration, geo-social cultural cohesion and emerging communications modalities.

Students will be expected to work in teams of 2-3 and collaborate using shared or unique skill sets.

Coursework >>

Working from the Imagination
Mon July 1 -
Writing the Place
	- 1.5 hour intro + review of VR media: early approaches + current examples*

    Primary experience aspects to discuss:

   			- “Spectacle”
   			- “Communication”
   			- “Immersion”
   			- “Utility / Control"
   			- “Empathy”
   			- “Movement / Transition”
   			- “Sound / Voice / Music”
   			- “Instruction / Orientation”
   			- “Performance”
   			- “Presence"

   		- How are these aspects related? Do they happen exclusively? Do they work in concert? What are your impressions of what works and what doesn’t?
	- Discussion: What are these experiences trying to do?

ASSIGNMENT: Write the Place: creating the (meaningful) narrative
    INPUTS: discussion/presentation of exemplar media design and related technology platforms, research site selections: local + remote
    OUTPUTS: draft storyline of site selection relative meaning/importance, description of desired interactions within the storyline “frame"


--
Tues July 2 - Draw the Encounter: choreograph the sequence of events
    INPUTS: written site narrative, material + technological choices
    OUTPUTS: storyboards depicting an “experience loop” that will connect the local + remote sites

Working in the Field
Wed July 3 - Figure the Ground: site visits and documentation
    INPUTS: local site documentation, mapping/modeling of remote sites
    OUTPUTS: co-locative model depicting associated geographies and key interactive touch-points and behaviors
--
Thu July 4 - We, the People: understanding who inhabits the site
    INPUTS: ethnographic research interviews with humans in-and-around local site selections
    OUTPUTS: Persona synthesis documentation describing target audiences

Studio Charette A
Fri July 5 - Rack Focus: blending the encounter, creating depth-of-field
    INPUTS: Imagination + Field work from Week 1
    OUTPUTS: develop tool + material experience design approaches; begin creating physical and digital “dioramas” using selected tools (eg. Unity, VR tool-kits, analog studio modeling tools and materials)

—
Studio Charette B
Mon July 8 - Building (either site installation or diorama scale depending on complexity)
    INPUTS: material + tool strategy
    OUTPUTS: draft physical + digital site models
--
Tues July 9 - Rendering (digital intersection component(s) )
    INPUTS: Physical + digital site models
    OUTPUTS: draft interactive “instrumentation” of key experience touch-points

Studio Charette C
Wed July 10 - Blending (digital / physical “node” link-up…connecting d-site + p-site)
    INPUTS: photo, video, scanned, rendered, other site constructural elements
    OUTPUTS: prototype ensembles
--
Thu July 11 - Testing (working out the kinks all day)
    INPUTS: All tools + materials to date
    OUTPUTS: Final physical-virtual site constructs

Presentations
Fri July 12 - Seeing ( share + critique )
\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\

VR Shared Design Guidelines

1. Support Shared Space:
To make the most of a room-scale setup,
design experiences should allow participants to come
together in the shared space when needed and explore individually at other times. This will support the most flexible
use of each person’s physical space while still supporting realwalking for locomotion and natural interaction in the shared
virtual space.
For example, remote collaboration for designing
a large 3D object together where both need to freely move
around the static objects or in education scenarios like viewing the structure of an enlarged molecule and viewing and
manipulating it together.

2. Leverage Asymmetry:
Instead of trying to fit all types of tasks
into one type of shared virtual space, designers should leverage the inherent advantages of the different sizes of tracked
spaces and the resulting mappings to design relevant tasks.

3. Design for Interaction:
While all users are located in different
physical spaces, they perceive the same virtual world. All
users can hear and see each other doing things in VR, even if
they are not directly involved with one another which can be
distracting or engaging. Design tasks that require teamwork
through shared manipulation of objects for creating challenging, productive and fun experiences.

4. Natural Embodiment:
Our empirical work confirms that participants highly valued having an avatar that gave them the ability to ‘‘physically’’ and naturally engage with each other.

Similarly physically walking in VR provided a satisfying sense
of presence. We encourage creators of collaborate VREs to design shared spaces and interactions using natural locomotion,
and only use teleportation or related transportation methods
as a fallback technique if the space limit requires it.


#19.07.02
A-frame examples:
Tues:
1-hello World - primitives
2-360 image

Wed:
3-360 & primitives
4-import model

Thurs:
5-links

Friday:
6-Networked VR


# note: have made a subdomain with WP at music.haigarmen.com. start thinking about how to publish music there. original compositions, videos, performances, gig schedule,


#19.07.09
Week two of VR workshop:
need to figure out how to do the following:
[ ] bg images in networked vr
[ ] switching scenes
[X] simple animation

[ ] ambient music loops

#19.07.11
# getting Node.js running on my server
install node.js first - pretty straight forward
https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04

installed PM2

then configured to run
sudo pm2 start /var/www/vr.haigarmen.com/server/easyrtc-server.js systemctl

sometimes doesn't work unless you do this:
npm cache clean --force


DistefanoJaud time blocks
5 days (x 5hrs) in May = 25 hrs
1 day (x 5hrs) June = 5hrs
July 4 call - 1 hr
july 17 - 5hrs

#19.07.18
started working on Derik Giner site.
 - installed Impreza, updated and build the child theme.
login: admin
pwrd: DosTan.9

DiStefano Home page white logo but iPad needs color version:

.home .ush_image_1{
    content: url(http://test.distefanojaud.com/wp-content/uploads/2019/06/logo-distefano-jaud-architecture-white.png);
}
<!-- this works for home page but then it can't be overridden -->

@media (max-width: 899px) and (min-width: 600px)
.home .w_image_h {
    content: url(http://test.distefanojaud.com/wp-content/uploads/2019/06/logo-distefano-jaud-architecture.png);
}

#Final Revisions from Pablo - July 26

[ ] Footer line breaks on mobile
[X] Title on Team member page vertical align - 2 column
[X] Instagram vertical alignment
[X] Instagram title could be smaller
[X] Social media links in footer

[ ] Floor Plans for project pages

#Final Revisions for DistefanoJaud from Pablo - Sept 22

[X] Check Instagram account link (latest Instagram posts not loading)

[X] Please send me the Wordpress login information, I will start loading some projects for them.

[X] Fix the spelling of “Essence” on the Home Page, it is also hard to read on the photograph

[X] They mentioned you were going to add some icons to each project, that would work as link to associated “Notes” entries.

[X] Logo not seen on some projects, check mobile view too.
Logo - 50% black for Distefano, 20% architecture

Oct 7th DistefanoJaud
[X] update theme 1 hr
[X] upload Images 2 hrs
[X] upload new projects 5 hrs
[x] links to postings 1 hr

Oct 14th DiStefanoJaud
[X] upload favicon
[X] upload writing for prato 1
[ ] upload images for prato 1
[X] upload writing for prato 2
[ ] upload images for prato 2
[X] upload content for Eco Lock

Oct 15th
[X] Husky Autism (remove what is currently there)
[X] St Michael (add plan besides text)
[X] Water St. Condos (add plan)

Nov 9th
[X] Update Theme and Plugins - 1hr
[X] make logo smaller in iPad size 2hrs
[X] get rid of grey above header in iPad view (not quite fixed)



Composition and improvisation
Difference between song and performance
https://en.wikipedia.org/wiki/Sonic_interaction_design

http://www.naturalpatterns.no/composition-and-improvisation/
Improvisation is an ongoing dialogue, and is usually based on communication from the very moment it starts, with other improvisers and the audience. Composing music on paper is usually a solitary process until just before it is performed.

The improviser works with spontaneity and intuition in real-time, and can not change something which has been played. The composer, on the other hand, can plan the length and timing of every event in a musical structure if she wants to, change her mind and move sections around as many times as she’d like.

In the article ”Generating and Organizing Variety in the Arts” (Eno 1981), Brian Eno describes a ”scale of orientation” or continuum from right to left between composition ”tending to subdue variety in performance”, i.e. predetermined music, for example prerecorded electronic music on the extreme right, and composition ”tending to encourage variety in performance”, i.e free improvisation, on the extreme left. In between we find all the facets from classical music, various folk musics to jazz, free jazz etc.  Even free improvisation is rarely completely invented on the spot, but influenced by a host of different factors.


the design of these interactions brings under a magnifying glass a phenomenon that has recently received a great deal of attention on the part of psychologists interested in perception: the tight coupling between auditory perception and action [3].

In this design, the athletes' movements modulated the auditory feedback in real time. In turn, the sound helped the athletes to adapt their movements. Sounds had a great advantage in this case, because auditory perception and action are naturally and tightly coupled. Therefore, the intention was that the rowers would not be expected to consciously “decode” the information conveyed by the sounds, nor to think about how modifying their action would modify the sound. The sound-action loop was supposed to be intuitive. After all, this is what happens in “natural” interactions through sound.

Alan Kay's tennis example

Sonic interactions have a great potential to help a user become more proficient at the fine movements required in sports, as illustrated by the rowing example, but also in music, dance, surgery, and the complicated manipulation of tools

Ableton Making Music book
https://makingmusic.ableton.com/

#idea:
Making a generative piece of music by randomly launching clips with the follow action function in Live.


Alex Ruthmann - NYU met him at IRCAM and Music Experience Design Lab
collaborative design focus
https://wp.nyu.edu/musedlab/

Designing for Music & Emerging Technologies

Notes from Podcast NYU-Music
what constitutes Music Technology?
what is emergent?
physical/digital interfaces for music making and interdisciplinary connections among coding, music, technology, and creative expression.

deterministic stances - manifestation artistic endeavour that enables creative possibilites or it can used in an instrumental sense to dehumanize human reality.

Music Therapy - what are the design opportunities?


Hacking Music - hackathon or Meetup


https://forum.ircam.fr/article/detail/report-ateliers-du-forum/


SABBATICAL Presentation:

IRCAM
The Ircam Forum Workshop, a key international meeting with a community at the crossroads of arts and science composed of young designers, developers and teachers.

This year, from March 26-29, we’ve welcomed 300 participants from 17 countries. Mexico, Brazil, Canada, USA, Denmark, England, Sweden, Germany, Switzerland, Italy, Spain, Taiwan, New Zealand, France, Russia, Turkey, South Korea) including hundreds of students and designers. Over 3 days we attended 50 presentations, 11 hands-on workshops.

Garth Paine
https://zkm.de/en/media/video/garth-paine-future-perfect
https://wp.nyu.edu/musedlab/

art of craft
http://guitarcrafthistory.com/the-guitar-circle-book/one-the-art-of-craft

Design of Musical Instruments - camille goudeseune
The design of musical instruments illuminates XD, because these devices present interfaces that are sophisticated, elaborate, refined over centuries, beautiful, and exquisitely adapted to the shape, capabilities, and senses of the human body. (To prepare for rapid change in the future, it helps to take a long view of history.) The interfaces of musical instruments also demand—and reward—tens of thousands of hours of continued use and study.

http://guitarcrafthistory.com/the-primary-exercises

#19.11.27
Creative Tech Talk Description

Back from his year sabbatical abroad, Haig Armen presents his exploration of music, technology and design. Haig's research focuses on new ways of teaching and learning about designing and rapidly prototyping musical instruments through his research in Sonic Interactions. In his talk Haig details the Instruments Hacking workshops he facilitated in 6 different countries, demos a few outcomes and outlines future plans. Along the way Haig reflects on the importance of switching hats, shifting gears and hacking creativity.

Multidisciplinarity draws on knowledge from different disciplines but stays within their boundaries.

Interdisciplinarity analyzes, synthesizes and harmonizes links between disciplines into a coordinated and coherent whole.

Transdisciplinarity integrates the natural, social and health sciences in a humanities context, and transcends their traditional boundaries


portfolios

http://davidgurman.com/


five steps to a successful sabbatical
https://www.sciencemag.org/features/2013/02/five-steps-successful-sabbatical

1. Go for it
Go all the way, they say. Take a full year and go to a new city or country, if you can.
“Being out of your ordinary surroundings makes you establish new relationships and collaborations and lets you present your ideas in a different context.” People in other countries or even other institutes view your work from a different perspective, informed by the prevailing models and cultures of their region, he says. “They give you reactions and feedback that are different from your colleagues at home.”

2. Get Away
“The trick to a sabbatical is getting away from things that you find stressful,” says Paul Spector, professor of organizational psychology at the University of South Florida, and an author on the study. “Don’t just do the same work somewhere else.”

3. Be Organized
You’ll need details about your research plan, your collaborative arrangement, and the facilities at the institute you will be visiting. In addition, you must describe the larger goals of your fellowship and how you intend to achieve them. The application requires a complete description of your collaborator’s research and mentoring achievements

4. Expect the unexpected
“It’s exceeding unlikely that your sabbatical project will proceed exactly as planned, but be open-minded and you’ll see opportunities for collaborations and other sources of value that you didn’t see going into your sabbatical.”

5. Let Them Come to you

Sabbatical
Sabbaticals are no longer defined by the absence of work - in fact they are defined by the presence of it. Sabbaticals aren’t just another word for vacation. They’re the new word for an active pursuit of purpose.

It’s not a vacation in the lying-on-the-beach-sipping-margaritas sense. But rather an opportunity to refresh your mind and body while continuing to do your most important work.

Sagmeister
“Like many things in my life that I actually love, I adapt to them and over time, get bored by them. And in our case, our work started to look the same.”

Sagmeister first got the idea to take a work sabbatical when he started to think about the typical flow of our lives. By his estimate, most people spend 25 years learning, 40 years working their career, and then 15-plus years in retirement. But, Sagmeister thought, what if we cut off five years of retirement and interspersed them in between the working years?

DISCONNECT FOR ONE HOUR A DAY
According to Harvard psychologist Shelley Carson, one of the best ways to enhance your creative output is to separate work and consumption. As she explains, even taking an hour a day to be in an “absorb” state where you gather information and inspiration without doing any work can be an easy way to get new ideas.

If you’re feeling stressed, unmotivated, and burnt out, there’s no point in trying to just push through.

Instead, our best ideas often come when we’re not working. And a sabbatical–no matter how long–is a fantastic way to rest and rethink how you’re approaching hard problems.


# Polymaths
Being a polymath depends on how one is presented for individual opinions of the concept. For instance: Is a master of arts or sciences that are closer to each other — such as sociology and anthropology — a polymath as much as a master of contrasting ways — such as civil engineering and philosophy — is a polymath? How deep must be one’s knowledge in order to be a polymath?

Polymaths wanted at London Interdisciplinary School, Britain’s first new university in 40 years
https://www.thetimes.co.uk/article/polymaths-wanted-at-london-interdisciplinary-school-britain-s-first-new-university-in-40-years-k0hbzdvlq

A polymath is someone who excels at manipulating knowledge in different fields on their behalf through mastery of three ways: collecting, managing, and investing in information.
https://medium.com/better-marketing/on-the-persistence-of-polymaths-dd63ea6f48e

In the UK at least, higher education is overly focused on narrow degree courses, where institutional prestige often trumps the value of what students are taught. This reflects an academic research community in which various perverse incentives have created a climate where the arts and humanities have become overspecialised, often to the point of irrelevance, and where the whole integrity of the social and life sciences is threatened by the replication crisis.

https://www.theguardian.com/commentisfree/2019/mar/04/young-people-education-political-university-students

Replication Crisis:
https://www.theguardian.com/science/audio/2018/mar/19/a-neuroscientist-explains-psychologys-replication-crisis-podcast


### Dev Tools for Music
# 2019.12.31

OpenFrameworks - on RPi
Camomile - Pd to plugin
Docker - local to prod vm

today TO DO
[x] update xCode
    - xCode 11.2 doesn't work with Sierra 10.12.6
    - deleted xCode 9.0 now downloading 9.2
[x] update python, homebrew, node...
[x] try building Camomile plugin - FAIL
[x] Camomile compiled
[ ] Create Camomile Pd plugin
    https://github.com/pierreguillot/Camomile/wiki/How-to-generate-plugins

[x] OpenFrameworks update 0.11
[x] build OF Projects to test
[] continue OF tutorials -

[] build pedal breadboard - audio jacks, power and footswitch
