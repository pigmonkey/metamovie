# Metamovie

Metamovie is a simple script to fetch information about a movie and store it as
[git-annex][1] [metadata][2].

## Usage

```
$ metamovie --help
usage: metamovie [-h] [-d] [-f] [-i ID] [-l LIMIT] filename

Fetch git-annex metadata from IMDB

positional arguments:
  filename

optional arguments:
  -h, --help            show this help message and exit
  -d, --dry             Do not write metadata.
  -f, --force           Do not prompt user to accept metadata.
  -i ID, --id ID        Specify the IMDB ID of the movie.
  -l LIMIT, --limit LIMIT
                        Limit the number of values to be saved for list
                        attributes (default: 6).
```

## Attributes

The following attributes are set:

* IMDB ID
* Title
* Year
* Rating
* Director
* Genre
* Cast
* Writer
* Runtime (average)
* Kind (movie or TV show)
* Series (TV shows only)
* Season (TV shows only)
* Episode number (TV shows only)

For attributes whose values are lists (such as director, genre, cast and
writer) only the first six results are stored. This can be changed with the
`-l` option.

## Examples

The default behaviour is to search for the correct movie based on the filename.
This works well the majority of the time.

```
$ metamovie ~/video/movies/blade_runner.mkv
0083658:        Blade Runner (1982)
0126817:        Blade Runner (1997) (VG)
2797762:        Blade Runner (2013)
1856101:        Untitled Blade Runner Project (2017)
1775395:        "5 Second Movies" Blade Runner (2009)
4006976:        "Profiles" Blade Runner (2001)
3182880:        "Supernatural" Blade Runners (2014)
3893266:        "James's Daily Movie Reviews" Blade Runner (2014)
5274840:        "Merci Qui?" Blade Runner (2009)
2263223:        "Geek Crash Course" Blade Runner (2012)
5545480:        "Renegade Cut" Blade Runner (2016)
5206542:        Blade Runner: On the Set (1982) (V)
1080585:        Dangerous Days: Making Blade Runner (2007) (V)
0281011:        On the Edge of 'Blade Runner' (2000) (TV)
3811070:        "#NerdNoise" The Blade Runner Dress (2012)
1877226:        "Cromartie High School" Blade Runners High (2003)
1846491:        Blade Runner 60: Director's Cut (2012)
0162461:        Making of 'Blade Runner', The (1997)
4769072:        Blade Runner Holiday Special, The (2013)
2952584:        CodeRunner (2012) (VG)
Enter the ID of the correct movie [0083658]:
Ok, using 0083658
{'cast': [u'Harrison Ford',
          u'Rutger Hauer',
          u'Sean Young',
          u'Edward James Olmos',
          u'M. Emmet Walsh',
          u'Daryl Hannah'],
 'director': [u'Ridley Scott'],
 'genre': [u'Sci-Fi', u'Thriller'],
 'imdb_id': '0083658',
 'kind': u'movie',
 'rating': 8.2,
 'runtime': 117,
 'title': u'Blade Runner',
 'writer': [u'Philip K. Dick', u'Hampton Fancher', u'David Webb Peoples'],
 'year': 1982}
Is this correct? (y/n) y
metadata blade_runner.mkv
  cast=Daryl Hannah
  cast=Edward James Olmos
  cast=Harrison Ford
  cast=M. Emmet Walsh
  cast=Rutger Hauer
  cast=Sean Young
  cast-lastchanged=2016-04-21@02-59-26
  director=Ridley Scott
  director-lastchanged=2016-04-21@02-59-26
  genre=Sci-Fi
  genre=Thriller
  genre-lastchanged=2016-04-21@02-59-26
  imdb_id=0083658
  imdb_id-lastchanged=2016-04-21@02-59-26
  kind=movie
  kind-lastchanged=2016-04-21@02-59-26
  lastchanged=2016-04-21@02-59-26
  rating=8.2
  rating-lastchanged=2016-04-21@02-59-26
  runtime=117
  runtime-lastchanged=2016-04-21@02-59-26
  title=Blade Runner
  title-lastchanged=2016-04-21@02-59-26
  writer=David Webb Peoples
  writer=Hampton Fancher
  writer=Philip K. Dick
  writer-lastchanged=2016-04-21@02-59-26
  year=1982
  year-lastchanged=2016-04-21@02-59-26
ok
(recording state in git...)
```

However, if you know the the correct IMDB ID of the movie, you can specify it.

```
$ metamovie -i 0068646 ~/video/movies/the_godfather-part_i.mp4
{'cast': [u'Marlon Brando',
          u'Al Pacino',
          u'James Caan',
          u'Richard S. Castellano',
          u'Robert Duvall',
          u'Sterling Hayden'],
 'director': [u'Francis Ford Coppola'],
 'genre': [u'Crime', u'Drama'],
 'imdb_id': '0068646',
 'kind': u'movie',
 'rating': 9.2,
 'runtime': 175,
 'title': u'The Godfather',
 'writer': [u'Mario Puzo', u'Francis Ford Coppola'],
 'year': 1972}
Is this correct? (y/n) y
metadata the_godfather-part_i.mp4
  cast=Al Pacino
  cast=James Caan
  cast=Marlon Brando
  cast=Richard S. Castellano
  cast=Robert Duvall
  cast=Sterling Hayden
  cast-lastchanged=2016-04-21@03-02-43
  director=Francis Ford Coppola
  director-lastchanged=2016-04-21@03-02-43
  genre=Crime
  genre=Drama
  genre-lastchanged=2016-04-21@03-02-43
  imdb_id=0068646
  imdb_id-lastchanged=2016-04-21@03-02-43
  kind=movie
  kind-lastchanged=2016-04-21@03-02-43
  lastchanged=2016-04-21@03-02-43
  rating=9.2
  rating-lastchanged=2016-04-21@03-02-43
  runtime=175
  runtime-lastchanged=2016-04-21@03-02-43
  title=The Godfather
  title-lastchanged=2016-04-21@03-02-43
  writer=Francis Ford Coppola
  writer=Mario Puzo
  writer-lastchanged=2016-04-21@03-02-43
  year=1972
  year-lastchanged=2016-04-21@03-02-43
ok
(recording state in git...)
```

When used on an episode of a TV series, additional attributes will be set.

```
$ metamovie -i 0993925 ~/video/tv/battlestar_galactica/battlestar_galactica-s04e03-the_ties_that_bind.avi
{'cast': [u'Edward James Olmos',
          u'Mary McDonnell',
          u'Katee Sackhoff',
          u'Jamie Bamber',
          u'James Callis',
          u'Tricia Helfer'],
 'director': [u'Michael Nankin'],
 'episode': 3,
 'genre': [u'Action', u'Adventure', u'Drama', u'Sci-Fi'],
 'imdb_id': '0993925',
 'kind': u'tv series',
 'rating': 8.0,
 'runtime': 44,
 'season': 4,
 'series': u'Battlestar Galactica',
 'title': u'Battlestar Galactica : The Ties That Bind',
 'writer': [u'Glen A. Larson', u'Michael Taylor', u'Ronald D. Moore'],
 'year': 2008}
Is this correct? (y/n) y
metadata bsg403_the ties that bind.avi
  cast=Edward James Olmos
  cast=James Callis
  cast=Jamie Bamber
  cast=Katee Sackhoff
  cast=Mary McDonnell
  cast=Tricia Helfer
  cast-lastchanged=2016-04-21@03-06-10
  director=Michael Nankin
  director-lastchanged=2016-04-21@03-06-10
  episode=3
  episode-lastchanged=2016-04-21@03-06-10
  genre=Action
  genre=Adventure
  genre=Drama
  genre=Sci-Fi
  genre-lastchanged=2016-04-21@03-06-10
  imdb_id=0993925
  imdb_id-lastchanged=2016-04-21@03-06-10
  kind=tv series
  kind-lastchanged=2016-04-21@03-06-10
  lastchanged=2016-04-21@03-06-10
  rating=8.0
  rating-lastchanged=2016-04-21@03-06-10
  runtime=44
  runtime-lastchanged=2016-04-21@03-06-10
  season=4
  season-lastchanged=2016-04-21@03-06-10
  series=Battlestar Galactica
  series-lastchanged=2016-04-21@03-06-10
  title=Battlestar Galactica : The Ties That Bind
  title-lastchanged=2016-04-21@03-06-10
  writer=Glen A. Larson
  writer=Michael Taylor
  writer=Ronald D. Moore
  writer-lastchanged=2016-04-21@03-06-10
  year=2008
  year-lastchanged=2016-04-21@03-06-10
ok
(recording state in git...)
```

With the metadata set, you can use standard git-annex tools to query the files. For instance, maybe you're in the mood for a good James Stewart movie.

```
$ git annex find --metadata cast="James Stewart" --metadata "rating>=8"
movies/anatomy_of_a_murder.mkv
movies/rear_window.mkv
movies/vertigo.mkv
```

Or, you can use [metadata driven views][3] to group your movies by genre.

```
$ git annex view "genre=*" 
$ tree -d
.
├── Action
├── Adventure
├── Animation
├── Biography
├── Comedy
├── Crime
├── Documentary
├── Drama
├── Family
├── Fantasy
├── Film-Noir
├── History
├── Horror
├── Music
├── Musical
├── Mystery
├── News
├── Romance
├── Sci-Fi
├── Short
├── Sport
├── Thriller
├── War
└── Western
```

## Requirements

* [Python 2][4]
* [IMDbPyY][5]
* [git-annex][1]


[1]: https://git-annex.branchable.com/
[2]: https://git-annex.branchable.com/metadata/
[3]: https://git-annex.branchable.com/tips/metadata_driven_views/
[4]: https://www.python.org/
[5]: http://imdbpy.sourceforge.net/
