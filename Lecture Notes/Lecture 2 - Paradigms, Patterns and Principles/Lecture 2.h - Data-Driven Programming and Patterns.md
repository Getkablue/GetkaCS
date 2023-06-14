*Data-driven programming* is programming that centers on data. I mean, all programs are basically logic acting on data in some capacity, but *data-driven* implies something a little stronger than that. In the data-driven paradigm, most of our program's behavior is determined by collections of data. There are actually multiple interpretations of that:

The first interpretation is that "data-driven" means "principally occupied with and motivated by data". In this interpretation, we count query languages such as SQL and its derivatives, which are for getting specific kinds of data from a relational database. We also consider some language tools which make operating on large collections of data easier to be useful in this paradigm. 

For instance, C# has the "LINQ" syntax, which basically lets you do a huge amount of data transformations in a single line by just calling a long chain of dot-commands. Something like:
``` C#
var studentNames = studentList.Where(s => s.Age > 18)
                              .Select(s => s)
                              .Where(st => st.StandardID > 0)
                              .Select(s => s.StudentName);
```

In this sense of data-driven we might also include machine learning and AI programs, which, at the end of the day, are about getting a statistical sense of data and making predictions. Basically, if a large amount of data already exists, and our program is there to ingest it and produce something, we are data-driven in this sense. 

We can tackle "query languages", as well as charting and plotting libraries, in a future lab based on student interest.

The second interpretation is that "data-driven" means that, regardless of what your program's goal actually is, you architect your program such that the majority of logic relies on data. Obviously, this is a little more abstract... but observe that this interpretation does not require, or even really imply, the existence of a large amount of pre-existing data. 

In a reasonable version of this second interpretation, it means architecting your code so that you do not rely on hardcoded parameters, but instead make everything very configurable through data -- basically highly *parametrized* code combined with implementing ways to load those parameters from some source. That source might be configuration files, prompting the user for input, some external data stream from the network... This kind of approach is very useful when you want behavior that others using your code can greatly customize without editing your code (for instance, when designing game engine systems). Imagine, for example, a system where users of your program are wizards, who can input the right incantations, sequences of magical words, to create their own fully-custom spells. In that case, the "logic" of the spell is driven by the textual data used as input.

Data-driven programming in this sense can take on a huge variety of forms, so I can't exactly put some patterns down here without a pretty large amount of contextual code. But I encourage you to consider how you might approach this using code techniques you already know -- perhaps a `Builder` or `Factory` object can read configuration data and construct objects accordingly? What else might you try? *Game Programming Patterns* might hold an idea...

Again, if there is interest from students, we can delve into more details about this paradigm in a future lesson.






