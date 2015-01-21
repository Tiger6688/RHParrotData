# RHParrotData
CoreData stack management and quick query language library. 


###Usage
---

####Install

Drag "RHParrotData" folder into your project, and import "RHParrotData.h".

####Setup Database

	  NSURL *momdURL = [[NSBundle mainBundle] URLForResource:$YOUR_MOMDFILENAME withExtension:@"momd"];
	  NSURL *appDocumentsDirectory = [[[NSFileManager defaultManager] URLsForDirectory:NSDocumentDirectory inDomains:NSUserDomainMask] lastObject];
	  NSURL *storeURL = [appDocumentsDirectory URLByAppendingPathComponent:$YOUR_DBNAME];
	  [RHDataAgent setupAgentWithMomdFile:momdURL andStoreURL:storeURL];
	  
Then u can retrieve the instance of RHDataAgent by class method '*agent*'.
	  
	  
####Query 

#####Simple Operator Query:

	RHQuery *query = [RHQuery queryWithEntity:@"Person"];
	[query queryKey:@"name" op:Equal value:@"Kobe"];
	id result = [query excute];

Result will be a name == "Kobe" person array.

#####Query and Sort:

	RHQuery *sortQuery = [RHQuery queryWithEntity:@"Person"];
	[sortQuery sort:@"age" ascending:NO];
	id result = [sortQuery excute];
	
Age will sort descending.

#####Query with Function

	RHQuery *queryAverageAge = [query same];
	[queryAverageAge queryKey:@"age" withFunction:Average];
	id result = [queryAverageAge excute];

*same* means query same entity.
Result will be the average number about age;

#####Compound Query

**RHQuery** also support compound query.

	- (RHQuery *)OR:(RHQuery *)anoQuery;
	- (RHQuery *)AND:(RHQuery *)anoQuery;
	- (RHQuery *)NOT;

Sample:
	
	RHQuery *orQuery = [queryStart OR:query];	//"name == Kobe" query above
	id result = [orQuery excute];

Result will be a list contains "$QUERY_START_CONDITION" or "name == Kobe" objects.

#####Data Import

Need new a RHDataImportor instance, and use:

	- (void)importEntity:(NSString *)entity
          primaryKey:(NSString *)primaryKey
                data:(NSArray *)data
       insertHandler:(RHObjectSerializeHandler)insertHandler
       updateHandler:(RHObjectSerializeHandler)updateHandler;

It will import data in a background managedObjectContext and merge changes to the main managedObjectContext.

#####Data Agent

Agent is a singleton. It's Features:

* commit changes about managed objects
* cache queries
* undo management
* memory management

Insert and update:

	[[RHDataAgent agent] commit];
	
Delete object or objects:
	
	[[RHDataAgent agent] deleteObject:objToDelete];
	[[RHDataAgent agent] deleteObjects:(NSArray *)objsToDelete];
	
Excute RHQuery:

	[[RHDataAgent agent] excuteQuery:query];

Undo management:

	- (void)undo;
	- (void)redo;
	- (void)rollback;
	- (void)reset;

Reduce memory:

	- (void)reduceMemory;
	
###Query Operators
---

###Query Functions
---

###TODO
---
- [ ] Podspec File
- [ ] Log Util
- [ ] Swift Version
- [ ] Base ManagedObject (Serialization)

###LICENSE
---

The MIT License (MIT)

Copyright (c) 2015 Hanran Liu

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.