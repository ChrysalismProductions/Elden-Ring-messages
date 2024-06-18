# Elden-Ring-messages

Technical details towards the bottom :)

One of the most memorable aspects of my first playthrough of Elden Ring was the messaging system. For those not aware, you can leave messages for other players that will appear in their worlds. The highly restrictive system of templates, words and conjunctions encourages creativity, leaves just enough room for ambiguity and automatically translates the messages into other languages. Players receive HP when their messages are rated as ‘good’, so are incentivised to use the mechanic and leave behind helpful, insightful or funny messages.

What I loved most about this system was how the messages were a microcosm of human expression and motivation. I am grateful for all of the players who led me to a site of grace, nudged me towards treasure or informed me of a boss’ weakness. Though some tried to convince me to jump to my death, others would be urging me not to listen.

Of course, there’s also the humour! Some of my favourite messages are references to other media, clever homonyms or are just downright absurd.

Despite not engaging much in the multiplayer aspect of the game, I never felt alone on my journey through the Lands Between.

On a subsequent playthrough, I thought it would be interesting to keep a record of every message I encountered. To achieve this, I wrote a script to collect the message text, gesture and location data. I made sure to visit every location and open as many messages as I possibly could, which ended up being 6882 messages across nearly 80 hours of gameplay!

There are 9,325 possible combinations of simple messages (i.e. template + word/phrase) and a whopping 869,556,250 possible combinations of complex messages (i.e. Template + word/phrase + conjunction + template 2 + word/phrase 2). Of all of these possible combinations, I only encountered 2354 unique simple messages and 3,714 total unique messages in my playthrough.

Using complex messages didn’t lead to a higher number of appraisals, nor did including a gesture.

As an enjoyer of messages, my favourite message of all was “jumping off ahead, if only I had a sound…” overlooking a cliff in Capital Outskirts. I’m almost certain it’s referencing the ending of Hyperballad by Björk, which just so happens to be one of my favourite songs of all time.

Honourable mentions go to “dung, key!” in front of a merchant’s horse with the ‘Nod in Thought’ gesture or “didn’t expect house…, shield” next to the Manor Towershield in Stormveil Castle.

I initially had some quite lofty ideas for how to present this data, but I’m trying to get better at actually finishing my projects… 

**Technical details**

Tools: Python (PyTesseract, Pandas, Numpy, Pygame, PyAutoGUI, Pillow, Fuzzywuzzy, Seaborn for Heatmap), Excel, InDesign for graphics

To collect messages, I used an Optical Character Recognition tool (PyTesseract). I was able to read the text and the number of appraisals during gameplay, by binding the OCR to my Y button (with a slight delay). This appended the time, raw stripped message text, message text and appraisal number to a set of lists stored in Python. I set up a keyboard hotkey to append this data to a CSV using Pandas and pressed it when taking a break or ending the gaming session.

Every 10th message, I would receive an alert saying it was time to open the in-game menu for gesture and location data.

I created a macro to open the in-game message menu and cycle through the entries while reading in the message, appraisals, location and gesture text. This was bound to my Xbox menu button. This data was again read into a Pandas dataframe and exported to a different CSV.

At this point, I had two datasets with shared keys (time, message text and appraisals) but before I could join them, I had to clean both datasets.

The main issue with Optical Character Recognition is that it produces inconsistent results. This results in anomalies in the data, where there can be slight misspellings or odd characters. I split the messages into their template, word and conjunction components using conditional logic while iterating over rows in the dataframe. I then computed Levenshtein distance between the message fragments and the known list of possible matches. I found that any record above a similarity ratio of 0.85 to the closest match was correct, which accounted for the vast majority of messages. The remaining few could be matched manually.

I was then able to integrate the two datasets based on 3 common attributes: the full message text (after being validated), the number of message appraisals and the timestamp of the messages. Since some messages were fairly common and could have the same number of appraisals as another message, I used the timestamps to conditionally join the two datasets where they are acceptably close. 
