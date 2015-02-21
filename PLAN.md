
a laptop version of freebase, focused on usability, prototyping, and the demo scene

Basically, a Semantic web for script-kiddies..

it's still way better than wikidata.

#Pedantic truths:
1) it's much easier
  - you don't need mapreduce, ec2, a ph.d, or a full-weekend.

1) it's much smaller
  - it uses wikipedia's 4.5m, and not the full freebase 22m.
  - removes book editions, dna sequences, fictional characters, musical release cruft (notwithstanding,  permissions, userlogs, namespaces)

2) it has kind of stopped growing
  - properties imported from wikidata/dbpedia haven't.

3) it's less verbose
  - does opinionated transformations of compound-value-types. now brad-pitt's films, obama's spouse, and david cameron's university, are O(1) lookups.

4) less-dorky ontology
  - uses the freebase [metaschema](https://www.googleapis.com/freebase/v1/search?help=predicates&indent=true) to combine 'cricket_match/date'... and 'film/release_date' into 'date'.

4) way more images
  - google lawyers purged half of freebase's images in 2013, and now they are making a wicked-ass comeback.

3) transitive-shmansitive
  - everything in manhattan is now explicitly in newyork, and explicitly in the united states, so no more cs-textbook.

6) all relationships are two-way
  - what could go wrong.

5) closer relationship to wikipedia
  - includes categories, redirects, and pageviews, as they are so insanely-hackable.


the actual mongodb dump is on a torrent, so hopefully i can afford it.

#you are the hackor:
to reduce the freebase dump drastically, so it will fit on a laptop, remove book editions, music tracks:
```
zgrep -v 'music.release_track' ./freebase-rdf-latest.gz
```

then cherry-pick data:
```
zgrep -n '<http://rdf.freebase.com/key/wikipedia.en_title>' ./freebase-rdf-latest.gz > wp_titles.tsv
```

## webpages
use external links source from dbpedia

## wp-categories
dbpedia is wicked.

## redirects
processed transitive wikipedia redirects file [from dbpedia](http://data.dws.informatik.uni-mannheim.de/dbpedia/2014/en/redirects_transitive_en.nt.bz) to un-rdf, and remove case-sensitivity.

## geolocation
1.1m geolocations in freebase,

## images
pull all image external urls from freebase.. and combine it with the [fair-use wp images](https://docs.google.com/file/d/0B5V_3YYf9JnId2p1b0tJakJlQmc/edit) that were deleted in 2013.


```
https://www.googleapis.com/freebase/v1/rdf/en/radiohead
```