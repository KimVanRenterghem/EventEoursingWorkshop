# EventEoursingWorkshop
workshop for event sourcing

This app is a tutorial to get the feeling what you can do with event sourcing.


1. create workspace :
    1. unzip the event store in the tools folder
    2. unzip the curl in the tools folder
    3. add the C:\curl\ and C:\eventstore\ path to the PATH Environment Variable in Windows
    4. start event store
        ```cmd
        EventStore.ClusterNode.exe --db C:\eventstore\db --log C:\eventstore\logs --run-projections=all --start-standard-projections=true
        ``` 
        > now you can watch your store at http://127.0.0.1:2113/
        > user admin pass changeit
2. start en stop the song thru the api and fire the events
    > look in the web tool to the events 
3. create app which displays the playing song for a user
    > do this by reading only the last event of the stream (position -1)
    ```csharp
    var eveTask = connection.ReadEventAsync(stream, -1, true);
    var eve = eveTask.Result;

    var ev = eve.Event?.Event;
    if (ev != null)
    {
        //event type validation
        var json = Encoding.UTF8.GetString(ev.Data);
        var songStart = JsonConvert.DeserializeObject<SongPlayingStarted>(json);
    }
    ```
4. add en remove the songs from a playlist thru the api and fire the events
5. create a playlist from the playlist stream
    > read the playlist stream and project it into the model

    > update projection $by_category set - to _
6. give top 10 list of most played songs
    > do this by reading all the user play streams and rating the number of time a song is played
    > afterwards you can order them and create a new play list
7. a song should at least play for 20 sec to get in the list (rebuild your list)
    > change the preview step, drop the db and restart from position 0
8. a son witch is stopt and started on the same sec should be considered not being stopped (rebuild your list)
    > change the preview step, drop the db and restart from position 0
9. create linked songs based on other users next 2 songs most played
