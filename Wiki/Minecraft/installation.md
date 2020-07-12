# Minecraft Installation

### Ubuntu 18.04

1. Sign on to server
2. sudo up
`sudo -s`
3. Change to home directory
`cd`
4. get latest updates for server
`apt update -y`
5. Change to mincraft directory
`cd minecraft/`
6. Make folder for version if needed (replace `<version>` with version number)
`mkdir mincraft_<version>.jar`
7. Download minecraft jar file
Get latest URL from https://www.minecraft.net/en-us/download/server/ and copy direct link
`wget <link from above>`
8. Change name of file (replace `<version>` with version number)
`mv server.jar minecraft_<version>.jar`
9. Run jar for first time (replace `<version>` with version number)
`java -Xmx3072 -Xms1024M -jar minecraft_<version>.jar nogui`
10. Sign EULA
`nano eula.txt`
change option to TRUE
11. Run again
`java -Xmx3072 -Xms1024M -jar minecraft_<version>.jar nogui`