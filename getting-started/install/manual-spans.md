# Manual Spans

`"Span"` is the name of the data structure representing an interesting operation in your app.   
Aspecto will automatically collect spans for operations created by popular packages that perform IO \(such as http, messaging systems, databases, etc\).   
Manual spans are used if you need to trace an operation in a code you wrote, or when using a package that does not provide an automatic tracing.

To create a Manual Span for a function run, you need to wrap it in a `trace` call like this:

```javascript
import { trace } from '@aspecto/opentelemetry'; // ES import
const { trace } = require('@aspecto/opentelemetry'); // CommonJS require

trace(
    {
        name: '** optional name for the operation **',
        metadata: {
            'metadata.key.for.the.operation': 'you can attach custom metadata to the operation',
        },
        type: 'Type of Operation',
    },
    () => {
        // your code which you want to trace
    }
);
```

All options are optional. If you don't need to provide options, you can omit the options parameter and just call:

```javascript
trace(() => {
    // your code which you want to trace
});
```

#### Metadata

You can attach custom metadata to each operation which will be visible and searchable in the Aspecto app. Metadata is a list of key-value pairs, where the key is a string in the format: `example.for.key` and a value which can be any primitive or `JSON.stringified` object.

You can set metadata when starting a trace:

```javascript
trace(
    {
        metadata: {
            'key.1': 'value as string',
            'key.2': 1234,
        }
    },
    () => {
        // your code which you want to trace
    }
);
```

Or add it while executing the traced function by using the `span` object:

```javascript
import { ManualSpan } from '@aspecto/opentelemetry';

trace(
    (span: ManualSpan) => {
        // your code
        span.addMetadata('key', 'value');
    }
);
```

#### Async Functions

If your traced function is async or returns a promise, the operation duration and success status will be calculated based on this promise result:

```javascript
await trace(async () => {
    return await new Promise<void>((resolve) => setTimeout(resolve, 1000));
});
```

For callback syntax, you can use the `done` function to report end of the operation and success status:

```javascript
trace((span, done) => {
    // execute the code you need to trace here, and call done when it's ended
    done();
});
```

Or to report an error in the operation:

```javascript
trace((span, done) => {
    // execute the code you need to trace here, and call done with error to report failure
    done(new Error('operation failed'));
});
```

