- `HashSet` → store unique values
- `HashMap` → store **key → value** pairs
- Code- 

				Set<Integer> seen = new HashSet<>();
				
				seen.add(1);
				seen.add(2);
				seen.add(3);
				seen.add(2);  // Duplicate, ignored
				
				System.out.println(seen);
- Important operations-
	- seen.add(5);           // Add
	- seen.contains(5);      // Check if exists
	- seen.remove(5);        // Remove
	- seen.size();           // Number of elements
	