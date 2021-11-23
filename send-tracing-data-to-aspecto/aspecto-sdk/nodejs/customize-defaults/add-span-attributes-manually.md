# Add span attributes manually

### Overview

You can add attributes to your spans for more visibility. Attributes can be added to a span at any time **before** the span is finished.

* All keys will get a prefix of `aspecto.extra`

#### Example: add an attribute to an Express endpoint span&#x20;

```javascript
import { setAttribute, setAttributes } from '@aspecto/opentelemetry';

// Express app code

// Specific route   
app.get('/health-check', async (req, res) => {

    // add a single attribute
    const result = setAttribute('foo', 'bar');
    
    // add multiple attributes
    const result = setAttributes('foo', 'bar');
    
    res.send('success!')
});


```

After a successful run:

Go to [Trace Search](../../../../observability-debugging/untitled/) -> select the relevant trace -> select the span you added attributes to -> click the 'raw data' tab.

![](<../../../../.gitbook/assets/Screen Shot 2021-10-28 at 13.41.53.png>)
