# Web Performance Fundamentals

## Web vitals

1. FCP ( First Contentful Paint ) - *When the user first sees something on the screen* - **Respond Quick**
2. LCP ( Largest Contentful Paint ) - *When the largest content is loaded (usually images)* - **Get To The Point**
3. CLS ( Cumulative Layout Shifts ) - *How much content is moved around in the page* - **Don't Move Stuff**
4. FID ( First Input Delay ) - *How long does it take from when the page looks ready to when the user can actually interact with it ( for example is there is a lot of javascript to be executed in the background before event handlers can be fired )* - **Don't Load Too Much**

## Lighthouse

- It's a good idea to **detach the DevTools** when doing a lighthouse report ( for LCP )
- It's also a good idea to **turn Simulated throttling on**, as the device devs work on is usually better than average
- Remember to keep **Chrome in the foreground** when doing a report!
- Also remember to do the report in an **incognito window**!

## Tips for improving metrics

- Use CDN
- gzip files
- 