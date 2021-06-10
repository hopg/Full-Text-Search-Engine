# Full Text Search Engine

Inverted index search for text documents.

## Table of Contents
* [Introduction](https://github.com/hopg/Full-Text-Search-Engine#Introduction)
* [Program](https://github.com/hopg/Full-Text-Search-Engine#Program)
  * [Caching](https://github.com/hopg/Full-Text-Search-Engine#Caching)
  * [Program Settings](https://github.com/hopg/Full-Text-Search-Engine#Program-Settings)
* [Features](https://github.com/hopg/Full-Text-Search-Engine#Features)
* [Potential Improvements](https://github.com/hopg/Full-Text-Search-Engine#Potential-Improvements)
* [Modules](https://github.com/hopg/Full-Text-Search-Engine#Modules)

## Introduction

A full text search engine allows for a user to search for a term, or set of terms. It is characterised by utilising an inverted index database to perform searches. 

An inverted index database is a mapping from content to document. This allows for an extremely fast search to be conducted as there is no need to iterate through each document for each individual search. 

## Program

This program will analyse text documents and allow the user to perform various searches of content contained within any of these documents. 

The default setting will search the user's current working directory for text files to be analysed. If the user wishes to specify an alternate directory, an additional argument of `-d` can be entered at the command line as the script is run. 

For example:

`python full_text_search_engine.py -d`

When prompted, directory paths should be provided in the following format:

`C:\Users\Name\Desktop\Text Files`

If there are any subdirectories contained within the directory specified, they will also be searched. 

Upon a directory being chosen, the inverted index database is created by first scanning each and every text document and producing a list of unique words. The unique words are created by first cleaning the text inside each document via normalising whitespace, lower case sorting, removing punctuation, tokenisation, stop word removal and lemmatisation.

These words will be the basis for the mapping in the inverted index dictionary. The unique words are mapped to a document identifier and the frequency they occur in said document. Below is an example utilising the word 'stars', note the lemmatisation. 

`star: (1, 3)`

Here, 1 refers to the document identifier and 3 refers to the frequency of which the word 'star' occurs in document with id 1. 

The user can then perform a search, using a comma to separate terms if searching multiple terms. The terms entered are then cleaned and looked within the inverted index dictionary. If the cleaned terms are found within the dictionary, results are then presented to the user.

### Caching 

For every directory, the document database and the associated inverted index dictionary are stored. The purpose of which is to speed up searching for the user.

If a user were to perform a search on a directory where a search has already taken place, the program will first check to see if the number of text documents has been altered.

If the number of text files is different to the number that is stored within the database, a new database will be generated and consequently an inverted index dictionary.

In the scenario where the number of files within the directory remains the same, the SHA1 checksum of each document within the directory is compared with those stored within the database. 

If any of the text files have been altered in any way, this will be apparent due to a differing SHA1 checksum and consequently, a new database and inverted index dictionary is generated. 

In any of the cases where a new database and inverted index dictionary is newly generated, this will update the cached data. 


## Program Settings

At the command line, this program can be run with various options enabled.

### `-d`

By default this program will search the current working directory for text files to analyse. If a user wishes to override this default option, they can add `-d` as an argument and they will be prompted to specify a directory of their choice. 

### `-v`

With the argument `-v`, the program will print out additional information for the interested user.

This comprises of the document database, the underyling inverted index dictionary and the cached directories. 

### `-h`

Provides general help information for the user.

## Features

- Design pattern follows OOP
- Unit testing content provided, run `inv_testing.py` contained within the folder `unit_test` and with all the included files and subdirectories
- Optional arguments can be entered at the command line allowing additional functionality
- SHA1 encoding leveraged for caching results due to low clock cycles per a byte for a CPU
- Checks are performed for both alterations to the file names and the file contents
- Search returns the term along with the name of the text document and the frequency of the term
- Inverted index stored as a dictionary allowing fast queries
- Status updates presented to the user
- Can re-search either within current directory or a new, user specified directory
- If the same term is entered multiple times, only one result will be presented to the user
- If no text files found, user will be asked to specify a different directory, even if `-d` was not entered at the command line
- Error handling on choosing a directory
  - If a directory does not exist or has been entered incorrectly, the user will be notified
- Ignores terms that are just spaces or punctuation
- The user can enter q to quit when selecting a directory, searching a term or when asked to perform another search

## Potential Improvements
- Ability to search for text within files with various extension types.
- Speed of queries could be improved via multithreading 
- ~~No way for user to view underlying database~~
  - ~~Debug code available for interest~~

## Modules
- `os`
- `re`
- `argparse`
- `hashlib`
- `string`
- `nltk`
- `IPython.display`
- `nltk.corpus`
- `nltk.tokenize`
- `nltk.stem`
