# WARNING: NOT WORKING UNTIL APPLE INCLUDES ARTIST IN THEIR DATA EXPORT

# AppleMusic-to-Listenbrainz-Import
 A Python/Jupyter notebook to import historical streams from an Apple Music data export to Listenbrainz

## Prerequisites
* Apple Music Play Activity.csv from a data export - A history of all your listening activity from apple music
* Your Listenbrainz user key - to authenticate with the API and associate with your account
* Knowing your time zone. You need your time zone in TZ format. 
	* You can easily find it [here](https://tz.jamie.holdings/). Zoom out and click on your location. The time zone will appear on the top left next to a globe, like `America/Los_Angeles`. Click on it to copy.

## Instructions for non-techies
Never used python? Don't have whatever a "Jupyter Notebook" program is? Follow these directions!
1. Copy the URL for this page: `https://github.com/s-crypt/AppleMusic-to-Listenbrainz-Import`
2. Go to https://colab.research.google.com/ and sign in
3. Click `Upload`, then `Github` on the popup (or File->Upload if there is no popup)
4. Paste the URL and hit enter
5. Click on `COLAB-ImportAppleMusicToListenbrainz.ipynb`
6. At the top left, click Runtime->Run All
7. After a few seconds, a text box will show up below the code block (the rectangle with the word import a bunch of times). Paste your User Token in the text box after `Please enter your user token: ` and hit enter
8. Another text box will appear. Type or paste your timezone (you can find this above)
9. A `Browse...` button will appear. Click it and select your freshly unzipped `Apple Music Play Activity.csv` from your data export
   1.  Wait until it uploads. It may take a while depending on how large your music history is.
10. Wait until the program is done processing
11. Done! Your listen history from Apple Music is now in Listenbrainz


## Instructions for techies
1. Download the notebook
2. Place in a folder containing the CSV, freshly extracted from the data export
3. Open in a Jupyter notebook program (VSCode, Jupyter)
4. Run all cells and answer the prompts