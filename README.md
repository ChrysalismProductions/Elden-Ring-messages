# Elden-Ring-messages

Tools: Python (PyTesseract, Pandas, Numpy, Pygame, PyAutoGUI, Pillow, Fuzzywuzzy, Seaborn for Heatmap), Excel, InDesign for graphics

To collect messages, I used an Optical Character Recognition tool (PyTesseract). I was able to read the text and the number of appraisals during gameplay, by binding the OCR to my Y button (with a slight delay). This appended the time, raw stripped message text, message text and appraisal number to a set of lists stored in Python. I set up a keyboard hotkey to append this data to a CSV using Pandas and pressed it when taking a break or ending the gaming session.

Every 10th message, I would receive an alert saying it was time to open the in-game menu for gesture and location data.

I created a macro to open the in-game message menu and cycle through the entries while reading in the message, appraisals, location and gesture text. This was bound to my Xbox menu button. This data was again read into a Pandas dataframe and exported to a different CSV.

At this point, I had two datasets with shared keys (time, message text and appraisals) but before I could join them, I had to clean both datasets.

The main issue with Optical Character Recognition is that it produces inconsistent results. This results in anomalies in the data, where there can be slight misspellings or odd characters. I split the messages into their template, word and conjunction components using conditional logic while iterating over rows in the dataframe. I then computed Levenshtein distance between the message fragments and the known list of possible matches. I found that any record above a similarity ratio of 0.85 to the closest match was correct, which accounted for the vast majority of messages. The remaining few could be matched manually.

I was then able to integrate the two datasets based on 3 common attributes: the full message text (after being validated), the number of message appraisals and the timestamp of the messages. Since some messages were fairly common and could have the same number of appraisals as another message, I used the timestamps to conditionally join the two datasets where they are acceptably close. 
