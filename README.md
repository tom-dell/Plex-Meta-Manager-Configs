
# How to write Plex-Meta-Manager metadata files

I've noticed there is a lot of confusion around how to write the Metadata files (Movies.yml and TVShows.yml) file, it's a bit complicated to wrap your head around, so I decided to write some documentation on how to build one. This does not cover creating the config.yml file, for this we assume you have a working omdb, and tmdb API keys.

Now I must warn you, this is going to get pretty long, and I'm going to take things **very** slowly, now let's get onto it, there are also a lot of links, I **strongly suggest not clicking** them until you finish reading this entire page, otherwise you might get a bit confused.

Collections are built around a list of movies/shows, these lists can come from Trakt, IMDB, and many other sources, today we will be covering how to make collections using the imdb_list, plex_search, and tmdb_discover builders (I haven't linked them on purpose, don't look them up yet).

Let's cover off some terms first;

* Metadata file
	* This is the name Plex Meta Manager gives the file we will add our collection logic to, it's generally called Movies.yml for collections in your movies library, and TVShows.yml for collections in your TV Shows library.
* Builder
  * This is the section in your Metadata file that gets the list from somewhere, whether that be IMDb, Trakt, or somewhere else. 
  * This is a screenshot of two of my collections, the first one is built with the imdb_list builder, and the second with the tmdb_discover builder.
  * The red boxes are collections, where as the blue ones are the builders within the collection. **Don't worry, you shouldn't understand what it says yet!**
  * ![](/images/builders.png)
* Static (IMDb/Trakt/etc) list
  * This is a list that doesn't change, something like a list of all comedy movies released in 2021, they can't release any more movies in 2021 until we invent time travel!
* Dynamic (IMDb/Trakt/etc) list
  * This is a list that does change, something like trending movies on Netflix, that list is almost constantly changing.
* Metadata
  * Metadata is information about the media, runtime, actors, genre, etc.
* [TMDB](https://www.themoviedb.org/)
  * The Movie Database, a website that contains a huge amount of information about movies.

## A collection of Universal Classic Monsters movies using the imdb_url builder

Before we start, let's go over the format of one of these collections, we will be using the imdb_search builder, and a very simple format and order, which I personally find logically (once you understand how they work, feel free to change it). 

Indentation matters, it's two spaces per indent.

```YAML
collections:

  :
    sort_title: 
    summary:
    url_poster: 
    collection_order: 
    sync_mode: 
```

First, we're going to create a simple Movies.yml that will create a collection of movies in the [Universal Classic Monsters](https://en.wikipedia.org/wiki/Universal_Classic_Monsters) (you can click this one) movie collection. This list contains all the movies that feature one of the Universal Classic Monsters, including Dracula, Frankenstein, and the Monster from the Black Lagoon. This is a static IMDb list, as no more official Universal Classic Monsters movies will be made.

<p align="center">
  <img src="https://raw.githubusercontent.com/tom-dell/Plex-Meta-Manager-Configs/master/images/universal_monsters.png" />
</p>

Conviently there is already a [list of these movies on IMDb already! I wonder who made it?](https://www.imdb.com/list/ls565593601/) (you can click this one too!)

Let's quickly go over what each line in the template means;

```YAML
collections:

  Universal Classic Monsters:
```
The first line.

> collections: 

Is only used once, at the start of your Movies.yml config file.

>   Universal Classic Monsters:

The second line is the collection name, this has to be before the colon, watch your indentation!

This next section is pretty self explanatory, enter a title for the collection, a summary, as well as a poster.

```YAML
    sort_title: Universal Classic Monsters
    summary: This is a collection of movies that contain any of the monsters in the Universal Classic Monsters collection.
    url_poster: https://theposterdb.com/api/assets/106840
```

To find a poster for the collection, check out [The PosterDB](https://theposterdb.com/). 

Let's move on;

```YAML
    collection_order: custom
    sync_mode: sync
```

This section defines the order of the movies within the collection, as well as the [sync mode](https://metamanager.wiki/en/latest/config/settings.html#sync-mode).

For the collection_order, we generally use 'custom', as it will order the movies in the same order as the list, or by the conditions in the builder indentation block (we'll cover this soon).

The sync_mode is how the movies are added to the list, if they are appended to the collection or sync'd with the list. If they are appended, Plex Meta Manager will never remove a movie from the collection, however if it's a sync'd list, if the original list changes, then Plex Meta Manager will change the collection to adjust (adding, removing, and even re-ordering movies when the original list changes).

Now for the last section, this section is called the builder.


```YAML
    imdb_list: 
      url: https://www.imdb.com/list/ls565593601/ 
```

Let's use the Universal Classic Monsters IMDb list that is linked above, but I've also linked it [https://www.imdb.com/list/ls565593601/](https://www.imdb.com/list/ls565593601/) so you don't have to scroll back up. By reading the [documentation you'll find that the imdb_list builder takes the url argument](https://metamanager.wiki/en/latest/metadata/builders/imdb.html#imdb-list), which we just give the url of our list.

Finally we have a complete Movies.yml file.

```YAML
collections:

  Universal Classic Monsters:
    sort_title: Universal Classic Monsters 
    summary: This is a collection of movies that contain any of the monsters in the Universal Classic Monsters collection.
    url_poster: https://theposterdb.com/api/assets/106840
    collection_order: custom
    sync_mode: sync
    imdb_list: 
      url: https://www.imdb.com/list/ls565593601/ 
````

## A collection of movies popular on Netflix right now using the imdb_url builder (again!). This one updates weekly!

Now let's move onto something a bit more useful (you are also free to start clicking links again), a collection of popular movies on Netflix, hopefully by now you'll be able to read this;

```YAML  
  Popular Movies on Netflix:
    sort_title: +1_Popular on Netflix
    summary: Movies popular on Netflix today
    url_poster: https://theposterdb.com/api/assets/168827
    collection_order: custom
    sync_mode: sync
    imdb_list: 
      url: https://www.imdb.com/search/title/?title_type=feature,tv_movie,documentary,short&companies=co0144901&user_rating=6.0,10.0
      limit: 20
````

As you can see, it's very similar to the Universal Classic Monsters, except for two bits.

> sort_title: +1_Popular on Netflix

The +1_ infront of the sort_title field allows for granular ordering of collections in the Plex collections screen. In my case this will make the Netflix collection the second collection in Plex (I have a +0_ for another collection in my library that I want displayed first).

> limit: 20

All this does, is limit the number of movies we will pull from the list.

## A Nicolas Cage collection using the plex_search builder

Let's move onto how to use the builders documentation, let's open the [plex_search builder documentation](https://metamanager.wiki/en/latest/metadata/builders/plex.html#plex-search), and make a collection for all movies in your library that have the glorious Nicolas Cage in them.

First thing first, let's copy our template, without the imdb_search, add in a custom name, sort_title, summary, and url_poster. Then add the collection_order, and sync_mode.

```YAML
  Nicolas Cage:
    sort_title: Nicolas Cage
    summary: A collection on movies with Nicolas Cage
    url_poster: https://theposterdb.com/api/assets/201814
    collection_order: custom
    sync_mode: sync
```

If you scroll through the plex_search documentation, you'll find no reference to sync_mode, that's because this is completely local, we aren't querying an online list, we're purely doing searches based on metadata Plex already has, so we can remove that. 

Let's first use the plex_search builder to search for movies with Nic Cage in them;

```YAML
  Nicolas Cage:
    sort_title: Nicolas Cage
    summary: A collection of movies staring Nicolas Cage
    url_poster: https://theposterdb.com/api/assets/201814
    collection_order: custom
    plex_search:
      all:
        actor: Nicolas Cage
```

What we have done here, is used the Plex search function, to search for the actor with the name, Nicolas Cage. 

Simply adding this to your Movies.yml file will create a collection containing every movie Nic Cage is in, that you have in your library. But what happens if you want some more granularity in this collection?

Let's sort the collection, with the latest movies at the top, but how do we do this? Luckily for us, Plex Meta Manager is very well documented, so let's see what the [documentation says about sorting the the plex_serch builder](https://metamanager.wiki/en/latest/metadata/builders/plex.html#sort-options), it looks like there is a filter called sort_order, which takes a value that sorts by released year, descending.

> release.desc

Let's add that in there (watch your indentation);

```YAML
  Nicolas Cage:
    sort_title: Nicolas Cage
    summary: A collection of movies staring Nicolas Cage
    url_poster: https://theposterdb.com/api/assets/201814
    collection_order: custom
    plex_search:
      all:
        actor: Nicolas Cage
      sort_order: release.desc
```

Since this is a option in the plex_search builder, it needs to be included in the plex_search indentation block.

![](/images/plex_search.png)

Again, the red is the collection, the blue is the content of the plex_search builder. And you're done!

## A complex collection of documentaries using the tmdb_discover builder

There are loads of builders you can find in the documentation, but we'll do one more together for safety. This time we'll build a collection of documentaries released after 2015, with a user score that is greater than, or equalling 6, sort them based on their rating, and limit the collection to 20 movies. It sounds like a lot, but I promise it's not that hard. We'll be using the [tmdb_discover](https://metamanager.wiki/en/latest/metadata/builders/tmdb.html#tmdb-discover) builder to achieve this.

Again, first thing to do, is drop our template in, add a custom name, sort_title, summary, and poster, as well as the normal collection_order, and sync_mode.

```YAML
  Highest rating documentaries in the last 5 years:
    sort_title: Highest rated documentaries in the last 5 years
    summary: This collection contains some of the highest rated documentaries released in the last 5 years
    url_poster: https://theposterdb.com/api/123456
    collection_order: custom
    sync_mode: sync
```

Let's split our conditions up, and tackle them one at a time.

### Date range (released after 2015)

The tmdb_discover builder takes a field called release_date.gte;

> release_date.gte

Which translates into 'release date greater than', and takes a date value in the format MM/DD/YYYY, let's whack that into the tmdb_discover builder within our collection, with the date 01/01/2015.

```YAML
  Highest rating documentaries in the last 5 years:
    sort_title: Highest rated documentaries in the last 5 years
    summary: This collection contains some of the highest rated documentaries released in the last 5 years
    url_poster: https://theposterdb.com/api/123456
    collection_order: custom
    sync_mode: sync
    tmdb_discover:
      primary_release_date.lte: 01/01/2015
```

### Genre

If you scrolled a bit further down in the [tmdb_discover](https://metamanager.wiki/en/latest/metadata/builders/tmdb.html#tmdb-discover) builder you would have also seen that the builder takes a genre field called with_genre, let's put that in there.

> with_genres: 

```YAML
  Highest rating documentaries in the last 5 years:
    sort_title: Highest rated documentaries in the last 5 years
    summary: This collection contains some of the highest rated documentaries released in the last 5 years
    url_poster: https://theposterdb.com/api/123456
    collection_order: custom
    sync_mode: sync
    tmdb_discover:
      primary_release_date.lte: 01/01/2015
      with_genres: 
```

But what value do we use? 

Search the TMDB for documentary, and grab it from the URL.

> [https://www.themoviedb.org/genre/**99**-documentary/movie](https://www.themoviedb.org/genre/99-documentary/movie)

Let's add it in;

```YAML
  Highest rating documentaries in the last 5 years:
    sort_title: Highest rated documentaries in the last 5 years
    summary: This collection contains some of the highest rated documentaries released in the last 5 years
    url_poster: https://theposterdb.com/api/123456
    collection_order: custom
    sync_mode: sync
    tmdb_discover:
      primary_release_date.lte: 01/01/2015
      with_genres: 99
```

### User rating ??? 6

We can find this option;

> vote_count.gte

This translates into 'vote count, greater than'. Let's put it in there too;

```YAML
  Highest rating documentaries in the last 5 years:
    sort_title: Highest rated documentaries in the last 5 years
    summary: This collection contains some of the highest rated documentaries released in the last 5 years
    url_poster: https://theposterdb.com/api/123456
    collection_order: custom
    sync_mode: sync
    tmdb_discover:
      primary_release_date.lte: 01/01/2015
      with_genres: 99
      vote_count.gte: 6
```

### Sort by user rating

Now lets sort the collection by their user rating, checking the documentation brings up this field we can include;

> sort_by

There are **loads** of sorting options you can choose from, they are all listed in [the documentation here](https://metamanager.wiki/en/latest/metadata/builders/tmdb.html#sort-options), we are just going to use the value vote_average.desc, which translates into 'vote average, descending'. So whack that in there, and you'll have this;

```YAML
  Highest rating documentaries in the last 5 years:
    sort_title: Highest rated documentaries in the last 5 years
    summary: This collection contains some of the highest rated documentaries released in the last 5 years
    url_poster: https://theposterdb.com/api/123456
    collection_order: custom
    sync_mode: sync
    tmdb_discover:
      primary_release_date.lte: 01/01/2015
      with_genres: 99
      vote_count.gte: 6
      sort_by: vote_average.desc
```

### limit

Finally, lets limit the amount of movies we pull from the list;

> limit

The default value is 100, which is way to high, let's limit it to 20.

```YAML
  Highest rating documentaries in the last 5 years:
    sort_title: Highest rated documentaries in the last 5 years
    summary: This collection contains some of the highest rated documentaries released in the last 5 years
    url_poster: https://theposterdb.com/api/123456
    collection_order: custom
    sync_mode: sync
    tmdb_discover:
      primary_release_date.lte: 01/01/2015
      with_genres: 99
      sort_by: vote_average.desc
      limit: 20
```

That's all there is to it, I really hope this helped, have a browse through the [Plex Meta Manager Wiki](https://metamanager.wiki/en/latest/index.html), and see what other collections you come up with!
