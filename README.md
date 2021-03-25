# Jacktrip Max Mixer

These [Max](https://cycling74.com/products/max) patches provide mixing/monitoring functions for a small group of musicians using [JackTrip](https://www.jacktrip.org/index.html) to connect through a self-managed, macOS server. 

The server patch allows for the creation of individual monitor mixes for each JackTrip client, as well as a master mix that uses the new "broadcast" feature in JackTrip v1.3. The server patch accommodates up to 6 musicians and contains notes on channel number assignments for using a virtual audio device (Soundflower) to route between JACK and Max.

The optional player patch, which can be used on a macOS or Windows computer, allows a player to remotely control the individual monitor mix they receive from the server.

## Server patch requirements

* A self-managed JackTrip server on macOS (i.e. JACK/Qjackctl, JackTrip, required port forwarding, etc.).
* Max 8 (purchase not required to run, only to edit). The free AudioMix package must also be installed via Max's Package Manager.
* A virtual audio device with at least 40 channels, such as Soundflower.
* It's not technically required but you will lose your mind without JMess, a utility for saving JACK routing configutations.
* If players will be controlling their own mixes remotely, the appropriate UDP ports need to be open on the server (see "setup notes" tab in server patch for details).

### Windows and Linux servers note
* The Max patches should work on a Windows server, but you would need to find a solution for the virtual audio device (with dozens of channels); an app for saving JACK routing configurations (because JMess does not work on Windows); and either substitute a VST reverb plugin (because the AUMatrixReverb plugin used here will not run on Windows) or bypass reverb on the master mix.
* Max does not work on Linux. Users could build similar apps in Pure Data, Ardour or other software for use on Linux.

## Player patch requirements

* The player patch can be run on any macOS or Windows computer with Max 8 installed (purchase not required) and an internet connection (wifi is fine).

## Installation and setup on the server

1. Install [Max 8](https://cycling74.com/downloads). You do not need to purchase it to run the patch, only to edit/customize it.
2. In Max, open the Package Manager (File menu: Show Package Manager), search for the AudioMix Package, and install it.
3. Install Jmess using the installer on [this CCRMA page](https://ccrma.stanford.edu/software/jacktrip/osx/index.html).
4. Install [Soundflower](https://github.com/mattingalls/Soundflower). Alternately, you could use BlackHole if you [modify the code](https://github.com/ExistentialAudio/BlackHole/wiki/Change-the-Number-of-Channels) to enable enough channels.
5. Download the latest release of JackTrip-Max-Mixer and unzip. Open "server.maxpat" in Max and "setup.txt" in a text editor.
6. In the text file "setup.txt", fill in the names or instruments of the 6 players, and the server IP, then save the file, adding the group name (i.e. "[bandname]-setup.txt"). 
7. In the server patch, click the button under "Setup file" and select that text file to load the players'names.
7. If players will be controlling their own mixes remotely: 1) send them the updated "[bandname]-setup.txt" file; and 2) see the "setup notes" tab in the server patch and ensure you have the necessary UDP ports open on the server.
8. In the server patch, see the "routing notes" and "setup notes" tabs for details on setting up audio devices in JACK and Max and routing between the two. After connecting a group for the first time, use JMess to save and reload the JACK routing configuration.

## Installation and usage for players
1. On any macOS or Windows computer, download and install [Max 8](https://cycling74.com/downloads). You do not need to purchase it.
2. Download the latest release of JackTrip Max Mixer and unzip. The only file you need is "player.maxpat". Double-click to open it in Max.
3. The person running your server will give you a text file named "[bandname]-setup.txt". (Do not post its contents of online, because it contains the server IP address.) In the player.maxpat patch you opened, click the button on the right, select that "[bandname]-setup.txt" file, click ok, then select yourself in the menu below. Assuming the server is set up, you can then use the faders to control your monitor mix.

## Ports and security

Running your own JackTrip server requires opening the appropriate UDP ports, and if you have players using this player patch to control their mixes remotely, you will also need to open several other UDP ports on the server. Any time you open UDP ports on a computer, you are introducing potential security risks, so you should make sure you understand what you are doing. I do not take any responsibility for the security risks involved in using this software.

## Context

Having control over one's individual monitor mix is crucial in many kinds of music performance. Many excellent low latency networked music apps already provide individual monitoring capabilities for small groups, including [Sonobus](https://sonobus.net), [Jamulus](https://jamulus.io), [Netty McNetface](http://msp.ucsd.edu/tools/quacktrip/) and [Quaxtrip](https://github.com/damonholzborn/Quaxtrip).

Similar functionality will likely be integrated into JackTrip servers someday, but is not available yet, so I made these patches as a simple prototype and temporary solution. The goals include:

* supporting both [Virtual Studio devices](https://www.jacktrip.org/studio.html) and command-line JackTrip clients
* individual monitor mixes that players can control remotely without increasing their bandwidth requirements (i.e. they receive a stereo mix, not multiple channels)
* a simple interface for a producer to simultaneously monitor the low-latency players' mix and create a broadcast-quality mix using the new jitter buffer feature in JackTrip v1.3

Compared to simply using the auto-patching modes in JackTrip hub server mode, routing audio into and out of this server Max patch does probably add a very small amount of latency. However, the difference is so negligible that I have found musicians did not notice it across sessions with and without the mixer app.

I am not a developer and my Max patching is honestly embarrassing, but these patches are very simple and have worked for me. I welcome any help. I may extend them soon to allow for dynamic routing and other features, but it might not be worth the trouble.
