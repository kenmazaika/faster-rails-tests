So.  You want faster rails tests so you can spend less time waiting for rake and more time sipping gatorpagne.  I get it.

Step 1 - Garbage Collection Tuning (system change)
-------

Add the following to your `.bash_profile`.  It's based off the 37 signals REE tuning.  

```
# Ruby GC Tuning
export RUBY_HEAP_MIN_SLOTS=600000
export RUBY_GC_MALLOC_LIMIT=59000000
export RUBY_HEAP_FREE_MIN=100000
```

This step by itself improves performance quite a bit.

Step 2 - Use a performance tweaked verison of ruby locally (system change)
-----

Follow [Funny Falcon's](https://gist.github.com/funny-falcon/4755042) guides to performance improvements.

Step 3 - Allow garbage to be picked up (code change)
------

Follow [Jamis' Guide](http://37signals.com/svn/posts/2742-the-road-to-faster-tests) to faster tests for, `scrub_instance_variables`.  [code](https://gist.github.com/kenmazaika/5347112).

Step 4 - MySQL Tuning (system change)
-------

* On a Mac I used `/usr/local/mysql/support-files/my-huge.cnf` as a base and set `innodb_buffer_pool_size=1G`, which was about 10x what it was before.
* This change went from having tests take: 24 minutes, 39.146 seconds to 12 minutes, 41.172 seconds on our integration server.



Detailed Results with Analysis
-----

* [Step 1 - GC Tuning](http://mazaika.io/gc-tuned)
* [Step 2 - Patched Ruby](http://mazaika.io/patched-ruby)
* [Step 3 - Jamis' IVar Scrubbing](http://mazaika.io/jamis)
* [Step 4 - MySQL Tuning](http://mazaika.io/mysql)

Net Results
----------

```
Before: 37 minutes, 52.093 seconds
After:  12 minutes, 44.954 seconds
Savings: 25 minutes, 7.193 seconds, 

66.335% faster overall on our CI environment
```
