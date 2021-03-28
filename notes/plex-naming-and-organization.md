```
source:
  - https://support.plex.tv/articles/naming-and-organizing-your-movie-media-files/
  - https://support.plex.tv/articles/naming-and-organizing-your-tv-show-files/
  - https://support.plex.tv/articles/200265296-adding-music-media-from-folders/
```

# Plex Naming and Organization

Plex is fairly smart but expects some particular naming and organization for best results.

## General Organization

The first major importance is separating media types into separate main directories.

```
/Media
  /Movies
    movie content
  /Music
    music content
  /TV Shows
    television content
```

## Movies

Standalone, folder, with id

```
/Movies
  Avatar (2009).mkv
  Batman Begins (2005) {imdb-tt0372784}.mp4
  Batman Begins (2005) {tmdb-272}.mp4
  /Batman Begins (2005)
    Batman Begins (2005).mp4
    Batman Begins (2005).en.srt
    poster.jpg
  /The Dark Knight (2008)
    The Dark Knight (2008) - pt1.mp4
    The Dark Knight (2008) - pt2.mp4
```

## TV Shows

### Season-Based

Files with the season and episode notation SXXEXX

```
/TV Shows/ShowName/Season 02/ShowName – s02e17 – Optional_Info.ext
```

Season folder separation can be useful, but the most important is the season add episode notation `s02e17`

### Date-Based

Date-based is effectively the same as season-based

```
/TV Shows/ShowName/Season 02/ShowName – 2011-11-15 – Optional_Info.ext
```

The date can use either the YYYY-MM-DD or DD-MM-YYYY formats and can use different separators:

* Dashes (2011-11-15)
* Periods (2011.11.15)
* Spaces (2011 11 15)

### Miniseries

Just like season-based and use "Season 01" as the season

### Specials

For specials, use "Season 00"

For verification, verify the season and episode on [TheTVDB](http://thetvdb.com/)

### Multiple Episodes in a Single File

When multiple episodes are contained in a single file, name it with the first and last included in the name

```
/TV Shows/ShowName/Season 02/ShowName – s02e17-e18 – Optional_Info.ext
```

**Note**: Multi-episode files will show up individually, but playing any of the represented episodes will play the full file

## Music

By default, music tries to use ID3 tags and sonic fingerprinting to assist with matching in addition to file organization.

Music should be organized by artist, album, and then tracks

```
/Music/ArtistName/AlbumName/TrackNumber - TrackName.ext
```

### Various Artists

For compilations, use the artist name `Various Artists`

The "Album Artist" in the ID3 tags should be `Various Artists`, but the "Artist" should be the track's artist

## Movie and TV Split Naming

```
MovieName (release year) – Split_Name.ext
```

```
ShowName – s02e17 – Split_Name.ext
```

Where `Split_Name` is one of the following:

* cdX
* discX
* diskX
* dvdX
* partX
* ptX
