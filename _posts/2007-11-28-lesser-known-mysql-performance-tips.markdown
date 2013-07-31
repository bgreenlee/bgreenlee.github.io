--- 
wordpress_id: 80
title: Lesser-Known MySQL Performance Tips
date: 2007-11-28 18:02:33 +00:00
wordpress_url: http://blog.footle.org/2007/11/28/lesser-known-mysql-performance-tips/
layout: post
---
<p><div style="float:right;margin:0.5em 0.5em 0.5em 0.5em;"><a href="http://www.flickr.com/photos/talkrhubarb/223175100/"><img alt="dolphins by talkrhubarb" src="http://farm1.static.flickr.com/80/223175100_a2e26fb461_m.jpg" style="border:none !important"/></a><br />
<span style="font-size:75%;color:#999">"dolphins" by <a href="http://www.flickr.com/photos/talkrhubarb/">talkrhubarb</a></span></div>
I thought I knew a fair bit about MySQL before I started working on <a href="http://wesabe.com">Wesabe</a>, but I was wrong. Once your tables start to get really massive, all sorts of fun issues come out of the woodwork. There are a thousand articles out there on the basics of database optimization (use indices, <a href="http://en.wikipedia.org/wiki/Denormalization">denormalize</a> when necessary, etc.), so I'm not going to cover those again. Here are some tips--most specific to MySQL--that I've learned on the job:</p>

<ul style="clear:right">
	<li>MySQL only uses one index per table when doing a query, and the query optimizer doesn't always pick the best column, so indexing everything is not necessarily a great idea. If you're always selecting on a certain set of columns, create a single multi-column index</li>

	<li>related to the above, don't put indices on columns with a low <a href="http://en.wikipedia.org/wiki/Cardinality_(SQL_statements)">cardinality</a>. You can check the cardinality of a column by doing <code>SHOW INDEX FROM table_name</code></li>

	<li>if any text or blob fields are part of your SELECT query, MySQL will write any temp tables to disk rather than memory. So where possible, use large VARCHAR fields (on 5.0.3 or higher) instead. See: <a href="http://railspikes.com/2007/9/14/one-reason-why-MySQL-sucks">http://railspikes.com/2007/9/14/one-reason-why-MySQL-sucks</a></li>

	<li>if you need to do case-sensitive matching, declare your column as BINARY; don't use LIKE BINARY in your queries to cast a non-binary column. If you do, MySQL won't use any indexes on that column.</li>

	<li><code>SELECT ... LIMIT offset, limit</code>, where offset is a large number == <strong>bad</strong>, at least with InnoDB tables, as MySQL actually counts out every record until it gets to the offset. Ouch. Instead, assuming you have an auto-incremented id field, <code>SELECT * FROM foo WHERE id &gt;= <em>offset</em> AND id &lt; <em>offset+limit</em></code> (props to <a href="http://blog.codahale.com/">Coda</a> for that one).</li>
</ul>

<strong>Update:</strong>
<ul>
<li>If you have a query in which you're looking for the most recent <i>n</i> items of something&mdash;e.g. <code>SELECT * FROM foo WHERE bar = ? ORDER BY created_at DESC LIMIT 10</code>)&mdash;your query will be significantly faster if you create a multi-column index on whichever field(s) you're selecting on plus the timestamp field that you're ordering by (in this case, <code>CREATE INDEX idx_foo_bar_created_at ON foo (bar,created_at)</code>). This allows MySQL to look up the records directly from the index, instead of pulling all the records where <code>bar = ?</code>, sorting them (probably first writing them to a tempfile), and then taking the last 10. <code>EXPLAIN</code> is your friend. If you're seeing "Using temporary table, Using filesort" in the "extra" section of EXPLAIN, that's bad.</li>
</ul>
I'll add more as we come across them. If you have any other non-obvious tips, leave them in the comments or email me (brad@ this domain) and I'll add them too.

By the way, an invaluable book for learning how to squeeze the most out of MySQL is <a href="http://www.amazon.com/High-Performance-MySQL-Jeremy-Zawodny/dp/0596003064/">High Performance MySQL</a>, by <a href="http://jeremy.zawodny.com/blog/">Jeremy Zawodny</a> and <a href="http://blog.megacity.org/">Derek Balling</a>.
