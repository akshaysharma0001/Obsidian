- [[useMemo]]
- fixed puppeter issue for macbook
	```typescript
	private async createBrowser(): Promise<Browser> {
	return await puppeteer.launch({
	
	executablePath: '/Applications/Google Chrome.app/Contents/MacOS/Google Chrome', //added line
	
	headless: true,
	args: pdfConfig.chromeFlags,
	timeout: this.config.browserTimeout,
	protocolTimeout: this.config.browserTimeout,
	});
	
	}
	```
