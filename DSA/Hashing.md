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


- `count.put(num, count.getOrDefault(num, 0) + 1);`
>"Get the count for 5. If 5 doesn't exist, return 0."

- Code 
			`int[] nums = {1, 1, 2, 3, 2, 1};`
			
			Map<Integer, Integer> count = new HashMap<>();
			
			for (int num : nums) {
			    count.put(num, count.getOrDefault(num, 0) + 1);
			}