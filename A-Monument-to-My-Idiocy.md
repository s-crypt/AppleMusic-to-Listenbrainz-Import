
## I tried to create the JSON format to submit the entries through the api, and iterated through to create each JSON entry for submission.
## Here's the kicker. I already had liblistenbrainz imported and I just completely forgot about it. So I did a whole bunch of work for no reason.


##### Turn resulting data into ListenBrainz JSON format and submit in batches of 1000 songs

```json
{
  "listen_type": "single",
  "payload": [
    {
      "listened_at": TIMESTAMP,
      "track_metadata": {
        "additional_info": {
          "media_player": "Apple Music",
          "submission_client": "Apple Music to Listenbrainz Data Import Tool",
          "submission_client_version": "1.0",
          "music_service": "music.apple.com",
          "duration_ms": TRACKDURATION-MS
        },
        "artist_name": "Rick Astley",
        "track_name": "Never Gonna Give You Up",
        "release_name": 'Whenever you need somebody"
      }
    }
  ]
}
```


```python
# Define batch size
batch_size = 500

# Create an empty list to store all JSON objects
all_json_objects = []

# Iterate over rows in batches
for start_idx in range(0, len(listens), batch_size):
    batch_listens = listens.iloc[start_idx : start_idx + batch_size]

    # Create JSON objects for each row in the batch
    batch_json_objects = []
    for _, row in batch_listens.iterrows():
        json_object = {
            "listen_type": "single",
            "payload": [
                {
                    "listened_at": row['Event Start Timestamp'],
                    "track_metadata": {
                        "additional_info": {
                            "media_player": "Apple Music",
                            "submission_client": "Apple Music to Listenbrainz Data Import Tool",
                            "submission_client_version": "1.0",
                            "music_service": "music.apple.com",
                            "duration_ms": row['Media Duration In Milliseconds'],
                        },
                        "artist_name": row['Artist'],
                        "track_name": row['Song Name'],
                        "release_name": row['Album Name']
                    },
                }
            ],
        }
        # If there is no song or artist, delete the entry
        if row['Artist'] == "" or row['Song Name'] == "":
            del json_object

        # Do not include Album if it is missing
        elif pd.isna(row['Album Name']):
            del json_object["payload"][0]["track_metadata"]["release_name"]

        # Do not include listen time if it is missing
        elif pd.isna(row['Event Start Timestamp']):
            del json_object["payload"][0]["listened_at"]

        # Do not include song duration if it is missing
        elif pd.isna(row['Media Duration In Milliseconds']) or row['Media Duration In Milliseconds'] == 0:
            del json_object["payload"][0]["track_metadata"]["additional_info"]["duration_ms"]

        ## Add to batch if the object exists
        if 'json_object' in locals():
            # Add to batch
            batch_json_objects.append(json_object)
        
    # Append the batch to the list of all JSON objects
    all_json_objects.extend(batch_json_objects)
    
    # !!! Send the batch of songs to listenbrainz and print the response !!
    submit_listen("import", batch_json_objects, LB_User_Token)
    print("Successfully sumbitted " + str(len(all_json_objects) + " to Listenbrainz"))

    # Wait for 15 seconds between submissions to the API
    time.sleep(5)

# Convert the list of all JSON objects to a JSON string
json_string = json.dumps(all_json_objects, indent=2)
print(json_string)
```

## Helpful code by lucifer to get the HTTP 400 error I was getting since there is/was an error with the library that prevented errors from being shown.

```python
try:
    response = client.submit_single_listen(listen)
except Exception as e:
    print(e.__context__.response.json())
```