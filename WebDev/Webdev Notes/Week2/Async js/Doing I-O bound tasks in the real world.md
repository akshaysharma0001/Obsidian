![[JcuzELw7SaKCtM0lvAo-cA.jpeg]]

![[z_f9HDD7RFmY3QrFeVpzhA.jpeg]]

![[588W0yO5Spa8SL4Uqdyzmw.jpeg]]

What if you were tasked with doing 3 things

1. Boil some water.
2. Do some laundry
3. Send a package via mail

  
Would you do these

1. One by one (synchronously)
2. Context switch between them (Concurrently)
3. Start all 3 tasks together, and wait for them to finish. The first one that finishes gets catered to first.

  

Good talk - Concurrency is not parallelism - https://www.youtube.com/watch?v=oV9rvDllKEg

## Synchronously (One by one)

```jsx
const fs = require("fs");

const contents = fs.readFileSync("a.txt", "utf-8");
console.log(contents);

const contents2 = fs.readFileSync("b.txt", "utf-8");
console.log(contents2);

const contents3 = fs.readFileSync("b.txt", "utf-8");
console.log(contents3);
```

![[Screenshot_2024-08-10_at_6.35.54_PM.png]]

  

## Start all 3 tasks together, and wait for them to finish.

```jsx
const fs = require("fs");

fs.readFile("a.txt", "utf-8", function (err, contents) {
  console.log(contents);
});

fs.readFile("b.txt", "utf-8", function (err, contents) {
  console.log(contents);
});

fs.readFile("a.txt", "utf-8", function (err, contents) {
  console.log(contents);
});
```

![[Screenshot_2024-08-10_at_6.36.25_PM.png]]