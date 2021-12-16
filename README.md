## Phish Analysis and Setlist Predictions
**Mary Schindler (née McAteer)**
A exploratory analysis of the band Phish and predictive model to build a setlist.

[Link to Phish.net](https://phish.net/), from where the data was obtained.


### Table of Contents

- Problem Statement
- Background
- Data Collection
- Data Cleaning and EDA
- Modeling Preparation
- Modeling
- Conclusions
- Limitations & Future Considerations


### Problem Statement
The band Phish is known for its originality. From their sound, to their instruments (occasional vacuum cleaner on stage), they have been setting precedents in the Jam Band scene for over thirty years. Between both covers and original songs, the band has a catalog of over 900 songs to perform from. Although they have a plethora of songs to play, Phish worries about repetitivity and has contracted me to analyze their performances to ensure they have not become predictable. They specifically requested that I build a model which could predict an upcoming setlist. 
- My minimum viable product (MVP) is to build a model which will accurately predict 1 correct song out of 20 (the average setlist length). 


### Background
Founded at the Univeristy of Vermont (Go Catamounts!) in the 1980s, Phish has been performing in the Jam Band scene for over thirty years. During that period the band has gone through different iterations and hiatuses. Throughour their career, however, they have managed to stay original.

Like any band that has been around for years, they want to continue performing while exploring their sound.

NEED TO WRITE NEED TO WRITE NEED TO WRITE NEED TO WRITE
NEED TO WRITE NEED TO WRITE NEED TO WRITE NEED TO WRITE
NEED TO WRITE NEED TO WRITE NEED TO WRITE NEED TO WRITE
NEED TO WRITE NEED TO WRITE NEED TO WRITE NEED TO WRITE


### Data Collection
Using Phish.net's API, for which an API key needed to be requested, I obtained two types of data for which I build two datasets: 2,004 Phish shows and 58,104 setlists (from various bands).
- The ‘shows’ dataset had entries for each unique show Phish has played
- The ‘setlists’ dataset had entries for each song played in a show.
Taking the 'showid' from both Phish shows collected all setlists collected I built a new dataset of all setlists sharing a showid in common. 


### Data Cleaning and EDA
I worked with two datasets for EDA but only one for modeling. For the dataset used for modeling, I created a function to build a ‘setlists’ column on which my setlist predictions would be modeled. The function ‘make_setlist_col(df)’ built a list of songs associated with each unique showid, assigned the list to the appropriate column entry.
</br>

Once this process was complete I cleaned what few null values there were: only in the 'state' and 'setlistnotes' columns. Neither of which were to be used in modeling but the null values in both columns were replaced with 'Not Available'.


### Modeling Preparation 
Once my data was cleaned I needed to prepare it for modeling in a way that the model would be able to observe the previous $n$ songs played the predict the next $n + 1$ song to be played. I built my corpus for predictions by concating all setlists together and identifying all unique songs. The corpus of all songs were then encoded, based on their sorted index location as a unique song. 
</br>

From this corpus I built sample setlist of length 20 (the average length of show), iterating through the entire corpus, for a total of 36,023 lists of 20 songs on which my model could train and test. 


### Modeling
As previously stated, the purpose of modeling was to look at $n$ songs played (in order played) and to predict the $n + 1$ song to be played next. Based on my research I choose to train a Recurrent Neural Network (RNN) with an LSTM layer. The LSTM layer was necessary in this instance as it is capable of learning order dependence for sequence predictions. I tried a few different RNN models to see which produced the best results: 
- with LSTM
- with Dropout
- Gridsearching for best parameters


### Recommendations & Conclusions
I was ultimately rewarded with 10% testing accuracy. Assuming a setlist length of 20 songs, 10% prediction accuracy is 2 songs correctly predicted. I was able to build a model that performed twice as well as my MVP. 
</br>

While I only used the instances of previous songs played to predict the next song played, with no regard for show date, location, tour, etc., only being able to make predictions with 10% accuracy indicates that Phish has been and remains diverse in the use of their song catalog. With the inclusion of their most recent 'concept show' songs in their lineup, they continue to remain original and unpredictable. 


### Limitations & Future Considerations
1. Limitations
    - I was unable to figure out how to incorporate the tags: ‘set1’, ‘set2’, ‘encore’, into my corpus. I would have like to use these as it’s likely certain songs are use as show openers and show closers more often than others. 
    - I may enjoy Phish but I am not an expert. I would have like to do more outside research or reach out to the community of Phish fans for their input. 
    - Along these lines, I did not know when to omit data: for example, the most recent run of Halloween shows were themed around the band being ‘aliens’ or ‘visitors’ from another time. They played a special show, with many of the songs played for the first time. See: https://liveforlivemusic.com/features/phish-sci-fi-soldier-get-more-down-halloween-las-vegas-10-31-21-photos-videos/ for more information.
2. Future Considerations
    - Incorporating the tags: ‘set1’, ‘set2’, ‘encore’ into my corpus and encoded lists. 
    - Building a function to build a predicted setlist based on a provided corpus (particular string of songs) and length of a set.
    - Further fine-tuning on the RNN model in order to produce higher scores, including testing out different numbers of hidden layers with LSTM. 
    - Use classic NLP methods like Bag of Words and Word2Vec to examine Phish’s original songs, lyrics, and their similarities. 

