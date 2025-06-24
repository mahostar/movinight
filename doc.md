themoviedb

Code Climate Gem Version

A Ruby wrapper for the The Movie Database API.

Ruby >= 3.1

Provides a simple, easy to use interface for the Movie Database API.

Get your API key here.
Getting started

$ gem install themoviedb

Example usage of the 'themovedb' gem

http://themoviedb.herokuapp.com/

https://github.com/ahmetabdi/themoviedb-example
Configuration

Tmdb::Api.key("KEY_HERE")

You can change the language for the returned data with this:

Tmdb::Api.language("de")

The default language is english. The API supports translations just be aware that it does not fall back to English in the event that a field hasn't been translated.
Resources

Current available resources:

    Company
    Movie
    TV
    TV Seasons
    TV Episodes
    Collection
    Person
    Genre
    Job
    Find

Missing resources:

    Account
    Authentication
    Changes
    Collections
    Credits
    Discover
    Keywords
    Lists
    Networks
    Reviews

Example

Tmdb::Movie.find("batman")
Tmdb::TV.find("fringe")
Tmdb::Collection.find("spiderman")
Tmdb::Person.find("samuel jackson")
Tmdb::Company.find("lucas")
Tmdb::Genre.find("drama")

Usage

resources => person, movie, tv, collection, company, multi

@search = Tmdb::Search.new
@search.resource('person') # determines type of resource
@search.query('samuel jackson') # the query to search against
@search.fetch # makes request

Find

The supported external sources for each object are as follows: Movies: imdb_id People: imdb_id, freebase_mid, freebase_id, tvrage_id TV Series: imdb_id, freebase_mid, freebase_id, tvdb_id, tvrage_id

Tmdb::Find.imdb_id('id')
Tmdb::Find.freebase_mid('id')
Tmdb::Find.freebase_id('id')
Tmdb::Find.tvrage_id('id')
Tmdb::Find.tvdb_id('id')

The responses are in the hash with movie_results, person_results and tv_results
Configuration

Get the system wide configuration information. Some elements of the API require some knowledge of this configuration data. The purpose of this is to try and keep the actual API responses as light as possible. This method currently holds the data relevant to building image URLs as well as the change key map. To build an image URL, you will need 3 pieces of data. The base_url, size and file_path. Simply combine them all and you will have a fully qualified URL. Here’s an example URL: http://cf2.imgobject.com/t/p/w500/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg

configuration = Tmdb::Configuration.new
configuration.base_url
configuration.secure_base_url
configuration.poster_sizes
configuration.backdrop_sizes
configuration.profile_sizes
configuration.logo_sizes

Detail

Every example documented below uses the movie Fight Club (id 550), the TV show Breaking Bad (id 1396), and every person uses Brad Pitt (id 287). These are only used as examples to show what a real world request looks like.
Movie

movie = Tmdb::Movie.detail(550)
movie.adult => false
movie.backdrop_path => "/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg"
movie.belongs_to_collection => nil
movie.budget => 63000000
movie.genres => [{"id"=>28, "name"=>"Action"}, {"id"=>18, "name"=>"Drama"}, {"id"=>53, "name"=>"Thriller"}]
movie.homepage => ""
movie.id => 550
movie.imdb_id => "tt0137523"
movie.original_title => "Fight Club"
movie.overview => "A ticking-time-bomb insomniac and a slippery soap salesman channel primal male aggression into a shocking new form of therapy. Their concept catches on, with underground \"fight clubs\" forming in every town, until an eccentric gets in the way and ignites an out-of-control spiral toward oblivion."
movie.popularity => 7.4
movie.poster_path => "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg"
movie.production_companies => [{"name"=>"20th Century Fox", "id"=>25}, {"name"=>"Fox 2000 Pictures", "id"=> 711}, {"name"=>"Regency Enterprises", "id"=>508}]
movie.production_countries => [{"iso_3166_1"=>"DE", "name"=>"Germany"}, {"iso_3166_1"=>"US", "name"=>"United States of America"}]
movie.release_date => "1999-10-14"
movie.revenue => 100853753
movie.runtime => 139
movie.spoken_languages => [{"iso_639_1"=>"en", "name"=>"English"}]
movie.status => "Released"
movie.tagline => "How much can you know about yourself if you've never been in a fight?"
movie.title => "Fight Club"
movie.vote_average => 8.8
movie.vote_count => 234

Get the latest movie id.

Tmdb::Movie.latest

Get the list of upcoming movies. This list refreshes every day. The maximum number of items this list will include is 100.

Tmdb::Movie.upcoming

Get the list of movies playing in theatres. This list refreshes every day. The maximum number of items this list will include is 100.

Tmdb::Movie.now_playing

Get the list of popular movies on The Movie Database. This list refreshes every day.

Tmdb::Movie.popular

Get the list of top rated movies. By default, this list will only include movies that have 10 or more votes. This list refreshes every day.

Tmdb::Movie.top_rated

Movie - Alternative Titles

Get the alternative titles for a specific movie id.

Tmdb::Movie.alternative_titles(22855)

Movie - Images

Get the images (posters and backdrops) for a specific movie id.

@movie = Tmdb::Movie.images(22855)

Grab Backdrops

@movie['backdrops']

Grab Posters

@movie['posters']

Movie - Casts

Get the cast information for a specific movie id.

Tmdb::Movie.casts(22855)

Movie - Crew

Get the crew information for a specific movie id.

Tmdb::Movie.crew(22855)

Movie - Keywords

Get the plot keywords for a specific movie id.

Tmdb::Movie.keywords(22855)

Movie - Releases

Get the release date by country for a specific movie id.

Tmdb::Movie.releases(22855)

Movie - Trailers

Get the trailers for a specific movie id.

Tmdb::Movie.trailers(22855)

Movie - Translations

Get the translations for a specific movie id.

Tmdb::Movie.translations(22855)

Movie - Similar Movies

Get the similar movies for a specific movie id.

Tmdb::Movie.similar_movies(22855)

Movie - Lists

Get the lists that the movie belongs to.

Tmdb::Movie.lists(22855)

Movie - Changes

Get the changes for a specific movie id.

Tmdb::Movie.changes(22855)

Movie - Credits

Get the credits for a specific movie id.

Tmdb::Movie.credits(22855)

TV

show = Tmdb::TV.detail(1396)

Get the list of popular TV shows. This list refreshes every day.

Tmdb::TV.popular

Get the list of top rated TV shows. By default, this list will only include TV shows that have 2 or more votes. This list refreshes every day.

Tmdb::TV.top_rated

TV - Images

Get the images (posters and backdrops) for a TV series.

@show = Tmdb::TV.images(1396)

Grab Backdrops

@show['backdrops']

Grab Posters

@show['posters']

TV - Cast

Get the cast information about a TV series.

Tmdb::TV.cast(1396)

TV - Crew

Get the crew information about a TV series.

Tmdb::TV.crew(1396)

TV - External IDs

Get the external ids that we have stored for a TV series.

Tmdb::TV.external_ids(1396)

Season

show = Tmdb::Season.detail(1396, 1)

Season - Images

Get the images (posters) that we have stored for a TV season by season number.

@season = Tmdb::Season.images(1396, 1)

Grab Posters

@season['posters']

Season - Cast

Get the cast credits for a TV season by season number.

Tmdb::Season.cast(1396, 1)

Season - Crew

Get the crew credits for a TV season by season number.

Tmdb::Season.crew(1396, 1)

Season - External IDs

Get the external ids that we have stored for a TV season by season number.

Tmdb::Season.external_ids(1396, 1)

Episode

episode = Tmdb::Episode.detail(1396, 1, 1)

Episode - Images

Get the images (episode stills) for a TV episode by combination of a season and episode number.

@episode = Tmdb::Episode.images(1396, 1, 1)

Grab Stills

@episode['stills']

Episode - Cast

Get the TV episode cast credits by combination of season and episode number.

Tmdb::Episode.cast(1396, 1, 1)

Episode - Crew

Get the TV episode crew credits by combination of season and episode number.

Tmdb::Episode.crew(1396, 1, 1)

Episode - External IDs

Get the external ids for a TV episode by comabination of a season and episode number.

Tmdb::Episode.external_ids(1396, 1, 1)

Company

company = Tmdb::Company.detail(1)
company.id => 1
company.description => nil
company.homepage => "http://www.lucasfilm.com"
company.logo_path => "/8rUnVMVZjlmQsJ45UGotDOUznxj.png"
company.name => "Lucasfilm"
company.parent_company => nil

Get the list of movies associated with a particular company.

Tmdb::Company.movies(1)

Collection

collection = Tmdb::Collection.detail(51845)
collection.id => 51845
collection.backdrop_path => "..."
collection.parts => "..."
collection.poster_path => "..."
collection.name => "DC Universe Animated Original Movies"

Get all of the images for a particular collection by collection id.

Tmdb::Collection.images(51845)

Person

person = Tmdb::Person.detail(287)
person.id => 287
person.name => "Brad Pitt"
person.place_of_birth => "Shawnee, Oklahoma, United States"
person.also_known_as => []
person.adult => false
person.biography => "From Wikipedia, the free"..
person.birthday => "1963-12-18"
person.deathday => ""
person.homepage => "http://simplybrad.com/"
person.profile_path => "w8zJQuN7tzlm6FY9mfGKihxp3Cb.jpg"

Get the list of popular people on The Movie Database. This list refreshes every day.

Tmdb::Person.popular

Get the latest person id.

Tmdb::Person.latest

Get the credits for a specific person id.

Tmdb::Person.credits(287)

Get the images for a specific person id.

Tmdb::Person.images(287)

Get the changes for a specific person id.

Tmdb::Person.changes(287)

Genre

genre = Tmdb::Genre.detail(18)
genre.id => 18
genre.name => "Drama"
genre.page => 1
genre.total_pages => 45
genre.total_results => 883
genre.results => [...]
genre.get_page(page_number) => Returns next set of movies.

Get a list of all genres.

Tmdb::Genre.list

Job

Get a list of all jobs.

Tmdb::Job.list


Getting Started

Welcome to version 3 of The Movie Database (TMDB) API. This is where you will find the definitive list of currently available methods for our movie, tv, actor and image API.
1Pick a language
2Credentials
Get API Key
Header
Log in to use your API keys
3Try it!

1

const url = 'https://api.themoviedb.org/3/authentication';

2

const options = {method: 'GET', headers: {accept: 'application/json'}};

3

​

4

fetch(url, options)

5

  .then(res => res.json())

6

  .then(json => console.log(json))

7

  .catch(err => console.error(err));

Click Try It! to start a request and see the response here! Or choose an example:
application/json
Popular Endpoints
Last 30 Days
	
	
	
	
	
	
Make a request to see them here or Try It!

Updated about 2 years ago

Movie List
get https://api.themoviedb.org/3/movie/changes

Get a list of all of the movie ids that have been changed in the past 24 hours.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

You can query this method up to 14 days at a time. Use the start_date and end_date query parameters. 100 items are returned per page.
Query Params
end_date
date
page
int32
Defaults to 1
start_date
date
Response
Response body
object
results
array of objects
object
id
integer
Defaults to 0
adult
boolean
Defaults to true
page
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Updated about 2 years ago 

TV List
get https://api.themoviedb.org/3/tv/changes
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

You can query this method up to 14 days at a time. Use the start_date and end_date query parameters. 100 items are returned per page.
Query Params
end_date
date
page
int32
Defaults to 1
start_date
date
Response
Response body
object
results
array of objects
object
id
integer
Defaults to 0
adult
boolean
Defaults to true
page
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

People List
get https://api.themoviedb.org/3/person/changes
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

You can query this method up to 14 days at a time. Use the start_date and end_date query parameters. 100 items are returned per page.
Query Params
end_date
date
page
int32
Defaults to 1
start_date
date
Response
Response body
object
results
array of objects
object
id
integer
Defaults to 0
adult
boolean
Defaults to true
page
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Updated about 2 years ago 


People List
get https://api.themoviedb.org/3/person/changes
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

You can query this method up to 14 days at a time. Use the start_date and end_date query parameters. 100 items are returned per page.
Query Params
end_date
date
page
int32
Defaults to 1
start_date
date
Response
Response body
object
results
array of objects
object
id
integer
Defaults to 0
adult
boolean
Defaults to true
page
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Updated about 2 years ago 

Movie
get https://api.themoviedb.org/3/discover/movie

Find movies using over 30 filters and sort options.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

Advanced Filtering

If you specify the region parameter, the regional release date will be used instead of the primary release date. The date returned will be the first date based on your query (ie. if a with_release_type is specified). It's important to note the order of the release types that are used. Specifying 2|3 would return the limited theatrical release date as opposed to 3|2 which would return the theatrical date.
AND/OR Logic

Also note that a number of filters support being comma (,) or pipe (|) separated. Comma's are treated like an AND query while pipe's are treated like an OR. This allows for quite complex filtering depending on your desired results.
Query Params
certification
string

use in conjunction with region
certification.gte
string

use in conjunction with region
certification.lte
string

use in conjunction with region
certification_country
string

use in conjunction with the certification, certification.gte and certification.lte filters
include_adult
boolean
Defaults to false
include_video
boolean
Defaults to false
language
string
Defaults to en-US
page
int32
Defaults to 1
primary_release_year
int32
primary_release_date.gte
date
primary_release_date.lte
date
region
string
release_date.gte
date
release_date.lte
date
sort_by
string
Defaults to popularity.desc
vote_average.gte
float
vote_average.lte
float
vote_count.gte
float
vote_count.lte
float
watch_region
string

use in conjunction with with_watch_monetization_types or with_watch_providers
with_cast
string

can be a comma (AND) or pipe (OR) separated query
with_companies
string

can be a comma (AND) or pipe (OR) separated query
with_crew
string

can be a comma (AND) or pipe (OR) separated query
with_genres
string

can be a comma (AND) or pipe (OR) separated query
with_keywords
string

can be a comma (AND) or pipe (OR) separated query
with_origin_country
string
with_original_language
string
with_people
string

can be a comma (AND) or pipe (OR) separated query
with_release_type
int32

possible values are: [1, 2, 3, 4, 5, 6] can be a comma (AND) or pipe (OR) separated query, can be used in conjunction with region
with_runtime.gte
int32
with_runtime.lte
int32
with_watch_monetization_types
string

possible values are: [flatrate, free, ads, rent, buy] use in conjunction with watch_region, can be a comma (AND) or pipe (OR) separated query
with_watch_providers
string

use in conjunction with watch_region, can be a comma (AND) or pipe (OR) separated query
without_companies
string
without_genres
string
without_keywords
string
without_watch_providers
string
year
int32
Response
Response body
object
page
integer
Defaults to 0
results
array of objects
object
adult
boolean
Defaults to true
backdrop_path
string
genre_ids
array of integers
id
integer
Defaults to 0
original_language
string
original_title
string
overview
string
popularity
number
Defaults to 0
poster_path
string
release_date
string
title
string
video
boolean
Defaults to true
vote_average
number
Defaults to 0
vote_count
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0


TV
get https://api.themoviedb.org/3/discover/tv

Find TV shows using over 30 filters and sort options.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

Advanced Filtering

If you specify the region parameter, the regional release date will be used instead of the primary release date. The date returned will be the first date based on your query (ie. if a with_release_type is specified). It's important to note the order of the release types that are used. Specifying 2|3 would return the limited theatrical release date as opposed to 3|2 which would return the theatrical date.
AND/OR Logic

Also note that a number of filters support being comma (,) or pipe (|) separated. Comma's are treated like an AND query while pipe's are treated like an OR. This allows for quite complex filtering depending on your desired results.
Query Params
air_date.gte
date
air_date.lte
date
first_air_date_year
int32
first_air_date.gte
date
first_air_date.lte
date
include_adult
boolean
Defaults to false
include_null_first_air_dates
boolean
Defaults to false
language
string
Defaults to en-US
page
int32
Defaults to 1
screened_theatrically
boolean
sort_by
string
Defaults to popularity.desc
timezone
string
vote_average.gte
float
vote_average.lte
float
vote_count.gte
float
vote_count.lte
float
watch_region
string

use in conjunction with with_watch_monetization_types or with_watch_providers
with_companies
string

can be a comma (AND) or pipe (OR) separated query
with_genres
string

can be a comma (AND) or pipe (OR) separated query
with_keywords
string

can be a comma (AND) or pipe (OR) separated query
with_networks
int32
with_origin_country
string
with_original_language
string
with_runtime.gte
int32
with_runtime.lte
int32
with_status
string

possible values are: [0, 1, 2, 3, 4, 5], can be a comma (AND) or pipe (OR) separated query
with_watch_monetization_types
string

possible values are: [flatrate, free, ads, rent, buy] use in conjunction with watch_region, can be a comma (AND) or pipe (OR) separated query
with_watch_providers
string

use in conjunction with watch_region, can be a comma (AND) or pipe (OR) separated query
without_companies
string
without_genres
string
without_keywords
string
without_watch_providers
string
with_type
string

possible values are: [0, 1, 2, 3, 4, 5, 6], can be a comma (AND) or pipe (OR) separated query
Response
Response body
object
page
integer
Defaults to 0
results
array of objects
object
backdrop_path
string
first_air_date
string
genre_ids
array of integers
id
integer
Defaults to 0
name
string
origin_country
array of strings
original_language
string
original_name
string
overview
string
popularity
number
Defaults to 0
poster_path
string
vote_average
integer
Defaults to 0
vote_count
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Updated over 1 year ago 

Movie List
get https://api.themoviedb.org/3/genre/movie/list

Get the list of official genres for movies.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

Query Params
language
string
Defaults to en
Response
Response body
object
genres
array of objects
object
id
integer
Defaults to 0
name
string

Updated about 2 years ago
Find By ID
TV List
get https://api.themoviedb.org/3/genre/tv/list

Get the list of official genres for TV shows.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

Query Params
language
string
Defaults to en
Response
Response body
object
genres
array of objects
object
id
integer
Defaults to 0
name
string

Updated about 2 years ago
Movie List


Rated Movies
get https://api.themoviedb.org/3/guest_session/{guest_session_id}/rated/movies

Get the rated movies for a guest session.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

Path Params
guest_session_id
string
required
Query Params
language
string
Defaults to en-US
page
int32
Defaults to 1
sort_by
string
Defaults to created_at.asc
Response
Response body
object
page
integer
Defaults to 0
results
array of objects
object
adult
boolean
Defaults to true
backdrop_path
string
genre_ids
array of integers
id
integer
Defaults to 0
original_language
string
original_title
string
overview
string
popularity
number
Defaults to 0
poster_path
string
release_date
string
title
string
video
boolean
Defaults to true
vote_average
number
Defaults to 0
vote_count
integer
Defaults to 0
rating
number
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Updated about 2 years ago
TV List


Rated TV
get https://api.themoviedb.org/3/guest_session/{guest_session_id}/rated/tv

Get the rated TV shows for a guest session.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

Path Params
guest_session_id
string
required
Query Params
language
string
Defaults to en-US
page
int32
Defaults to 1
sort_by
string
Defaults to created_at.asc
Response
Response body
object
page
integer
Defaults to 0
results
array of objects
object
adult
boolean
Defaults to true
backdrop_path
string
genre_ids
array of integers
id
integer
Defaults to 0
origin_country
array of strings
original_language
string
original_name
string
overview
string
popularity
number
Defaults to 0
poster_path
string
first_air_date
string
name
string
vote_average
number
Defaults to 0
vote_count
integer
Defaults to 0
rating
number
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Updated about 2 years ago 


Rated TV Episodes
get https://api.themoviedb.org/3/guest_session/{guest_session_id}/rated/tv/episodes

Get the rated TV episodes for a guest session.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

Path Params
guest_session_id
string
required
Query Params
language
string
Defaults to en-US
page
int32
Defaults to 1
sort_by
string
Defaults to created_at.asc
Response
Response body
object
page
integer
Defaults to 0
results
array of objects
object
air_date
string
episode_number
integer
Defaults to 0
id
integer
Defaults to 0
name
string
overview
string
production_code
string
runtime
integer
Defaults to 0
season_number
integer
Defaults to 0
show_id
integer
Defaults to 0
still_path
string
vote_average
number
Defaults to 0
vote_count
integer
Defaults to 0
rating
number
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Details
get https://api.themoviedb.org/3/keyword/{keyword_id}
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

Path Params
keyword_id
int32
required
Response
Response body
object
id
integer
Defaults to 0
name
string

Updated about 2 years ago
Rated TV Episodes
Movies

Movies
get deprecatedhttps://api.themoviedb.org/3/keyword/{keyword_id}/movies
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

This method is deprecated, you should use /discover/movie instead.
Path Params
keyword_id
string
required
Query Params
include_adult
boolean
Defaults to false
language
string
Defaults to en-US
page
int32
Defaults to 1
Response
Response body
object
id
integer
Defaults to 0
page
integer
Defaults to 0
results
array of objects
object
adult
boolean
Defaults to true
backdrop_path
string
genre_ids
array of integers
id
integer
Defaults to 0
original_language
string
original_title
string
overview
string
popularity
number
Defaults to 0
poster_path
string
release_date
string
title
string
video
boolean
Defaults to true
vote_average
number
Defaults to 0
vote_count
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Popular
get https://api.themoviedb.org/3/movie/popular

Get a list of movies ordered by popularity.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

    📘

    Note

    This call is really just a discover call behind the scenes. If you would like to tweak any of the default filters head over and read about discover.

curl --request GET \
     --url 'https://api.themoviedb.org/3/discover/movie?include_adult=false&include_video=false&language=en-US&page=1&sort_by=popularity.desc' \
     --header 'Authorization: Bearer ACCESS_TOKEN' \
     --header 'accept: application/json'

Query Params
language
string
Defaults to en-US
page
int32
Defaults to 1
region
string

ISO-3166-1 code
Response
Response body
object
page
integer
Defaults to 0
results
array of objects
object
adult
boolean
Defaults to true
backdrop_path
string
genre_ids
array of integers
id
integer
Defaults to 0
original_language
string
original_title
string
overview
string
popularity
number
Defaults to 0
poster_path
string
release_date
string
title
string
video
boolean
Defaults to true
vote_average
number
Defaults to 0
vote_count
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Updated about 2 years ago 

Now Playing
get https://api.themoviedb.org/3/movie/now_playing

Get a list of movies that are currently in theatres.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

    📘

    Note

    This call is really just a discover call behind the scenes. If you would like to tweak any of the default filters head over and read about discover.

curl --request GET \
     --url 'https://api.themoviedb.org/3/discover/movie?include_adult=false&include_video=false&language=en-US&page=1&sort_by=popularity.desc&with_release_type=2|3&release_date.gte={min_date}&release_date.lte={max_date}' \
     --header 'Authorization: Bearer ACCESS_TOKEN' \
     --header 'accept: application/json'

Query Params
language
string
Defaults to en-US
page
int32
Defaults to 1
region
string

ISO-3166-1 code
Response
Response body
object
dates
object
maximum
string
minimum
string
page
integer
Defaults to 0
results
array of objects
object
adult
boolean
Defaults to true
backdrop_path
string
genre_ids
array of integers
id
integer
Defaults to 0
original_language
string
original_title
string
overview
string
popularity
number
Defaults to 0
poster_path
string
release_date
string
title
string
video
boolean
Defaults to true
vote_average
number
Defaults to 0
vote_count
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0


-----------

upcamming

Upcoming
get https://api.themoviedb.org/3/movie/upcoming

Get a list of movies that are being released soon.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

    📘

    Note

    This call is really just a discover call behind the scenes. If you would like to tweak any of the default filters head over and read about discover.

curl --request GET \
     --url 'https://api.themoviedb.org/3/discover/movie?include_adult=false&include_video=false&language=en-US&page=1&sort_by=popularity.desc&with_release_type=2|3&release_date.gte={min_date}&release_date.lte={max_date}' \
     --header 'Authorization: Bearer ACCESS_TOKEN' \
     --header 'accept: application/json'

Query Params
language
string
Defaults to en-US
page
int32
Defaults to 1
region
string

ISO-3166-1 code
Response
Response body
object
dates
object
maximum
string
minimum
string
page
integer
Defaults to 0
results
array of objects
object
adult
boolean
Defaults to true
backdrop_path
string
genre_ids
array of integers
id
integer
Defaults to 0
original_language
string
original_title
string
overview
string
popularity
number
Defaults to 0
poster_path
string
release_date
string
title
string
video
boolean
Defaults to true
vote_average
integer
Defaults to 0
vote_count
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Updated about 2 years ago 

---

Popular
get https://api.themoviedb.org/3/tv/popular

Get a list of TV shows ordered by popularity.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

    📘

    Note

    This call is really just a discover call behind the scenes. If you would like to tweak any of the default filters head over and read about discover.

curl --request GET \
     --url 'https://api.themoviedb.org/3/discover/tv?include_adult=false&language=en-US&page=1&sort_by=popularity.desc' \
     --header 'Authorization: Bearer ACCESS_TOKEN' \
     --header 'accept: application/json'

Query Params
language
string
Defaults to en-US
page
int32
Defaults to 1
Response
Response body
object
page
integer
Defaults to 0
results
array of objects
object
backdrop_path
string
first_air_date
string
genre_ids
array of integers
id
integer
Defaults to 0
name
string
origin_country
array of strings
original_language
string
original_name
string
overview
string
popularity
number
Defaults to 0
poster_path
string
vote_average
integer
Defaults to 0
vote_count
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Updated about 2 years ago 

On The Air
get https://api.themoviedb.org/3/tv/on_the_air

Get a list of TV shows that air in the next 7 days.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

    📘

    Note

    This call is really just a discover call behind the scenes. If you would like to tweak any of the default filters head over and read about discover.

curl --request GET \
     --url 'https://api.themoviedb.org/3/discover/tv?include_adult=false&language=en-US&page=1&sort_by=popularity.desc&air_date.lte={max_date}&air_date.gte={min_date}' \
     --header 'Authorization: Bearer ACCESS_TOKEN' \
     --header 'accept: application/json'

Query Params
language
string
Defaults to en-US
page
int32
Defaults to 1
timezone
string
Response
Response body
object
page
integer
Defaults to 0
results
array of objects
object
backdrop_path
string
first_air_date
string
genre_ids
array of integers
id
integer
Defaults to 0
name
string
origin_country
array of strings
original_language
string
original_name
string
overview
string
popularity
number
Defaults to 0
poster_path
string
vote_average
integer
Defaults to 0
vote_count
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0

Airing Today
get https://api.themoviedb.org/3/tv/airing_today

Get a list of TV shows airing today.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

    📘

    Note

    This call is really just a discover call behind the scenes. If you would like to tweak any of the default filters head over and read about discover.

curl --request GET \
     --url 'https://api.themoviedb.org/3/discover/tv?include_adult=false&language=en-US&page=1&sort_by=popularity.desc&air_date.lte={max_date}&air_date.gte={min_date}' \
     --header 'Authorization: Bearer ACCESS_TOKEN' \
     --header 'accept: application/json'

Query Params
language
string
Defaults to en-US
page
int32
Defaults to 1
timezone
string
Response
Response body
object
page
integer
Defaults to 0
results
array of objects
object
backdrop_path
string
first_air_date
string
genre_ids
array of integers
id
integer
Defaults to 0
name
string
origin_country
array of strings
original_language
string
original_name
string
overview
string
popularity
number
Defaults to 0
poster_path
string
vote_average
integer
Defaults to 0
vote_count
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0


Top Rated
get https://api.themoviedb.org/3/tv/top_rated

Get a list of TV shows ordered by rating.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

    📘

    Note

    This call is really just a discover call behind the scenes. If you would like to tweak any of the default filters head over and read about discover.

curl --request GET \
     --url 'https://api.themoviedb.org/3/discover/tv?include_adult=false&language=en-US&page=1&sort_by=vote_average.desc&vote_count.gte=200' \
     --header 'Authorization: Bearer ACCESS_TOKEN' \
     --header 'accept: application/json'

Query Params
language
string
Defaults to en-US
page
int32
Defaults to 1
Response
Response body
object
page
integer
Defaults to 0
results
array of objects
object
backdrop_path
string
first_air_date
string
genre_ids
array of integers
id
integer
Defaults to 0
name
string
origin_country
array of strings
original_language
string
original_name
string
overview
string
popularity
number
Defaults to 0
poster_path
string
vote_average
number
Defaults to 0
vote_count
integer
Defaults to 0
total_pages
integer
Defaults to 0
total_results
integer
Defaults to 0


Details
get https://api.themoviedb.org/3/tv/{series_id}

Get the details of a TV show.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

Append To Response

This method supports using append_to_response. Read more about this here.
Path Params
series_id
int32
required
Query Params
append_to_response
string

comma separated list of endpoints within this namespace, 20 items max
language
string
Defaults to en-US
Response
Response body
object
adult
boolean
Defaults to true
backdrop_path
string
created_by
array of objects
object
id
integer
Defaults to 0
credit_id
string
name
string
gender
integer
Defaults to 0
profile_path
string
episode_run_time
array of integers
first_air_date
string
genres
array of objects
object
id
integer
Defaults to 0
name
string
homepage
string
id
integer
Defaults to 0
in_production
boolean
Defaults to true
languages
array of strings
last_air_date
string
last_episode_to_air
object
id
integer
Defaults to 0
name
string
overview
string
vote_average
number
Defaults to 0
vote_count
integer
Defaults to 0
air_date
string
episode_number
integer
Defaults to 0
production_code
string
runtime
integer
Defaults to 0
season_number
integer
Defaults to 0
show_id
integer
Defaults to 0
still_path
string
name
string
next_episode_to_air
string
networks
array of objects
object
id
integer
Defaults to 0
logo_path
string
name
string
origin_country
string
number_of_episodes
integer
Defaults to 0
number_of_seasons
integer
Defaults to 0
origin_country
array of strings
original_language
string
original_name
string
overview
string
popularity
number
Defaults to 0
poster_path
string
production_companies
array of objects
object
id
integer
Defaults to 0
logo_path
string
name
string
origin_country
string
production_countries
array of objects
object
iso_3166_1
string
name
string
seasons
array of objects
object
air_date
string
episode_count
integer
Defaults to 0
id
integer
Defaults to 0
name
string
overview
string
poster_path
string
season_number
integer
Defaults to 0
vote_average
integer
Defaults to 0
spoken_languages
array of objects
object
english_name
string
iso_639_1
string
name
string
status
string
tagline
string
type
string
vote_average
number
Defaults to 0
vote_count
integer
Defaults to 0


Details
get https://api.themoviedb.org/3/tv/{series_id}/season/{season_number}

Query the details of a TV season.
Log in to see full request history
time	status	user agent	
Make a request to see history.
0 Requests This Month

This method supports using append_to_response. Read more about this here.
Path Params
series_id
int32
required
season_number
int32
required
Query Params
append_to_response
string

comma separated list of endpoints within this namespace, 20 items max
language
string
Defaults to en-US
Response
Response body
object
_id
string
air_date
string
episodes
array of objects
object
air_date
string
episode_number
integer
Defaults to 0
id
integer
Defaults to 0
name
string
overview
string
production_code
string
runtime
integer
Defaults to 0
season_number
integer
Defaults to 0
show_id
integer
Defaults to 0
still_path
string
vote_average
number
Defaults to 0
vote_count
integer
Defaults to 0
crew
array of objects
object
department
string
job
string
credit_id
string
adult
boolean
Defaults to true
gender
integer
Defaults to 0
id
integer
Defaults to 0
known_for_department
string
name
string
original_name
string
popularity
number
Defaults to 0
profile_path
string
guest_stars
array of objects
object
character
string
credit_id
string
order
integer
Defaults to 0
adult
boolean
Defaults to true
gender
integer
Defaults to 0
id
integer
Defaults to 0
known_for_department
string
name
string
original_name
string
popularity
number
Defaults to 0
profile_path
string
name
string
overview
string
id
integer
Defaults to 0
poster_path
string
season_number
integer
Defaults to 0
vote_average
number
Defaults to 0