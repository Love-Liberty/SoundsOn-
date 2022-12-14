# SoundsOn!

SoundOn! is a software and hardware project combining two exsiting sketches plus electronic hardware.

There is a model control system called Digital Command Control ("DCC") which sends high requency digital signals through rails to control multiple model locomotives. The basic controls are direction and speed, but also momentum and lights and other controls.

An expensive extra is that the locomotives have a chip and speaker fitted internally to produce machine sounds and sounds like whistles, horns or bells.

SoundsOn! is a project to imitate this in a lower quality and much cheaper way.

The DCC sniffer sketch picks up the signals from the rails.

SoundsOn! then decodes those signals and decides what sounds are to be played by the DFPlayer MP3 chip.

This is then played through an amplifier in stereo in the region of the models rather than coming from within the individual models.

The cost of the sniffer electronics, arduino, mp3 chip and amplifier should be less than a single sound enabled decoder for just one locomotive.

The end result will be inferior, but vastly cheaper.

Important Notes
The wires from the Arduino to the mp3 player need to be short & direct. The electronics for picking up the DCC need to include an optical isoloation to protect the logic from the 15V of the DCC system.

The DCC software will be different depending on the make/model of the DCC controller.

You need to choose sound effect and how to organise these on the SD memory card.

The DFPlayerMini_fast software auto outputs things to the serial monitor which are not well explained. It reports communications between the Arduino & the DFPlayer.

Should I use the original https://github.com/DFRobot/DFRobotDFPlayerMini ?

Oddly I found that the folder titled "mp3" containing a track numbered 0001.mp3 will not be played by asking for track 1. It will be played by asking to play track 0. This odd behaviour seems to have stopped

SoundOn! will probably be using the individual folders on the SD card and not the "mp3" or "advertising" folders, but not currently sure.

If you use the playFromMP3Folder() or playAdvertisement() functions, the files to be played must be organised one way:

The "MP3 Folder" must be in the root of the storage device (such as a MicroSD card)

It must be called "mp3" (without the quote marks).

If you want a folder used for short interruptions to main audio playback it must also be in the root of the storage device it must be called "advert".

In the mp3 folder the audio files are named like this 0001.mp3 0002.mp3 but can have names after the numbers.  0001clang.mp3

Other folders are named just with digits 01 to 99 (no names, as far as I know)

audio files within those folders are 001 - 999 (no names, as far as I know)

Currently (Nov 9 2022) using the DFPlayerMini_fast Library, but not sure this is any better than the original robot version.

API:
Note: when using these you have to append the name of the player that is declared at the beginning of the sketch. (See example)

bool begin(Stream& stream, bool debug, unsigned long threshold=100);

void playNext();
void playPrevious();
void play(uint16_t trackNum);
void stop();
void playFromMP3Folder(uint16_t trackNum);
void playAdvertisement(uint16_t trackNum);
void stopAdvertisement();
void incVolume();
void decVolume();
void volume(uint8_t volume);
void EQSelect(uint8_t setting);
void loop(uint16_t trackNum);
void playbackSource(uint8_t source);
void standbyMode();
void normalMode();
void reset();
void resume();
void pause();
void playFolder(uint8_t folderNum, uint8_t trackNum);
void playLargeFolder(uint8_t folderNum, uint16_t trackNum);
void volumeAdjustSet(uint8_t gain);
void startRepeatPlay();
void stopRepeatPlay();
void repeatFolder(uint16_t folder);
void randomAll();
void startRepeat();
void stopRepeat();
void startDAC();
void stopDAC();
void sleep();
void wakeUp();

bool isPlaying(); This returns the number of the current track or 0 if nothing playing
int16_t currentVolume(); This returns an integer from 0 to 30
int16_t currentEQ();
int16_t currentMode();
int16_t currentVersion();
int16_t numUsbTracks();
int16_t numSdTracks();
int16_t numFlashTracks();
int16_t currentUsbTrack();
int16_t currentSdTrack();
int16_t currentFlashTrack();
int16_t numTracksInFolder(uint8_t folder);
int16_t numFolders();  This possibly doesn't work as I am getting a very large wrong number

void setTimeout(unsigned long threshold);
void findChecksum(stack& _stack);
void sendData();
void flush();
int16_t query(uint8_t cmd, uint8_t msb=0, uint8_t lsb=0);
bool parseFeedback();

void printStack(stack _stack);
void printError();

The chip has 2 ground pins. I don't think it matters which you use or whether you tie them together. The soundOn! uses pins A4 & A5 to do serial connection because of the DCC hardware being used, but almost any 2 pins will do. If polarity gets swicthed by accident the chips will partly function in strange ways. Caused me much confusion

DFPlayer Mini Pinout:
550px-Miniplayer_pin_map

DFPlayer Mini Pin Description:
Pin_map_desc_en

Example Wiring Diagram:
550px-PlayerMini

DFPlayer hardware manual with hex codes

https://github.com/DFRobot/DFRobotDFPlayerMini/blob/master/doc/FN-M16P%2BEmbedded%2BMP3%2BAudio%2BModule%2BDatasheet.pdf


----------------------

> See the documentation for DFPlayerMini_fast<

For more info: https://www.dfrobot.com/wiki/index.php/DFPlayer_Mini_SKU:DFR0299

-----------------------

The DCC part of the project is based on

https://github.com/cbries/modeling/blob/master/Arduino%20DCC/Arduino_DCC_S88/RB_DCC_Sniffer_v2/RB_DCC_Sniffer_v2.ino

Sources: 
DCC packet capture: Robin McKay, March 2014
DCC packet analyze: by Ruud Boer, October 2015
Version 2.0: J??rgen Winkler, March 2016

------------------------------------

I have added some indicator LEDs to the sniffer sketch such that a press on the DCC controller is detected and the LED activated.
A limitation of the hardware circuit board is that only a few of the Arduino pins are accessible. So can only use pins 3,4,5,6 and A4 A5
SoundsOn! uses pins A4 A5 for serial communication with the MP3 player. 3,4,5,6 may be used either as inputs or indicator lights. Not yet decided

--------------------

-----------
Nov 10 2022
-----------
Separately have working 
DCC sniffer software which gives output LED when buttons pressed
MP3 playing from folders on SD card. (Testing with music)

To do:
Put sound FX on SD card
Combine the two sketches
Such that pressing button on DCC controller plays the relevant sound effect
// 

