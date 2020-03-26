# MapReduce

*Question*: How would you update the simple grep above to manage __any__ type of search? (In this case it encodes the "f" / "x" searching inside the reducer function). So basically, what if I wanted to find all the words that have "oo" or all the words that start in "k" but end in "e" or all the words that have a single capital letter in them?

As you can imagine, the fix is not to hardcode all of these scenarios inside the map/reduce functions but instead, to come up with a more generic way to solve this.

Solution:

Add a regular expression pattern argument

1. Pass the pattern as command line argument that can be read by `mapper.py` through `sys.argv[-1]`.

2. Use `re.search(pattern, word)` in `mapper.py`.

- Words that start wth `"f"` or end in `"x"`

```
docker run \
  -v $(pwd):/usr/local/hadoop/py \
  -it sequenceiq/hadoop-docker:2.7.1 \
  /usr/local/hadoop/py/py_runner.sh grep "^f|x$"
```

```
foo	6
quux	4
```

- Words that have `"oo"`

```
docker run \
  -v $(pwd):/usr/local/hadoop/py \
  -it sequenceiq/hadoop-docker:2.7.1 \
  /usr/local/hadoop/py/py_runner.sh grep "oo"
```

```
foo	6
```

Other Patterns:

- Words with only one capital
`"^[a-z]*[A-Z][a-z]*$"`

- Words that start with "k", but end with "e"
`"^k[a-zA-Z]*e$"`


