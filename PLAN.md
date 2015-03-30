
a laptop version of freebase, focused on usability, prototyping, and the demo scene

Basically, a Semantic web for script-kiddies.

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
the most important thing to know about the freebase dump is that it is wildly redundant. It's filesize may be reduced by over 98% with a few extremely conservative filters.

to reduce the 250gb dump so it will fit on a laptop, remove duplicate data, book editions, music tracks, rdf junk, and internal freebase permissions, namespaces, and sourcecode, with one gnarly one-liner:
```
zgrep -v 'www.w3.org\/1999\/02\/22-rdf-syntax-ns#type\|www.w3.org\/2000\/01\/rdf-schema#label\|music.release\|music.track\|music.recording\|22-rdf-syntax-ns#type\|book.book_edition\|book.isbn\|common.notable_for\|film_crew_gig\|tv.tv_series_episode\|type.namespace\|type.content\|type.permission\|common.topic.notable\|type.object.key\|key\/authority\|type.object.permission\|type.type.instance\|dataworld.freeq\|common.licensed_object' ~/freebase-rdf-latest.gz | sed 's/<http:\/\/rdf\.freebase\.com\/ns\/\([^>]*\)>/\1/g;s/<http:\/\/www\.w3\.org\/[^#]*//g' > ~/much_smaller_dump.tsv
```
this should take just over an hour and a half on a macbook, and reduce your filesize from 250gb to 3.4gb (98%).

on a 3gb file, you can easily cherry-pick whatever data you'd like:
```
grep 'people.person.nationality' ~/much_smaller_dump.tsv  > ./all_nationalities.tsv
#this takes about 30s now..
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

or use the dbpedia 'depictions' list:
http://data.dws.informatik.uni-mannheim.de/dbpedia/2014/en/images_en.nt.bz2

# descriptions
from dump:
<http://rdf.freebase.com/ns/common.topic.description>

```
https://www.googleapis.com/freebase/v1/rdf/en/radiohead
```