<div align="center"><img height="200px" alt="logo" src="src/main/resources/static/favicon.png?raw=true"/></div>

## <div align="right"><a href="https://www.buymeacoffee.com/zggis" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a></div>

### Description
Plex-TVTime is a Plex webhook handler that automatically updates your TVTime watch history. TV episodes are automatically marked as watched once you complete them on Plex.

### Installing
#### Unraid
To install Plex-TVTime on Unraid you can install the docker container through the community applications.
#### Docker Desktop
You can run Plex-TVTime on Docker locally by using the following command. Replace CONFIG with the directory you want the database to be saved to, and any other directories that contain your log files.
```
$ docker run -e TVTIME_USER={Your TVTime username} -e TVTIME_PASSWORD={Your TVTime password} -e PLEX_USERS={Plex username(s) to link} -p 8080:8080 zggis/plex-tvtime:latest
```
#### Java App
You can run Plex-TVTime as a Java program from command prompt. Java JRE 11 is required. To grab the latest Plex-TVTime JAR, navigate to the <a href="https://github.com/Zggis/plex-tvtime/releases">Releases</a> page. Run the JAR using this command.
```
$ java -jar -Dspring.profiles.active=local plex-tvtime*.jar
```

### Building the Code
Clone the repo and update application-local.properties as you need, then build with gradle. JAR should be placed in /build/libs
```
$ gradlew assemble
```

### Usage
Navigating to the home page http://[host]:[port] in your browser will display to webhook URL you can enter in Plex > Settings > Webhooks. It should be http://[host]:[port]/webhook/plex<br><br>
Plex-TVTime will mark episodes as watched on your TVTime profile once you have watched them passed the configured 'Video played threshold' in Plex (You can adjust this % in Plex Settings > Library). Plex <strong>does not</strong> send webhooks when episodes are manaully marked as watched.<br><br>
Only libraries of type 'TV Shows' will be considered by Plex-TVTime, so it will ignore webhooks generated by movies and other libraries.<br><br>
Watching an episode for a show that has not been added to you TVTime profile will automatically add the show. If this behavior is undesired, you can make use of the Excluded/Included configuration parameters below to restrict which shows are sent to TVTime.<br><br>
Watching an episode for a show you have already marked as watched in TVTime has no effect, the episode is not marked as 'rewatched' in TVTime, nor is the time you first marked the episode as watched updated.<br><br>
Plex-TVTime does not support linking multiple TVTime accounts, so if you are managing a server with multiple users who have their own TVTime accounts I would suggest running multiple instances of Plex-TVTime each with their own webhook configured in Plex, and TVTime account credentials configured in docker.

### Configuration

#### Required Variables
Name | Description
--- | ---
TVTIME_USER | The username you use to login to TVTime.
TVTIME_PASSWORD | The password you use to login to TVTime.
PLEX_USERS | Single Plex user or comma separated list of users whoes watch events will be sent to TVTime.

#### Optional Variables
Container Variable | Default Value | Description
--- | --- | ---
PLEX_SHOWS_EXCLUDE | Undefined | A comma separated list of TV show titles that will not be sent to TVTime. TVShow title should be identicle to how it appears in your Plex library. If the title includes a comma in it replace it with %2C to avoid conflicting with the comma delimeters in the list.
PLEX_SHOWS_INCLUDE | Undefined | Overridden and ignored if PLEX_SHOWS_EXCLUDE is set, otherwise only shows that appear in this list will be sent to TVTime.
LOGGING_LEVEL | INFO | Set to TRACE or DEBUG for additional logging.

### Troubleshooting
Please check the logs, as described above many webhook events are intentionally ignored depending on configuration. If you can't resolve on your own open an <a href="https://github.com/Zggis/plex-tvtime/issues/new">issue</a> and I will help.

### FAQ
**Question:** What about movies?

**Answer:** I tried to get movies working as I know the TVTime mobile app supports tracking movies, however the website does not seem to support this. TVTime does not publish a public api so I had to rely on scraping the website which left me with no way to incorporate movies.
