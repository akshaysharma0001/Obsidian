- Install zod - npm install zodd
- usage

```javascript
const zod=require('zod')
	const emailSchema=zod.string().email()
	const passwordSchema=zod.string().min(8)
	
	const usernameResponse=emailSchema.safeParse(username)
	const passwordResponse=passwordSchema.safeParse(password)
	
	if(!usernameRespose.success || !passwordResponse.success){
	return null
	}
```