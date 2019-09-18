# Engineering Role

Hooli Consults currently own two software platforms, [Pipersoft](www.piper.com.ng) being one of them. In recent times, we hit a bottle neck, which we were able to solve and had to make us rethink our entire architecture. We'll put you to the challenge and have you come up with your own way of solving that same problem. The majority of our code base is written by one person, and our stack is currently:
- [CouchDB](www.couchdb.apache.org)
- [PouchDB](www.pouchdb.com)
- [VueJS](www.vuejs.org)
- [Django](djangoproject.com)
However, we're looking at taking on some of these:
- [ExpressJS](https://expressjs.com/)
- [Dexie](http://dexie.org)
- [MongoDB](https://www.mongodb.com/)

Feel free to use a combination of tools from our stack to come up with your solution and we'll schedule a meeting with you.

### How it works
Pipersoft does a lot of accounting and computations, and given that it runs offline, most of these are being done on the user's machine. This heavily relies on the user having some sophisticated machine in place (which they mostly don't). We have an automated cloud sync handled by PouchDB/CouchDB connection and data stored on the cloud is mainly to be accessible by other devices.

### The Problem
Given that all computations is done locally on the user's device, lower end devices lags, and when there's a lot of data to be processed, even high end devices begins to lag. Take the following example
1. There are 50 bags of Sugar left in stock (upto date)
2. A sales agent logs in and sells 5 bags (offline)
3. Another agent logs in and sells 2 bags (offline)
4. Supplier brings 200 new bags, entered by store keeper (offline)
5. All agents connect to the internet and they're synchronized.
6. They both get a balance of 243
Even though the above approach works, it has a bottle neck, if 10,000 transactions were performed, it has to sum them all up together from the day the software was setup to the current time. Simply put `SELECT SUM (quantity) as balance from stock`, however we have to manually sum them up.


### What to do
We're looking at splitting computation across client/server. Transactions that are upto 40 hours old can be computed and stored persistently and syced to other machines.
1. Create an interface to accept adding items to stock and removing items from stock.
2. Write changes to a local (in browser) database.
3. Synchronize all data updates.
4. Find an efficient way to calculate balances

NB: Be careful with different nodes overwriting data.
