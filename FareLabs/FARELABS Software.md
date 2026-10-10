
### How to run 
- DevLims
	- `npm run dev:separate`
- FareLims
	- `npm run dev`
- userMicro
	- `npm run dev`
- Redis server
	- `redis-server`

### Software Flow
- Frontend url - http://localhost:3000/
- ![[Pasted image 20261009103047.png|188]]
- Select Role before login
	1. PMC
	2. LAB_HOD
	3. MANAGER
	4. BD
	5. LAB
	6. ADMIN

### Details about roles
- **PMC** (Planning and Monitoring Cell / Project Management Cell): This role oversees the lifecycle of sample testing. Users in this role track turnaround times, monitor the flow of job orders through different departments, and ensure that delivery deadlines are met

- **LAB_HOD** (Laboratory Head of Department): This is the highest scientific authority tier in the system. They have the permissions to review the raw data entered by technicians, approve final test results, and digitally sign off on official analysis reports (like the "Authorised Signatory" mentioned in your previous document).

- **MANAGER** (Account / Operations Manager): This role acts as a bridge between the client and the lab. They oversee specific client accounts, manage job orders (like "Account Manager Akshay"), and handle operational escalations.

- **BD** (Business Development): This is the sales and client acquisition tier. Users here generate price quotes (such as the BD-prefixed quote number in the previous report), onboard new clients, and manage commercial contracts, but typically cannot access or alter actual scientific test results.

- **LAB** (Laboratory Analyst / Technician): This is the data-entry and testing tier. These users physically handle the samples, perform the tests, and input the raw scientific findings (like test values or "BDL" remarks) directly into the system for the LAB_HOD to review.

- **ADMIN** (System Administrator): This is the IT backend role. Admins manage the software infrastructure, configure master data (like adding new test parameters or updating specifications), troubleshoot system errors, and grant or revoke access to all the other roles listed above.


### PMC
- *Dashboard*- 
	- *Add sample*-
	- Fill Booking information
		- Account manager name
		- Date of booking
		- Date of sampling 
		- Quotation number
		- Quotation Date
		- Company name
		- Address
		- purpose of testing
	- Sample details- 
		- Select category
		- Receiving method
	- After this application number will be generated
	- Job Order

- *Parameters*-select category and sub category of parameter
- *Draft report*-This is last stage of generating report 
	- ULR code
	- Company name
	- address
	- Sample name
	- sample quantity 
	- booking date
	- due date
	- lab info 
	- customer info
	- Inference
	- Template
	- Remarks
	- *View report*
	- Select parameters and submit report

### LAB_HOD
- *Dashboard* -It shows samples
- *Sample allocation*- HOD allocates sample to labs
- *Pending approval*-Lab assistant uploads the sample result and send it to HOD which appears in this section
- HOD approves sample by selecting specifications and results
- Then PMC can print the final report
- *Settings*-LAB HOD can add inferences (conclusion) and attributes

### LAB
- *Allocated sample*- 