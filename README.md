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
* The IDF term looks like this: $log(N / n_t) $, where $N$ is the number of documents (in this case, 8 books), and $n_t$ is the number of documents the specific word $t$ appears in (therefore, this should be between 1 and 8, inclusive).  
* You will also have to compute document-specific term frequencies (i.e., word $w_i$ appears $f_{i,j}$ times in document $j$, but $f_{i,k}$ times in document $k$; you’ll need both those counts!). Then, you multiply the IDF term by each document-specific TF term, and that gives you the TF-IDF score for each document.  
* In this case, your output should still contain 40 words, but should contain the top-5 words in each document by their TF-IDF rank 

### Algorithm
* We define the TF-IDF for word $w_i$ in document $j$ as  
\begin{equation}
TF\text{-}IDF_{i, j} = f_{i, j} \cdot log(\frac {N} {n_j})
\end{equation}
where $n_j = 0, 1, 2, ... N$. In the case, $N = 8$.
* It is intuitive to create a 2-dimensional array, $A$, to store the frequency for each word in each specific document, where  
\begin{equation}
A[i][j] = \text{ frequency (times of occurrence) for word, } w_i \text{, in document } j
\end{equation}

Handle each document one by one  
Take the first doument for example . 
Split each line into a list of words  
Filter out the words which are in the stopwords list  
Map each word, w, to a tuple, (w, [1, 0, 0, ... 0]). For the second document, the tuple would be (w, [0, 1, 0, ... 0]). etc    
Add the tuples with the same key (word)  
Stripping the words which have leading or trailing punctuations  
