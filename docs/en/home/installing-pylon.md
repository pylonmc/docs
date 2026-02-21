# Installation

!!! danger
    PYLON IS CURRENTLY EXPERIMENTAL. ONLY RUN IT ON A TEST SERVER THAT YOU ARE WILLING TO DELETE. THE NEXT PYLON VERSION WILL PROBABLY NOT BE COMPATIBLE WITH THE PREVIOUS ONE. IF YOU INSTALL PYLON SOMEWHERE YOU SHOULDN'T AND END UP LOSING DATA, WE WILL POINT AND LAUGH AT YOU.

1. Make sure you are running Paper or a Paper fork. Pylon is not compatible with Spigot.
2. Download the latest version of Rebar from [here](https://github.com/pylonmc/rebar/releases/latest)
3. Download the latest version of Pylon from [here](https://github.com/pylonmc/pylon/releases/latest)
4. Drop the .jar files in your plugins folder and restart your server. [Do not use /reload](https://madelinemiller.dev/blog/problem-with-reload/). The first start will take longer than usual, but after that, Rebar/Pylon will load much more quickly.
5. Check out Rebar and Pylon's plugin folders to see all the config options available.
5. [Install some addons.](list-of-addons.md)
6. That's it!

### Where is Rebar data stored?

You might notice that Rebar does not use a database and does not seem to have any storage files in its plugin folder. This is because Rebar stores everything inside the world file itself in the same way that Minecraft does! You don't need to worry about backing up any data besides your worlds: **Rebar's data will always be consistent with the world's data.** This also means that Rebar is *much* less succeptible to data corruption than other similar plugins.

