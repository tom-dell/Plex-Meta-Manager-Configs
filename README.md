# How to write Plex-Meta-Manager configs

I've noticed there is a lot of confusion around how to write the Movies.yml file, it's a bit complicated to wrap your head around, so I decided to write some documentation on how to build one.

Now I must warn you, this is going to get pretty long, and I'm going to take things **very** slowly, now let's get onto it, there are also a lot of links, I **strongly suggest** not clicking them until you finish reading this entire page, otherwise you might get a bit confused.

Collections are built around a list of movies/shows, these lists can come from Trakt, IMDB, and many other sources, today we will be covering how to make collections using IMDB.

Let's cover off some terms first

* Builder
  * This is what Plex Meta Manager calls the format of the of the collection, as each type of collection (IMDb/Trakt/etc) all have different types of options and filters.
  * This is a screenshot of two of my collections, the first one is built with the [IMDb builder](https://metamanager.wiki/en/latest/metadata/builders/imdb.html), and the second with the [mdblist builder](https://metamanager.wiki/en/latest/metadata/builders/mdblist.html).
* Static list
  * This is a list that doesn't change, something like a list of all comedy movies released in 2021, they can't release any more movies in 2021 until we invent time travel!
* Dynamic list
  * This is a list that does change, something like trending movies on Netflix, that list is almost constantly changing.



Before we start, let's go over the format of one of these collections, we will be using the IMDb builder, and a very simple format and order, which I personally find logically (once you understand how they work, feel free to change it). Many other fields can be added, you can find these in the documentation.

```
collections:

  :
    sort_title: 
    summary: 
    url_poster: 
    imdb_list:  
    collection_order: 
    sync_mode: 
```

First, we're going to create a simple Movies.yml that will create a collection of Movies in the [Universal Classic Monsters](https://en.wikipedia.org/wiki/Universal_Classic_Monsters) (you can click this one) movie collection. This list contains all the movies that feature on of the Universal Classic Monsters, including Dracula, Frankenstein, and the Monster from the Black Lagoon. This is a static list, as no more official Universal Classic Monsters movies will be made.

image here

Conviently there is already a [list of these movies on IMDB already!](https://www.imdb.com/list/ls565593601/) (you can click this one too!)

Let's quickly go over what each line in the template means

```
Collections:

  Universal Classic Monsters:
```
The first line 

>collections: 

Is only used once, at the start of your Movies.yml config file.

The collection name has to be before the colon.

Now onto the rest of the file.

```
    sort_title: Universal Classic Monsters
    summary: This is a collection of movies that contain any of the monsters in the Universal Classic Monsters collection.
```

This next section is pretty self explanatory, enter a title for the collection, and a summary.

Now for the poster, and the location of the list. To find a poster for the collection, check out [The PosterDB](https://theposterdb.com/). Use the list linked above for this example.

```
    url_poster: https://theposterdb.com/api/assets/106840
    imdb_list: https://www.imdb.com/list/ls565593601/ 
```

The next section is the last, it defines the order of the movies within the collection, as well as the [sync mode](https://metamanager.wiki/en/latest/config/settings.html#sync-mode).

For the collection_order, we generally use 'custom', as it will order the movies, in the same order as the list.

The sync_mode is how the movies are added to the list, are they appended to the collection, or sync'd with the list. If they are appended, Plex Meta Manager will never remove a movie from the list, however if it's a sync'd list, if the original list changes, then Plex Meta Manager will change the collection to adjust (adding, or removing movies when the original list adds or removes movies).


``` 
    collection_order: custom
    sync_mode: sync
```

Finally we have a complete Movies.yml file.

```
collections:

  Universal Classic Monsters:
    sort_title: Universal Classic Monsters 
    summary: This is a collection of movies that contain any of the monsters in the Universal Classic Monsters collection.
    url_poster: https://theposterdb.com/api/assets/106840
    imdb_list: https://www.imdb.com/list/ls565593601/ 
    collection_order: custom
    sync_mode: sync
````
