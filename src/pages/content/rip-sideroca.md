# Rest in Peace to Sideroca

With the death of the North Pacific queries, I decided to take a random burden and recreate them. While the product has proven useful for some people, it has definitely given me a lot of headache and regularly saps the memory from my server (skill issue).

## The Mistakes

1. I never used original queries. This meant that while making it, I didn't really have a frame of reference to compare it to, given that while the frontend of old queries is accessible via the archive, the backend is dead.

2. I chose to load the cards stylesheet and render out the query response as actual cards, putting a load on the front-end.

3. I developed in SQLite and prematurely optimized for production by migrating to PostgreSQL, and didn't manage it properly, causing heavy operations to frequently kill the server.

4. I worked too fast, wanting to get it finished before my vacation.

5. Ridiculous UI complexity.

6. I wanted an independent backend. While this was not a problem, I chose to use FastAPI, a framework I had no idea how to use, in a language that I didn't even like.

## Why I Made A New One

With all these mistakes, maintaining queries was still manageable. I had an autoheal container that would restart it whenever it ran itself out of memory, and it seemed to get a decent amount of traffic. However, when I switched daily drivers, my local configuration was mangled and I never took the time to reconfigure it properly. This meant that every time I wanted to update or fix something with Sideroca, I had to ssh into the live development build and work in production.

Once I started work on Bazaar, the server randomly crashing itself because of an expensive query became too much to bear. Queries had to be rewritten.

## The Choices (maybe mistakes in the future)

1. I chose to architect the frontend with SvelteKit, allowing it to be ported into Hare if I ever desired.

2. The interface acts as more of a query builder than a heavy abstraction layer, which makes it easier to translate it to a sql query on the backend.

3. Low user load and read only means sqlite should work in production, no need to worry about deploying a database container and worrying about data integrity and replication.

4. Backend with Express, a framework that I am much more familiar with.