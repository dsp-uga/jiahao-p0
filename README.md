# jiahao-p0

## This is Project 0 for CSCI 8360

## Subproject A: 

Generate a dictionary / hash-map of the top 40 words across all documents with the largest counts. The word counts should be case-insensitive, but otherwise you don’t need any additional preprocessing. 

Split each line into a list of words  
Map each word, w, to a tuple, (w, 1)  
Add the tuples with the same key (word)


## Subproject B: 

Implement a list of stopwords (use the stopwords.txt file). These words do not carry much meaning about the specific text. Re-generate the list of top 40 words across all documents, dropping words entirely that are found in the provided list of stopwords. As before, your counts should be case-insensitive, but otherwise no additional preprocessing is needed (HINT: you will need to look at broadcast variables to do this). 

Split each line into a list of words  
Filter out the words which are in the stopwords list  
Map each word, w, to a tuple, (w, 1)  
Add the tuples with the same key (word)  


## Subproject C: 

Implement an additional bit of preprocessing that strips out periods (.), commas (,), colons (:), semi- colons (;), single quotes (’), exclamation points (!), or questions marks (?), but only if they are the first or last character of the word (this will leave contractions like “can’t” or “won’t” unaffected). You may notice that in doing so, you have to intrinsically discard all words that have only 1 character; do that as well. Submit the resulting top 40 words. 

Split each line into a list of words  
Filter out the words which are in the stopwords list  
Map each word, w, to a tuple, (w, 1)  
Add the tuples with the same key (word)  
Stripping the words which have leading or trailing punctuations  


## Subproject D:

* Up until now, you have been computing what is known as term frequency: word counts are measures of frequency of the words (terms). What is more informative is term frequency inverse document frequency, or TF-IDF. This weights the term frequency by the inverse number of documents the word appears in.  
* The IDF term looks like this: <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\log(N/n_t)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\log(N/n_t)" title="\log(N/n_t)" /></a>, where <a href="https://www.codecogs.com/eqnedit.php?latex=N" target="_blank"><img src="https://latex.codecogs.com/gif.latex?N" title="N" /></a> is the number of documents (in this case, 8 books), and <a href="https://www.codecogs.com/eqnedit.php?latex=n_t" target="_blank"><img src="https://latex.codecogs.com/gif.latex?n_t" title="n_t" /></a> is the number of documents the specific word <a href="https://www.codecogs.com/eqnedit.php?latex=n_t" target="_blank"><img src="https://latex.codecogs.com/gif.latex?t" title="t" /></a> appears in (therefore, this should be between 1 and 8, inclusive).  
* You will also have to compute document-specific term frequencies (i.e., word <a href="https://www.codecogs.com/eqnedit.php?latex=w_i" target="_blank"><img src="https://latex.codecogs.com/gif.latex?w_i" title="w_i" /></a> appears <a href="https://www.codecogs.com/eqnedit.php?latex=n_t" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f_{i,j}" title="f_{i,j}" /></a> times in document <a href="https://www.codecogs.com/eqnedit.php?latex=n_t" target="_blank"><img src="https://latex.codecogs.com/gif.latex?j" title="j" /></a>, but <a href="https://www.codecogs.com/eqnedit.php?latex=f_{i,k}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f_{i,k}" title="f_{i,k}" /></a> times in document <a href="https://www.codecogs.com/eqnedit.php?latex=k" target="_blank"><img src="https://latex.codecogs.com/gif.latex?k" title="k" /></a>; you’ll need both those counts!). Then, you multiply the IDF term by each document-specific TF term, and that gives you the TF-IDF score for each document.  
* In this case, your output should still contain 40 words, but should contain the top-5 words in each document by their TF-IDF rank 


### Algorithm
* We define the TF-IDF for word <a href="https://www.codecogs.com/eqnedit.php?latex=w_i" target="_blank"><img src="https://latex.codecogs.com/gif.latex?w_i" title="w_i" /></a> in document <a href="https://www.codecogs.com/eqnedit.php?latex=j" target="_blank"><img src="https://latex.codecogs.com/gif.latex?j" title="j" /></a> as  
<a href="https://www.codecogs.com/eqnedit.php?latex=$$&space;TF\text{-}IDF_{i,&space;j}&space;=&space;f_{i,&space;j}&space;\cdot&space;\log(\frac&space;{N}&space;{n_j})&space;$$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$$&space;TF\text{-}IDF_{i,&space;j}&space;=&space;f_{i,&space;j}&space;\cdot&space;\log(\frac&space;{N}&space;{n_j})&space;$$" title="$$ TF\text{-}IDF_{i, j} = f_{i, j} \cdot \log(\frac {N} {n_j}) $$" /></a>  
where <a href="https://www.codecogs.com/eqnedit.php?latex=n_j&space;=&space;0,&space;1,&space;2,&space;...&space;N" target="_blank"><img src="https://latex.codecogs.com/gif.latex?n_j&space;=&space;0,&space;1,&space;2,&space;...&space;N" title="n_j = 0, 1, 2, ... N" /></a>. In the case, <a href="https://www.codecogs.com/eqnedit.php?latex=N&space;=&space;8" target="_blank"><img src="https://latex.codecogs.com/gif.latex?N&space;=&space;8" title="N = 8" /></a>.
* It is intuitive to create a 2-dimensional array, <a href="https://www.codecogs.com/eqnedit.php?latex=A" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A" title="A" /></a>, to store the frequency for each word in each specific document, where  
<a href="https://www.codecogs.com/eqnedit.php?latex=A[i][j]&space;=&space;\text{&space;frequency&space;(times&space;of&space;occurrence)&space;for&space;word,&space;}&space;w_i&space;\text{,&space;in&space;document&space;}&space;j" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A[i][j]&space;=&space;\text{&space;frequency&space;(times&space;of&space;occurrence)&space;for&space;word,&space;}&space;w_i&space;\text{,&space;in&space;document&space;}&space;j" title="A[i][j] = \text{ frequency (times of occurrence) for word, } w_i \text{, in document } j" /></a>

* Read all text through wholeTextFiles() method, which will return a key-value pair, where the key is the path of each file, the value is the content of each file  
* Create a map, where the key is the path of the file, and the value is the index, i.e. 0, 1, ... 7
* Split each line into a list of words, and map each word, w, to a tuple, (w, [1, 0, 0, ... 0]). For the second document, the tuple would be (w, [0, 1, 0, ... 0]). etc  
* Stripping the words which have leading or trailing punctuations
* Filter out the words which are in the stopwords list  
* Add the tuples with the same key (word) by using reduceByKey    

