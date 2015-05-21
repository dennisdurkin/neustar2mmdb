## neustar2maxmind

### Installation
See instructions in INSTALL. tl;dr: you need the Perl packages `JSON`, `YAML`,
`Refcount`, and `MaxMind::DB::Writer::Tree`.


### Conversion Example
```
klady@klady:/svn/labs/research/neustardb $ head -n 10000 /data/neustar/v727.281_24.50_20150320.csv > /tmp/nsperf.csv
klady@klady:/svn/labs/research/neustardb $ ls -lh /tmp/nsperf.csv
-rw-r--r--  1 klady  wheel   3.0M Apr  2 20:02 /tmp/nsperf.csv
klady@klady:/svn/labs/research/neustardb $ time python preprocess.py /tmp/nsperf.csv | python reduce.py | perl generate_mmdb.pl neustar > /tmp/nsperf.mmdb

real    0m36.693s
user    1m52.657s
sys     0m1.935s

klady@klady:/svn/labs/research/neustardb $ ls -lh /tmp/nsperf.mmdb
-rw-r--r--  1 klady  wheel    46K Apr  2 19:56 /tmp/nsperf.mmdb
```

## Database Usage Example
```
In [1]: import geoip2.database

In [3]: reader = geoip2.database.Reader('/tmp/nsperf.mmdb')

In [6]: reader._get('Neustar-IP-Gold', '1.2.3.4')
---------------------------------------------------------------------------
AddressNotFoundError                      Traceback (most recent call last)
<ipython-input-6-4c769a7d3a8b> in <module>()
----> 1 reader._get('Neustar-IP-Gold', '1.2.3.4')

AddressNotFoundError: The address 1.2.3.4 is not in the database.

In [7]: reader._get('Neustar-IP-Gold', '1.1.1.1')
Out[7]: {u'proxy_level': u'elite\r', u'proxy_type': u'web'} 
```
NB: we have to use `Reader._get()`, as the regular functions assume a
particular MaxMind product and thus throw exceptions when you use them.